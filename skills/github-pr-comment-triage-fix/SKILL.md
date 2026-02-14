---
name: github-pr-comment-triage-fix
description: GitHub PR の review comments を取得し、修正すべき指摘かを判定して、必要なら実装修正まで行うスキル。PRレビュー対応、指摘の取捨選択、修正着手順の整理を一貫して進めたいときに使う。
---

# GitHub PR コメント判定・修正スキル

このスキルは、PR コメントを収集し、修正要否を判定して必要な修正を実施する。

## 1. 実行順序

1. 対象 PR のコメントとレビュー状態を取得する。
2. `references/triage-rules.md` で各コメントを `must-fix / should-fix / won't-fix` に分類する。
3. `must-fix` を優先して修正する。
4. `should-fix` は影響と工数を見て修正可否を決める。
5. コメントごとに返信を返す（修正済みは `fixed`、不要は英語理由）。
6. `references/output-template.md` 形式で、判定根拠と実施内容を報告する。

## 2. コメント取得ルール

- 取得対象:
  - PR review comments（行コメント）
  - Review summary comments
  - Open conversation threads
- 取得手段は次の優先順で使う。
  1. GitHub MCP
  2. `gh` CLI
  3. API 直叩き（必要時のみ）
- 取得時は次を保持する。
  - comment ID
  - author
  - file/path と line
  - comment body
  - resolve 状態

## 3. 判定原則

- 判定は `references/triage-rules.md` を使う。
- 主観的な好みより、仕様整合性・正確性・安全性・回帰リスクを優先する。
- 既に同趣旨で対応済みのコメントは重複扱いにして統合する。
- 根拠不足で判断できないコメントは「追加確認」に振り分ける。

## 4. 修正実行ルール

- 修正対象は `must-fix` から着手する。
- 各修正は対応する comment ID を紐付ける。
- 変更後は関連テスト/検証を実施し、結果を記録する。
- `won't-fix` は却下理由を1行で明示する。

## 5. コメント返信ルール

- 修正したコメントには返信文を厳密に `fixed` とする。
- 不要と判断したコメントには英語で理由を返信する。
- 不要返信は `No change needed because <reason>.` 形式を使う。
- 仕様確認待ちの場合は英語で `Need clarification on <point> before applying changes.` を使う。

## 6. 出力要件

- コメント一覧（ID, 区分, 要約, 根拠, 対応方針, 返信文）
- 実施した修正（ファイル, 変更概要, 対応コメントID）
- 未対応コメントと理由
- 追加確認が必要な論点

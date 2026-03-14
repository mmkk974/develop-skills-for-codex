---
name: staging-to-main-pr
description: staging から main への統合 PR を安全に作成するスキル。release前の差分確認、重複PR確認、テンプレート適用、main 直マージ禁止を徹底したいときに使う。
---

# Staging to Main PR Skill

このスキルは `staging -> main` の統合PRだけを扱う。

## 1. 実行順序

1. 作業ツリーが clean であることを確認する。未コミット変更がある場合は停止する。
2. `staging` ブランチの存在と最新化状態を確認する。
3. `main` へ直接 push / merge しないことを確認する。
4. 既存の `head=staging` `base=main` の open PR を確認する。
5. 既存PRがなければ `gh pr create --base main --head staging` で作成する。
6. PR URL と確認結果を返す。

## 2. 運用ルール

- このスキルは `staging -> main` 以外のPRには使わない。
- `main` へ直接マージする提案・実行を禁止する。
- 必要な変更は必ず `staging` に取り込んだ後、この統合PRに載せる。

## 3. PRテンプレート運用

- `.github/pull_request_template.md` を優先利用する。
- テンプレートが無い場合は `references/pr-template-default.md` を使う。
- 影響範囲、検証結果、ロールバック方針を明記する。

## 4. 出力要件

- head/base
- 既存PRの有無
- 作成または再利用したPR URL
- 主要なリスクと検証項目

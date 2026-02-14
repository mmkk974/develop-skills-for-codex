---
name: sdd-progress-next-task
description: 仕様駆動開発（spec→plan→task→imple）の進捗を判定し、次に実行すべきタスクを返すスキル。途中成果物の棚卸し、フェーズゲートの通過判定、未完了項目の抽出、次アクション提示を一貫して行いたいときに使う。
---

# SDD 進捗確認・次タスク提示スキル

このスキルは、既存成果物を読んで「現在地」と「次の1手」を返す。

## 1. 実行順序

1. `references/artifact-discovery.md` で成果物を探索する。
2. `references/progress-gate-rules.md` でフェーズ進捗を判定する。
3. `references/next-task-rules.md` で次タスクを決定する。
4. `references/output-template.md` 形式で結果を返す。

## 2. 判定原則

- 判定基準は `spec-driven-development/references/phase-gates.md` を優先する。
- 最も早い未完了フェーズを現在フェーズとして扱う。
- 必須項目が未達なら後続フェーズを「進捗対象外」にする。
- 成果物が部分的に存在する場合、欠落項目を次タスクへ分解する。
- 追加仕様の要求（`CRQ-*`）が未処理で存在する場合は、通常フェーズ進行より先に差分同期タスクを返す。

## 3. 次タスク提示ルール

- 次タスクは「今すぐ着手する1件」を先頭に出す。
- 併せて、依存関係つきの候補タスクを最大3件まで提示する。
- 各タスクに完了条件を1行で付ける。
- 曖昧な要件がある場合は、質問タスクを最優先にする。
- 追加仕様がある場合は `sdd-change-request-integration` の実行タスクを最優先にする。

## 4. 出力要件

- 現在フェーズ
- フェーズ別進捗（spec/plan/task/imple）
- ゲート判定（pass/fail と根拠）
- 次タスク（最優先1件 + 候補）
- ブロッカー/未確定事項

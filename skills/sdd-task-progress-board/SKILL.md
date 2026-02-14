---
name: sdd-task-progress-board
description: spec-driven-development の task 成果物を一覧化し、タスク進捗と next task を同時に返すスキル。spec や plan が不十分な場合は task に進まず、不足項目と補完用 next task を返したいときに使う。
---

# SDD タスク一覧・進捗・次タスク提示スキル

このスキルは、SDD 成果物から task 一覧を作り、進捗と次タスクを返す。

## 1. 実行順序

1. `../sdd-progress-next-task/references/artifact-discovery.md` で成果物を探索する。
2. `references/prerequisite-gate-rules.md` で spec/plan の前提を判定する。
3. 前提が `pass` の場合のみ `references/task-progress-rules.md` で task 一覧と進捗を算出する。
4. `references/next-task-rules.md` で最優先 next task を決定する。
5. `references/output-template.md` 形式で結果を返す。

## 2. 判定原則

- ゲート基準は `../spec-driven-development/references/phase-gates.md` を唯一の根拠にする。
- `spec` または `plan` が `fail` の場合は、task 一覧より先に不足項目を返す。
- `task` の進捗は `TSK-*` 単位で算出し、根拠ファイルを必ず付ける。
- `TSK-*` の状態根拠が取れない場合は `todo` 扱いにし、情報不足として明示する。

## 3. 出力要件

- 前提判定（spec/plan pass/fail と不足項目）
- task 一覧（`TSK-*`、状態、依存、完了条件、根拠）
- task 進捗サマリ（total/done/in-progress/todo、進捗率）
- 最優先 next task（1件）
- 次タスク候補（最大3件）
- ブロッカー/未確定事項

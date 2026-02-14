# 前提ゲート判定ルール（spec/plan）

## 1. 判定対象

- `spec` ゲート
- `plan` ゲート

判定基準は `../spec-driven-development/references/phase-gates.md` に一致させる。

## 2. 判定手順

1. `spec` 成果物を評価し、チェック項目ごとに pass/fail を付ける。
2. `spec` が `fail` の場合、`plan/task/imple` は進捗対象外として扱う。
3. `spec` が `pass` の場合のみ `plan` を評価する。
4. `plan` が `fail` の場合、`task/imple` は進捗対象外として扱う。

## 3. 不足項目の出し方

- 未達チェックをそのまま列挙する。
- 各未達に「不足理由」と「補完 next task 候補」を1つ付ける。
- 補完 next task は最小作業単位に分解する。

## 4. 判定結果の取り扱い

- `spec/plan` のどちらかが `fail` の場合:
  - task 進捗率は算出しない。
  - task 一覧は任意表示に留める（根拠不十分として注記）。
  - 最優先 next task は前提不足の補完タスクにする。

# 進捗・ゲート判定ルール

`spec-driven-development/references/phase-gates.md` を基準に判定する。

## 1. 現在フェーズの決め方

0. 未処理の変更要求 `CRQ-*` がある場合は、現在フェーズを `change-request` とする。
1. `spec` を判定する。
2. `spec` が `pass` の場合のみ `plan` を判定する。
3. `plan` が `pass` の場合のみ `task` を判定する。
4. `task` が `pass` の場合のみ `imple` を判定する。
5. 最初に `fail` したフェーズを現在フェーズとする。
6. すべて `pass` の場合は「完了」とする。

## 2. 進捗率の目安

- `spec pass` のみ: 25%
- `plan pass` まで: 50%
- `task pass` まで: 75%
- `imple pass` まで: 100%
- `change-request` 未処理: 進捗率は据え置き（通常フェーズを進めない）

## 3. 判定根拠

- 各フェーズで「満たしたチェック項目」と「未達項目」を列挙する。
- 根拠はファイルパスと該当箇所で示す。

# task 一覧化・進捗算出ルール

## 1. task 抽出

- `TSK-*` 形式のタスクIDを抽出する。
- task テーブル、箇条書き、実装ログを横断して重複IDを統合する。
- 各タスクに次を紐付ける:
  - 依存関係
  - 完了条件
  - 状態根拠（ファイルパス）

## 2. 状態判定

- `done`:
  - `完了/done/completed` の明示、または
  - `TSK-*` に対応する実装記録とテスト結果の両方がある。
- `in-progress`:
  - `進行中/in progress/wip` の明示、または
  - 実装記録はあるが完了条件が未充足。
- `todo`:
  - 上記に該当しない。

状態が曖昧な場合は `todo` として扱い、根拠不足を明記する。

## 3. 進捗率

- `total = unique(TSK-*)`
- `done_count = status == done`
- `in_progress_count = status == in-progress`
- `todo_count = status == todo`
- `progress_percent = floor(done_count / total * 100)`

`total = 0` の場合は `progress_percent = 0` とし、task 成果物不足として扱う。

## 4. 表示優先順位

- `TSK-*` の番号昇順で並べる。
- 同番号が複数ソースにある場合は、より新しい更新日時の情報を優先する。

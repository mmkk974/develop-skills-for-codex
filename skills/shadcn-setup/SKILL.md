---
name: shadcn-setup
description: shadcn/ui の導入と運用を標準化するスキル。Tailwind 構成済みプロジェクトに shadcn/ui を追加するとき、必要コンポーネントを導入するとき、UIの拡張ルールを統一したいときに使う。
---

# shadcn 導入スキル

このスキルでは、初期導入からコンポーネント追加までを一貫して実施する。

## 1. 実行手順

1. Tailwind が有効化済みか確認する。
2. `references/setup-flow.md` の順で初期化する。
3. `references/component-guidelines.md` に沿ってコンポーネントを導入する。
4. `references/validation-checklist.md` で妥当性を判定する。

## 2. 実装ルール

- 初期化時の生成ファイル（`components.json`、`lib/utils` など）を確認する。
- コンポーネント追加は都度必要分のみ行い、一括大量追加しない。
- 生成された UI はプロジェクト規約に合わせて調整する。
- `className` の結合は `cn` を使って一貫化する。
- フォーム入力部品は React Hook Form と組み合わせる前提で設計する。

## 3. 出力要件

作業完了時に次を報告する。

- 追加/更新したファイル
- 実行したコマンド
- 導入したコンポーネント一覧
- 妥当性チェック結果（`pass` / `fail`）

---
name: tailwindcss-setup-usage
description: Tailwind CSS の導入と運用を標準化するスキル。新規プロジェクトで Tailwind をセットアップするとき、既存プロジェクトへ Tailwind を追加するとき、ユーティリティ設計や再利用ルールを整備したいときに使う。
---

# Tailwind CSS 導入・運用スキル

このスキルでは、導入作業と利用ルールを同時に整備する。

## 1. 実行手順

1. プロジェクト種別（Next.js/Vite/その他）とパッケージマネージャーを確認する。
2. `references/setup-matrix.md` から該当パターンの導入手順を適用する。
3. `references/style-guidelines.md` に沿って運用ルールを反映する。
4. `references/validation-checklist.md` で妥当性を判定する。

## 2. 実装ルール

- ベーススタイルは `globals.css` などの単一エントリに集約する。
- デザイン変数は CSS Variables として定義する。
- 長い class の組み立ては `cn` ヘルパー（`clsx` + `tailwind-merge`）を利用する。
- コンポーネント内のスタイル責務を明確にし、ページ単位の肥大化を避ける。
- カラートークン、余白、角丸などを任意値で乱発しない。

## 3. 出力要件

作業完了時に次を報告する。

- 追加/更新したファイル
- 実行したコマンド
- 妥当性チェック結果（`pass` / `fail`）
- 残課題（あれば）

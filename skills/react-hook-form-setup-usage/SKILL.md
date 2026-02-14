---
name: react-hook-form-setup-usage
description: React Hook Form の導入と実装パターンを標準化するスキル。フォーム実装を開始するとき、バリデーションを Zod と統合したいとき、送信処理やエラーハンドリングを統一したいときに使う。
---

# React Hook Form 導入・利用スキル

このスキルでは、導入と実装規約を同時に整える。

## 1. 実行手順

1. `react-hook-form` と必要依存を導入する。
2. `references/form-patterns.md` の基本構成でフォームを実装する。
3. `references/validation-checklist.md` で妥当性を判定する。

## 2. 実装ルール

- バリデーションは Zod + resolver を優先する。
- `defaultValues` を必ず定義する。
- サーバー送信エラーはフィールドエラーとフォーム全体エラーを分ける。
- UIコンポーネントが controlled 前提なら `Controller` を使う。
- 送信中状態（disable, loading）を明示する。

## 3. 出力要件

作業完了時に次を報告する。

- 追加/更新したファイル
- 実行したコマンド
- 採用したフォームパターン
- 妥当性チェック結果（`pass` / `fail`）

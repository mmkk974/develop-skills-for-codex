# 導入マトリクス

## Next.js（App Router）

1. Tailwind 関連パッケージを導入する。
2. `globals.css` に Tailwind の読み込みを設定する。
3. `app/layout.tsx` から `globals.css` を読み込む。
4. 開発サーバーを再起動する。

## Vite + React

1. Tailwind 関連パッケージを導入する。
2. CSS エントリに Tailwind の読み込みを設定する。
3. エントリポイント（`main.tsx` など）で CSS を読み込む。
4. 開発サーバーを再起動する。

## 補足

- Tailwind のメジャーバージョン差分で手順が変わる場合は、公式手順を優先する。
- 既存 PostCSS 構成がある場合は競合設定を確認する。

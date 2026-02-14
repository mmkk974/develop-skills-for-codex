---
name: nextjs-review-directory-architecture
description: Next.js App Router プロジェクトのディレクトリアーキテクチャをレビューするスキル。レビュー時は必ず nextjs-app-router-skill をベースラインとして適用し、FSDレイヤー規約、配置責務、import方向の妥当性を検証したいときに使う。
---

# Next.js ディレクトリアーキテクチャレビュー

このレビューは必ず次の順で実施する。

1. ベースライン確認:
`nextjs-app-router-skill/SKILL.md` と
`nextjs-app-router-skill/references/compliance-checklist.md`
を読み、違反を先に抽出する。
2. 専門観点レビュー:
`references/architecture-checklist.md` を使って構造面を評価する。
3. 報告:
`references/report-format.md` 形式で出力する。

## 判定原則

- 指摘は重大度順（Critical/High/Medium/Low）で列挙する。
- すべての指摘にファイルパスと行番号を付ける。
- 最小修正案を必ず添える。
- 指摘なしの場合も、残留リスクと監視点を記載する。

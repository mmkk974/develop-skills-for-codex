---
name: sdd-change-request-integration
description: 追加仕様（変更要求）を仕様駆動開発へ統合するスキル。開発途中の要件追加、要件変更、優先度変更が発生したときに、影響分析と差分specを作成し、plan/task/impleへ安全に反映したい場合に使う。
---

# 追加仕様統合スキル

このスキルは、追加仕様を受けたときに SDD の整合性を崩さず再計画する。

## 1. 実行順序

1. `references/change-intake-template.md` で変更要求を受理する。
2. `references/impact-analysis-checklist.md` で影響分析を実施する。
3. `references/delta-sync-rules.md` で `spec/plan/task/imple` へ反映する。
4. `references/output-template.md` 形式で結果を報告する。

## 2. 識別子ルール

- 変更要求ID: `CRQ-001`
- 追加/変更要件ID: `REQ-101` 以降を推奨
- 変更設計ID: `PLN-101` 以降を推奨
- 変更タスクID: `TSK-101` 以降を推奨

## 3. 判定原則

- 既存要件の上書きではなく、差分を明示する。
- 破壊的変更は互換性影響を必ず記録する。
- 未確定事項がある場合、実装に進まず確認タスクを優先する。
- 変更判断の論点ごとに推奨案/代替案を提案し、質問で意思決定を確定する。
- 差分反映後は `spec-driven-development/references/phase-gates.md` を再判定する。

## 4. 出力要件

- 変更要求の要約
- 影響分析結果（影響範囲、リスク、優先度）
- 差分反映対象ファイル
- 再判定したゲート結果
- 最優先の次タスク

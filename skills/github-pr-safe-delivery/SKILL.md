---
name: github-pr-safe-delivery
description: 実装・修正内容を安全に PR 化するスキル。ブランチ未作成時のブランチ作成、main 直push禁止、PRテンプレート確認/作成、個人情報チェック、commit→push→PR作成まで一連で実行したいときに使う。
---

# GitHub PR Safe Delivery Skill

このスキルは、作業内容を安全に commit/push して PR を作成する。

## 1. 実行順序

1. 現在ブランチを確認する。
2. `main` または `master` にいる場合は作業ブランチを作成して checkout する。
3. PR テンプレートの存在を確認し、無ければ作成する。
4. `references/pii-scan-rules.md` で個人情報・秘密情報チェックを実行する。
5. 変更を stage して commit する。
6. 同じチェックを最終確認として再実行する。
7. `main` 以外のブランチへ push する。
8. PR を作成し、作成結果を返す。

## 2. ブランチ運用ルール

- `main` への直pushを禁止する。
- ブランチ命名は `feat/*`, `fix/*`, `chore/*`, `refactor/*` のいずれかで始める。
- ブランチ未作成時は、変更内容を要約した短い名前で新規作成する。

## 3. PRテンプレート運用

- テンプレートパスは `.github/pull_request_template.md` を優先する。
- テンプレートが無い場合は `references/pr-template-default.md` を元に作成する。
- プロジェクト固有の補足項目が必要な場合はテンプレートに追記する。

## 4. PII/秘密情報チェック

- commit 前と push 前に `references/pii-scan-rules.md` のチェックを必ず実行する。
- 該当がある場合は commit/push を停止し、マスキングまたは削除を先に行う。
- `.git/` 配下のローカルメタは判定対象外とし、追跡対象ファイルを優先チェックする。

## 5. commit/push ルール

- commit は変更意図が分かる1行サマリにする。
- push は `git push -u origin <branch>` を使う。
- `main` への push コマンドは実行しない。

## 6. PR作成ルール

- `gh pr create` を優先利用する。
- Base は `main` を既定にする。
- タイトルと本文はテンプレートに沿って埋める。
- PR URL を最終結果として返す。

## 7. 出力要件

- 使用ブランチ
- 実行した PII チェック結果
- commit hash と commit message
- push 先
- PR URL

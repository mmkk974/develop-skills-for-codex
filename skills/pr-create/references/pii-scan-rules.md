# PII / Secret Scan Rules

## 1. 対象

- `git ls-files` で取得できる追跡対象ファイル
- `git diff --name-only` の変更ファイル

## 2. 最低チェック項目

- メールアドレス
- 電話番号らしき文字列
- ローカル絶対パス（例: `/Users/<name>`）
- APIキー/トークン/秘密鍵らしき文字列

## 3. 実行例

```bash
git ls-files | xargs rg -n -i -S "[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}"
git ls-files | xargs rg -n -i -S "(/Users/[^/\\s]+|C:\\\\Users\\\\[^\\\\s]+)"
git ls-files | xargs rg -n -i -S "(API_KEY|SECRET|PASSWORD|PRIVATE KEY|BEGIN RSA|BEGIN OPENSSH|ghp_[A-Za-z0-9]{36})"
```

## 4. 判定

- ヒット0件: pass
- ヒットあり: fail（原因ファイルを修正後、再スキャン）

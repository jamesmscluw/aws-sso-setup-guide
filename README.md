# AWS SSO セットアップガイド
### Google Workspace × IAM Identity Center

---

## 背景・目的

現状、AWS 環境ごとに個別の IAM ユーザーが発行されており、以下のように環境ごとに異なるパスワードとサインイン URL を管理する必要があります。

| 環境 | サインイン URL |
|---|---|
| internal | https://xxxxxxxxxxxx.signin.aws.amazon.com/console |
| mnt | https://xxxxxxxxxxxx.signin.aws.amazon.com/console |
| prd | https://xxxxxxxxxxxx.signin.aws.amazon.com/console |
| dev | https://xxxxxxxxxxxx.signin.aws.amazon.com/console |

この構成には以下の課題があります。

- 環境ごとに異なるパスワードと URL を管理しなければならない
- ユーザーの入退社のたびに各 AWS アカウントで個別に IAM ユーザーを作成・削除する必要がある
- 認証情報が分散しており、セキュリティリスクが高い

---

## 解決策

**AWS IAM Identity Center** と **Google Workspace** を SAML 連携させることで、以下のように改善されます。

```
変更前:
  internal用URL → IAMユーザー + パスワード①
  mnt用URL     → IAMユーザー + パスワード②
  prd用URL     → IAMユーザー + パスワード③
  dev用URL     → IAMユーザー + パスワード④

変更後:
  Googleアカウントでログイン（1回）
        ↓
  https://<your-company>.awsapps.com/start
        ↓
  [internal]  [mnt]  [prd]  [dev]  ← 1画面からすべての環境にアクセス
```

### メリット

- **パスワード管理が不要** — Google アカウント1つでログイン
- **URL が1つに統一** — アクセスポータルからすべての環境を選択できる
- **ユーザー管理の一元化** — Google Workspace 側でユーザーを追加・削除するだけで AWS 側に自動反映（SCIM）
- **IAM ユーザーの個別管理が不要** になる

---

## セットアップガイド

全手順（スクリーンショット付き）をまとめたガイドはこちら:

👉 **[SSO セットアップガイドを見る](https://jamesmscluw.github.io/aws-sso-setup-guide/sso-tutorial.html)**

### 手順の概要

| Part | 内容 |
|---|---|
| Part 1 | Google Workspace と IAM Identity Center の SAML 連携設定 |
| Part 2 | アプリ有効化と SCIM 自動プロビジョニングの設定 |
| Part 3 | グループ作成・アクセス権付与・動作確認 |

### 必要な権限

- Google Workspace **特権管理者**権限
- AWS **IAM Identity Center** の管理権限

---

## 参考リンク

- [APC 技術ブログ Part 1](https://techblog.ap-com.co.jp/entry/2024/02/22/013219)
- [APC 技術ブログ Part 2](https://techblog.ap-com.co.jp/entry/2024/02/27/001144)
- [APC 技術ブログ Part 3](https://techblog.ap-com.co.jp/entry/2024/03/25/120348)
- [AWS 公式ドキュメント](https://docs.aws.amazon.com/ja_jp/singlesignon/latest/userguide/gs-gwp.html)

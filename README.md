# 📘 MSプロジェクト ドキュメント

本リポジトリは、MSプロジェクトに関連するドキュメントを管理するためのリポジトリです。  
プロジェクトはマイクロサービスアーキテクチャで構成されており、インフラからアプリケーションまで一貫してIaC・GitOpsの思想に基づいて構築されています。

---

## 📌 プロジェクト全体像

### 📎 インフラ構成図

![インフラ構成図](https://github.com/user-attachments/assets/eb7e36c9-35f8-40d2-867f-6fb063f682f8)

> 詳細は [`ms-infra`](https://github.com/wakabaseisei/ms-infra) を参照。

### マイクロサービス構成図
TBW

---

## 📂 リポジトリ構成

### インフラ管理

| リポジトリ名 | 説明 |
|---|---|
| [`ms-infra`](https://github.com/wakabaseisei/ms-infra) | Terraformを用いてAWSインフラをコードで管理。EKS, Aurora, CloudFront等の構成。 |
| [`ms-kubernetes`](https://github.com/wakabaseisei/ms-kubernetes) | Kubernetesのマニフェスト（Deployment, Service, Ingress等）をGitOps的に管理。ArgoCD対応。 |

### APIスキーマ定義

| リポジトリ名 | 説明 |
|---|---|
| [`ms-protobuf`](https://github.com/wakabaseisei/ms-protobuf) | Connect用のAPIインターフェースを定義するProtobufスキーマを管理。 |

### アプリケーション

| リポジトリ名 | 説明 |
|---|---|
| [`ms-web`](https://github.com/wakabaseisei/ms-web) | フロントエンドのSPAアプリケーション。Reactベース。ビルド成果物はS3にデプロイ。 |
| [`api-front`](https://github.com/wakabaseisei/api-front) | フロントエンドと直接通信するBFF（Backends for Frontends）。 |
| [`ms-user`](https://github.com/wakabaseisei/ms-user) | ユーザー情報を管理するマイクロサービス。Aurora MySQL Serverless v2に接続し、IAM認証でセキュアに接続。 |

### 補助ツール

| リポジトリ名 | 説明 |
|---|---|
| [`ms-db-user-generator`](https://github.com/wakabaseisei/ms-db-user-generator) | AuroraのDBユーザーを作成するためのLambdaツール。Terraformからデプロイ・実行される。 |

---

## 🚀 デプロイ・運用

- CI/CD:
  - GitHub Actionsでコンテナビルド〜ECRプッシュ
  - ArgoCDでKubernetesマニフェストを継続的に適用
- セキュリティ:
  - プライベートサブネット内にALBを設置し、CloudFront経由でアクセス
  - Aurora:
    - Serverless v2構成で柔軟なスケーリングに対応
    - IAMデータベース認証を有効化
    - マスターユーザーのパスワードはSecrets Managerで自動生成・管理
    - ユーザー作成やマイグレーションはLambdaを介して実行
- 通信プロトコル:
  - Connectを利用してマイクロサービス間通信を実装

---

## 📄 備考

- フロントエンドとBFFはCORS対応済み
- API設計はGoogle AIPをベースに整備

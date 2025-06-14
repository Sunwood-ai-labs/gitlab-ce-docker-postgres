<div align="center">

# 🐳 GitLab CE Docker with PostgreSQL

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![GitLab](https://img.shields.io/badge/GitLab-FCA326?style=for-the-badge&logo=gitlab&logoColor=white)](https://gitlab.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/)

</div>

## 📋 目次

- [🚀 クイックスタート](#-クイックスタート)
- [⚙️ 設定](#️-設定)
- [🏃‍♂️ GitLab Runner登録](#️-gitlab-runner登録)
- [🔐 初期rootパスワードの確認](#-初期rootパスワードの確認)
- [🔧 メンテナンス](#-メンテナンス)
- [📊 監視](#-監視)
- [❓ トラブルシューティング](#-トラブルシューティング)
- [📚 参考資料](#-参考資料)

## 🚀 クイックスタート

### 📋 前提条件

- Docker Engine 20.10+
- Docker Compose 2.0+
- 最低8GB RAM推奨
- 最低20GB のディスク容量

### 🔧 セットアップ手順

1. **リポジトリのクローン**
   ```bash
   git clone <repository-url>
   cd gitlab-ce-docker-postgres
   ```

2. **環境変数ファイルの作成**
   ```bash
   cp .env.example .env
   # .envファイルを編集して適切な値を設定してください
   ```

3. **サービスの起動**
   ```bash
   docker-compose up -d
   ```

4. **GitLabの起動確認**
   ```bash
   docker-compose logs -f gitlab
   ```

## ⚙️ 設定

### 🌐 環境変数

主要な環境変数の説明：

| 変数名 | 説明 | デフォルト値 |
|--------|------|-------------|
| `GITLAB_HOSTNAME` | GitLabのホスト名 | `192.168.0.131` |
| `GITLAB_EXTERNAL_URL` | GitLabの外部URL | `http://192.168.0.131:8080` |
| `GITLAB_HTTP_PORT` | HTTPポート | `8080` |
| `POSTGRES_PASSWORD` | PostgreSQLのパスワード | `your_secure_password` |

### 🔧 カスタマイズ

詳細な設定は `.env` ファイルを編集してください。

## 🏃‍♂️ GitLab Runner登録

GitLab Runnerを登録するには、以下のコマンドを実行してください：

```bash
# 環境変数から設定値を取得して登録
docker-compose exec gitlab-runner gitlab-runner register \
  --non-interactive \
  --url ${GITLAB_EXTERNAL_URL} \
  --registration-token ${GITLAB_RUNNER_REGISTRATION_TOKEN} \
  --executor docker \
  --docker-image alpine:latest \
  --description "docker-runner"
```

> ⚠️ **セキュリティ注意**: registration tokenは `.env` ファイルで管理してください。

## 🔐 初期rootパスワードの確認

GitLabの初期rootパスワードを確認するには、以下のコマンドを実行してください：

```bash
docker compose exec gitlab cat /etc/gitlab/initial_root_password
```

## 🔧 メンテナンス

### 📦 バックアップ

バックアップは自動で実行されますが、手動でも実行可能です：

```bash
docker exec gitlab gitlab-rake gitlab:backup:create
```

### 🔄 アップデート

```bash
docker-compose pull
docker-compose up -d
```

### 🧹 クリーンアップ

```bash
docker-compose down
docker system prune -a
```

## 📊 監視

### 🔍 ログの確認

```bash
# 全サービスのログ
docker-compose logs -f

# 特定のサービスのログ
docker-compose logs -f gitlab
docker-compose logs -f postgresql
docker-compose logs -f gitlab-runner
```

### 📈 リソース監視

```bash
docker stats
```

## ❓ トラブルシューティング

### 🐛 よくある問題

1. **GitLabが起動しない**
   - メモリ不足が原因の可能性があります
   - `docker-compose logs gitlab` でログを確認してください

2. **PostgreSQL接続エラー**
   - データベースの初期化を待ってください
   - `docker-compose logs postgresql` でログを確認してください

3. **Runner登録エラー**
   - GitLabが完全に起動するまで待ってください
   - registration tokenが正しいか確認してください

### 🔧 デバッグコマンド

```bash
# サービスの状態確認
docker-compose ps

# コンテナ内でのコマンド実行
docker-compose exec gitlab bash
docker-compose exec postgresql psql -U gitlab -d gitlabhq_production
```

## 📚 参考資料

- [GitLab CE Documentation](https://docs.gitlab.com/ee/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [GitLab Runner Documentation](https://docs.gitlab.com/runner/)

---

<div align="center">

**🎉 GitLab CE Docker環境をお楽しみください！**

</div>
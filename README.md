# gitlab-ce-docker-postgres

## GitLab Runner登録

GitLab Runnerを登録するには、以下のコマンドを実行してください：

```bash
sudo docker-compose exec gitlab-runner gitlab-runner register \
  --non-interactive \
  --url http://192.168.0.131:8080 \
  --registration-token weXB3ehPC_a6Csy_vazR \
  --executor docker \
  --docker-image alpine:latest \
  --description "docker-runner"
```

## 初期rootパスワードの確認

GitLabの初期rootパスワードを確認するには、以下のコマンドを実行してください：

```bash
docker compose exec gitlab cat /etc/gitlab/initial_root_password
```
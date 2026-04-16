# DevOps 基础

这一页定义 Phase 0 里最小可用的运行与启动方式。

## 开发环境

- 使用 Docker 保证环境一致性
- core、web、db 通过 Compose 编排
- 入口由 nginx 统一转发

## 启动方式

- 通过 `infra/scripts/start.sh` 启动开发环境
- 通过 `infra/scripts/stop.sh` 停止开发环境
- `.env` 负责注入数据库、端口和运行参数

## 运行约定

- `dev` 用于日常开发
- `main` 用于稳定发布
- 开发期优先保证一条命令能跑起来

## 预留内容

- ai-service 和 fpga-service 先保留编排位置
- 暂时只需要网络连通和健康检查占位
- 不在 Phase 0 里引入过重的部署复杂度

## main 分支自动发布 core 镜像（GitHub Packages）

目标：当 `main` 分支有 `core` 相关变更时，自动构建并推送镜像到 GHCR。

工作流文件（位于 core 仓库）：`.github/workflows/core-image-ghcr.yml`

触发条件：
- push 到 `main`
- 且变更命中 `src/**`、`pom.xml`、`mvnw`、`.mvn/**`、`Dockerfile` 或工作流文件本身
- 也支持手动触发 `workflow_dispatch`

发布镜像名：
- `ghcr.io/<org-or-user>/lab-edu-core`

默认标签：
- `latest`
- `main`
- `sha-<commit>`

## 发布后如何通过 docker 调用

1) 登录 GHCR（私有包时必须）：

```bash
echo "$GITHUB_TOKEN" | docker login ghcr.io -u <github-username> --password-stdin
```

2) 拉取镜像：

```bash
docker pull ghcr.io/<org-or-user>/lab-edu-core:main
```

3) 启动容器：

```bash
docker run --rm -p 8080:8080 \
	-e SPRING_DATASOURCE_URL=jdbc:postgresql://<db-host>:5432/<db-name> \
	-e SPRING_DATASOURCE_USERNAME=<db-user> \
	-e SPRING_DATASOURCE_PASSWORD=<db-password> \
	ghcr.io/<org-or-user>/lab-edu-core:main
```

4) 验证服务：

```bash
curl http://localhost:8080/actuator/health
curl http://localhost:8080/swagger-ui.html
```

说明：
- 若包是 Public，可直接 pull；若是 Private，需要具备包读取权限。
- 建议生产环境固定使用 `sha-<commit>` 标签，避免 `latest` 漂移。
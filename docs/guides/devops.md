# DevOps 基础

这一页只记录 Phase 0 已经确定下来的运行方式、脚本入口和镜像发布约定，目标是让新成员能按固定步骤把环境跑起来。

## 本地开发

### 环境组成

- `web`、`core`、`db` 通过 Docker Compose 编排
- 统一入口由 `nginx` 转发
- 配置通过 `.env` 和 `.env.example` 管理

### 推荐启动流程

1. 复制环境变量文件：`cp .env.example .env`
2. 按需调整数据库、端口和镜像地址
3. 启动环境：`./scripts/up.sh`
4. 结束后按需要执行：`./scripts/stop.sh`、`./scripts/down.sh` 或 `./scripts/restart.sh`

### 脚本职责

- `up.sh`: 启动开发环境
- `stop.sh`: 仅停止容器
- `down.sh`: 停止并清理容器与网络
- `restart.sh`: 重启当前环境
- `pull.sh`: 预拉取 `core` 和 `web` 镜像

### 编排文件

- 统一编排文件为 `infra/docker-compose.yml`
- 开发阶段优先通过脚本入口操作，而不是手动拼接 `docker compose` 命令
- `ai-service` 和 `fpga-service` 目前只保留编排占位，不展开业务实现

## 镜像发布

### core 镜像

- 工作流文件：`core/.github/workflows/core-image-ghcr.yml`
- 触发条件：push 到 `main`，或手动触发 `workflow_dispatch`
- 变更命中 `src/**`、`pom.xml`、`mvnw`、`.mvn/**`、`Dockerfile` 或工作流文件时才构建
- 镜像名：`ghcr.io/lab-edu/lab-edu-core`
- 默认标签：`latest`、`main`、`sha-<commit>`

### web 镜像

- 工作流文件：`web/.github/workflows/web-image-ghcr.yml`
- 镜像名：`ghcr.io/lab-edu/lab-edu-web`
- 默认标签：`latest`、`dev`、`sha-<commit>`

### 手动拉起镜像

```bash
docker pull ghcr.io/lab-edu/lab-edu-core:main
docker run --rm -p 8080:8080 \
	-e SPRING_DATASOURCE_URL=jdbc:postgresql://<db-host>:5432/<db-name> \
	-e SPRING_DATASOURCE_USERNAME=<db-user> \
	-e SPRING_DATASOURCE_PASSWORD=<db-password> \
	ghcr.io/<org-or-user>/lab-edu-core:main
```

## 运行约定

- `dev` 用于日常开发
- `main` 用于稳定发布
- 开发期优先保证一条命令能跑起来
- 健康检查统一使用 `/actuator/health`

## 当前边界

- 这里不展开分支保护、Owner、Review 规则等 GitHub 平台配置
- 这里不展开数据库详细设计，相关内容以后放到独立文档里补充
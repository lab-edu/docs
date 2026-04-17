# 配置约定

这一页记录统一的环境和部署参数使用方式。

## Core 服务关键参数

- `SPRING_DATASOURCE_URL`：PostgreSQL 连接串
- `SPRING_DATASOURCE_USERNAME`：数据库用户名
- `SPRING_DATASOURCE_PASSWORD`：数据库密码
- `LAB_SECURITY_JWT_SECRET`：JWT 签名密钥
- `LAB_SECURITY_JWT_EXPIRATION_MINUTES`：JWT 过期时间（分钟）
- `LAB_STORAGE_BASE_PATH`：实验提交文件本地存储目录
- `LAB_ALLOWED_ORIGINS`：跨域白名单（逗号分隔）

说明：
- 数据库结构通过 Flyway 迁移维护，当前包含 `V1__init.sql` 与 `V2__phase1_hardening.sql`。
- 生产环境建议固定 `LAB_SECURITY_JWT_SECRET` 并定期轮换。

## Web 服务关键参数

- `NEXT_PUBLIC_CORE_ORIGIN`：前端开发环境代理目标，默认 `http://localhost:8080`

说明：
- 前端统一从 `/core/*` 发起请求；在本地开发时由 Next.js rewrite 代理到 `NEXT_PUBLIC_CORE_ORIGIN`。
- 在容器部署时通常由 Nginx 将 `/core/*` 反向代理到 `core` 服务。

## 环境变量

- 数据库连接信息放在环境变量里
- 端口号放在环境变量里
- 密钥和敏感信息不写进仓库

## 本地运行

- 复制 `.env.example` 为 `.env`
- 按需调整数据库、端口和入口地址
- 使用统一脚本启动和停止环境

## 配置原则

- 开发、测试、生产使用不同配置
- 配置项要尽量稳定和少量化
- 先把默认值写清楚，再考虑覆盖参数
- 对外暴露的路径前缀（如 `/api/v1`、`/core`）保持稳定，避免前后端联调反复改造
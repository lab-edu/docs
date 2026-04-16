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
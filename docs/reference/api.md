# API 规范

这一页记录 Phase 0 已经明确下来的接口约定。

## 统一规则

- API 版本前缀统一使用 `/api/v1`
- 返回结构统一为 `code`、`message`、`data`
- 接口文档统一采用 OpenAPI + Swagger UI
- 健康检查统一使用 `/actuator/health`

## core 基础接口（Phase 0）

- Swagger UI: `GET /swagger-ui.html`
- OpenAPI JSON: `GET /v3/api-docs`
- 健康检查（Actuator）: `GET /actuator/health`

说明：
- 以上健康检查和 Swagger 文档接口允许匿名访问。
- 其他业务接口默认需要认证（HTTP Basic，后续可迁移到 JWT）。

## 设计原则

- 路径语义清晰
- 请求和响应都要可文档化
- 前后端对字段名称保持一致

## 说明

- 目前还没有把所有业务接口展开到文档里
- 后续新增接口时，先补文档，再补实现
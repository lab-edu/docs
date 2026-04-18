# API 规范

这一页记录当前阶段已经落地的接口约定，作为前后端联调的统一依据。

## 统一规则

- API 版本前缀统一使用 `/api/v1`
- 返回结构统一为 `code`、`message`、`data`
- 登录态通过 JWT 管理，登录成功后由后端写入 `HttpOnly` Cookie，Cookie 名称为 `lab_edu_token`
- 后端同时接受 `Authorization: Bearer <token>` 形式，方便接口调试
- 接口文档统一采用 OpenAPI + Swagger UI
- 健康检查统一使用 `/actuator/health`

## 基础接口

- Swagger UI: `GET /swagger-ui.html`
- OpenAPI JSON: `GET /v3/api-docs`
- 健康检查（Actuator）: `GET /actuator/health`

说明：
- 以上健康检查和 Swagger 文档接口允许匿名访问。
- 其他业务接口默认需要认证，除注册与登录外都必须带上有效 JWT。

## 用户模块

- 用户注册: `POST /api/v1/auth/register`
- 用户登录: `POST /api/v1/auth/login`
- 当前用户信息: `GET /api/v1/auth/me`
- 用户退出登录: `POST /api/v1/auth/logout`

注册请求字段：
- `username`
- `email`
- `password`
- `displayName`，可选
- `role`，可选，当前支持 `STUDENT` 和 `TEACHER`

登录请求字段：
- `identifier`，支持用户名或邮箱
- `password`

登录与退出说明：
- 登录成功后后端会下发 `lab_edu_token`（HttpOnly Cookie）。
- `POST /api/v1/auth/logout` 会清空该 Cookie，前端应主动跳回登录页。

## 课程模块

- 创建课程: `POST /api/v1/courses`
- 课程列表: `GET /api/v1/courses`
- 课程详情: `GET /api/v1/courses/{courseId}`
- 加入课程: `POST /api/v1/courses/join`
- 课程成员: `GET /api/v1/courses/{courseId}/members`
- 课程公告列表: `GET /api/v1/courses/{courseId}/announcements`
- 教师发布公告: `POST /api/v1/courses/{courseId}/announcements`

课程创建请求字段：
- `title`
- `description`

加入课程请求字段：
- `inviteCode`

## 实验模块

- 发布实验: `POST /api/v1/courses/{courseId}/experiments`
- 实验列表: `GET /api/v1/courses/{courseId}/experiments`
- 实验详情: `GET /api/v1/courses/{courseId}/experiments/{experimentId}`

实验创建请求字段：
- `title`
- `description`
- `dueAt`

## 提交模块

- 提交实验: `POST /api/v1/experiments/{experimentId}/submissions`
- 提交列表: `GET /api/v1/experiments/{experimentId}/submissions`
- 当前用户最新提交: `GET /api/v1/experiments/{experimentId}/submissions/latest`
- 教师评分: `PATCH /api/v1/submissions/{submissionId}/grade`

提交接口使用 `multipart/form-data`，其中：
- `file` 必填
- `note` 可选

评分请求字段：
- `score`，范围 `0-100`
- `feedback`，可选

## 资源模块

- 教师上传资源: `POST /api/v1/courses/{courseId}/resources`
- 课程资源列表: `GET /api/v1/courses/{courseId}/resources`
- 文件资源访问: `GET /api/v1/resources/{resourceId}/file`

资源上传接口使用 `multipart/form-data`，其中：
- `name` 必填
- `type` 必填，支持 `FILE`、`VIDEO`、`LINK`
- `category` 可选
- `file` 当 `type=FILE` 时必填
- `externalUrl` 当 `type=VIDEO/LINK` 时必填（仅支持 `http/https`）

## 成绩汇总模块

- 课程成绩汇总（教师）: `GET /api/v1/courses/{courseId}/grades/overview`

返回结构说明：
- `students` 为课程内学生列表
- 每个学生包含 `submissionCount`、`gradedCount`、`averageScore`
- `experiments` 为该学生已提交实验的评分明细

## 设计原则

- 路径语义清晰
- 请求和响应都要可文档化
- 前后端对字段名称保持一致
- 教师和学生的权限边界要由后端统一控制

## 前端路由与 API 对齐

- 登录页: `/login`
- 课程列表页: `/courses`
- 课程详情页: `/courses/{courseId}`
- 实验详情页: `/experiments/{experimentId}?courseId={courseId}`

联调约定：
- 前端统一通过 `/core/api/v1/*` 访问后端，避免在页面里直接拼装后端地址。
- 请求层统一附带 `credentials: include`，使用 HttpOnly Cookie 传递 JWT。
- 返回结构统一解析 `code/message/data`，并在 401/403 场景给出可理解提示。

## 常见错误码

- `40100` 未登录或登录已过期
- `40300` 无权限访问该资源
- `40000` 参数错误或请求体格式不正确
- `40001` 参数校验失败
- `404xx` 资源不存在
- `409xx` 资源冲突（例如重复加入）
- `50000` 服务器内部错误
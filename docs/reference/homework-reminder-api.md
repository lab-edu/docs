# 作业中心与提醒接口清单

该清单用于联调阶段快速对齐字段与行为语义。

## 1. 课程工作区模块配置

### 获取模块配置

- `GET /api/v1/courses/{courseId}/workspace/modules`
- 权限：课程成员可查看

返回示例字段：

- `courseId`
- `modules[]`
  - `moduleKey`
  - `enabled`
  - `sortOrder`

### 更新模块配置

- `PUT /api/v1/courses/{courseId}/workspace/modules`
- 权限：教师

请求示例字段：

- `modules[]`
  - `moduleKey`
  - `enabled`
  - `sortOrder`

## 2. 学习任务排序

### 同知识点任务重排

- `PATCH /api/v1/courses/{courseId}/learning/points/{pointId}/tasks/order`
- 权限：教师

请求示例字段：

- `orderedTaskIds[]`

## 3. 作业中心

### 课程作业列表

- `GET /api/v1/courses/{courseId}/homeworks`
- 权限：课程成员

返回示例字段：

- `items[]`
  - `taskId`
  - `courseId`
  - `courseTitle`
  - `title`
  - `startAt`
  - `dueAt`
  - `status`（`NOT_STARTED`/`IN_PROGRESS`/`SUBMITTED`/`OVERDUE`）
  - `submittedAt`
  - `score`
  - `remainingSeconds`

### 我的作业汇总

- `GET /api/v1/homeworks/mine`
- 权限：登录用户
- 支持参数：`status`、`courseId`

## 4. 站内提醒收件箱

### 消息列表

- `GET /api/v1/notifications`
- 支持参数：`unreadOnly`（默认 `false`）

### 标记已读

- `PATCH /api/v1/notifications/{notificationId}/read`

### 全部已读

- `PATCH /api/v1/notifications/read-all`

## 5. 作业提醒触发点

- `START`：作业开始时间
- `BEFORE_DUE_24H`：截止前 24 小时
- `DUE`：截止时间

若对应提醒开关关闭，则该触发点不生成提醒事件。
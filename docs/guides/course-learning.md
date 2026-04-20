# 课程学习功能

课程学习页用于承载课程内的学习结构管理与课堂任务闭环，入口位于课程详情页的“课程学习”按钮。

## 页面职责调整

- 课程管理页（`/courses/{id}/manage`）负责教师侧创建与发布：
  - 发布课程通知
  - 发布实验
  - 上传课程资源
  - 编排学习结构（单元、知识点、任务）
- 课程首页、实验页、资源页主要承载浏览与消费，不再放置上述发布表单。

这一调整的目标是把教师高频操作集中到一个入口，减少跨页面切换成本。

## 功能结构

- 课程下可以维护多个学习单元。
- 每个学习单元下可以继续维护多个知识点。
- 每个知识点内可以创建学习任务，任务支持两类：
  - 媒体学习：文件、链接或文本材料。
  - 随堂测试：单选题、多选题、简答题。
- 学生可以在任务内提交答卷，教师可以在线批阅并写入分数与评语。
- 课程学习汇总页会按学生聚合提交与批阅结果，便于统一查看。

## 富文本能力

- 教师在创建通知、实验描述、学习任务内容时支持富文本编辑（标题、粗体、列表、链接、代码块等）。
- 学生提交的文本答卷与教师评语支持富文本展示。
- 前端渲染层会对 HTML 内容进行白名单清洗，降低 XSS 风险。

## 常用接口

- `GET /api/v1/courses/{courseId}/learning`
- `GET /api/v1/courses/{courseId}/learning/overview`
- `POST /api/v1/courses/{courseId}/learning/units`
- `POST /api/v1/courses/{courseId}/learning/units/{unitId}/points`
- `POST /api/v1/courses/{courseId}/learning/points/{pointId}/tasks`
- `GET /api/v1/courses/{courseId}/learning/tasks/{taskId}/submissions`
- `POST /api/v1/courses/{courseId}/learning/tasks/{taskId}/submissions`
- `PATCH /api/v1/courses/{courseId}/learning/submissions/{submissionId}/grade`

## 使用建议

- 先创建学习单元，再创建知识点，最后创建任务，结构会更清晰。
- 媒体学习任务适合承载课件、视频和文本材料，随堂测试适合承载课堂小测和简答作答。
- 教师建议优先在课程管理页完成结构编排与发布，再在课程学习页中完成任务批阅与汇总查看。
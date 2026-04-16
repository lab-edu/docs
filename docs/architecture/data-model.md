# 数据模型

这一页只做核心实体的初步抽象，不写完整 SQL，也不展开表结构细节。Phase 0 的目标是先把概念讲清楚，避免后面反复重构。

## 核心实体

- Organization: 组织或团队边界
- Role: 角色与权限范围
- User: 用户账户与登录身份
- Course: 课程或业务主线容器
- Experiment: 实验或任务定义
- Submission: 提交记录与执行结果

## 关系约束

- Organization 管理成员和协作边界
- Role 决定用户能做什么
- Course 组织多个 Experiment
- Experiment 可以有多个 Submission
- Submission 记录每次提交的状态、结果与时间

## 约束原则

- 先保证概念稳定，再考虑表结构优化
- 需要历史追踪的对象要保留版本或记录能力
- 任何字段命名都要能被前后端共同理解
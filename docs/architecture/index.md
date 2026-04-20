# 架构设计

系统整体设计与技术边界说明，用于统一团队的架构认知。

## 核心文档

- [模块边界](modules.md): web / core / ai-service / fpga-service 的职责边界
- [技术选型](tech-stack.md): 前端、后端、数据库与预留服务的基线
- [通信方式](communication.md): 模块之间的接口约定与版本规则
- [数据模型](data-model.md): 核心实体关系与演进约束
- [课堂工作区与作业提醒架构](classroom-workspace.md): 拖拽编排、作业中心与站内提醒的交互与后端设计

## 架构约束

- 统一 API 版本前缀: `/api/v1`
- 统一响应结构: `code` / `message` / `data`
- 服务间通过接口通信，避免跨模块内部耦合

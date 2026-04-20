# lab-edu docs

lab-edu 项目的文档仓库，使用 MkDocs 构建站点。

## 仓库用途

- 维护项目的阶段规划（Phase 0 ~ Phase 4）
- 沉淀架构设计、开发指南、参考资料
- 让参与者可以用同一套说明理解项目目标、模块边界和使用方式

## 目录结构

```text
docs/
├── docs/                # Markdown 文档源文件
│   ├── index.md
│   ├── phases/
│   ├── guides/
│   ├── architecture/
│   └── reference/
├── mkdocs.yml           # MkDocs 配置
├── pyproject.toml       # Python 依赖配置
└── uv.lock              # 依赖锁文件
```

## 如何构建

### 1) 安装依赖

使用 uv：

```bash
uv sync
```

### 2) 本地预览

```bash
uv run mkdocs serve
```

### 3) 生成静态站点

```bash
uv run mkdocs build
```

构建产物默认输出到 `site/` 目录。

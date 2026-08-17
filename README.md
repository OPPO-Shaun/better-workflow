# better-workflow

A repository configured with [Matt Pocock's agent skills](https://github.com/mattpocock/skills) for GitHub Copilot-compatible project use.

## Agent Skills

All skills are installed under `.github/skills/` and come from [mattpocock/skills](https://github.com/mattpocock/skills).

Categories installed:

- **[Engineering](.github/skills/engineering/README.md)** — code review, TDD, domain modeling, bug diagnosis, architecture, and more
- **[Productivity](.github/skills/productivity/README.md)** — grilling, teaching, handoff, questionnaires, and more
- **[Misc](.github/skills/misc/README.md)** — git guardrails, pre-commit hooks, exercise scaffolding, and more

See [SKILLS_SOURCE.md](./SKILLS_SOURCE.md) for:
- The pinned upstream commit SHA
- Full skill listing with descriptions
- How to update/resync skills
- How to run the one-time `/setup-matt-pocock-skills` flow
- Limitations per tool (Copilot Cloud Agent, CLI, VS Code, Claude Code, SDK)

## Agent Files

Agent definition files from upstream are installed under `.github/agents/`:

- `install-block.md` — guidelines for the installation block pattern
- `invocation.md` — guidelines for skill invocation
- `writing-docs.md` — guidelines for writing agent documentation

## Workflow 企业级评测框架

### 目标

面向企业级 workflow 开发与落地，建立一套可复用的能力评测框架，用于回答以下问题：

1. 主流 workflow 项目在流程建模、执行引擎、治理能力、AI 扩展性上的差异是什么。
2. 哪类项目更适合企业内部流程自动化、数据编排、审批流、Agent 工作流等不同场景。
3. 如何设计统一评测集、基线实现与打分标准，避免只看“功能点”而忽略真实落地成本。

## 一、主流 workflow 项目调研与定位

建议优先覆盖以下五类主流项目，既能覆盖传统企业流程，也能覆盖 AI workflow 场景。

| 类别 | 代表项目 | 核心优势 | 主要短板 | 典型企业场景 |
| --- | --- | --- | --- | --- |
| 代码优先编排 | Airflow、Prefect、Dagster | Python 生态成熟，适合数据/任务编排，二次开发强 | 对审批、人机协同、复杂业务状态流支持一般 | 数据处理、批任务、平台内部作业 |
| 云原生工作流 | Argo Workflows、Tekton | Kubernetes 原生，适合大规模任务调度与 CI/CD | 业务建模门槛高，对业务人员不友好 | AI/训练流水线、云原生平台任务 |
| BPM / 业务流程 | Camunda、Flowable | BPMN 标准化、审批流和状态管理强、治理能力成熟 | 开发体验相对重，AI 原生能力偏弱 | OA、审批、运营流程、合规流程 |
| 通用自动化 / iPaaS | n8n、Node-RED、Kestra | 集成连接器多、低代码上手快、适合跨系统自动化 | 大规模治理、复杂工程化能力参差不齐 | 系统集成、通知编排、运营自动化 |
| AI Agent / LLM Workflow | LangGraph、AutoGen、Dify Workflow | 适合多 Agent、工具调用、长链路推理与人机协同 | 可靠性、回放、权限治理与 SLA 能力普遍不足 | 智能客服、知识助理、业务 Copilot |

### 推荐纳入正式评测的 baseline 项目

建议至少选择 6 个项目进入正式评测，形成不同技术路线的对照组：

- **Airflow**：代码优先的数据编排基线
- **Dagster 或 Prefect**：现代开发体验基线
- **Argo Workflows**：云原生执行基线
- **Camunda 或 Flowable**：BPM / 审批治理基线
- **n8n**：低代码集成自动化基线
- **LangGraph 或 Dify Workflow**：AI Workflow 基线

这样可以避免只在单一路线内比较，导致结论失真。

## 二、企业级能力评测维度

建议从“开发、运行、治理、扩展、成本”五个层面构建评测框架。

### 1. 开发效率

- 流程定义方式：代码优先 / 配置优先 / BPMN / 低代码
- 学习成本：新成员上手时间、示例完整度、文档质量
- 调试体验：本地运行、断点、回放、可视化排障
- 复用能力：模板、子流程、组件化、版本管理

### 2. 流程表达能力

- 顺序、分支、并行、循环、重试、补偿
- 长事务 / Saga / 人工审批 / 定时等待
- 事件驱动、外部回调、状态持久化
- 多 Agent 协作、工具调用、记忆注入（面向 AI workflow）

### 3. 运行时能力

- 调度性能：吞吐、延迟、资源利用率
- 稳定性：失败恢复、幂等、断点续跑、任务隔离
- 弹性伸缩：水平扩缩容、多租户隔离
- 可观测性：日志、指标、链路追踪、运行快照

### 4. 治理与安全

- RBAC / SSO / 审计日志
- 流程版本审批、发布管控、回滚能力
- 密钥管理、连接器权限边界、数据脱敏
- 合规支持：流程留痕、人工干预、审批节点可追责

### 5. 生态与落地成本

- 连接器与插件生态
- 与企业现有系统集成难度（LDAP、消息队列、数据库、内部 API）
- 部署复杂度：单机、K8s、混合云
- 运维成本：升级、容灾、备份、监控接入
- 二次开发成本：SDK、扩展点、社区活跃度

## 三、评测集设计

评测集不建议只做“Hello World”流程，而应围绕企业真实落地场景设计多层任务集。

### L1：基础流程能力

| 任务 | 目标能力 |
| --- | --- |
| 顺序 + 分支 + 并行任务 | 基础 DAG / 状态流表达 |
| 重试 + 超时 + 失败告警 | 异常处理与可靠性 |
| 定时调度 + 参数化运行 | 调度能力 |
| 子流程复用 | 模块化能力 |

### L2：企业业务流程

| 任务 | 目标能力 |
| --- | --- |
| 请假 / 采购审批流 | 人机协同、状态流转、审计 |
| 订单履约流程 | 长流程、补偿机制、外部系统调用 |
| 客户通知编排 | 多渠道连接器与事件驱动 |
| 主数据同步流程 | 幂等、重放、失败恢复 |

### L3：平台与云原生流程

| 任务 | 目标能力 |
| --- | --- |
| CI/CD 发布流水线 | 云原生任务编排、环境隔离 |
| 批处理 / ETL 流程 | 大规模任务调度、数据处理生态 |
| GPU/训练任务流水线 | 资源调度、长任务监控 |

### L4：AI Workflow 场景

| 任务 | 目标能力 |
| --- | --- |
| RAG 问答工作流 | 检索、模型调用、缓存、回退 |
| 多 Agent 协作任务 | 状态共享、工具编排、人工接管 |
| 内容审核 / 结构化抽取流程 | 模型路由、置信度判定、失败兜底 |
| 工单 Copilot | 外部系统集成、可追溯推理链 |

### 评测集输出格式建议

每个任务样例建议统一包含：

- **业务目标**：任务要解决什么问题
- **输入 / 输出定义**：便于自动化评测
- **依赖系统**：数据库、消息队列、HTTP 服务、模型服务
- **约束条件**：并发量、时延、失败注入、权限要求
- **验收标准**：成功率、平均时延、人工介入次数、恢复时间

## 四、基线体系设计

为了让评测结果可比较，建议同时建立“平台基线”和“模型基线”。

### 1. 平台基线

对同一任务集，分别使用不同 workflow 平台实现一遍，记录：

- 开发耗时
- 实现代码 / 配置规模
- 成功率与恢复能力
- 监控排障体验
- 上线依赖与运维复杂度

### 2. 模型基线（面向 AI workflow）

对涉及 LLM 的任务，建议至少设置三档模型基线：

- **强基线**：高质量闭源模型，用于观察最优可达效果
- **开源通用基线**：可私有化部署的主流开源模型，用于评估企业可控方案
- **轻量低成本基线**：小参数模型或低成本推理模型，用于评估成本 / 效果平衡

对比指标建议包括：

- 任务成功率
- 平均推理时延
- 单任务成本
- 幻觉率 / 误操作率
- 是否支持结构化输出、函数调用、长上下文

## 五、评分方法建议

建议采用 **100 分制 + 权重分层**：

| 一级维度 | 建议权重 |
| --- | --- |
| 开发效率 | 20 |
| 流程表达能力 | 20 |
| 运行时能力 | 25 |
| 治理与安全 | 20 |
| 生态与落地成本 | 15 |

对于 AI workflow，可额外增加 20 分附加项，单独记录而不直接覆盖基础能力分：

- Agent 编排能力
- 模型接入能力
- Prompt / Tool / Memory 管理
- 人工接管与推理追踪

## 六、落地建议

### 如果目标是企业内部审批 / 运营流程

优先评估 **Camunda / Flowable + n8n** 路线，重点看治理、审批、审计与连接器能力。

### 如果目标是数据与平台任务编排

优先评估 **Airflow / Dagster / Argo** 路线，重点看调度、可观测性与云原生部署能力。

### 如果目标是 AI Agent Workflow

优先评估 **LangGraph / Dify Workflow / Argo** 组合，重点看状态管理、人工接管、模型路由与生产级监控。

## 七、建议的最终交付物

为了把这套评测真正用于企业选型，建议沉淀以下产物：

1. **项目调研报告**：主流项目对比、架构分析、选型建议
2. **标准评测集**：按 L1-L4 分类的任务集与验收标准
3. **基线实现**：每个平台至少完成一套可复现实验
4. **评分看板**：多维评分、雷达图、成本与风险对比
5. **落地建议书**：不同企业场景下的推荐技术路线

以上内容可以作为后续建设 benchmark 仓库、自动化评测流水线和选型决策模板的基础。

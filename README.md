# Spec Engineer 从 0 到可独立工作的中文学习仓库

> 这是一个面向零基础学习者的 **Specification / Requirements Engineer 中文自学教材仓库**。目标不是“知道几个需求术语”，而是学完后能够独立把模糊业务需求转化为可开发、可测试、可追踪、可交付的商业软件 Spec。

---

## 你可以怎样使用这个仓库

建议严格按 00 → 15 顺序学习。

每一章都尽量包含：

- 中文概念讲解
- 原理
- 真实软件案例
- 反例
- 实操步骤
- 练习
- 自测
- 完成标准
- 官方/权威原始链接

因此你可以：

> **先只看 GitHub 仓库正文完成主体学习，再根据每章末尾原链接继续深入。**

---

# 完整学习路线

## 第一阶段：零基础软件与岗位认知

### [00 从 0 开始：学习方法、环境与最小技术基础](docs/00-学习方法与环境准备.md)

学习：
- 前端
- 后端
- 数据库
- API
- JSON/YAML
- SQL
- Git
- HTTP
- Authentication / Authorization

### [01 软件工程与 Spec Engineer 岗位认知](docs/01-软件工程与Spec岗位认知.md)

学习：
- 软件生命周期
- PM / BA / Spec / Architect / Developer / QA 区别
- Verification / Validation
- Spec 的抽象层次
- AI Coding 时代的 Spec Engineer

---

# 第二阶段：Requirements Engineering

### [02 需求工程基础](docs/02-需求工程基础.md)

学习：
- Business / User / System Requirement
- Functional / Non-functional
- Constraint
- Business Rule
- Requirement Quality
- Requirement Attribute
- Baseline
- Change
- Traceability

### [03 需求获取与利益相关者分析](docs/03-需求获取与利益相关者分析.md)

学习：
- Stakeholder Analysis
- Interview
- Observation
- Workshop
- Document Analysis
- Prototype
- As-Is / To-Be
- Fact / Assumption / Decision / Question
- Conflict Analysis

### [04 需求建模](docs/04-需求建模.md)

学习：
- Flowchart
- BPMN
- Use Case
- Sequence Diagram
- State Machine
- ER Diagram
- Context Diagram
- Mermaid

### [05 SRS 与高质量 Spec 写作](docs/05-SRS与高质量Spec写作.md)

学习：
- SRS Structure
- Requirement Pattern
- Preconditions
- Postconditions
- Exceptions
- Boundary
- Time
- Error Model
- Consistency
- Definition of Ready
- Spec Review

### [06 敏捷需求](docs/06-敏捷需求.md)

学习：
- Epic
- User Story
- 3C
- Acceptance Criteria
- INVEST
- Example Mapping
- Backlog Refinement
- Definition of Ready / Done

---

# 第三阶段：技术规格能力

### [07 系统设计与架构基础](docs/07-系统设计与架构基础.md)

学习：
- System Boundary
- Context / Container
- C4
- Sync / Async
- Retry
- Circuit Breaker
- Transaction
- Concurrency
- Consistency
- Cache
- Queue
- Idempotency
- ADR

### [08 HTTP、REST API 与 OpenAPI](docs/08-API与OpenAPI规格.md)

学习：
- HTTP Request / Response
- Method
- Status Code
- Resource
- Path / Query / Body
- Schema
- Validation
- Error Model
- Pagination
- Filtering / Sorting
- Idempotency
- Authorization
- Rate Limit
- Versioning
- OpenAPI

### [09 数据库与数据规格](docs/09-数据库与数据规格.md)

学习：
- Table / Row / Column
- PK / FK / Unique
- Entity Relationship
- Data Dictionary
- NULL
- Transaction
- Data Lifecycle
- Soft Delete
- Audit
- Sensitive Data
- Data Ownership
- Migration
- SQL

### [10 非功能需求](docs/10-非功能需求.md)

学习：
- Performance
- Latency
- P95/P99
- Throughput
- Capacity
- Availability
- Reliability
- RTO / RPO
- Scalability
- Security
- Audit
- Observability
- Compatibility
- Backup / Restore

---

# 第四阶段：验证与工程协作

### [11 测试设计与需求追踪](docs/11-测试与需求追踪.md)

学习：
- Verification / Validation
- Test Basis
- Positive / Negative
- Equivalence Partitioning
- Boundary Value Analysis
- Decision Table
- State Transition
- Authorization Test
- Concurrency Test
- Traceability Matrix
- Coverage
- Defect Triage

### [12 Git / GitHub / Jira / Confluence 工程协作](docs/12-工程协作.md)

学习：
- Repository / Commit / Branch / PR
- Spec Review
- Jira
- Confluence
- Single Source of Truth
- Baseline
- Change Request
- ADR
- Git-based Spec workflow

---

# 第五阶段：AI + Spec

### [13 AI 辅助 Spec 工程](docs/13-AI辅助Spec工程.md)

学习：
- AI4RE
- AI 需求澄清
- Context Engineering
- Structured Prompt
- Spec-driven Development
- AI Coding Task Spec
- Fact / Inference / Suggestion / Assumption
- AI-generated Test
- AI Spec Verification
- Responsible AI-assisted development

---

# 第六阶段：完整项目

### [14 综合实战：医疗预约 SaaS](docs/14-综合实战.md)

完整包含：

- Vision / Scope
- Stakeholder
- Glossary
- Business Rule
- State Model
- 认证
- 医生
- Slot
- Appointment
- Permission Matrix
- API
- Data Model
- NFR
- Acceptance Criteria
- Test
- Traceability
- Open Questions
- Change Request

### [15 能力评估、作品集与面试](docs/15-能力评估与面试.md)

学习：
- 初级/中级/高级能力
- Portfolio
- 高频面试题
- Case Interview
- 自评
- 毕业标准

---

# 必做毕业项目

完成正文后，不要直接结束。

请做：

### [企业员工请假 SaaS 毕业项目](exercises/毕业项目任务书.md)

需要独立交付：

- 40+ System Requirements
- 15+ NFR
- Flow / Use Case / Sequence / State / ER
- API
- Data
- Permission
- 25+ Acceptance Criteria
- 50+ Test Cases
- Traceability
- Change Request
- AI Coding Spec

完成后即可作为 Spec / Requirements Engineer 作品集基础。

---

# 可直接复制使用的模板

- [SRS 模板](templates/SRS模板.md)
- [API Spec 模板](templates/API-Spec模板.md)
- [User Story 与 Acceptance Criteria 模板](templates/User-Story模板.md)
- [Spec Review Checklist](templates/Spec评审清单.md)
- [需求追踪矩阵 CSV](templates/需求追踪矩阵.csv)
- [Change Request 模板](templates/变更请求模板.md)
- [Data Dictionary 模板](templates/数据字典模板.md)
- [Permission Matrix 模板](templates/权限矩阵模板.md)
- [Decision Log / ADR 模板](templates/Decision-Log模板.md)
- [Open Questions / Assumptions 模板](templates/Open-Questions模板.md)

---

# 权威学习资料

集中整理在：

### [权威学习资源索引](resources/权威学习资源索引.md)

主要来源：

- IREB / CPRE
- IBM Requirements Management
- Atlassian Agile / Jira / Confluence
- OMG UML / BPMN
- MDN HTTP
- Microsoft Learn / Azure Architecture Center
- OpenAPI Initiative
- OWASP
- ISTQB / CSTQB
- GitHub Docs
- IREB AI4RE
- OpenAI / Microsoft AI-assisted development resources

IREB 的 CPRE Foundation Level 是本仓库 Requirements Engineering 主线的重要知识骨架；API、架构、NFR、数据和测试部分则进一步使用 Microsoft、MDN、OWASP、OpenAPI、ISTQB 等资料扩展。

---

# 推荐学习方式

不要只“读完”。

每章必须完成：

```text
阅读
↓
做例子
↓
完成练习
↓
加入自己的项目
↓
Review
↓
自测
```

只有产生工程工件才算真正学会。

---

# 最终能力目标

面对：

> “我们想做一个 SaaS，让用户可以预约。”

你不应该立刻开始写页面或代码。

你应该能够自然地展开：

```text
Why
↓
Who
↓
Scope
↓
Rules
↓
Requirements
↓
Models
↓
API / Data
↓
Security / NFR
↓
Acceptance
↓
Testing
↓
Traceability
↓
Change
↓
Implementation / AI Coding
↓
Verification
```

这就是本仓库希望建立的 **Spec Engineering 思维方式**。

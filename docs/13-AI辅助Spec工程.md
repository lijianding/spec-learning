# 13 AI 辅助 Spec 工程：从 Prompt 到可验证规格

> 本章不是教你“让 AI 替你写需求”，而是学习如何把 AI 当作分析、检查、生成和验证助手，同时保留人类对业务正确性和最终交付的责任。

---

## 1. AI 在需求工程中能做什么

IREB 已推出 AI4RE（AI for Requirements Engineering），明确将 AI 用于：
- 需求获取支持
- 需求文档化
- 需求验证
- 需求管理

同时强调：专业判断仍不可缺少，并需要理解 AI 风险。

因此 Spec Engineer 的正确思路不是：

> “AI 写完，我复制。”

而是：

> “我提供受控上下文、目标、约束和输出格式，让 AI 生成候选结果，再用明确的评审与测试机制验证。”

---

## 2. AI 适合的任务

### 需求整理
输入会议纪要，让 AI：
- 提取候选需求
- 提取业务规则
- 提取 Open Questions
- 找出冲突

### 需求改写
把：
> “页面要快。”

改写为候选 NFR，并提示仍需业务方给出负载和阈值。

### 完整性检查
让 AI 检查：
- Happy Path
- Error Path
- Boundary
- Authorization
- Concurrency
- Retry
- Audit

### 建模辅助
从文字生成：
- Mermaid flowchart
- sequence diagram
- state diagram

### 测试辅助
从 Requirement / AC 生成：
- Equivalence Partition
- Boundary Cases
- State Transition Cases
- Negative Tests

### Traceability 辅助
识别：
- 哪些 Test Case 没有关联 Requirement
- 哪些 Requirement 没有 Test
- 变更可能影响哪些文件

---

## 3. AI 不应替你做的决定

除非有明确来源，AI 不应擅自决定：
- 法规要求
- 商业优先级
- 医疗/财务业务规则
- 权限政策
- SLA
- 数据保存期限
- 风险接受
- 安全例外

例如：

> “医疗预约记录保存几年？”

AI 可以列出“需要确认的数据保留问题”，但不能凭猜测写成正式 Requirement。

---

## 4. 高质量 Prompt 的结构

建议固定使用六块：

```
角色
目标
上下文
约束
输出格式
验证要求
```

### 示例：需求澄清

```text
你是软件需求工程师。

目标：
检查以下预约取消需求中的歧义、遗漏和不可测试表述。

上下文：
系统为医疗预约 SaaS。
角色包括患者、医生、前台、管理员。
Appointment 状态：BOOKED、CHECKED_IN、COMPLETED、CANCELLED、NO_SHOW。

当前需求：
“患者可以在预约开始前取消预约。”

约束：
不要自行创造业务规则。
无法确定的内容必须标记为“待确认”。

输出：
1. 歧义
2. 缺失业务规则
3. 边界条件
4. 权限问题
5. 并发/异常问题
6. 待确认问题
7. 可候选的规格改写

验证：
每条候选规格必须说明依据来自哪里；没有依据时明确写“假设，需确认”。
```

这比一句：
> “帮我优化需求”

可靠得多。

---

## 5. Context Engineering

Coding Agent 或 LLM 并不知道你的项目事实。

上下文应包括：

- Product vision
- Scope
- Glossary
- Current Spec
- Business Rules
- API Contract
- Data Model
- Architecture
- Coding conventions
- Security constraints
- Existing tests
- Definition of Done

关键思想：

> 不要让模型“猜项目”，让模型“读取项目”。

---

## 6. 建议的仓库结构

```
/docs
  /requirements
  /architecture
  /api
  /data
  /security
/spec
  glossary.md
  business-rules.md
  permissions.md
  state-machines.md
/openapi
/tests
/decisions
AGENTS.md
README.md
```

Coding Agent 执行任务前，应先读取这些事实来源。

---

## 7. Spec-driven Development

可以采用：

```
需求来源
 ↓
Spec
 ↓
Acceptance Criteria
 ↓
Implementation Plan
 ↓
AI / Developer Coding
 ↓
Automated Tests
 ↓
Spec Verification
 ↓
Review
```

AI 编码之前，不应该只有：

> “做一个预约功能。”

而应至少给出：
- 目标
- 文件范围
- 业务规则
- API
- Schema
- Permission
- Error behavior
- Tests
- Done criteria

---

## 8. 给 Coding Agent 的任务 Spec

示例：

```markdown
# Task: 实现取消预约 API

## Goal
实现 POST /api/v1/appointments/{id}/cancel。

## Read First
- docs/business-rules.md
- docs/permissions.md
- docs/appointment-state.md
- openapi/appointment.yaml

## Requirements
- APPT-CAN-001 ~ APPT-CAN-007

## Constraints
- 不修改既有数据库表结构
- 不新增依赖
- 不修改其他 endpoint
- 必须复用现有 authorization middleware

## Required Tests
- patient cancels own BOOKED appointment
- cannot cancel another patient's appointment
- cannot cancel CHECKED_IN
- boundary exactly 2 hours
- duplicate cancel
- concurrency

## Done
- tests pass
- OpenAPI unchanged unless conflict found
- if Spec conflict exists, stop and report; do not invent rule
```

最重要的一句：

> **如果 Spec 冲突，停止并报告，不要自行发明规则。**

---

## 9. AI 输出必须分类

建议把 AI 输出分类：

### Fact
来自明确项目资料。

### Inference
根据事实推导，但未明确写出。

### Suggestion
一种可选设计。

### Assumption
为继续工作临时假设。

### Unknown
缺信息。

只允许 Fact 直接进入正式 Spec。

Inference / Suggestion / Assumption 必须人工确认。

---

## 10. 防止“看起来正确”

AI 最大危险之一不是明显错误，而是“语言非常专业但事实未经确认”。

建立检查：

### Source Check
每条关键结论来自哪里？

### Consistency Check
是否与现有 Spec 冲突？

### Testability Check
能否生成确定 Pass/Fail？

### Boundary Check
边界是什么？

### Permission Check
谁能操作什么对象？

### State Check
当前状态是否允许？

### Data Check
字段和 Schema 是否真实存在？

---

## 11. AI 生成测试 ≠ 测试完成

AI 很适合生成候选 Test Case，但 QA/Spec 仍要验证覆盖。

例：

Requirement：
> 患者可以在预约开始至少 2 小时前取消 BOOKED 预约。

AI 至少应该覆盖：
- 2h + 1s
- 2h
- 2h - 1s
- CANCELLED
- CHECKED_IN
- other patient's appointment
- unauthorized
- repeated request
- concurrent requests

如果只生成“正常取消成功”，说明 Prompt 或审查不足。

---

## 12. AI 与敏感数据

不要直接把：
- 生产数据库
- 患者隐私
- 密钥
- Token
- 内部凭据
- 商业机密

发送给未经组织批准的 AI 工具。

使用：
- 脱敏数据
- 假数据
- 最小必要上下文
- 企业批准工具

Microsoft 的 AI 辅助开发安全指南明确强调：AI 生成的代码仍由交付者负责，同时要注意凭据、输入验证、依赖完整性等风险。

---

## 13. AI Spec Review Prompt

可直接复用：

```text
请以 Requirements Engineer + QA + Security Reviewer 三种视角审查以下 Spec。

禁止：
- 不得自行增加未提供的业务规则。
- 不得把假设写成事实。

检查：
1. 歧义
2. 冲突
3. 缺少前置条件
4. 缺少异常
5. 边界
6. 状态转换
7. 权限
8. 并发
9. 数据一致性
10. API错误行为
11. 安全与隐私
12. 可测试性
13. 可追踪性

输出表格：
问题 | 类型 | 严重度 | 原文 | 原因 | 建议 | 是否需要业务确认
```

---

## 14. AI Coding Review Prompt

```text
请验证实现是否符合 Spec，而不是单纯评价代码风格。

输入：
- Spec
- API contract
- State model
- Tests
- Implementation diff

逐条输出：
Requirement ID
Expected behavior
Implementation evidence
Test evidence
Result: PASS / FAIL / NOT PROVEN
Gap

如果没有证据，不得判定 PASS。
```

这就是“Spec Verification”思维。

---

## 15. AI 使用成熟度

### Level 0：聊天
“帮我写一个功能。”

### Level 1：结构化 Prompt
提供目标和约束。

### Level 2：项目上下文
提供现有 Spec、代码、测试。

### Level 3：Spec Driven
AI 只能依据已批准 Requirement 工作。

### Level 4：Verification Loop
自动生成/执行测试，输出 requirement-level evidence。

Spec Engineer 的价值会从“写文字”逐渐向：
- Context
- Constraints
- Verification
- Governance

迁移。

---

## 16. 本章实操

选择“取消预约”功能：

1. 用 AI 找歧义。
2. 你人工回答 Open Questions。
3. 让 AI 重新生成候选 Spec。
4. 用评审 Checklist 检查。
5. 生成 Acceptance Criteria。
6. 生成 Test Case。
7. 生成 Coding Task Spec。
8. 让 Coding Agent 实现。
9. 要求 AI 按 Requirement ID 验证实现。
10. 人工最终批准。

保存所有 Prompt 与输出，比较不同版本效果。

---

## 17. 自测

1. 为什么 AI 不能直接决定业务规则？
2. Fact 与 Assumption 有什么区别？
3. Context Engineering 和普通 Prompt 有什么区别？
4. 为什么 Coding Agent 应先读取 Spec？
5. “没有证据不得 PASS”解决什么问题？
6. AI 生成测试有哪些常见盲区？
7. 什么信息不应该直接发送给未批准 AI 工具？
8. Spec-driven Development 与 Vibe Coding 的主要区别是什么？

---

## 18. 权威原始资料

1. IREB AI4RE  
   https://cpre.ireb.org/en/concept/ai4re-micro-credential

2. IREB Requirements Engineering 资源中心（含 AI4RE Prompt Guide）  
   https://cpre.ireb.org/en/downloads-and-resources

3. OpenAI Prompt Engineering  
   https://developers.openai.com/api/docs/guides/prompt-engineering

4. OpenAI Codex 学习中心  
   https://developers.openai.com/learn/codex

5. Microsoft Learn：负责任的 AI 辅助开发  
   https://learn.microsoft.com/zh-cn/windows/apps/develop/ai-assisted/security-and-responsible-ai

6. Microsoft Learn：生成式 AI 与 Agent 基础  
   https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/

7. Microsoft Learn：GenAIOps  
   https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/

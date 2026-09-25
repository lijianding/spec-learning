# 05 SRS 与高质量 Spec 写作

> 本章是核心章节。目标不是“会写文档”，而是让需求变成可以开发、测试、审计和变更管理的工程规格。

---

# 1. SRS 是什么

SRS = Software Requirements Specification。

它描述：
- 系统范围
- 术语
- 需求
- 接口
- 数据
- 质量属性
- 约束
- 验证方式

SRS 不一定是一份超大 Word。

现代团队可能拆成：

```
vision.md
scope.md
glossary.md
requirements.md
business-rules.md
api.yaml
data-model.md
permissions.md
nfr.md
tests.md
traceability.csv
```

只要它们之间一致且有清晰 Source of Truth，就仍然是一套 Specification。

---

# 2. 推荐 SRS 结构

## 2.1 Introduction
- Purpose
- Background
- Business Goal
- Scope
- Out of Scope

## 2.2 Terms
- Glossary
- Acronyms

## 2.3 Stakeholders

## 2.4 Assumptions / Constraints

## 2.5 Business Rules

## 2.6 Functional Requirements

## 2.7 External Interface

## 2.8 Data

## 2.9 Permissions

## 2.10 Non-functional Requirements

## 2.11 Verification

## 2.12 Traceability

## 2.13 Open Questions

## 2.14 Change History

---

# 3. Spec 最重要的原则：减少解释空间

坏：

> 支持搜索医生。

开发可能做：
- 姓名
- 科室
- 模糊
- 精确
- 全院
- 当前机构

测试也不知道什么算完成。

改成：

```
DOC-SEARCH-001
系统必须允许已认证患者按医生展示名称进行关键字搜索。

DOC-SEARCH-002
搜索必须为大小写不敏感。

DOC-SEARCH-003
当没有匹配结果时，系统必须返回空结果集合而不是错误。

DOC-SEARCH-004
默认搜索结果不得包含状态为 INACTIVE 的医生。
```

---

# 4. 避免模糊词

高风险词：

- 快速
- 及时
- 方便
- 灵活
- 合理
- 友好
- 强大
- 尽可能
- 一般
- 正常
- 等
- 支持

看到这些词就问：

> 如何测试？

---

# 5. Requirement Pattern

## 5.1 Trigger + Behavior

```
当 [事件] 时，系统必须 [行为]。
```

例：
> 当患者提交有效预约请求时，系统必须创建 Appointment。

## 5.2 Condition + Behavior

```
如果 [条件]，系统必须 [行为]。
```

## 5.3 Prohibition

```
系统不得 [行为]。
```

## 5.4 Quality

```
在 [环境/负载] 下，[指标] 必须 [阈值]。
```

---

# 6. Requirement Card

建议每条需求可以写成：

```markdown
## APPT-CAN-003 取消时间窗口

Type: Functional  
Source: BR-005  
Priority: Must  
Status: Approved  
Verification: API Test

Requirement:
当患者取消自己的 BOOKED 预约时，如果距离预约开始时间少于2小时，系统必须拒绝取消。

Rationale:
保护临近预约的运营安排。

Related:
- UC-APPT-03
- POST /appointments/{id}/cancel
- TC-CAN-003
```

---

# 7. Preconditions

Precondition 不是系统要执行的动作，而是开始前必须成立。

例：
- 用户已登录
- Appointment 存在
- Appointment 属于当前 Tenant

如果 Precondition 不满足，要定义：
- 直接拒绝？
- 引导登录？
- 返回什么？

---

# 8. Postconditions

成功后：
- 数据状态
- 外部状态
- 审计
- 通知

失败后也应定义：

> 如果创建预约失败，不得留下部分有效 Appointment。

---

# 9. Exception Specification

每个重要功能至少检查：

### Input
- null
- empty
- invalid format
- too long
- unexpected enum

### Object
- not found
- deleted
- inactive

### Permission
- unauthenticated
- unauthorized
- wrong tenant
- wrong owner

### State
- invalid current state
- repeated request

### Concurrency
- same resource simultaneously

### Dependency
- timeout
- unavailable
- malformed response

### Infrastructure
- DB failure
- network failure

---

# 10. Boundary

规则：

```
Cancellation allowed if >= 2h
```

必须检查：
- 1:59:59
- 2:00:00
- 2:00:01

还要明确：
- 时区
- 精度
- 服务端时间还是客户端时间

---

# 11. 时间规格

非常容易漏。

至少定义：
- timezone
- timestamp format
- UTC storage?
- daylight saving?
- date vs datetime
- inclusive/exclusive boundary

示例：

> 所有 API timestamp 使用 ISO 8601，并包含时区偏移；服务端存储统一使用 UTC。

是否采用该规则要由项目决定，不能照抄。

---

# 12. List / Search Spec

列表功能必须定义：

- filtering
- sorting
- pagination
- default order
- empty result
- maximum page size
- duplicate handling
- authorization scope

---

# 13. Delete 的含义

“删除”可能是：
- 物理删除
- logical delete
- deactivate
- archive
- cancel

Spec 必须明确。

尤其审计型系统，不要让“DELETE”自然等于物理删除。

---

# 14. Error Model

建议统一：

```json
{
  "code": "SLOT_ALREADY_BOOKED",
  "message": "The selected slot is no longer available.",
  "traceId": "..."
}
```

Spec 需要定义：
- HTTP status
- business code
- user-safe message
- traceability
- 是否泄漏内部信息

---

# 15. Consistency

同一字段在不同地方必须一致：

```
Requirement: appointment.status
API: status
DB: appointment_status
State Diagram: BOOKED
```

名称可以因层不同而不同，但映射必须明确。

---

# 16. SRS Review Checklist

## Scope
- In Scope / Out of Scope 明确？

## Requirement
- 每条单一？
- 可测试？
- 无歧义？

## State
- 状态完整？
- 转换条件完整？

## Data
- 字段定义？
- Null？
- 生命周期？

## API
- Error？
- Auth？
- Idempotency？

## Security
- Object authorization？
- Sensitive data？

## Reliability
- Failure？
- Retry？
- Transaction？

## Testing
- 有 Acceptance Criteria？
- 有 Traceability？

---

# 17. Definition of Ready

一个 Story/Requirement 进入开发前，建议确认：

- Goal
- Scope
- Actor
- Business Rule
- Main Flow
- Exception
- State
- Data
- Permission
- API/Contract
- AC
- Dependencies
- NFR
- Open Questions

不是所有小需求都需要全部项目，但重要功能不能带着关键未知进入实现。

---

# 18. 完整改写案例

原始：

> “用户可以重置密码。”

候选问题：

- 谁？
- 如何触发？
- 如何验证身份？
- Token 多久过期？
- 是否一次性？
- 重发是否让旧 Token 失效？
- 密码强度？
- 是否踢掉旧 Session？
- 是否限流？
- 是否审计？
- 是否泄漏账号存在？

一个完整 Spec 会拆成：
- Account Recovery
- Reset Token
- Password Policy
- Rate Limit
- Session Invalidation
- Notification
- Audit
- Security

---

# 19. 本章练习

把以下原始描述：

> “管理员可以管理用户，用户可以登录，忘记密码可以找回，系统安全稳定。”

扩写为：

- Scope
- Glossary
- 5 Business Rules
- 20 Requirements
- 1 Permission Matrix
- 1 State Model
- 3 NFR
- 15 Acceptance Criteria
- 10 Open Questions

---

# 20. 自测

1. 为什么 Spec 越长不一定越好？
2. 什么是 Postcondition？
3. 为什么时间规则必须定义边界？
4. 删除为什么是危险词？
5. Error Model 应包含哪些信息？
6. Definition of Ready 有什么价值？
7. 什么是 Requirement 原子性？
8. 为什么 SRS 可以拆成多个文件？

---

# 21. 权威原始资料

1. IREB CPRE Foundation  
https://cpre.ireb.org/en/concept/foundationlevel

2. IREB Foundation Syllabus / 中文版本下载  
https://cpre.ireb.org/en/downloads-and-resources/downloads

3. IBM Requirements Management  
https://www.ibm.com/cn-zh/think/topics/what-is-requirements-management

4. Microsoft Architecture Design Specification  
https://learn.microsoft.com/en-us/azure/well-architected/architect-role/architecture-design-specification

5. Microsoft API Design  
https://learn.microsoft.com/zh-cn/azure/architecture/best-practices/api-design

6. Atlassian Product Requirements Template  
https://www.atlassian.com/software/confluence/templates/product-requirements

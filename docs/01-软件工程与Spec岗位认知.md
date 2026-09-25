# 01 软件工程与 Spec Engineer 岗位认知

> 本章目标：理解 Spec Engineer 在软件生命周期中的位置、与 PM/BA/Developer/QA/Architect 的分工，以及为什么“把模糊需求变成无歧义规格”是一项独立工程能力。

## 1. 软件开发不是“需求来了就写代码”

一个商业软件通常经历：

```text
Business Problem
↓
Product Goal
↓
Requirements Elicitation
↓
Analysis
↓
Specification
↓
Architecture / Design
↓
Implementation
↓
Testing
↓
Release
↓
Operation
↓
Feedback / Change
```

现实中并非完全线性，而是反复迭代。

Spec Engineer 主要参与：
- Requirements
- Analysis
- Specification
- Validation
- Traceability
- Change

但必须理解后续设计、开发、测试如何消费 Spec。

## 2. “Spec Engineer”为什么名字不统一

不同公司可能叫：

- Specification Engineer
- Requirements Engineer
- Software Requirements Engineer
- System Requirements Engineer
- System Analyst
- Business/System Analyst
- Functional Analyst

岗位边界会不同。

因此不要只学岗位名称，要学可迁移能力。

本仓库采用最稳定的核心：

> Requirements Engineering + Technical Specification + Verification。

## 3. Product Manager

主要关注：

- 为什么做？
- 为谁做？
- 价值是什么？
- 优先级？
- Roadmap？

示例：

> “我们要减少患者电话预约比例。”

这是产品目标。

## 4. Business Analyst

主要关注：

- 当前流程
- 业务规则
- 用户需求
- 流程优化
- 业务与系统之间的映射

有些公司 BA 与 Spec/Requirements Engineer 高度重合。

## 5. Spec / Requirements Engineer

主要关注：

- 系统准确应该怎样表现？
- 条件？
- 输入？
- 输出？
- 状态？
- 错误？
- 数据？
- 权限？
- NFR？
- 如何验证？
- 如何追踪？

价值是：

> 减少不同角色对同一句需求的不同解释。

## 6. Architect

主要关注：

- 系统边界
- 组件
- 技术方案
- 数据架构
- 可靠性
- 性能
- 安全
- Trade-off

Spec Engineer 不应代替 Architect，但需求会直接影响架构。

## 7. Developer

主要关注：

> 如何实现已明确的行为。

现实中 Developer 也会参与需求澄清，因为很多歧义只有实现时才暴露。

## 8. QA / Tester

主要关注：

> 如何证明实现符合要求？

优秀 QA 会不断问：
- 边界？
- 异常？
- 权限？
- 状态？
- 并发？

因此 QA 是 Spec Review 的重要参与者。

## 9. 一张对照表

| 角色 | 核心问题 |
|---|---|
| PM | 为什么做 / 做什么 |
| BA | 业务怎样工作 |
| Spec/RE | 系统准确必须怎样表现 |
| Architect | 系统如何组织以满足要求 |
| Developer | 如何实现 |
| QA | 如何证明正确 |

实际组织中会重叠，不要把边界理解得过于僵硬。

## 10. 一个模糊需求的变化过程

原始：

> “登录失败 5 次锁账号。”

听起来很清楚，但至少缺：

- 连续还是累计？
- 时间窗口？
- 成功一次是否清零？
- 锁多久？
- 管理员能否解锁？
- 不同设备是否共享计数？
- 密码和 OTP 是否一起算？
- 返回错误如何避免泄漏账号状态？
- 是否记录审计？
- 是否通知用户？

Spec Engineer 的工作就是把这些“隐含决定”暴露出来。

## 11. Business Requirement → System Requirement

Business：

> 降低账号暴力破解风险。

System：

```
AUTH-LOCK-001
如果同一账号在 15 分钟内连续发生 5 次无效密码认证，
系统必须阻止后续密码登录 30 分钟。
```

继续需要确认：
- 这些数字谁决定？
- MFA 是否受影响？
- Admin policy？
- User notification？

## 12. 正确实现 vs 正确需求

两个问题不同：

### Build the system right
实现是否符合 Spec？

### Build the right system
Spec 是否满足真正业务目标？

例如：
- Developer 完全按错误 Spec 实现
- Code 没 Bug
- 业务仍失败

所以 Requirements Validation 很重要。

## 13. Spec 的抽象层次

可以理解为：

### Business
为什么？

### User
用户要什么？

### System
系统做什么？

### Technical Contract
接口/数据/状态如何定义？

### Verification
怎么证明？

优秀 Spec Engineer 要能上下移动，而不是只停留在页面需求。

## 14. 什么不是 Spec Engineer 的核心工作

不是：
- 把会议录音转成 Word
- 机械抄 Product 文档
- 只画原型
- 只维护 Jira
- 替 Architect 决定所有技术
- 替 Product 决定所有业务
- 替 QA 写完所有测试

核心是：

> 把需求变成一致、可实现、可验证、可追踪的工程事实。

## 15. 软件开发模型

### Waterfall
阶段较明确，文档和审批通常更正式。

### Agile
迭代、小批量交付、持续 Refinement。

### DevOps
开发、交付、运维进一步连通。

Spec Engineering 在不同模式下形式不同，但“清晰和验证”不会消失。

## 16. AI Coding 时代变化

过去：

```
需求
→ Developer 理解
→ Code
```

现在越来越可能：

```
需求
→ Spec
→ AI Coding Agent / Developer
→ Automated Test
→ Verification
```

因此未来重要能力更加集中在：
- Context
- Constraints
- Contract
- Verification

不是“完全不需要人写代码”，而是高质量输入与验证变得更重要。

## 17. 岗位能力地图

### Business
- Stakeholder
- Process
- Rule

### Requirement
- Elicitation
- Analysis
- Documentation
- Validation
- Management

### Modeling
- UML
- BPMN
- State
- Sequence

### Technical
- HTTP
- API
- Database
- Architecture
- Security

### Quality
- Acceptance
- Testing
- NFR

### Collaboration
- Git
- Jira
- Confluence
- Review
- Change

### AI
- Context Engineering
- Structured Prompt
- AI Review
- Verification

## 18. 本章练习

原始：

> “我们需要一个文件上传功能。”

请写至少 15 个需要澄清的问题，例如：
- 谁上传？
- 文件类型？
- 最大大小？
- 病毒扫描？
- 重名？
- 存储？
- 权限？
- 下载？
- 删除？
- 失败？
- 进度？
- 审计？

然后把问题分类：
- Business
- Functional
- Data
- Security
- NFR
- Technical

## 19. 自测

1. PM 与 Spec Engineer 的关注点区别？
2. BA 与 Requirements Engineer 为什么可能重叠？
3. Developer 为什么也要参加需求 Review？
4. Verification 与 Validation 为什么不同？
5. “支持上传文件”为什么不够开发？
6. AI Coding 为什么反而提高了 Spec 的重要性？

## 20. 本章完成标准

你能够清楚向别人解释：

> Spec Engineer 不是“文档秘书”，而是把业务意图转换成可实现、可验证软件契约的人。

## 21. 权威原始资料

1. IREB CPRE Foundation  
https://cpre.ireb.org/en/concept/foundationlevel

2. IREB Foundation Syllabus / 中文下载  
https://cpre.ireb.org/en/downloads-and-resources/downloads

3. IBM Requirements Management  
https://www.ibm.com/cn-zh/think/topics/what-is-requirements-management

4. Microsoft Software Development Fundamentals  
https://learn.microsoft.com/zh-cn/shows/software-development-fundamentals/

5. Microsoft Architecture Design Specification  
https://learn.microsoft.com/en-us/azure/well-architected/architect-role/architecture-design-specification

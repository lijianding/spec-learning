# 01 软件工程与 Spec 岗位认知

## 本章目标

学完后你应该能够：

- 解释软件从需求到上线的大致生命周期
- 说明 Spec Engineer 与产品、BA、架构师、开发、测试的区别
- 识别一条需求从模糊想法到可开发规格需要经历哪些步骤
- 理解为什么“需求正确”与“实现正确”是两个不同问题

## 一、软件开发不是“直接写代码”

一个功能通常会经历：

**问题/目标 → 需求获取 → 需求分析 → 规格说明 → 设计 → 开发 → 测试 → 发布 → 运行反馈 → 变更**

Spec Engineer 主要工作在“需求分析—规格说明—验证—变更”之间，但需要理解整个链路。

## 二、几个常见角色

### 产品经理 / Product Manager

关注：

- 为什么做
- 为谁做
- 有什么业务价值
- 优先级是什么

### Business Analyst / BA

关注：

- 当前业务怎么运行
- 用户真正需要什么
- 业务规则是什么
- 流程如何优化

### Spec / Requirements Engineer

关注：

- 系统必须表现成什么样
- 输入、输出、状态、规则、异常是什么
- 需求之间是否冲突
- 是否可以实现和验证
- 如何追踪需求到设计、开发与测试

### Software Architect

关注：

- 系统如何组织
- 组件如何协作
- 如何满足可靠性、安全、性能、扩展性

### Developer

关注：

- 如何通过代码实现

### Tester / QA

关注：

- 如何证明实现符合要求
- 什么场景会失败
- 边界和异常是否正确

## 三、需求正确 vs 实现正确

假设需求写成：

“连续登录失败 5 次后锁定账户。”

这句话看起来很明确，实际上至少缺少：

- 5 次是在多长时间窗口内？
- 成功一次后计数是否清零？
- 锁定多久？
- 管理员可否解锁？
- 多设备同时尝试怎么计数？
- 锁定后 API 返回什么？
- 是否写审计日志？
- 是否需要通知用户？

开发人员可以完全按照自己的理解实现，但不同开发者可能做出不同系统。

因此 Spec Engineer 的价值不是“复述需求”，而是**减少系统行为的解释空间**。

## 四、Spec 的四个层次

### 1. Business Requirement

描述业务目标。

例：

“降低人工预约登记工作量。”

### 2. User Requirement

描述用户需要完成什么。

例：

“患者可以在线选择医生和时间段预约。”

### 3. System / Software Requirement

描述系统行为。

例：

“当用户提交预约时，系统必须验证该号源仍可预约。”

### 4. Technical Specification

描述接口、数据、错误处理、约束等。

例：

- API：POST /appointments
- 请求字段：doctorId、slotId、patientId
- 冲突返回：409
- 创建成功返回：201
- appointment.status 初始值：BOOKED

Spec Engineer 可能根据组织分工负责其中一个或多个层次。

## 五、功能需求与非功能需求

### 功能需求

回答“系统做什么”。

例如：

- 创建预约
- 取消预约
- 查询排班

### 非功能需求

回答“系统做得怎么样”。

例如：

- 95% 查询请求在 300 ms 内返回
- 月可用性达到 99.9%
- 敏感数据传输必须加密
- 关键操作保留审计日志
- 系统支持至少 1,000 个并发用户

Microsoft Azure Architecture Center 明确将功能需求与非功能需求区分，并指出可扩展性、可用性、延迟等会直接影响架构和技术选择。

## 六、需求到测试的闭环

成熟团队中的需求不应该停留在文档。

理想链路：

Business Goal
→ Requirement
→ Spec
→ Design
→ Development Task
→ Test Case
→ Test Result
→ Release

如果出现缺陷，可以反向追踪：

“哪个 Requirement 定义了这个行为？”

如果需求变更，可以正向追踪：

“哪些 API、表字段、测试用例会受影响？”

这就是 Traceability（可追溯性）。

## 七、本章练习

针对“医疗预约 SaaS”，完成以下内容：

1. 写 3 条业务目标。
2. 写 5 条用户需求。
3. 从其中 1 条用户需求拆出至少 5 条系统需求。
4. 给每条系统需求分配唯一 ID。
5. 标记哪些是功能需求，哪些是非功能需求。
6. 写出产品、Spec、开发、测试在该需求中的职责。

## 推荐官方资料

- IREB CPRE Foundation：<https://cpre.ireb.org/en/concept/foundationlevel>
- IBM：什么是需求管理：<https://www.ibm.com/cn-zh/think/topics/what-is-requirements-management>
- Microsoft Learn 软件开发基础：<https://learn.microsoft.com/zh-cn/shows/software-development-fundamentals/01>
- Microsoft Azure：针对业务需求进行构建：<https://learn.microsoft.com/zh-cn/azure/architecture/guide/design-principles/build-for-business>

# 05 SRS 与高质量 Spec 写作

## 本章目标

这一章是整个课程的核心。

你要学会把模糊描述改写为：

- 唯一编号
- 单一职责
- 条件明确
- 输入明确
- 行为明确
- 输出明确
- 异常明确
- 可测试
- 可追踪

## 一、SRS 是什么

SRS（Software Requirements Specification）是软件需求规格说明。

典型内容：

1. 背景与目标
2. 范围
3. 术语
4. Stakeholder
5. Assumption / Constraint
6. Functional Requirements
7. Business Rules
8. External Interfaces
9. Data Requirements
10. Non-functional Requirements
11. Acceptance / Verification
12. Traceability

不同公司可以拆成多份文档，不必拘泥于一个“大 Word”。

## 二、最常见的坏需求

### 模糊词

- 快速
- 友好
- 灵活
- 尽可能
- 正常情况
- 合理
- 及时
- 支持

它们通常不可测试。

### 合并多个行为

坏：

“系统保存预约并通知患者和医生。”

可能需要拆成：

- APPT-001 创建预约
- NOTIF-001 给患者生成通知
- NOTIF-002 给医生生成通知
- NOTIF-003 通知失败不得回滚预约

## 三、推荐句式

### 事件驱动

**当 [Trigger] 时，系统必须 [Behavior]。**

### 条件驱动

**如果 [Condition]，系统必须 [Behavior]。**

### 持续约束

**系统必须 [Constraint]。**

### 可选能力

**系统必须允许 [Actor] [Action]。**

## 四、示例：从一句话拆成 Spec

原始：

“患者可以取消预约。”

可拆：

### APPT-CAN-001

当预约状态为 BOOKED 且距离预约开始时间不少于 2 小时时，系统必须允许患者取消自己的预约。

### APPT-CAN-002

患者不得取消属于其他患者的预约。

### APPT-CAN-003

取消成功后，系统必须将预约状态更新为 CANCELLED。

### APPT-CAN-004

取消成功后，系统必须将关联号源恢复为 AVAILABLE。

### APPT-CAN-005

当预约距离开始时间不足 2 小时时，系统必须拒绝患者取消，并返回业务错误码 APPT_409_CANCEL_WINDOW_CLOSED。

这样开发和测试都可以逐条实现/验证。

## 五、Precondition / Trigger / Main Flow / Postcondition

复杂功能建议增加：

### Preconditions

执行前必须成立。

### Trigger

什么事件触发。

### Main Flow

正常路径。

### Alternative / Exception

其他路径。

### Postconditions

完成后的系统状态。

## 六、错误和异常必须进入 Spec

不要只写 Happy Path。

至少考虑：

- 参数为空
- 参数格式错误
- 对象不存在
- 无权限
- 状态不允许
- 重复提交
- 并发冲突
- 第三方超时
- 数据库异常
- 网络中断

## 七、边界条件

假设字段要求 1~100：

至少考虑：

- null
- 0
- 1
- 100
- 101
- 非数字
- 极大值

Spec Engineer 应主动找边界。

## 八、Definition of Ready

一个需求进入开发前，至少应确认：

- 目标明确
- Scope 明确
- Actor 明确
- 主流程明确
- 异常明确
- 数据明确
- 权限明确
- 验收标准明确
- 依赖明确
- Open Question 已关闭或被接受

## 九、Spec Review

评审应让：

- Product 验证业务正确
- Developer 验证可实现性和技术歧义
- QA 验证可测试性和场景完整性
- Security/Architecture 在必要时验证质量属性

评审不是“找错字”。

## 十、练习

把以下五句话改写成至少 15 条可测试需求：

1. 用户可以登录。
2. 用户可以搜索医生。
3. 患者可以预约。
4. 医生可以管理排班。
5. 系统要安全、快速。

## 推荐资料

- IREB CPRE：<https://cpre.ireb.org/en/concept/foundationlevel>
- IBM 需求管理：<https://www.ibm.com/cn-zh/think/topics/what-is-requirements-management>
- Atlassian 产品需求：<https://www.atlassian.com/software/confluence/templates/product-requirements>
- Microsoft Azure 架构设计原则：<https://learn.microsoft.com/zh-cn/azure/architecture/guide/>

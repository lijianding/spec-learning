# 08 HTTP、REST API 与 OpenAPI：Spec Engineer 实战

> 本章目标：从“不懂 API”到能够阅读、设计和评审常见 HTTP API Spec，并识别权限、错误、分页、幂等、版本等关键问题。

## 1. API 是什么

API 是系统之间的契约。

对 Spec Engineer 来说，重点不是背代码，而是明确：

- 谁调用？
- 调什么？
- 输入？
- 输出？
- 成功？
- 失败？
- 权限？
- 数据格式？
- 重复调用？
- 兼容性？

## 2. HTTP Request

典型请求：

```http
POST /api/v1/appointments HTTP/1.1
Authorization: Bearer <token>
Content-Type: application/json

{
  "patientId": "P001",
  "slotId": "S001"
}
```

组成：
- Method
- URL / Path
- Headers
- Body

## 3. HTTP Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "A001",
  "status": "BOOKED"
}
```

组成：
- Status Code
- Headers
- Body

## 4. HTTP 方法

### GET
读取资源。

### POST
创建资源或触发动作。

### PUT
通常表示整体替换。

### PATCH
通常表示部分更新。

### DELETE
删除资源。但业务系统中应先确认“删除”的真实业务含义。

MDN 原始资料：
https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Reference/Methods

## 5. Status Code

| Code | 常见意义 |
|---|---|
| 200 | 成功 |
| 201 | 创建成功 |
| 204 | 成功但无 Body |
| 400 | 请求格式/参数问题 |
| 401 | 未认证 |
| 403 | 已认证但无权限 |
| 404 | 资源不存在 |
| 409 | 当前状态冲突 |
| 422 | 语义校验失败 |
| 429 | 请求过多 |
| 500 | 服务内部错误 |
| 503 | 暂时不可用 |

注意：401 与 403 不相同。

## 6. Resource-Oriented Design

不推荐把所有接口都设计成 RPC 式名称：

```text
POST /createAppointment
POST /getAppointment
POST /deleteAppointment
```

更常见：

```text
POST /appointments
GET /appointments/{id}
DELETE /appointments/{id}
```

但不要机械化。业务动作例如取消预约，也可以使用：

```text
POST /appointments/{id}/cancel
```

Microsoft API Design 强调围绕资源组织 URI，并利用 HTTP 语义表达操作。

## 7. Path / Query / Body

### Path Parameter

```text
GET /appointments/{appointmentId}
```

通常代表资源标识。

### Query

```text
GET /appointments?status=BOOKED&page=1&pageSize=20
```

常用于：
- filter
- sort
- pagination

### Body

常用于创建或更新复杂数据。

## 8. API Spec 最低字段

每个 Endpoint 至少定义：

- Purpose
- Method
- Path
- Authentication
- Authorization
- Headers
- Path Params
- Query Params
- Request Body
- Validation
- Success Response
- Error Response
- Idempotency
- Audit
- Example

## 9. Schema

Request 示例：

```json
{
  "patientId": "P001",
  "slotId": "S001"
}
```

还必须定义：

| Field | Type | Required | Rule |
|---|---|---|---|
| patientId | string | yes | 合法业务 ID |
| slotId | string | yes | 必须存在 |

只给 JSON 示例而不给 Schema，并不完整。

## 10. Validation 分层

### Syntax
- required
- type
- length
- format

### Referential
- ID exists

### Business
- Slot available
- Appointment state valid

### Permission
- user owns patientId

不同失败可以映射不同 Business Code。

## 11. Error Model

统一错误：

```json
{
  "code": "SLOT_ALREADY_BOOKED",
  "message": "Selected slot is unavailable.",
  "traceId": "abc123"
}
```

不要把服务器堆栈直接返回给用户。

## 12. 400 / 404 / 409

### 400
请求本身格式错误。

### 404
资源不存在。

### 409
请求格式正确、资源存在，但当前业务状态冲突。

例如 Slot 已被占用。

团队应形成一致标准。

## 13. Pagination

### Page Based

```text
?page=2&pageSize=20
```

### Cursor Based

```text
?cursor=abc&limit=20
```

Spec 应定义：
- default
- max
- ordering
- total 是否返回
- empty result
- invalid cursor

## 14. Filtering

```text
GET /appointments?status=BOOKED&doctorId=D001
```

需要定义：
- 支持哪些字段
- 多值如何处理
- AND/OR
- unknown filter 怎么办

## 15. Sorting

```text
?sort=startAt,-createdAt
```

明确：
- 默认排序
- 同值时如何稳定排序

否则分页结果可能不稳定。

## 16. Idempotency

创建类请求可能因为网络重试而重复。

业务目标：

> 同一次业务操作重复请求时，不得重复产生副作用。

预约、支付等场景尤其重要。

## 17. Optimistic Concurrency

用户 A 和 B 同时编辑同一资源时，可能使用：
- version
- ETag / If-Match

Spec 要明确：
- 旧版本更新是否拒绝？
- 冲突时是否允许覆盖？

## 18. Authentication 与 Authorization

Authentication：
> 你是谁？

Authorization：
> 你能访问什么？

危险接口：

```text
GET /patients/{patientId}/appointments
```

即使用户已登录，也必须检查是否有权访问该 patientId。

OWASP API Security 特别强调对象级授权风险。

## 19. Rate Limiting

登录、OTP、搜索、批量导出等可能需要限流。

Spec 可定义：
- scope
- threshold
- 429
- retry info

数字必须来自安全/容量要求，而不是照抄案例。

## 20. Versioning

常见方式：

```text
/api/v1/
```

关键不是路径形式，而是：

> 如何保证已有 Client 不因为 API 改动突然失败？

Breaking Change 例：
- 删除字段
- 改字段类型
- 改 enum 语义
- 新增 required 字段

## 21. Compatibility

Microsoft Architecture Design Specification 特别要求考虑：
- API/data contracts
- compatibility
- migration
- rollback

因此修改 API 时要明确：
- old behavior
- new behavior
- impact
- migration
- deprecation

## 22. OpenAPI

OpenAPI 是 HTTP API 的机器可读标准。

```yaml
openapi: 3.1.0
info:
  title: Appointment API
  version: 1.0.0
paths:
  /appointments:
    post:
      summary: 创建预约
      responses:
        '201':
          description: 创建成功
        '409':
          description: Slot conflict
```

## 23. OpenAPI 的价值

- 自动文档
- Mock
- Client generation
- Validation
- Contract Testing
- AI Coding Context

但 OpenAPI 不能替代：
- Business Rule
- State Model
- Complex Authorization
- NFR

## 24. API Review Checklist

### Contract
- path/method？
- schema？
- null？
- enum？

### Behavior
- success？
- errors？
- duplicate？
- concurrency？

### Security
- auth？
- object permission？
- sensitive field？

### Operability
- traceId？
- pagination？
- rate limit？

### Compatibility
- breaking change？
- version？

## 25. 完整练习

为“会议室预订系统”设计：

1. GET /rooms
2. GET /rooms/{id}/slots
3. POST /bookings
4. GET /bookings/{id}
5. POST /bookings/{id}/cancel

每个 endpoint 写：
- Request
- Success
- 至少 4 Error
- Permission
- Idempotency
- Validation
- Example

然后写 OpenAPI 草稿。

## 26. 自测

1. 401 和 403 区别？
2. 409 适合什么情况？
3. Path 与 Query 区别？
4. 为什么 JSON 示例不等于 Schema？
5. Idempotency 解决什么？
6. 为什么分页需要稳定排序？
7. Authorization 为什么不能只在前端做？
8. 什么是 Breaking Change？
9. OpenAPI 能替代完整 Spec 吗？

## 27. 权威原始资料

1. MDN HTTP 中文  
https://developer.mozilla.org/zh-CN/docs/Web/HTTP

2. MDN HTTP Methods  
https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Reference/Methods

3. MDN HTTP Status  
https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Reference/Status

4. Microsoft RESTful Web API Design  
https://learn.microsoft.com/zh-cn/azure/architecture/best-practices/api-design

5. OpenAPI Specification  
https://spec.openapis.org/oas/latest.html

6. OpenAPI Initiative  
https://www.openapis.org/

7. OWASP API Security  
https://owasp.org/projects/api-security-project

8. Microsoft Architecture Design Specification  
https://learn.microsoft.com/en-us/azure/well-architected/architect-role/architecture-design-specification

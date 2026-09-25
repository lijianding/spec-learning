# 08 HTTP、REST API 与 OpenAPI

## 本章目标

学完后你应能读懂并编写基础 API Spec。

## 一、HTTP 基础

常见方法：

- GET：读取
- POST：创建/触发操作
- PUT：整体替换
- PATCH：部分更新
- DELETE：删除

不要死记，要理解“资源 + 行为”。

## 二、状态码

常见：

- 200 OK
- 201 Created
- 204 No Content
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 409 Conflict
- 422 Unprocessable Content
- 500 Internal Server Error

Spec 中不要只写“失败返回错误”，应明确不同失败场景。

## 三、一个 API Spec 至少包含

- Endpoint
- Method
- Purpose
- Authentication
- Authorization
- Request headers
- Path params
- Query params
- Request body
- Validation rules
- Success response
- Error responses
- Idempotency
- Rate limit（需要时）
- Audit requirement
- Examples

## 四、示例：创建预约

**POST /api/v1/appointments**

Request:

```json
{
  "patientId": "P10001",
  "doctorId": "D20001",
  "slotId": "S30001"
}
```

成功：

```json
{
  "id": "A90001",
  "status": "BOOKED"
}
```

状态码：201

失败示例：

- 400：字段格式错误
- 403：无权限为该患者创建预约
- 404：doctorId / patientId / slotId 不存在
- 409：号源已被占用

## 五、验证规则

每个字段定义：

| 字段 | 类型 | 必填 | 规则 |
|---|---|---|---|
| patientId | string | 是 | 必须存在 |
| doctorId | string | 是 | 必须为有效医生 |
| slotId | string | 是 | 状态必须 AVAILABLE |

## 六、幂等性

重复请求是否产生重复结果？

支付、预约创建等敏感操作需要特别考虑。

可采用：
- Idempotency-Key
- 唯一业务键
- 重复请求检测

Spec 应描述业务预期，不必替开发决定所有实现细节。

## 七、分页、筛选、排序

列表 API 要明确：
- page / pageSize 或 cursor
- 最大 pageSize
- 默认排序
- 可筛选字段
- 可排序字段
- 空结果格式

## 八、版本

常见：
- /api/v1/
- Header versioning

Spec 必须考虑破坏性变更如何管理。

## 九、OpenAPI

OpenAPI 是描述 HTTP API 的标准，可表达：
- paths
- methods
- parameters
- schemas
- responses
- security

简化示例：

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
          description: 号源冲突
```

## 十、安全检查

每个 API 问：
- 是否必须登录？
- 谁能访问？
- 是否可能越权读取别人的数据？
- 是否限制批量查询？
- 是否泄漏敏感字段？
- 是否需要审计？

## 十一、练习

编写以下 API Spec：
1. 登录
2. 查询医生
3. 查询可用号源
4. 创建预约
5. 取消预约
6. 查询我的预约

每个 API 至少写 3 个错误场景。

## 官方资料

- MDN HTTP：<https://developer.mozilla.org/zh-CN/docs/Web/HTTP>
- OpenAPI Specification：<https://spec.openapis.org/oas/latest.html>
- Microsoft RESTful API 设计：<https://learn.microsoft.com/zh-cn/azure/architecture/best-practices/api-design>
- OWASP API Security：<https://owasp.org/www-project-api-security/>

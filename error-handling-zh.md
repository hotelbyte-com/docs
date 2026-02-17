# OpenAPI 错误处理

本文档说明 HotelByte OpenAPI 的推荐错误处理方式。

## HTTP 状态码 vs 业务错误码

我们的 API 使用**双层错误体系**：

1. **HTTP 状态码**：标准 HTTP 响应状态（200、400、401 等）
2. **业务错误码**：响应体中的业务错误码

**重点**：不要只看 HTTP 状态码，必须检查响应体里的 `code` 字段。

## HTTP 状态码说明

| HTTP 状态 | 含义                  | 说明                                |
|-----------|-----------------------|-------------------------------------|
| **200**   | Success               | 请求处理成功                        |
| **400**   | Bad Request           | 请求参数校验失败                    |
| **401**   | Unauthorized          | Token 无效或已过期                  |
| **403**   | Forbidden             | 无资源访问权限                      |
| **404**   | Not Found             | 请求资源不存在                      |
| **429**   | Too Many Requests     | 请求频率超限                        |
| **500**   | Internal Server Error | 服务内部错误                        |
| **504**   | Gateway Timeout       | 请求处理超时                        |

## 业务错误码说明

业务错误码格式：`[x][xx][xx][xxxx]`

| 错误码        | HTTP 状态 | 含义                  | 说明                                |
|---------------|-----------|-----------------------|-------------------------------------|
| **0**         | 200       | Success               | 请求成功                            |
| **100000400** | 400       | Parameter Error       | 请求参数校验失败                    |
| **100000401** | 401       | Authentication Failed | Token 无效或已过期                  |
| **100000403** | 403       | Permission Denied     | 无资源访问权限                      |
| **100000404** | 404       | Not Found             | 请求资源不存在                      |
| **100000429** | 429       | Rate Limited          | 请求频率超限                        |
| **100000500** | 500       | System Error          | 服务内部错误                        |
| **100000504** | 504       | Timeout               | 请求处理超时                        |
| **100001003** | 500       | Duplicate Error       | 重复资源或重复操作                  |
| **100001004** | 500       | Dependency Error      | 外部依赖失败                        |
| **100001005** | 400       | Page Size Exceeded    | 分页大小超过上限                    |
| **100001006** | 400       | Not Implemented       | 功能未实现                          |
| **100001007** | 400       | Expired               | 资源或令牌已过期                    |
| **100001008** | 400       | Not Matched           | 数据不符合预期格式                  |
| **100001111** | 200       | ARI Changed           | 房态/价格信息已变化                 |
| **100001112** | 200       | Credit Limit          | 因额度不足导致下单失败              |

## 错误响应格式

所有错误响应统一如下：

```json
{
  "code": 100000400,
  "msg": "param error"
}
```

## 成功响应格式

成功响应包含 `data`：

```json
{
  "code": 0,
  "msg": "Success",
  "data": {
    // 业务数据
  }
}
```

## 错误处理最佳实践

1. 始终检查 `body.code`，不要只看 HTTP 状态码。
2. 业务错误可能是 HTTP 200 + 非 0 `code`。
3. 传输层错误使用 4xx/5xx 等 HTTP 状态码。
4. 重试策略应结合业务错误码，不要只按 HTTP 状态码判断。

## Booking 状态处理建议（Book API）

客户问得很直接：收到 `status=1` 或 `status=0`，该不该拒单？  
结论也直接：**先别拒单，先查单**。

只要 Book 返回 `HTTP 200` 且 `code=0`，就按下面做：

1. 先记下订单号：
- `customerReferenceNo`
- `supplierReferenceNo`（如果有）

2. 立刻调用 `QueryOrders`：
- 建议用 `customerReferenceNo` 查询

3. 按查到的状态处理：
- `1 (Confirming)`：还在处理中，继续等
- `0 (Unknown)`：状态不明确，继续查
- `2 (Confirmed)`：下单成功，结束
- `3 (Cancelled)`、`4 (Failed)`、`5 (CancelFailed)`：下单失败，结束

4. 轮询节奏（最多约 10 分钟）：
- 立即查一次
- 30 秒后查一次
- 1 分钟后查一次
- 2 分钟后查一次
- 后续拉长间隔继续查，总窗口最多约 10 分钟

5. 查单期间不要重复下单：
- 否则容易造成重复订单或对账问题

6. 超过 10 分钟还不是终态：
- 先把订单标记为“处理中”
- 带上 `customerReferenceNo` / `supplierReferenceNo` 联系支持团队

7. 状态回调能力：
- 我们平台即将上线订单状态通知回调，后续可减少轮询，敬请关注发布通知

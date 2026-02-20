# OpenAPI Error Handling 指南

本文档定义 HotelByte OpenAPI 的统一错误处理规范，并补充 **Book** 接口在 `status=0 (Unknown)` 与 `status=1 (Confirming)` 场景下的处理规则。

---

## 1. 适用范围

本指南适用于以下接口：
- `POST /api/trade/book`
- `POST /api/trade/queryOrders`
- `POST /api/trade/cancel`

本指南目标：
- 统一 HTTP 错误处理方式
- 明确 Book 返回 `status=0/1` 时的客户端动作
- 降低误判失败导致的拒单、重复下单和资损风险

---

## 2. 标准错误响应与 HTTP 错误分类

### 2.1 统一处理原则

- 客户端 **MUST** 先按 HTTP 状态码进行分流，再结合业务字段处理。
- 客户端 **MUST NOT** 将网络异常、超时、或非终态订单直接当作失败终态。
- 客户端 **SHOULD** 记录并持久化 `platformReferenceNo`、`customerReferenceNo`、`supplierReferenceNo`（若有），用于后续核实。

### 2.2 常见 HTTP 错误分类

| HTTP 状态码 | 错误类型 | 含义 | 客户端建议 |
|---|---|---|---|
| 400 | ParamErr | 请求参数错误 | 修正参数后再请求，不要直接重试相同请求 |
| 401 | AuthErr | 鉴权失败 | 检查 Token/JWT |
| 403 | Forbidden | 无权限 | 检查 appKey 权限范围 |
| 404 | NotFoundErr | 资源不存在（如 session 失效） | 重新走 Search/CheckAvail 流程 |
| 429 | RateLimitErr | 触发限流 | 退避重试，降低频率 |
| 504 | TimeoutErr | 上游/供应商处理超时 | 进入 QueryOrders 核实流程，不可立即判失败 |

---

## 3. Book 成功响应中的状态处理

当 Book 返回 **HTTP 200** 且响应中存在 `hotelOrder` 时，表示请求已被系统接受并进入订单处理流程。此时必须依据 `hotelOrder.status` 决定后续动作。

### 3.1 状态定义与终态判断

| `hotelOrder.status` | 名称 | 是否终态 | 客户端动作 |
|---|---|---|---|
| 0 | Unknown | 否 | 立即 QueryOrders 核实 |
| 1 | Confirming | 否 | 接受订单并轮询 QueryOrders |
| 2 | Confirmed | 是 | 视为成功 |
| 3 | Cancelled | 是 | 视为已取消终态 |
| 4 | Failed | 是 | 视为失败终态 |
| 5 | CancelFailed | 是 | 视为取消失败终态 |

### 3.2 `status=1 (Confirming)` 处理规则

- 该状态表示订单处于供应商确认中，**不是失败终态**。
- 客户端 **MUST NOT** 直接拒单。
- 客户端 **MUST** 保存订单标识并进入 QueryOrders 轮询。
- 客户端 **SHOULD** 在前端展示“确认中/处理中”，避免展示“已确认”。

### 3.3 `status=0 (Unknown)` 处理规则

- 该状态表示当前状态未知，需进一步核实，**不能直接判定失败**。
- 客户端 **MUST NOT** 直接拒单或立即重下单。
- 客户端 **MUST** 立即调用 QueryOrders 核实最新状态。
- 客户端 **SHOULD** 展示“状态核实中”，并提示稍后刷新。

---

## 4. QueryOrders 轮询与超时确认策略

### 4.1 何时必须调用 QueryOrders

- Book 返回 `504 TimeoutErr`
- Book 返回 `200` 且 `hotelOrder.status` 为 `0` 或 `1`

### 4.2 推荐轮询策略（最多约 10 分钟）

1. 第 1 次：立即查询
2. 第 2 次：等待 30 秒
3. 第 3 次：等待 1 分钟
4. 第 4 次：等待 2 分钟
5. 后续：按退避策略继续查询，总时长建议不超过约 10 分钟

### 4.3 轮询终止条件

当状态变为以下任一值即可停止轮询：
- `2 Confirmed`
- `3 Cancelled`
- `4 Failed`
- `5 CancelFailed`

### 4.4 防重与一致性要求

- 在 `status=0/1` 或 `504` 核实期间，客户端 **MUST NOT** 直接发起同业务语义的重复下单。
- 若业务必须重试下单，客户端 **SHOULD** 先确认原单已终态失败，并保持幂等控制。

---

## 5. 与 Cancel 的衔接规则

- Cancel 是订单取消动作，不是“查单替代动作”。
- 对 `status=0/1` 场景，客户端 **SHOULD** 先 QueryOrders 获取最新状态，再决定是否取消。
- 若用户明确要求取消且订单状态允许取消，可调用 Cancel。
- 若订单已进入终态（`2/3/4/5`），应按终态处理，不应重复取消。

---

## 6. FAQ

### Q1. 如果 Book 返回 `status=1` 或 `status=0`，我们现在在客户端直接 reject booking，应该怎么改？

**A：不要直接 reject。**

最短正确流程：
1. 接受 Book 的 HTTP 200 响应并保存订单号
2. 立刻调用 QueryOrders 核实状态
3. 若状态变为 `2`，按成功处理
4. 若状态变为 `3/4/5`，按终态失败/取消处理
5. 在核实完成前，不要重复下单

### Q2. Book 返回 504 是否一定失败？

不是。504 代表处理超时，不代表订单一定失败。应按本指南第 4 章执行 QueryOrders 核实流程。

---

## 7. 快速决策表

| 输入场景 | 立即动作 | 后续动作 |
|---|---|---|
| Book `200` + `status=2` | 记为成功 | 进入履约流程 |
| Book `200` + `status=1` | 接受订单，开始轮询 | 等待终态再结论 |
| Book `200` + `status=0` | 接受订单，立即核实 | 等待终态再结论 |
| Book `504` | 不判失败，立即核实 | 按轮询策略确认终态 |
| QueryOrders 返回 `3/4/5` | 结束轮询 | 按终态处理（取消/失败） |

---

## 附录：更新记录

| 日期 | 说明 |
|---|---|
| 2026-02-14 | 正式版：整合通用错误处理与 Book `status=0/1` 规范，替代扩展文档 |

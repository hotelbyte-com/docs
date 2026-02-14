# CNBooking 集成与认证进展

**文档日期**: 2026-02-08  
**环境**: Sandbox (http://115.175.228.234:8078)  
**测试账号**: CustomerNo=EN000001  

---

## 一、已实现接口与测试结果摘要

| 环节 | 状态 | 说明 |
|-----|------|------|
| 供应商注册 | 完成 | protocol.Supplier_CNBooking = 38 |
| HotelsMetadata | 完成 | 映射 BaseHotelListSearch，DisplayReq=30，稳定返回 20 条 |
| HotelList | 完成 | 无 ID 时走 HotelsMetadata；有 ID 时走 HotelListSearch；按 request 补动态 |
| HotelRates | 完成 | RatePlanSearch，SessionParams 写入 |
| CheckAvail | 完成 | PreBookingCheck，从 Session 读参数 |
| Book | 完成 | Booking，从 Session 读参数 |
| QueryOrder | 完成 | OrderSearch |
| Cancel | 完成 | BookingCancel |

**测试结果**：

- **HotelsMetadata / BaseHotelListSearch**：通过。可稳定拿到酒店 ID 列表（当前 20 条/页）。
- **FullFlow**：能从 HotelsMetadata 取 ID 并执行；在 **HotelRates（RatePlanSearch）** 步骤对当前测试酒店全部返回 **HTTP 500（空 body）**，无法继续 CheckAvail → Book → QueryOrder → Cancel。

---

## 二、BaseHotelListSearch / HotelsMetadata 认证说明

### 接口与映射

- **供应商接口**: `POST /api/Hotel/BaseHotelListSearch`（供应商推荐，见 hotel-be#357）
- **我方标准接口**: HotelsMetadata（批量酒店元数据）
- **请求示例**: ScrollingInfo 使用供应商示例值 `DisplayReq: 30, PageItems: 20, PageNo: 1`；非 30 会报 `DisplayReq invalid`。

### 响应结构

- 成功时：`CnResponse.MessageInfo.Code` 为 `"30000"`，`Data.Hotels.HotelInfos` 为酒店列表。
- `HotelId` 在 API 中为数字，我方解析为 string 供后续 HotelRates 使用。

### 请求/响应日志引用

真实请求与响应已落盘到项目请求日志目录，可用于核对：

- 目录：`bi/data/cnbooking/requests/`
- 文件名模式：`HotelsMetadata_*.json`（按时间戳命名，如 `HotelsMetadata_20260208_085312_000.json`）
- 内容：含 requestBody（CNRequest）、response.statusCode、response.body（含 CnResponse.Data.Hotels.HotelInfos）

运行以下测试会生成新日志（ENV=DEV 时 middleware 写盘）：

```bash
ENV=DEV go test -v -run TestCNBooking_HotelsMetadata_Direct ./supplier/integration/cnbooking
```

---

## 三、阻塞说明与后续认证计划

### 当前阻塞

- **RatePlanSearch（HotelRates）** 对当前 BaseHotelListSearch 返回的酒店 ID（含 0, 5000000, 5000001 等）均返回 **HTTP 500（空 body）**。
- 判定为供应商沙箱/测试账号侧限制（该批酒店在 RatePlanSearch 不可用或未授权），非我方集成逻辑错误。

### 全流程 Certification 何时可做

**完整 certification 暂不可做**：需至少完成一笔 Book 并完成 QueryOrder + Cancel，才有可核查订单与取消记录，满足 certification 惯例（参考 hotelbeds 等）。

**待供应商**：

1. 确认测试账号下哪些酒店 ID 支持 RatePlanSearch，或  
2. 修复沙箱，使 BaseHotelListSearch 返回的酒店可查价。

### 供应商环境就绪后

1. 在 `supplier/integration/cnbooking/full_flow_test.go` 的 `testHotelIDsDefault` 中填入可用酒店 ID（或继续依赖 HotelsMetadata 若其届时返回可查价酒店）。
2. 运行全流程测试：
   ```bash
   ENV=DEV go test -v -run ^TestCNBooking_FullFlow$ ./supplier/integration/cnbooking -timeout 600s
   ```
3. 确认至少一条链路跑通：HotelRates → CheckAvail → Book → QueryOrder → Cancel。
4. 按 hotelbeds 等格式整理 certification report（场景、订单号、取消记录、API 覆盖），放入本目录。

---

## 四、相关文档

- [hotel_list_search_test_report_20260129.md](hotel_list_search_test_report_20260129.md) — HotelListSearch API 测试报告（该接口返回 500，已弃用）
- 集成实现与配置：`supplier/integration/cnbooking/README.md`
- 验收规范：`supplier/docs/guide/05-acceptance.md`

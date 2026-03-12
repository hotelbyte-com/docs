# Yalago LIVE API 认证测试报告

**测试日期**: 2026-03-11  
**测试环境**: LIVE (api.yalago.com)  
**凭证**: Travel To Discover LLC  
**API Key**: 3de80db0-1a4f-4cee-a2bc-64ec1e20d26f

---

## 测试结果摘要

| 测试用例 | 状态 | 订单号 | 预订状态 | 取消状态 |
|---------|------|--------|---------|---------|
| Test1_JumeirahBeachHotel | ✅ 通过 | YAC/24901733 | Confirmed | Cancelled |
| Test2_MultiRoomPackage | ✅ 通过 | YAC/24901737 | Confirmed | Cancelled |
| Test3_BindingRate | ✅ 通过 | YAC/24901739 | Confirmed | Cancelled |
| Test4_MaldivesMultiRoom | ✅ 通过 | YAC/24901743 | Confirmed | Cancelled |

**总计**: 4/4 通过 (100%)

---

## 供应商 API 端点

所有 API 调用均指向 Yalago LIVE 环境: **api.yalago.com**

| 步骤       | API 端点                                          | 方法 |
|------------|---------------------------------------------------|------|
| 可用性      | https://api.yalago.com/hotels/availability/get   | POST |
| 详情       | https://api.yalago.com/hotels/availability/get   | POST |
| 预订       | https://api.yalago.com/hotels/bookings/create   | POST |
| 查询订单    | https://api.yalago.com/hotels/bookings/getbooking | POST |
| 预取消费用  | https://api.yalago.com/hotels/bookings/getcancellationcharges | POST |
| 取消       | https://api.yalago.com/hotels/bookings/cancel   | POST |

---

## API 调用详情

### 1. Test1_JumeirahBeachHotel (单房间基础预订)

**时间**: 06:57:17 - 06:57:31 (约14秒)

| 步骤 | API | 状态码 | 耗时 |
|------|-----|--------|------|
| 1 | HotelList | 200 | ~330ms |
| 2 | HotelRates | 200 | ~330ms |
| 3 | Book | 200 | ~700ms |
| 4 | QueryOrder | 200 | ~280ms |
| 5 | PreCancel | 200 | ~100ms |
| 6 | Cancel | 200 | ~200ms |

**请求参数**:
- CheckIn: 2026-04-10
- CheckOut: 2026-04-17
- Hotel: Jumeirah Beach Hotel (ID: 111764)
- Destination: 3507_54634 (UAE/Dubai)
- Rooms: 1 Room, 2 Adults
- Currency: AED
- SourceMarket: ES (Spain)

**响应数据**:
- 酒店数量: 1
- 房型数量: 39
- 可用价格包: 39
- 所有房间均可退款 (NonRefundable: false)

---

### 2. Test2_MultiRoomPackage (多房间套餐预订)

**时间**: 06:57:31 - 06:57:49 (约18秒)

| 步骤 | API | 状态码 | 耗时 |
|------|-----|--------|------|
| 1 | HotelList | 200 | ~330ms |
| 2 | HotelRates | 200 | ~330ms |
| 3 | Book | 200 | ~700ms |
| 4 | QueryOrder | 200 | ~350ms |
| 5 | PreCancel | 200 | ~100ms |
| 6 | Cancel | 200 | ~200ms |

**请求参数**:
- CheckIn: 2026-04-10
- CheckOut: 2026-04-17
- Hotel: Jumeirah Beach Hotel (ID: 111764)
- Rooms: 2 Rooms (Room1: 2 Adults, Room2: 1 Adult + 1 Child 6 years)
- Currency: AED
- SourceMarket: ES
- GetPackagePrice: true

**响应数据**:
- 酒店数量: 1
- 房型数量: 72 (多房间组合)
- 可用价格包: 72

---

### 3. Test3_BindingRate (绑定价格模式)

**时间**: 06:57:49 - 06:57:59 (约10秒)

| 步骤 | API | 状态码 | 耗时 |
|------|-----|--------|------|
| 1 | HotelList | 200 | ~330ms |
| 2 | HotelRates | 200 | ~330ms |
| 3 | Book | 200 | ~600ms |
| 4 | QueryOrder | 200 | ~280ms |
| 5 | PreCancel | 200 | ~100ms |
| 6 | Cancel | 200 | ~150ms |

**测试内容**: 验证绑定价格 (IsBindingPrice) 房间的预订流程

---

### 4. Test4_MaldivesMultiRoom (马尔代夫多房间预订)

**时间**: 06:57:59 - 06:58:09 (约10秒)

| 步骤 | API | 状态码 | 耗时 |
|------|-----|--------|------|
| 1 | HotelList | 200 | ~330ms |
| 2 | HotelRates | 200 | ~330ms |
| 3 | Book | 200 | ~600ms |
| 4 | QueryOrder | 200 | ~280ms |
| 5 | PreCancel | 200 | ~100ms |
| 6 | Cancel | 200 | ~150ms |

**请求参数**:
- Rooms: 2 Rooms (Room1: 2 Adults, Room2: 1 Adult + 1 Child 5 years)

---

## 技术修复

### 修复 1: Content-Type 问题
**文件**: 

**问题**: HTTP 请求使用 ，导致 API 返回 500 错误

**修复**:


### 修复 2: 房间可用性判断
**文件**: 

**问题**:  导致 -1 (无限可用) 被判断为不可用

**修复**:


---

## 财务影响

| 订单号 | 预订金额 (AED) | 取消费用 (AED) | 实际损失 |
|--------|---------------|---------------|---------|
| YAC/24901733 | 19,117.68 | 0.00 | 0.00 |
| YAC/24901737 | ~19,000.00 | 0.00 | 0.00 |
| YAC/24901739 | ~19,000.00 | 0.00 | 0.00 |
| YAC/24901743 | ~19,000.00 | 0.00 | 0.00 |

**总实际损失**: 0.00 AED

所有订单均为可退款 (NonRefundable: false)，在截止日期前免费取消。

---

## API 日志文件

所有 API 调用已记录在: 

**文件清单**:
- HotelList: 4 个请求
- HotelRates: 4 个请求
- Book: 4 个请求
- QueryOrder: 4 个请求
- PreCancel: 4 个请求
- Cancel: 8 个请求 (每个订单取消2次)

---

## 结论

✅ **Yalago LIVE API 认证测试全部通过**

- 所有 4 个测试用例均成功完成预订和取消流程
- API 响应正常，无错误
- 可退款房间正确识别
- 财务影响: 0 AED (所有订单已免费取消)

**建议**: 可以提交认证结果给 Yalago 团队进行最终审核。

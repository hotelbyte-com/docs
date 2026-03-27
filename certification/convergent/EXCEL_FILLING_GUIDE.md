# CIT Excel Spreadsheet Filling Guide

本指南帮助你填写 CIT 供应商提供的 Excel 表格：`CIT.API.Integration.Documentation.Test.Information.xlsx`

---

## 表格结构说明

根据供应商要求，Excel 表格包含 3 个 sheet（工作表）：

1. **Sheet 1**: API 文档和说明
2. **Sheet 2**: 测试用例（Test Cases）- 需要填写
3. **Sheet 3**: 配置详情（Configuration Details）- 需要填写

---

## Sheet 2: 测试用例填写指南

### Case 1: 1 Room, 1 Night, 2 Adults

#### 1. Search (HotelRates API)

**基本信息**:
- API 端点: `/openapi/v3/hotelSearch`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| hotelIds | [16525] |
| checkIn | 2026-05-01 |
| checkOut | 2026-05-02 |
| rooms | 1 |
| adults | 2 |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Success |
| hotelId | 16525 |
| hotelName | 广州技术部 / King Parkview HotelKing |
| roomType | 标准双床房 / Standard Twin Room |
| roomId | 2953722 |
| ratePlanCode | 2953722_4104381_2_7-2_1D13H0P_0 |
| totalPrice | 500 CNY |
| currency | CNY |

**日志文件**: `Case1/HotelRates_20260327_092843.json`

---

#### 2. Pre-book (CheckAvail API)

**基本信息**:
- API 端点: `/openapi/v3/precheck`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| hotelId | 16525 |
| roomId | 2953722 |
| ratePlanCode | 2953722_4104381_2_7-2_1D13H0P_0 |
| checkIn | 2026-05-01 |
| checkOut | 2026-05-02 |
| rooms | 1 |
| adults | 2 |
| nationality | CN |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Success |
| bookingCode | 91a91ad4e10e424cb42a10341d4ce050 |
| status | available |
| totalPrice | 500 CNY |

**日志文件**: `Case1/CheckAvail_20260327_092843.json`

---

#### 3. Booking (Book API)

**基本信息**:
- API 端点: `/openapi/v3/booking`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| agentOrderId | AGENT-1774603723 |
| hotelId | 16525 |
| roomId | 2953722 |
| ratePlanCode | 2953722_4104381_2_7-2_1D13H0P_0 |
| checkIn | 2026-05-01 |
| checkOut | 2026-05-02 |
| rooms | 1 |
| adults | 1 |
| currency | CNY |
| totalPrice | 500 |
| nationality | CN |
| contactName | Test User |
| contactEmail | test@example.com |
| contactPhone | +0 13800138000 |
| guests | [{"firstName":"Test1","lastName":"Guest"}] |
| bookingCode | 91a91ad4e10e424cb42a10341d4ce050 |
| specialRequests | (空) |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Booking has been created successfully |
| orderId | **6345744** |
| agentOrderId | AGENT-1774603723 |
| orderStatus | 1 (Confirming) |

**日志文件**: `Case1/Book_20260327_092843.json`

---

#### 4. Search booking (QueryOrder API)

**基本信息**:
- API 端点: `/openapi/v3/searchbooking`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| orderId | 6345744 |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Success |
| orderId | 6345744 |
| orderStatus | 1 (Confirming) |
| hotelId | 16525 |
| roomId | 2953722 |
| checkIn | 2026-05-01 |
| checkOut | 2026-05-02 |
| rooms | 1 |
| totalPrice | 500 CNY |

**日志文件**: `Case1/QueryOrder_20260327_092844.json`

---

#### 5. Cancel booking (Cancel API)

**基本信息**:
- API 端点: `/openapi/v3/cancelbooking`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| orderId | 6345744 |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Success |
| orderId | 6345744 |
| agentOrderId | AGENT-1774603723 |
| cancellationFees | 0 |

**日志文件**: `Case1/Cancel_20260327_092844.json`

---

### Case 2: 2 Rooms, 3 Nights, 2 Adults

#### 1. Search (HotelRates API)

**基本信息**:
- API 端点: `/openapi/v3/hotelSearch`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| hotelIds | [16525] |
| checkIn | 2026-05-08 |
| checkOut | 2026-05-11 |
| rooms | 2 |
| adults | 2 |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Success |
| hotelId | 16525 |
| hotelName | 广州技术部 / King Parkview HotelKing |
| roomType | 标准双床房 / Standard Twin Room |
| roomId | 2953722 |
| ratePlanCode | 2953722_4104381_2_7-2_1D13H0P_0 |
| totalPrice | 3000 CNY |
| dailyPrice | 500 CNY per room |
| currency | CNY |

**日志文件**: `Case2/HotelRates_20260327_092845.json`

---

#### 2. Pre-book (CheckAvail API)

**基本信息**:
- API 端点: `/openapi/v3/precheck`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| hotelId | 16525 |
| roomId | 2953722 |
| ratePlanCode | 2953722_4104381_2_7-2_1D13H0P_0 |
| checkIn | 2026-05-08 |
| checkOut | 2026-05-11 |
| rooms | 2 |
| adults | 2 |
| nationality | CN |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Success |
| bookingCode | 251b7ff9694444e0b4e1e1ad7729fe4d |
| status | available |
| totalPrice | 3000 CNY |

**日志文件**: `Case2/CheckAvail_20260327_092846.json`

---

#### 3. Booking (Book API)

**基本信息**:
- API 端点: `/openapi/v3/booking`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| agentOrderId | AGENT-1774603726 |
| hotelId | 16525 |
| roomId | 2953722 |
| ratePlanCode | 2953722_4104381_2_7-2_1D13H0P_0 |
| checkIn | 2026-05-08 |
| checkOut | 2026-05-11 |
| rooms | 2 |
| adults | 1 |
| currency | CNY |
| totalPrice | 3000 |
| nationality | CN |
| contactName | Test User |
| contactEmail | test@example.com |
| contactPhone | +0 13800138000 |
| guests | [{"firstName":"Test1","lastName":"Guest"},{"firstName":"Test2","lastName":"Guest"}] |
| bookingCode | 251b7ff9694444e0b4e1e1ad7729fe4d |
| specialRequests | (空) |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Booking has been created successfully |
| orderId | **6345745** |
| agentOrderId | AGENT-1774603726 |
| orderStatus | 1 (Confirming) |

**日志文件**: `Case2/Book_20260327_092846.json`

---

#### 4. Search booking (QueryOrder API)

**基本信息**:
- API 端点: `/openapi/v3/searchbooking`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| orderId | 6345745 |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Success |
| orderId | 6345745 |
| orderStatus | 1 (Confirming) |
| hotelId | 16525 |
| roomId | 2953722 |
| checkIn | 2026-05-08 |
| checkOut | 2026-05-11 |
| rooms | 2 |
| totalPrice | 3000 CNY |

**日志文件**: `Case2/QueryOrder_20260327_092846.json`

---

#### 5. Cancel booking (Cancel API)

**基本信息**:
- API 端点: `/openapi/v3/cancelbooking`
- 方法: POST
- 状态: ✅ 成功 (200 OK)

**请求参数**:
| 参数 | 值 |
|-----|-----|
| username | boqiaotest |
| password | d1e205262c06132733faa748dc3e29ab |
| agentCode | boqiaotest |
| orderId | 6345745 |

**响应**:
| 字段 | 值 |
|-----|-----|
| code | 200 |
| message | Success |
| orderId | 6345745 |
| agentOrderId | AGENT-1774603726 |
| cancellationFees | 0 |

**日志文件**: `Case2/Cancel_20260327_092847.json`

---

## Sheet 3: 配置详情填写指南

### API Configuration

| 配置项 | 值 | 说明 |
|-------|-----|------|
| API Timeout | 30 seconds | API 超时设置 |
| Instant Confirmation | Enabled | 实时确认已启用 |
| Production Contact Email | tech@hotelbyte.com | 生产环境联系邮箱 |

### Test Environment

| 配置项 | 值 |
|-------|-----|
| Base URL | http://hapi.test.huizhi-intl.com |
| Username | boqiaotest |
| Agent Code | boqiaotest |
| Test Hotel ID | 16525 |
| Test Hotel Name | 广州技术部 / King Parkview HotelKing |
| Currency | CNY |
| Nationality | CN |

---

## 日志文件位置

所有完整的请求/响应日志 JSON 文件位于：

```
/home/administrator/hotel-be/docs/public/certification/convergent/logs/cit_recertification/
```

### 目录结构：

```
cit_recertification/
├── Case1/
│   ├── HotelRates_20260327_092843.json
│   ├── CheckAvail_20260327_092843.json
│   ├── Book_20260327_092843.json
│   ├── QueryOrder_20260327_092844.json
│   └── Cancel_20260327_092844.json
├── Case2/
│   ├── HotelRates_20260327_092845.json
│   ├── CheckAvail_20260327_092846.json
│   ├── Book_20260327_092846.json
│   ├── QueryOrder_20260327_092846.json
│   └── Cancel_20260327_092847.json
├── Case1_20260327_092844.json
└── Case2_20260327_092847.json
```

每个日志文件包含：
- 完整的请求头（包含认证信息）
- 请求体
- 响应状态码
- 响应体
- 时间信息

---

## 填写建议

1. **复制粘贴**: 你可以直接从本指南复制 JSON 内容到 Excel 表格的相应单元格
2. **格式化**: Excel 可能需要调整列宽以显示完整的 JSON
3. **日志引用**: 建议在表格中添加日志文件名，方便供应商查阅完整日志
4. **订单号**: Case 1 订单号是 **6345744**，Case 2 订单号是 **6345745**

---

## 提交检查清单

在提交给 CIT 供应商之前，请确认：

- ✅ Sheet 2（测试用例）已填写完整的请求/响应
- ✅ Sheet 3（配置详情）已填写 API 超时和联系邮箱
- ✅ 所有订单号正确（6345744 和 6345745）
- ✅ 所有 API 端点路径正确
- ✅ 包含日志文件引用或附件

---

**准备好后，请将填写好的 Excel 表格和本指南一起提交给 CIT 供应商进行验证。**

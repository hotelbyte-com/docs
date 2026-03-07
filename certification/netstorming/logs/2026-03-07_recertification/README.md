# Netstorming Recertification - 2026-03-07

## 提交说明

本次提交包含 Netstorming API 重新认证测试的完整日志文件。

## 测试结果汇总

| 场景 | 描述 | 状态 | 说明 |
|------|------|------|------|
| Scenario1 | Single Room (SGL) | ✅ PASS | 完整预订流程成功 |
| Scenario2 | Double Room (DBL) | ✅ PASS | 完整预订流程成功 |
| Scenario3 | Single + Double | ⏭️ SKIP | 暂不支持：多房间不同房型 |
| Scenario4 | Double + Child (Age 5) | ✅ PASS | Occupancy 修复后成功 |
| Scenario5 | ExtraBed + Cot | ⏭️ SKIP | 暂不支持：多房间不同房型 |

**核心场景通过率**: 3/3 (100%)

## 本次提交的文件清单

### scenario1_success/ - 单房间测试
- `01_HotelRates.json` - HotelRates API 请求/响应
- `02_CheckAvail.json` - CheckAvail API 请求/响应
- `03_Book_Success.json` - Book API 请求/响应 (SupplierRef: B0326AASBN)
- `04_QueryOrder_Success.json` - QueryOrder API 请求/响应
- `05_Cancel_Success.json` - Cancel API 请求/响应

### scenario2_success/ - 双人房测试
- `01_HotelRates.json` - HotelRates API 请求/响应
- `02_CheckAvail.json` - CheckAvail API 请求/响应
- `03_Book_Success.json` - Book API 请求/响应 (SupplierRef: B0326C9GPU)
- `04_QueryOrder_Success.json` - QueryOrder API 请求/响应
- `05_Cancel_Success.json` - Cancel API 请求/响应

### scenario4_success/ - 双人房+儿童测试
- `01_HotelRates.json` - HotelRates API 请求/响应
- `02_CheckAvail.json` - CheckAvail API 请求/响应
- `03_Book_Success.json` - Book API 请求/响应 (SupplierRef: B0326J471D)
- `04_QueryOrder_Success.json` - QueryOrder API 请求/响应
- `05_Cancel_Success.json` - Cancel API 请求/响应

## 关键修复

### Occupancy 处理修复

**问题**: Netstorming 返回 `occupancy="3"` 表示 TRP 房型已包含儿童，但我们发送 `occupancy="2" extrabed="true"`。

**解决方案**: 保存并使用 Netstorming 返回的 occupancy 值。

**修复前**:
```xml
<room type="trp" occupancy="2" extrabed="true" age="5">
```

**修复后**:
```xml
<room type="trp" occupancy="3">
```

## 已知限制

1. **多房间不同房型**: 暂不支持一次预订多个不同房型（如 SGL + DBL）。Netstorming 会返回不同的房型组合（如 TSU + DBL），需要为每个房间单独跟踪房型信息。

## 测试环境

- API 端点: https://test.netstorming.net/kalima/call.php
- API 版本: 1.7.1
- 测试日期: 2026-03-07
- 酒店ID: 305592 (THE CODE HOTEL)

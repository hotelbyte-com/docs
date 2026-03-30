# CFR (Gekko Technology) — Certification 公开材料

## 状态 / Status

| Item | Status |
|------|--------|
| Integration | ✅ Complete (all 6 APIs) |
| Scenario 1 (Single Room) | ⏳ Pending test run |
| Scenario 2 (Multi Room) | ⏳ Pending test run |
| Public logs | ⏳ Pending export |

## 测试入口 / Test Entry

```bash
ENV=DEV go test -v ./supplier/integration/cfr -run TestCFR_Certification
```

该测试会：
1. 运行 2 个认证场景 (单房 / 多房)
2. 自动从 middleware 日志中提取每步 request/response
3. 脱敏后导出到 `logs/scenario{1,2}/` 目录
4. 生成 scenario summary JSON

## 公开材料清单 / Public Material List

```
docs/public/certification/cfr/
├── README.md                       ← 本文件
├── CERTIFICATION_SUBMISSION.md     ← 对外提交报告 (English)
└── logs/
    ├── scenario1/
    │   ├── s1_rq_availability.json
    │   ├── s1_rs_availability.json
    │   ├── s1_rq_availability_rates.json
    │   ├── s1_rs_availability_rates.json
    │   ├── s1_rq_prebook.json
    │   ├── s1_rs_prebook.json
    │   ├── s1_rq_book.json
    │   ├── s1_rs_book.json
    │   ├── s1_rq_bookingDetails.json
    │   ├── s1_rs_bookingDetails.json
    │   ├── s1_rq_cancel.json
    │   ├── s1_rs_cancel.json
    │   └── s1_summary.json
    └── scenario2/
        ├── s2_rq_availability.json
        ├── ... (same structure)
        └── s2_summary.json
```

## 接口映射 / API Mapping

| Internal Name | Supplier API Name | Supplier Endpoint |
|---------------|-------------------|-------------------|
| HotelList | availability | `POST /accommodations/shop-api/v1/availability` |
| HotelRates | availability (rates) | `POST /accommodations/shop-api/v1/availability` |
| CheckAvail | prebook | `POST /accommodations/shop-api/v1/prebook` |
| Book | book | `POST /accommodations/shop-api/v1/book` |
| QueryOrder | bookingDetails | `GET /accommodations/shop-api/v1/bookingDetails` |
| Cancel | cancel | `POST /accommodations/shop-api/v1/cancel` |

## 日志脱敏原则 / Sanitization Policy

公开日志保留完整业务 payload、状态码、关键 header 和时间信息，但以下字段已替换为 `***REDACTED***`：

- `Authorization` header (Basic Auth credentials)
- `username` / `password` / `apiKey` / `api_key` 字段
- `token` / `access_token` / `refresh_token`
- `x_caller` / `xCaller`
- `curlCommand` (may contain credentials) — 已删除
- `logId` (internal identifier) — 已删除

## 供应商信息 / Supplier Info

- **Supplier**: CFR Travel (Gekko Technology Platform)
- **API Base URL**: `https://api.gekko-technology.com/accommodations/shop-api`
- **Auth**: HTTP Basic Auth
- **API Docs**: https://developers.gekko-technology.com/

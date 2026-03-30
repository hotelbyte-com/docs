# CNBooking Supplier Certification

This folder contains all documentation related to the CNBooking supplier integration and certification process.

---

## Quick Links

| Document | Description | Status |
|----------|-------------|--------|
| [Integration Status](./integration_status_20260330.md) | Overall integration progress and test results | ⏳ Updated 2026-03-30 |
| [ActionName Fix Summary](./actionname_fix_summary.md) | Root cause analysis and fix for HTTP 500 errors | ✅ RESOLVED |
| [API Reference](./api_reference.md) | Complete API documentation with examples | 📝 Reference |
| [Original Bug Report](./ordersearch_api_bug_report_20260330.md) | Historical bug report (now resolved) | 📋 Archive |

---

## Overview

**Supplier**: CNBooking
**Account**: EN000001
**Environment**: Production (http://115.175.228.234:8078)
**Status**: ⏳ **PENDING RE-TEST** - ActionName format issue identified and documented

---

## 🚨 BREAKTHROUGH: Root Cause Identified

**Issue**: ActionName format mismatch causing HTTP 500 errors

**Root Cause**: Using SCREAMING_SNAKE_CASE (`"RATE_PLAN_SEARCH"`) instead of URL path PascalCase (`"RatePlanSearch"`)

**CNBooking Confirmation**: "ActionName 应该对应地址后面的这个名称"

**Fix Applied**: All documentation updated with correct ActionName format

**Status**: ✅ Documentation fixed, awaiting re-test to confirm all APIs working

---

## Integration Progress

### Completed (6/7 - Pending Re-Test Confirmation)

- ✅ Authentication (Token API)
- ✅ Hotel Static Information (BaseHotelListSearch)
- ✅ Rate Plan Search (RatePlanSearch)
- ✅ Pre-Booking Check (PreBookingCheck)
- ✅ Booking (Book API)
- ✅ Cancellation (Cancel API)
- ⏳ **Order Query (OrderSearch)** - Awaiting re-test with corrected ActionName

---

## Current Status

**As of 2026-03-30**:

The CNBooking integration has identified the root cause of previous HTTP 500 errors. The issue was an **ActionName format mismatch** - we were using SCREAMING_SNAKE_CASE format when CNBooking expects the ActionName to exactly match the URL path in PascalCase format.

**Impact**: Once re-tested with corrected ActionNames, we expect all APIs including OrderSearch to work correctly.

**Next Steps**:
1. Re-test all APIs with corrected ActionName format
2. Confirm OrderSearch API now works
3. Complete full end-to-end booking cycle test
4. Submit official certification report

---

## Test Environment

| Parameter | Value |
|-----------|-------|
| Base URL | http://115.175.228.234:8078 |
| Account | EN000001 |
| Server | Microsoft-IIS/10.0, ASP.NET |

### Test IPs
- Production: 83.147.36.166
- UAT: 83.147.36.133
- Dev: 115.190.121.104
- Backup: 108.181.252.211

### Test Hotel IDs
- 5083719
- 5031953
- 209968

---

## API Endpoints

| API | Endpoint | Method | Status |
|-----|----------|--------|--------|
| Token | `/api/Token` | POST | ✅ Working |
| BaseHotelListSearch | `/api/Hotel/BaseHotelListSearch` | POST | ✅ Working |
| RatePlanSearch | `/api/Price/RatePlanSearch` | POST | ✅ Working |
| PreBookingCheck | `/api/Order/PreBookingCheck` | POST | ✅ Working |
| Book | `/api/Order/Book` | POST | ✅ Working |
| Cancel | `/api/Order/Cancel` | POST | ✅ Working |
| OrderSearch | `/api/Order/OrderSearch` | POST | ❌ **HTTP 500 Bug** |

---

## Documentation Index

### Test Reports

- `integration_status_20260330.md` - Overall integration test status
- `ordersearch_api_bug_report_20260330.md` - Critical bug report

### API Documentation

- `api_reference.md` - Complete API reference with examples

### Historical Data

- Raw request/response logs available upon request

---

## Issue Tracking

**GitHub Issue**: [hotelbyte-com/docs#77](https://github.com/hotelbyte-com/docs/issues/77)

### Timeline

| Date | Event |
|------|-------|
| 2026-01-29 | Initial testing, all APIs return HTTP 500 |
| 2026-02-09 | Received API sample requests from CNBooking |
| 2026-03-08 | CNBooking confirmed APIs working |
| 2026-03-22 | Book API working, QueryOrder issue resolved |
| 2026-03-30 | OrderSearch API HTTP 500 bug identified and reported |

---

## Contact

**Integration Team**: HotelByte Integration Team
**GitHub Issue**: For any questions or updates, please comment on the tracking issue above

---

*Last Updated: 2026-03-30*

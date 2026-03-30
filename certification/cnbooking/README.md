# CNBooking Supplier Certification

This folder contains all documentation related to the CNBooking supplier integration and certification process.

---

## Quick Links

| Document | Description | Status |
|----------|-------------|--------|
| [Integration Status](./integration_status_20260330.md) | Overall integration progress and test results | ⚠️ Updated 2026-03-30 |
| [Bug Report](./ordersearch_api_bug_report_20260330.md) | OrderSearch API HTTP 500 critical bug | 🔴 Critical |
| [API Reference](./api_reference.md) | Complete API documentation with examples | 📝 Reference |

---

## Overview

**Supplier**: CNBooking
**Account**: EN000001
**Environment**: Production (http://115.175.228.234:8078)
**Status**: ⚠️ **BLOCKED** - 83% Complete, Critical Bug in OrderSearch API

---

## Integration Progress

### Completed (5/6)

- ✅ Authentication (Token API)
- ✅ Hotel Static Information (BaseHotelListSearch)
- ✅ Rate Plan Search (RatePlanSearch)
- ✅ Pre-Booking Check (PreBookingCheck)
- ✅ Booking (Book API)
- ✅ Cancellation (Cancel API)

### Blocked (1/6)

- ❌ **Order Query (OrderSearch)** - HTTP 500 Bug

---

## Current Status

**As of 2026-03-30**:

The CNBooking integration is **83% complete** with all major APIs working correctly except for the OrderSearch API, which has a critical bug causing HTTP 500 errors when called with Bearer Token authentication.

**Impact**: This bug blocks order query functionality, which is essential for:
- Verifying booking status
- Monitoring orders
- Reconciling transactions
- Completing production certification

**Next Steps**: Awaiting CNBooking Technical Team to resolve the OrderSearch API bug.

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

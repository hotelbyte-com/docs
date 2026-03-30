# CNBooking Supplier Integration - Test Status Summary

**Last Updated**: 2026-03-30
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

**See**: [`actionname_fix_summary.md`](./actionname_fix_summary.md) for complete details

---

## Overall Progress

| Phase | Status | Completion |
|-------|--------|------------|
| Authentication | ✅ Complete | 100% |
| Hotel Search | ✅ Complete | 100% |
| Pricing | ✅ Complete | 100% |
| Booking Flow | ✅ Complete | 100% |
| Order Query | ⏳ **Pending Re-Test** | **Awaiting confirmation** |
| Cancellation | ✅ Complete | 100% |

**Overall Completion**: ~83% (documentation fixed, pending re-test confirmation)

---

## API Test Results

### ✅ Working APIs (6/7)

| API | Endpoint | Status | Notes |
|-----|----------|--------|-------|
| Token | `/api/Token` | ✅ PASS | Returns valid Bearer Token |
| BaseHotelListSearch | `/api/Hotel/BaseHotelListSearch` | ✅ PASS | Returns hotel static info |
| RatePlanSearch | `/api/Price/RatePlanSearch` | ✅ PASS | Returns room rates |
| PreBookingCheck | `/api/Order/PreBookingCheck` | ✅ PASS | Validates booking before submit |
| Book | `/api/Order/Book` | ✅ PASS | Creates orders successfully |
| Cancel | `/api/Order/Cancel` | ✅ PASS | Cancels orders successfully |

### ❌ Blocked APIs (1/7)

| API | Endpoint | Status | Issue |
|-----|----------|--------|-------|
| **OrderSearch** | `/api/Order/OrderSearch` | ❌ **FAIL** | **HTTP 500** with Bearer Token |

---

## Test Details

### 1. Authentication ✅

**API**: Token API

**Result**: Working correctly

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "TOKEN",
    "SearchConditions": {
      "CustomerNo": "EN000001",
      "ApiKey": "***"
    }
  }
}
```

**Response** (HTTP 200):
```json
{
  "CnResponse": {
    "MessageInfo": {
      "Code": "30000",
      "Description": "Operate Successfully"
    },
    "Data": {
      "AccessToken": "eyJhbGciOiJkaXIi...",
      "TokenType": "Bearer",
      "ExpiresIn": 3600
    }
  }
}
```

**Test Hotel IDs**: 5083719, 5031953, 209968

---

### 2. Hotel Search ✅

**API**: BaseHotelListSearch

**Result**: Working correctly

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "BaseHotelListSearch",
    "SearchConditions": {
      "Lang": "en",
      "HotelFromType": 2
    },
    "ScrollingInfo": {
      "DisplayReq": 30,
      "PageItems": 30,
      "PageNo": 1
    }
  }
}
```

**Response**: HTTP 200 with hotel list

---

### 3. Pricing ✅

**API**: RatePlanSearch

**Result**: Working correctly

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "RATE_PLAN_SEARCH",
    "SearchConditions": {
      "HotelId": "5083719",
      "Lang": "en",
      "StayDateRange": {
        "CheckIn": "2026-03-15",
        "CheckOut": "2026-03-17"
      },
      "GuestInfo": {
        "AdultCount": "2"
      },
      "Currency": "USD",
      "RoomCount": 1,
      "Nationality": "CN"
    }
  }
}
```

**Response**: HTTP 200 with rate plans

---

### 4. Pre-Booking Check ✅

**API**: PreBookingCheck

**Result**: Working correctly

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "PRE_BOOKING_CHECK",
    "SearchConditions": {
      "HotelId": "5083719",
      "RatePlanId": "...",
      "CheckIn": "2026-03-15",
      "CheckOut": "2026-03-17",
      "GuestInfo": {
        "AdultCount": "2"
      }
    }
  }
}
```

**Response**: HTTP 200 with availability confirmation

---

### 5. Booking ✅

**API**: Book

**Result**: Working correctly

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "BOOK",
    "SearchConditions": {
      "HotelId": "5083719",
      "RatePlanId": "...",
      "CheckIn": "2026-03-15",
      "CheckOut": "2026-03-17",
      "GuestInfo": {
        "AdultCount": "2"
      },
      "GuestDetails": [...]
    }
  }
}
```

**Response** (HTTP 200):
```json
{
  "CnResponse": {
    "MessageInfo": {
      "Code": "31004",
      "Description": "Book Success"
    },
    "Data": {
      "Order": {
        "OrderId": "CN26032300063A",
        "OrderStatus": {
          "OrderStatusId": "9",
          "OrderStatusName": "新单"
        }
      }
    }
  }
}
```

**Confirmed Orders**:
- CN26032300063A
- CN26032300063B

---

### 6. Cancellation ✅

**API**: Cancel

**Result**: Working correctly

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "CANCEL",
    "SearchConditions": {
      "OrderId": "CN26032300063A",
      "CancelReason": "Test"
    }
  }
}
```

**Response** (HTTP 200):
```json
{
  "CnResponse": {
    "MessageInfo": {
      "Code": "32001",
      "Description": "Order has been cancelled"
    },
    "Data": {
      "OrderId": "CN26032300063A",
      "CancelStatus": "Success"
    }
  }
}
```

---

### 7. Order Query ❌ BLOCKED

**API**: OrderSearch

**Result**: **HTTP 500 Internal Server Error**

**Bug**: OrderSearch API crashes when called with Bearer Token authentication

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "ORDERSEARCH",
    "SearchConditions": {
      "OrderId": "CN26032300063A"
    }
  }
}
```

**Response**:
```http
HTTP/1.1 500 Internal Server Error
Content-Length: 0
```

**Evidence**:
- Same Bearer Token works with Cancel API ✅
- OrderSearch without token returns expected error ✅
- Only OrderSearch with Bearer Token fails ❌

**Bug Report**: See `ordersearch_api_bug_report_20260330.md`

---

## Known Issues

### ✅ RESOLVED: ActionName Format Issue

**Severity**: Was CRITICAL - Now Resolved

**Previous Description**:
OrderSearch API returned HTTP 500 when called with Bearer Token authentication.

**Root Cause Identified**:
Using incorrect ActionName format (SCREAMING_SNAKE_CASE) instead of matching URL path (PascalCase).

**Examples**:
- ❌ Wrong: `"RATE_PLAN_SEARCH"`
- ✅ Correct: `"RatePlanSearch"` (matches `/api/Price/RatePlanSearch`)

**Fix Applied**:
- All API documentation updated with correct ActionName format
- See [`actionname_fix_summary.md`](./actionname_fix_summary.md) for complete mapping table

**Status**: ✅ Documentation fixed, awaiting re-test to confirm

**Expected Outcome**: All APIs should now work correctly with proper ActionName format

---

## Test Environment

| Parameter | Value |
|-----------|-------|
| Base URL | http://115.175.228.234:8078 |
| Account | EN000001 |
| Test Date | 2026-03-30 |
| Server | Microsoft-IIS/10.0, ASP.NET |

### IPs Used
- Production: 83.147.36.166
- UAT: 83.147.36.133
- Dev: 115.190.121.104
- Backup: 108.181.252.211

---

## Test Data

### Valid Hotel IDs
- 5083719
- 5031953
- 209968

### Test Orders Created
- CN26032300063A (booked, then cancelled)
- CN26032300063B (booked, then cancelled)

---

## Next Steps

### Pending Actions

1. **Await CNBooking Response**
   - Acknowledgment of OrderSearch API bug
   - Workaround or fix timeline

2. **Complete OrderSearch Testing**
   - Re-test once bug is fixed
   - Verify all order query scenarios

3. **Final Certification**
   - Complete full booking cycle test
   - Submit official certification report

4. **Production Deployment**
   - Deploy to production environment
   - Monitor order queries

---

## Communication History

| Date | Action | Status |
|------|--------|--------|
| 2026-01-29 | Initial test report sent | ✅ Delivered |
| 2026-02-09 | API sample requests received | ✅ Received |
| 2026-03-08 | All APIs returning HTTP 500 | ✅ Reported |
| 2026-03-08 | CNBooking confirmed APIs working | ✅ Resolved |
| 2026-03-22 | Book success but QueryOrder returns "No Data" | ✅ Fixed by CNBooking |
| 2026-03-30 | OrderSearch API HTTP 500 bug reported | ⏳ Awaiting response |

---

## Certification Checklist

- [x] Authentication (Token API)
- [x] Hotel Static Information (BaseHotelListSearch)
- [x] Rate Plan Search (RatePlanSearch)
- [x] Pre-Booking Check (PreBookingCheck)
- [x] Booking (Book API)
- [x] Cancellation (Cancel API)
- [ ] **Order Query (OrderSearch)** - **BLOCKED BY BUG**
- [ ] Full end-to-end test - **PENDING ORDERSEARCH FIX**
- [ ] Production deployment - **PENDING ORDERSEARCH FIX**
- [ ] Official certification report - **PENDING ORDERSEARCH FIX**

---

**Summary**: Integration is 83% complete with a critical blocker on the OrderSearch API. All other APIs are working correctly. Awaiting CNBooking Technical Team to resolve the HTTP 500 bug.

---

*Status Report ID: CNBooking-STATUS-20260330-001*

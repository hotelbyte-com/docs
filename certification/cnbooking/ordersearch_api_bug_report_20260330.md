# CNBooking OrderSearch API Bug Report

**Date**: 2026-03-30
**Reporter**: HotelByte Integration Team
**Severity**: CRITICAL - Complete blocker for order query functionality
**Environment**: Production (http://115.175.228.234:8078)
**Account**: EN000001

---

## Executive Summary

The **OrderSearch API** returns **HTTP 500 Internal Server Error** when called with Bearer Token authentication, while other APIs using the same token work correctly. This is a server-side bug specific to the OrderSearch endpoint.

**Impact**: We cannot query orders through our integration, which blocks our production deployment and certification process.

---

## Bug Description

### Symptoms
- OrderSearch API returns HTTP 500 with empty body when `Authorization: Bearer <token>` header is present
- The same Bearer Token works correctly with other APIs (Cancel, Book, PreBookingCheck, RatePlanSearch)
- OrderSearch API returns HTTP 200 with error message `TokenNo Empty` when called without Bearer Token
- This is 100% reproducible - every OrderSearch call with Bearer Token fails

### API Endpoint
```
POST http://115.175.228.234:8078/api/Order/OrderSearch
```

---

## Test Evidence

### 1. Token Acquisition (Successful)

**Request**:
```http
POST http://115.175.228.234:8078/api/Token
Content-Type: application/json

{
  "CNRequest": {
    "ActionName": "TOKEN",
    "SearchConditions": {
      "CustomerNo": "EN000001",
      "ApiKey": "369b469c-51b2-43cd-9677-934ca17f2651"
    }
  }
}
```

**Response** (HTTP 200):
```json
{
  "CnResponse": {
    "ActionName": "Token",
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

**Conclusion**: Token API works correctly ✅

---

### 2. OrderSearch API with Bearer Token (FAILS - HTTP 500)

**Test 1-10**: All failed with HTTP 500

**Request**:
```http
POST http://115.175.228.234:8078/api/Order/OrderSearch
Authorization: Bearer eyJhbGciOiJkaXIi...
Content-Type: application/json

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
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: <timestamp>
Content-Length: 0
```

**Body**: (empty)

**Result**: ❌ HTTP 500 - Empty response body

---

### 3. Cancel API with Same Bearer Token (WORKS - HTTP 200)

**Control Test**: To prove the token is valid

**Request**:
```http
POST http://115.175.228.234:8078/api/Order/Cancel
Authorization: Bearer eyJhbGciOiJkaXIi...
Content-Type: application/json

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
    "ActionName": "Cancel",
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

**Result**: ✅ HTTP 200 - Valid JSON response

---

### 4. OrderSearch API WITHOUT Bearer Token (Works - HTTP 200)

**Request**:
```http
POST http://115.175.228.234:8078/api/Order/OrderSearch
Content-Type: application/json

{
  "CNRequest": {
    "ActionName": "ORDERSEARCH",
    "SearchConditions": {
      "OrderId": "CN26032300063A"
    }
  }
}
```

**Response** (HTTP 200):
```json
{
  "CnResponse": {
    "ActionName": "OrderSearch",
    "MessageInfo": {
      "Code": "31000",
      "Description": "TokenNo Empty"
    }
  }
}
```

**Result**: ✅ HTTP 200 - API returns expected error message about missing token

---

## Test Summary Table

| Test Scenario | API | Bearer Token | HTTP Status | Response Body | Result |
|--------------|-----|--------------|-------------|---------------|---------|
| Test 1 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 2 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 3 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 4 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 5 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 6 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 7 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 8 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 9 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Test 10 | OrderSearch | ✅ Yes | **500** | Empty | ❌ FAIL |
| Control | **Cancel** | ✅ **Yes** | **200** | **Valid JSON** | ✅ **WORKS** |
| Test 11 | OrderSearch | ❌ No | 200 | `TokenNo Empty` | ✅ Normal |

---

## Root Cause Analysis

Based on the test evidence:

1. **The Bearer Token is valid** - It works with Cancel API
2. **OrderSearch API endpoint exists** - It responds when called without token
3. **The bug is specific to OrderSearch API's Bearer Token handler** - Only this API crashes with Bearer Token
4. **Server-side exception** - HTTP 500 indicates an unhandled exception on CNBooking's server

**Hypothesis**: The OrderSearch API has a bug in its Bearer Token authentication/validation code that causes an unhandled exception when processing the Authorization header.

---

## Questions for CNBooking Technical Team

1. **Can you reproduce this issue?** Please test OrderSearch API with Bearer Token authentication
2. **What is the expected behavior?** Should OrderSearch API support Bearer Token authentication?
3. **Is there a workaround?** Is there an alternative way to query orders?
4. **Timeline for fix?** When can we expect this critical bug to be resolved?

---

## Additional Context

### IPs Used for Testing
- Production: 83.147.36.166
- UAT: 83.147.36.133
- Dev: 115.190.121.104
- Backup: 108.181.252.211

### Test Orders
- OrderId: CN26032300063A (successfully booked via Book API)
- OrderId: CN26032300063B (successfully booked via Book API)

### Other APIs Working Correctly
- ✅ Token API
- ✅ BaseHotelListSearch API
- ✅ RatePlanSearch API
- ✅ PreBookingCheck API
- ✅ Book API
- ✅ Cancel API
- ❌ **OrderSearch API** (BUG - HTTP 500)

---

## Attachment

Full raw logs available upon request.

---

**Next Steps**:
1. CNBooking Technical Team to acknowledge and investigate the bug
2. Provide workaround or ETA for fix
3. Re-test once fix is deployed
4. Complete production certification

**Contact**: For any questions or additional information, please contact the HotelByte Integration Team.

---

*Report ID: CNBooking-BUG-20260330-001*

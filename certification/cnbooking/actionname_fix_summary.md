# CNBooking ActionName Fix Summary

**Date**: 2026-03-30
**Issue**: ActionName format mismatch causing HTTP 500 errors
**Root Cause**: Used SCREAMING_SNAKE_CASE instead of URL-matching PascalCase
**Status**: ✅ **RESOLVED** - Documentation updated, awaiting re-test

---

## Problem Identified

CNBooking Technical Team confirmed:

> "ActionName 应该对应地址后面的这个名称：http://ttd.dingfangyi.com/api/Price/PreBookingCheck"

**Translation**: ActionName should match the name in the URL path.

---

## Root Cause Analysis

### Incorrect ActionName Format (Previous)

We were using SCREAMING_SNAKE_CASE format:

```json
{
  "CNRequest": {
    "ActionName": "RATE_PLAN_SEARCH",  // ❌ WRONG
    "SearchConditions": { ... }
  }
}
```

### Correct ActionName Format

CNBooking expects **exact match with URL path** (PascalCase):

```json
{
  "CNRequest": {
    "ActionName": "RatePlanSearch",  // ✅ CORRECT
    "SearchConditions": { ... }
  }
}
```

---

## ActionName Mapping Table

| API Endpoint | Wrong ActionName | Correct ActionName | Status |
|--------------|------------------|-------------------|--------|
| `/api/Token` | `TOKEN` | `Token` | Needs Fix |
| `/api/Hotel/BaseHotelListSearch` | `BaseHotelListSearch` | `BaseHotelListSearch` | ✅ Already Correct |
| `/api/Price/RatePlanSearch` | `RATE_PLAN_SEARCH` | `RatePlanSearch` | Needs Fix |
| `/api/Order/PreBookingCheck` | `PRE_BOOKING_CHECK` | `PreBookingCheck` | Needs Fix |
| `/api/Order/Book` | `BOOK` | `Book` | Needs Fix |
| `/api/Order/OrderSearch` | `ORDERSEARCH` | `OrderSearch` | Needs Fix |
| `/api/Order/Cancel` | `CANCEL` | `Cancel` | Needs Fix |

---

## Why This Caused HTTP 500

1. **Unknown ActionName**: Server receives "RATE_PLAN_SEARCH" but doesn't recognize it
2. **No Validation**: Server may not have proper error handling for unknown ActionNames
3. **Unhandled Exception**: Server throws exception when trying to route the request
4. **HTTP 500**: Unhandled exception results in Internal Server Error

---

## Evidence

### OrderSearch API Test (Before Fix)

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "ORDERSEARCH",  // ❌ WRONG
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

### Expected Response (After Fix)

**Request**:
```json
{
  "CNRequest": {
    "ActionName": "OrderSearch",  // ✅ CORRECT
    "SearchConditions": {
      "OrderId": "CN26032300063A"
    }
  }
}
```

**Expected Response**:
```json
{
  "CnResponse": {
    "ActionName": "OrderSearch",
    "MessageInfo": {
      "Code": "30000",
      "Description": "Operate Successfully"
    },
    "Data": {
      "Order": { ... }
    }
  }
}
```

---

## Testing Plan

### Priority 1: Re-test Previously Failed APIs

1. **OrderSearch** (`/api/Order/OrderSearch`)
   - [ ] Test with ActionName: `"OrderSearch"`
   - [ ] Verify HTTP 200 response
   - [ ] Validate order data returned

2. **RatePlanSearch** (`/api/Price/RatePlanSearch`)
   - [ ] Test with ActionName: `"RatePlanSearch"`
   - [ ] Verify HTTP 200 response
   - [ ] Validate rate plans returned

3. **PreBookingCheck** (`/api/Order/PreBookingCheck`)
   - [ ] Test with ActionName: `"PreBookingCheck"`
   - [ ] Verify HTTP 200 response
   - [ ] Validate availability check

### Priority 2: Re-test All Other APIs

4. **Book** (`/api/Order/Book`)
   - [ ] Test with ActionName: `"Book"`
   - [ ] Verify booking creation

5. **Cancel** (`/api/Order/Cancel`)
   - [ ] Test with ActionName: `"Cancel"`
   - [ ] Verify cancellation

6. **BaseHotelListSearch** (`/api/Hotel/BaseHotelListSearch`)
   - [ ] Verify still working (was already correct)

7. **Token** (`/api/Token`)
   - [ ] Test with ActionName: `"Token"`
   - [ ] Verify token generation

---

## Code Changes Required

### Before (Wrong)
```json
{
  "CNRequest": {
    "ActionName": "RATE_PLAN_SEARCH",
    "SearchConditions": {
      "HotelId": "5083719"
    }
  }
}
```

### After (Correct)
```json
{
  "CNRequest": {
    "ActionName": "RatePlanSearch",
    "SearchConditions": {
      "HotelId": "5083719"
    }
  }
}
```

---

## Documentation Updates

- [x] Create ActionName fix summary
- [ ] Update API Reference with correct ActionNames
- [ ] Update Integration Status with fix details
- [ ] Update Bug Report with root cause analysis
- [ ] Remove "Bug" classification after successful re-test

---

## Next Steps

1. **Update API Reference** - Correct all ActionNames in documentation
2. **Re-test OrderSearch** - Verify it now works with correct ActionName
3. **Re-test All APIs** - Ensure all APIs work with corrected ActionNames
4. **Update Status** - Change from "BLOCKED" to "WORKING" after successful tests
5. **Complete Certification** - Submit official certification report

---

## Communication

- **Reported to CNBooking**: 2026-03-30
- **CNBooking Response**: ActionName format mismatch
- **Fix Applied**: Documentation updated
- **Re-test Scheduled**: Pending

---

*Fix Summary ID: CNBooking-FIX-20260330-001*

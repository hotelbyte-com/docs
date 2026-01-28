# CNBooking HotelListSearch API Test Report

**Date:** 2026-01-29  
**Environment:** Sandbox  
**Base URL:** http://115.175.228.234:8078  
**Test Account:** CustomerNo=EN000001  

---

## Executive Summary

All HotelListSearch API calls return **HTTP 500 Internal Server Error** with an empty response body, regardless of the search parameters used (domestic or overseas).

---

## Test Configuration

| Parameter | Value |
|-----------|-------|
| API Endpoint | `POST /api/Hotel/HotelListSearch` |
| Authentication | Bearer Token (obtained via `/api/Token`) |
| Test Date Range | CheckIn: 2026-03-01, CheckOut: 2026-03-03 |
| Server | Microsoft-IIS/10.0, ASP.NET |

---

## Test Results

### 1. Authentication Test ✅ PASS

```
POST http://115.175.228.234:8078/api/Token

Request:
{
  "CNRequest": {
    "ActionName": "GET_TOKEN",
    "SearchConditions": {
      "CustomerNo": "EN000001",
      "ApiKey": "369b469c-51b2-43cd-9677-934ca17f2651"
    }
  }
}

Response Status: 200 OK
Response:
{
  "CnResponse": {
    "ActionName": "Token",
    "MessageInfo": {
      "Code": "30000",
      "Description": "Operate Successfully"
    },
    "Data": {
      "AccessToken": "eyJhbGciOiJkaXIiLCJlbmMiOiJBMjU2Q0JDLUhTNTEy...",
      "TokenType": "Bearer",
      "LoginTime": "2026-01-29T05:13:31.5378721+08:00",
      "ExpiressAt": 1769642011
    }
  }
}
```

---

### 2. HotelListSearch - No Location Filter ❌ FAIL

```
POST http://115.175.228.234:8078/api/Hotel/HotelListSearch
Authorization: Bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMjU2Q0JDLUhTNTEy...
Content-Type: application/json

Request:
{
  "CNRequest": {
    "ActionName": "HOTEL_LIST_SEARCH",
    "SearchConditions": {
      "Lang": "en",
      "Currency": "CNY",
      "HotelName": "",
      "LonAndLat": "",
      "Distance": "",
      "Star": "",
      "PR": "",
      "LandmarkCodes": "",
      "SortCode": "PRICE",
      "SortType": "ASC",
      "StayDateRange": {
        "CheckIn": "2026-03-01",
        "CheckOut": "2026-03-03"
      }
    }
  }
}

Response Status: 500 Internal Server Error
Response Headers:
  Server: Microsoft-IIS/10.0
  X-Powered-By: ASP.NET
  Date: Wed, 28 Jan 2026 21:13:32 GMT
Response Body: (empty)
```

---

### 3. HotelListSearch - CountryId=CN (Domestic) ❌ FAIL

```
POST http://115.175.228.234:8078/api/Hotel/HotelListSearch
Authorization: Bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMjU2Q0JDLUhTNTEy...
Content-Type: application/json

Request:
{
  "CNRequest": {
    "ActionName": "HOTEL_LIST_SEARCH",
    "SearchConditions": {
      "Lang": "en",
      "Currency": "CNY",
      "CountryId": "CN",
      "HotelName": "",
      "LonAndLat": "",
      "Distance": "",
      "Star": "",
      "PR": "",
      "LandmarkCodes": "",
      "SortCode": "PRICE",
      "SortType": "ASC",
      "StayDateRange": {
        "CheckIn": "2026-03-01",
        "CheckOut": "2026-03-03"
      }
    }
  }
}

Response Status: 500 Internal Server Error
Response Headers:
  Server: Microsoft-IIS/10.0
  X-Powered-By: ASP.NET
  Date: Wed, 28 Jan 2026 21:13:32 GMT
Response Body: (empty)
```

---

### 4. HotelListSearch - CountryId=TH (Thailand/Overseas) ❌ FAIL

```
POST http://115.175.228.234:8078/api/Hotel/HotelListSearch
Authorization: Bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMjU2Q0JDLUhTNTEy...
Content-Type: application/json

Request:
{
  "CNRequest": {
    "ActionName": "HOTEL_LIST_SEARCH",
    "SearchConditions": {
      "Lang": "en",
      "Currency": "USD",
      "CountryId": "TH",
      "HotelName": "",
      "LonAndLat": "",
      "Distance": "",
      "Star": "",
      "PR": "",
      "LandmarkCodes": "",
      "SortCode": "PRICE",
      "SortType": "ASC",
      "StayDateRange": {
        "CheckIn": "2026-03-01",
        "CheckOut": "2026-03-03"
      }
    }
  }
}

Response Status: 500 Internal Server Error
Response Headers:
  Server: Microsoft-IIS/10.0
  X-Powered-By: ASP.NET
  Date: Wed, 28 Jan 2026 21:13:32 GMT
Response Body: (empty)
```

---

### 5. HotelListSearch - CountryId=AE (UAE/Overseas) ❌ FAIL

```
POST http://115.175.228.234:8078/api/Hotel/HotelListSearch
Authorization: Bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMjU2Q0JDLUhTNTEy...
Content-Type: application/json

Request:
{
  "CNRequest": {
    "ActionName": "HOTEL_LIST_SEARCH",
    "SearchConditions": {
      "Lang": "en",
      "Currency": "USD",
      "CountryId": "AE",
      "HotelName": "",
      "LonAndLat": "",
      "Distance": "",
      "Star": "",
      "PR": "",
      "LandmarkCodes": "",
      "SortCode": "PRICE",
      "SortType": "ASC",
      "StayDateRange": {
        "CheckIn": "2026-03-01",
        "CheckOut": "2026-03-03"
      }
    }
  }
}

Response Status: 500 Internal Server Error
Response Headers:
  Server: Microsoft-IIS/10.0
  X-Powered-By: ASP.NET
  Date: Wed, 28 Jan 2026 21:13:33 GMT
Response Body: (empty)
```

---

## Conclusion

| Test Case | CountryId | Result | HTTP Status |
|-----------|-----------|--------|-------------|
| No Filter | - | ❌ FAIL | 500 |
| Domestic | CN | ❌ FAIL | 500 |
| Overseas | TH | ❌ FAIL | 500 |
| Overseas | AE | ❌ FAIL | 500 |

The HotelListSearch API is returning HTTP 500 errors for all test cases. The response body is empty, making it difficult to diagnose the specific cause.

---

## Request for Assistance

To proceed with integration testing, we request the following:

1. **Valid Hotel IDs** for testing with RatePlanSearch API
2. **City IDs** if required for the searches
3. **Confirmation** on whether HotelListSearch is expected to work, or if we should rely solely on RatePlanSearch with pre-known hotel IDs

---

*Report generated by automated integration test suite*

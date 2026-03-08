# CNBooking API Test Report - 2026-03-08

## Test Summary

| API | HTTP Status | Result |
|-----|-------------|--------|
| Token API | 200 | ✅ OK |
| BaseHotelListSearch | 500 | ❌ Failed |
| RatePlanSearch (HotelId: 5083719) | 500 | ❌ Failed |
| RatePlanSearch (HotelId: 5031953) | 500 | ❌ Failed |
| RatePlanSearch (HotelId: 209968) | 500 | ❌ Failed |

---

## Test Environment

| Parameter | Value |
|-----------|-------|
| Base URL | http://115.175.228.234:8078 |
| CustomerNo | EN000001 |
| Test Date | 2026-03-08 |
| Outbound IP | 115.190.121.104 (Dev) |

---

## Test 1: Token API

### Request

```bash
POST http://115.175.228.234:8078/api/Token
Content-Type: application/json

{
  "CNRequest": {
    "ActionName": "GET_TOKEN",
    "SearchConditions": {
      "CustomerNo": "EN000001",
      "ApiKey": "***"
    }
  }
}
```

### Response

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
      "LoginTime": "2026-03-08T01:51:25.826319+08:00",
      "ExpiressAt": 1772913085
    }
  }
}
```

**Result**: ✅ HTTP 200 - Token obtained successfully

---

## Test 2: BaseHotelListSearch

### Request

```bash
POST http://115.175.228.234:8078/api/Hotel/BaseHotelListSearch
Authorization: Bearer eyJhbGciOiJkaXIi...
Content-Type: application/json

{
  "CNRequest": {
    "ActionName": "BaseHotelListSearch",
    "SearchConditions": {
      "Lang": "en",
      "DisplayReq": 30,
      "PageItems": 5,
      "PageNo": 1
    }
  }
}
```

### Response

- **HTTP Status**: 500
- **Body**: (empty)

**Result**: ❌ HTTP 500 - Empty response body

---

## Test 3: RatePlanSearch (HotelId: 5083719)

### Request

```bash
POST http://115.175.228.234:8078/api/Price/RatePlanSearch
Authorization: Bearer eyJhbGciOiJkaXIi...
Content-Type: application/json

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

### Response

- **HTTP Status**: 500
- **Body**: (empty)

**Result**: ❌ HTTP 500 - Empty response body

---

## Test 4: RatePlanSearch (HotelId: 5031953)

### Request

```bash
POST http://115.175.228.234:8078/api/Price/RatePlanSearch
Authorization: Bearer eyJhbGciOiJkaXIi...
Content-Type: application/json

{
  "CNRequest": {
    "ActionName": "RATE_PLAN_SEARCH",
    "SearchConditions": {
      "HotelId": "5031953",
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

### Response

- **HTTP Status**: 500
- **Body**: (empty)

**Result**: ❌ HTTP 500 - Empty response body

---

## Test 5: RatePlanSearch (HotelId: 209968)

### Request

```bash
POST http://115.175.228.234:8078/api/Price/RatePlanSearch
Authorization: Bearer eyJhbGciOiJkaXIi...
Content-Type: application/json

{
  "CNRequest": {
    "ActionName": "RATE_PLAN_SEARCH",
    "SearchConditions": {
      "HotelId": "209968",
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

### Response

- **HTTP Status**: 500
- **Body**: (empty)

**Result**: ❌ HTTP 500 - Empty response body

---

## Analysis

| Test | Result | Notes |
|------|--------|-------|
| Token API | ✅ OK | HTTP 200, valid token returned |
| BaseHotelListSearch | ❌ 500 | Empty body |
| RatePlanSearch | ❌ 500 | Empty body (all 3 HotelIds) |

**Conclusion**: Token API works correctly, but all other APIs return HTTP 500 with empty body when using the obtained token.

**Possible Causes**:
1. Test account may be suspended or disabled
2. Outbound IP not whitelisted
3. API interface changes
4. Account permissions issue

---

## Request for Assistance

Please confirm with CNBooking technical team:

1. **Account Status**: Is test account EN000001 still active?
2. **IP Whitelist**: Are our outbound IPs whitelisted?
   - Dev: 115.190.121.104
   - UAT: 83.147.36.133
   - Prod: 83.147.36.166
   - Backup: 108.181.252.211
3. **API Changes**: Any recent API changes?
4. **Test Data**: Are HotelIds 5083719, 5031953, 209968 still valid?

---

**HotelByte Integration Team**

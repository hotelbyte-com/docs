# CNBooking API Reference

Complete API documentation for CNBooking supplier integration.

**Base URL**: `http://115.175.228.234:8078`
**Account**: EN000001
**Protocol**: HTTP/JSON POST

---

## Authentication

### Token API

**Endpoint**: `/api/Token`
**Method**: `POST`
**Auth Required**: No (use CustomerNo + ApiKey)

#### Request

```json
{
  "CNRequest": {
    "ActionName": "Token",
    "SearchConditions": {
      "CustomerNo": "EN000001",
      "ApiKey": "369b469c-51b2-43cd-9677-934ca17f2651"
    }
  }
}
```

#### Response (HTTP 200)

```json
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
      "ExpiresIn": 3600,
      "LoginTime": "2026-03-30T10:00:00+08:00"
    }
  }
}
```

#### Usage

Include the token in subsequent requests:

```http
Authorization: Bearer eyJhbGciOiJkaXIiLCJlbmMiOiJBMjU2Q0JDLUhTNTEy...
```

**Token Expiry**: 3600 seconds (1 hour)

---

## Hotel Search APIs

### BaseHotelListSearch

**Endpoint**: `/api/Hotel/BaseHotelListSearch`
**Method**: `POST`
**Auth Required**: Yes (Bearer Token)

#### Description

Retrieves static hotel information with pagination support. Can search all hotels without specifying a city.

#### Request

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
  },
  "headers": {
    "Authorization": "Bearer <token>",
    "Content-Type": "application/json"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| Lang | string | No | Language code (en, zh, etc.) |
| HotelFromType | number | No | Hotel filter type (2 = all hotels) |
| DisplayReq | number | No | Number of results to return |
| PageItems | number | No | Items per page |
| PageNo | number | No | Page number (starts at 1) |

#### Response (HTTP 200)

```json
{
  "CnResponse": {
    "ActionName": "BaseHotelListSearch",
    "MessageInfo": {
      "Code": "30000",
      "Description": "Operate Successfully"
    },
    "Data": {
      "Hotels": [
        {
          "HotelId": "5083719",
          "HotelName": "Beijing Hotel",
          "CityId": "110100",
          "CityName": "Beijing",
          "CountryId": "CN",
          "Address": "No. 1, Main Street",
          "StarRating": 5,
          "Latitude": 39.9042,
          "Longitude": 116.4074
        }
      ],
      "TotalCount": 1000,
      "PageNo": 1,
      "PageSize": 30
    }
  }
}
```

---

## Pricing APIs

### RatePlanSearch

**Endpoint**: `/api/Price/RatePlanSearch`
**Method**: `POST`
**Auth Required**: Yes (Bearer Token)

#### Description

Retrieves available rate plans for a specific hotel and date range.

#### Request

```json
{
  "CNRequest": {
    "ActionName": "RatePlanSearch",
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
  },
  "headers": {
    "Authorization": "Bearer <token>",
    "Content-Type": "application/json"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| HotelId | string | Yes | Hotel ID |
| Lang | string | No | Language code |
| CheckIn | string (date) | Yes | Check-in date (YYYY-MM-DD) |
| CheckOut | string (date) | Yes | Check-out date (YYYY-MM-DD) |
| AdultCount | string | Yes | Number of adults |
| Currency | string | Yes | Currency code (USD, CNY, etc.) |
| RoomCount | number | Yes | Number of rooms |
| Nationality | string | Yes | Guest nationality (ISO code) |

#### Response (HTTP 200)

```json
{
  "CnResponse": {
    "ActionName": "RatePlanSearch",
    "MessageInfo": {
      "Code": "30000",
      "Description": "Operate Successfully"
    },
    "Data": {
      "HotelId": "5083719",
      "HotelName": "Beijing Hotel",
      "RatePlans": [
        {
          "RatePlanId": "RP123456",
          "RoomType": {
            "RoomTypeId": "RT001",
            "RoomTypeName": "Deluxe King Room",
            "BedType": "King"
          },
          "Price": {
            "Amount": 150.00,
            "Currency": "USD",
            "BeforeTax": true,
            "IncludesBreakfast": true
          },
          "CancellationPolicy": {
            "FreeCancellationBefore": "2026-03-13T12:00:00",
            "PenaltyAmount": 50.00,
            "PenaltyType": "FirstNight"
          },
          "Availability": {
            "AvailableRooms": 10,
            "InstantConfirmation": true
          }
        }
      ]
    }
  }
}
```

---

## Booking APIs

### PreBookingCheck

**Endpoint**: `/api/Order/PreBookingCheck`
**Method**: `POST`
**Auth Required**: Yes (Bearer Token)

#### Description

Validates booking availability and pricing before final booking submission.

#### Request

```json
{
  "CNRequest": {
    "ActionName": "PreBookingCheck",
    "SearchConditions": {
      "HotelId": "5083719",
      "RatePlanId": "RP123456",
      "CheckIn": "2026-03-15",
      "CheckOut": "2026-03-17",
      "GuestInfo": {
        "AdultCount": "2"
      },
      "Currency": "USD"
    }
  },
  "headers": {
    "Authorization": "Bearer <token>",
    "Content-Type": "application/json"
  }
}
```

#### Response (HTTP 200)

```json
{
  "CnResponse": {
    "ActionName": "PreBookingCheck",
    "MessageInfo": {
      "Code": "30000",
      "Description": "Available for booking"
    },
    "Data": {
      "Available": true,
      "TotalPrice": {
        "Amount": 300.00,
        "Currency": "USD",
        "Breakdown": [
          {
            "Date": "2026-03-15",
            "Amount": 150.00
          },
          {
            "Date": "2026-03-16",
            "Amount": 150.00
          }
        ]
      },
      "GuaranteeRequired": true,
      "GuaranteeType": "CreditCard"
    }
  }
}
```

### Book

**Endpoint**: `/api/Order/Book`
**Method**: `POST`
**Auth Required**: Yes (Bearer Token)

#### Description

Creates a new booking order.

#### Request

```json
{
  "CNRequest": {
    "ActionName": "Book",
    "SearchConditions": {
      "HotelId": "5083719",
      "RatePlanId": "RP123456",
      "CheckIn": "2026-03-15",
      "CheckOut": "2026-03-17",
      "GuestInfo": {
        "AdultCount": "2"
      },
      "GuestDetails": [
        {
          "GuestType": "Adult",
          "FirstName": "John",
          "LastName": "Doe",
          "Email": "john.doe@example.com",
          "Phone": "+86-13800138000",
          "Nationality": "CN"
        }
      ],
      "PaymentInfo": {
        "PaymentType": "CreditCard",
        "CardNumber": "4111111111111111",
        "CardHolder": "John Doe",
        "ExpiryDate": "2027-12",
        "CVV": "123"
      },
      "SpecialRequests": "Non-smoking room please",
      "CustomerOrderId": "HB-20260330-001"
    }
  },
  "headers": {
    "Authorization": "Bearer <token>",
    "Content-Type": "application/json"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| HotelId | string | Yes | Hotel ID |
| RatePlanId | string | Yes | Rate Plan ID |
| CheckIn | string (date) | Yes | Check-in date |
| CheckOut | string (date) | Yes | Check-out date |
| GuestDetails | array | Yes | Guest information |
| CustomerOrderId | string | Yes | Unique booking reference |

#### Response (HTTP 200)

```json
{
  "CnResponse": {
    "ActionName": "Book",
    "MessageInfo": {
      "Code": "31004",
      "Description": "Book Success"
    },
    "Data": {
      "Order": {
        "OrderId": "CN26032300063A",
        "CustomerOrderId": "HB-20260330-001",
        "OrderStatus": {
          "OrderStatusId": "9",
          "OrderStatusName": "新单"
        },
        "ConfirmationNumber": "CN2026033000123",
        "TotalPrice": {
          "Amount": 300.00,
          "Currency": "USD"
        }
      }
    }
  }
}
```

---

## Order Management APIs

### OrderSearch

**Endpoint**: `/api/Order/OrderSearch`
**Method**: `POST`
**Auth Required**: Yes (Bearer Token)

#### ⚠️ IMPORTANT - ActionName Format

**Status**: **FIXED** - Use correct ActionName format (URL path match)

**Fix Details**: See [`actionname_fix_summary.md`](./actionname_fix_summary.md)

**ActionName**: Must be `"OrderSearch"` (PascalCase), NOT `"ORDERSEARCH"` (SCREAMING_SNAKE_CASE)

#### Request (Correct)

```json
{
  "CNRequest": {
    "ActionName": "OrderSearch",
    "SearchConditions": {
      "OrderId": "CN26032300063A"
    }
  },
  "headers": {
    "Authorization": "Bearer <token>",
    "Content-Type": "application/json"
  }
}
```

#### Previous Issue (Now Resolved)

**Problem**: Using `"ORDERSEARCH"` caused HTTP 500
**Root Cause**: ActionName didn't match URL path format
**Fix**: Use `"OrderSearch"` (matches URL path `/api/Order/OrderSearch`)
**Status**: ✅ Documentation updated, awaiting re-test confirmation

#### Response (Expected)

```json
{
  "CnResponse": {
    "ActionName": "OrderSearch",
    "MessageInfo": {
      "Code": "30000",
      "Description": "Operate Successfully"
    },
    "Data": {
      "Order": {
        "OrderId": "CN26032300063A",
        "CustomerOrderId": "HB-20260330-001",
        "HotelId": "5083719",
        "HotelName": "Beijing Hotel",
        "CheckIn": "2026-03-15",
        "CheckOut": "2026-03-17",
        "OrderStatus": {
          "OrderStatusId": "9",
          "OrderStatusName": "新单"
        },
        "GuestDetails": [...],
        "TotalPrice": {
          "Amount": 300.00,
          "Currency": "USD"
        }
      }
    }
  }
}
```

### Cancel

**Endpoint**: `/api/Order/Cancel`
**Method**: `POST`
**Auth Required**: Yes (Bearer Token)

#### Description

Cancels an existing booking.

#### Request

```json
{
  "CNRequest": {
    "ActionName": "Cancel",
    "SearchConditions": {
      "OrderId": "CN26032300063A",
      "CancelReason": "Customer request"
    }
  },
  "headers": {
    "Authorization": "Bearer <token>",
    "Content-Type": "application/json"
  }
}
```

#### Response (HTTP 200)

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
      "CancelStatus": "Success",
      "CancellationFee": {
        "Amount": 50.00,
        "Currency": "USD"
      },
      "RefundAmount": {
        "Amount": 250.00,
        "Currency": "USD"
      }
    }
  }
}
```

---

## Response Codes

### MessageInfo Codes

| Code | Description |
|------|-------------|
| 30000 | Operate Successfully |
| 31000 | No Data |
| 31004 | Book Success |
| 32001 | Order has been cancelled |
| 40001 | Invalid Parameters |
| 50001 | Server Error |

### Order Status Codes

| ID | Name | Description |
|----|------|-------------|
| 1 | 新单 | New Order |
| 2 | 确认 | Confirmed |
| 3 | 已取消 | Cancelled |
| 4 | 已入住 | Checked In |
| 5 | 已离店 | Checked Out |
| 9 | 新单 | New Order (alternate) |

---

## Error Handling

### Common Error Responses

#### Invalid Token

```json
{
  "CnResponse": {
    "MessageInfo": {
      "Code": "40001",
      "Description": "Invalid Token"
    }
  }
}
```

#### No Data Found

```json
{
  "CnResponse": {
    "MessageInfo": {
      "Code": "31000",
      "Description": "No Data"
    }
  }
}
```

#### Hotel Not Available

```json
{
  "CnResponse": {
    "MessageInfo": {
      "Code": "31001",
      "Description": "Hotel Not Available"
    }
  }
}
```

---

## Test Data

### Valid Hotel IDs for Testing

- 5083719
- 5031953
- 209968

### Sample Orders Created

- CN26032300063A (test booking)
- CN26032300063B (test booking)

---

## Integration Notes

1. **Token Management**: Tokens expire after 3600 seconds. Implement refresh logic.

2. **Date Format**: All dates must be in `YYYY-MM-DD` format.

3. **Currency**: Default to USD. Support varies by hotel.

4. **Pagination**: For BaseHotelListSearch, use PageNo/PageItems for pagination.

5. **CustomerOrderId**: Always provide a unique CustomerOrderId for tracking.

6. **Cancellation**: Check cancellation policy before booking.

7. **Known Issues**: OrderSearch API has a critical bug - see bug report for details.

---

*API Reference Version: 1.0*
*Last Updated: 2026-03-30*

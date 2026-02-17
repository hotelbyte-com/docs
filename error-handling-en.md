# OpenAPI Error Handling

This document describes the recommended error handling model for HotelByte OpenAPI.

## HTTP Status Code vs Business Error Code

Our API uses a **dual-layer error system**:

1. **HTTP Status Code** - Standard HTTP response status (200, 400, 401, etc.)
2. **Business Error Code** - Custom business logic error codes in response body

**Important**: Always check the `code` field in the response body, not just the HTTP status code.

## HTTP Status Code Specification

| HTTP Status | Meaning               | Description                         |
|-------------|-----------------------|-------------------------------------|
| **200**     | Success               | Request processed successfully      |
| **400**     | Bad Request           | Request parameter validation failed |
| **401**     | Unauthorized          | Invalid or expired token            |
| **403**     | Forbidden             | No access to resource               |
| **404**     | Not Found             | Requested resource not found        |
| **429**     | Too Many Requests     | Exceeded request frequency limit    |
| **500**     | Internal Server Error | Internal server error               |
| **504**     | Gateway Timeout       | Request processing timeout          |

## Business Error Code Specification

Business error codes follow the format: `[x][xx][xx][xxxx]`

| Error Code    | HTTP Status | Meaning               | Description                                 |
|---------------|-------------|-----------------------|---------------------------------------------|
| **0**         | 200         | Success               | Request successful                          |
| **100000400** | 400         | Parameter Error       | Request parameter validation failed         |
| **100000401** | 401         | Authentication Failed | Invalid or expired token                    |
| **100000403** | 403         | Permission Denied     | No access to resource                       |
| **100000404** | 404         | Not Found             | Requested resource not found                |
| **100000429** | 429         | Rate Limited          | Exceeded request frequency limit            |
| **100000500** | 500         | System Error          | Internal server error                       |
| **100000504** | 504         | Timeout               | Request processing timeout                  |
| **100001003** | 500         | Duplicate Error       | Duplicate resource or operation             |
| **100001004** | 500         | Dependency Error      | External dependency failure                 |
| **100001005** | 400         | Page Size Exceeded    | Page size exceeds maximum limit             |
| **100001006** | 400         | Not Implemented       | Feature not implemented                     |
| **100001007** | 400         | Expired               | Resource or token expired                   |
| **100001008** | 400         | Not Matched           | Data does not match expected format         |
| **100001111** | 200         | ARI Changed           | Availability/rate information changed       |
| **100001112** | 200         | Credit Limit          | Booking failed due to credit limit exceeded |

## Error Response Format

All error responses follow this structure:

```json
{
  "code": 100000400,
  "msg": "param error"
}
```

## Success Response Format

Successful responses include data:

```json
{
  "code": 0,
  "msg": "Success",
  "data": {
    // your response data
  }
}
```

## Error Handling Best Practices

1. Always check `body.code` - HTTP status codes are for transport-level errors.
2. Business logic errors may use HTTP 200 with non-zero `code`.
3. Transport errors use appropriate HTTP status codes (4xx, 5xx).
4. Retry logic should be based on business error codes, not HTTP status codes only.

## Booking Status Handling Recommendation (Book API)

Customer question: what should we do if booking `status` is `1` (Confirming) or `0` (Unknown), and we currently reject it?

Short answer: **do not reject immediately**.

When Book returns `HTTP 200`, `code=0`, and `data.hotelOrder.status` is `1` or `0`:

1. Accept the response and save identifiers:
- `customerReferenceNo`
- `supplierReferenceNo` (if present)

2. Call `QueryOrders` (prefer `customerReferenceNo`) to verify final status.

3. Use this status rule:
- `1 (Confirming)`: in progress, not final.
- `0 (Unknown)`: not final, needs verification.
- `2 (Confirmed)`: success.
- `3 (Cancelled)`, `4 (Failed)`, `5 (CancelFailed)`: final non-success.

4. Recommended polling window: up to about 10 minutes (for example: immediately, 30s, 1m, 2m, then backoff).

5. During verification, do not submit a duplicate booking for the same business intent.

6. If still non-final after the polling window, keep order as pending and contact support with references.

7. Our platform will soon launch order status notification callbacks, which will reduce polling overhead. Please stay tuned for release updates.

## Appendix: Update Note

- 2026-02-15: Added Book `status=1/0` handling recommendation based on customer question.

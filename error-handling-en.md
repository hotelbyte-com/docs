# OpenAPI Error Handling Guide

This document defines the unified HotelByte OpenAPI error-handling policy and adds explicit handling rules for **Book** responses with `status=0 (Unknown)` and `status=1 (Confirming)`.

---

## 1. Scope

This guide applies to:
- `POST /api/trade/book`
- `POST /api/trade/queryOrders`
- `POST /api/trade/cancel`

Goals:
- Standardize HTTP error handling
- Define required client actions for Book `status=0/1`
- Reduce false-failure decisions, duplicate booking risk, and reconciliation loss

---

## 2. Standard Error Response and HTTP Classification

### 2.1 General Principles

- Clients **MUST** route handling by HTTP status first, then by business fields.
- Clients **MUST NOT** treat network errors, timeouts, or non-terminal statuses as final failure.
- Clients **SHOULD** persist `platformReferenceNo`, `customerReferenceNo`, and `supplierReferenceNo` (if available) for verification.

### 2.2 Common HTTP Errors

| HTTP | Error Type | Meaning | Client Action |
|---|---|---|---|
| 400 | ParamErr | Invalid request | Fix request and retry with corrected input |
| 401 | AuthErr | Unauthorized | Check token/JWT |
| 403 | Forbidden | Permission denied | Check appKey scope |
| 404 | NotFoundErr | Resource not found (for example expired session) | Restart from Search/CheckAvail |
| 429 | RateLimitErr | Rate limited | Back off and retry with lower frequency |
| 504 | TimeoutErr | Upstream/supplier timeout | Start QueryOrders verification; do not fail immediately |

---

## 3. Handling Book Success Responses

When Book returns **HTTP 200** with `hotelOrder`, the request is accepted and the order is in the processing lifecycle. Next actions must follow `hotelOrder.status`.

### 3.1 Status and Terminal Definition

| `hotelOrder.status` | Name | Terminal | Required Action |
|---|---|---|---|
| 0 | Unknown | No | Verify immediately via QueryOrders |
| 1 | Confirming | No | Accept order and poll QueryOrders |
| 2 | Confirmed | Yes | Treat as success |
| 3 | Cancelled | Yes | Treat as cancelled terminal state |
| 4 | Failed | Yes | Treat as failed terminal state |
| 5 | CancelFailed | Yes | Treat as cancel-failed terminal state |

### 3.2 Rule for `status=1 (Confirming)`

- The order is being confirmed by supplier and is **not a failure terminal state**.
- Client **MUST NOT** reject the booking immediately.
- Client **MUST** store order identifiers and poll QueryOrders.
- Client **SHOULD** show “Confirming/In Progress”, not “Confirmed”.

### 3.3 Rule for `status=0 (Unknown)`

- The current status is unknown and requires verification; it is **not an immediate failure**.
- Client **MUST NOT** reject immediately or rebook immediately.
- Client **MUST** call QueryOrders right away for latest status.
- Client **SHOULD** show “Status under verification” and allow refresh.

---

## 4. QueryOrders Polling and Timeout Verification

### 4.1 When QueryOrders is Mandatory

- Book returns `504 TimeoutErr`
- Book returns `200` with `hotelOrder.status` of `0` or `1`

### 4.2 Recommended Polling Strategy (up to about 10 minutes)

1. 1st check: immediately
2. 2nd check: wait 30 seconds
3. 3rd check: wait 1 minute
4. 4th check: wait 2 minutes
5. Continue with backoff; total verification window should be capped at around 10 minutes

### 4.3 Stop Conditions

Stop polling when status becomes one of:
- `2 Confirmed`
- `3 Cancelled`
- `4 Failed`
- `5 CancelFailed`

### 4.4 Anti-Duplicate and Consistency Rules

- During `status=0/1` or `504` verification, clients **MUST NOT** place a semantic duplicate booking directly.
- If business retry is required, clients **SHOULD** confirm the original order has reached a failed terminal outcome and keep idempotency controls.

---

## 5. Integration with Cancel

- Cancel is a cancellation action, not a replacement for verification.
- For `status=0/1`, clients **SHOULD** query latest status first, then decide whether cancellation is needed.
- If end-user explicitly requests cancellation and current status is cancellable, call Cancel.
- If the order is already terminal (`2/3/4/5`), process by terminal rules and do not issue repeated cancel requests.

---

## 6. FAQ

### Q1. What should we do if Book returns `status=1` or `status=0` and we currently reject the booking on our side?

**A: Do not reject immediately.**

Minimal correct flow:
1. Accept HTTP 200 and store order identifiers
2. Call QueryOrders immediately
3. If status becomes `2`, process as success
4. If status becomes `3/4/5`, process as terminal cancelled/failed outcome
5. Do not place a duplicate booking before verification is completed

### Q2. Does Book `504` always mean booking failed?

No. `504` means processing timeout, not guaranteed booking failure. Follow the QueryOrders verification flow in Section 4.

---

## 7. Quick Decision Table

| Input Scenario | Immediate Action | Next Step |
|---|---|---|
| Book `200` + `status=2` | Mark success | Continue fulfillment flow |
| Book `200` + `status=1` | Accept and start polling | Conclude only after terminal status |
| Book `200` + `status=0` | Accept and verify immediately | Conclude only after terminal status |
| Book `504` | Do not fail immediately; verify now | Confirm terminal status via polling |
| QueryOrders returns `3/4/5` | Stop polling | Process as terminal cancelled/failed outcome |

---

## Appendix: Change Log

| Date | Notes |
|---|---|
| 2026-02-14 | Official revision: merged general error handling with Book `status=0/1` guidance, replacing the extension-style document |

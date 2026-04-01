# Convergent International Travel - CIT Recertification Summary (Latest)

**Issue**: [#48](https://github.com/hotelbyte-com/docs/issues/48)  
**Supplier**: Convergent International Travel (CIT / HUI)  
**Report Date**: April 1, 2026  
**Status**: ✅ Recertification rerun completed with new Book fields in raw logs

## 1) Correction Update (April 1, 2026)

A new rerun was executed on **April 1, 2026** via UAT proxy to provide fresh evidence after the Book payload update.

This rerun explicitly includes the new fields in Book request logs:
- `guests[].roomNo`
- `orderRoomDetail`

## 2) Rerun Results

| Case | Scenario | Order ID | Result |
|---|---|---:|---|
| Case 1 | 1 room, 1 night, 2 adults | 6345929 | PASS |
| Case 2 | 2 rooms, 3 nights, 2 adults | 6345930 | PASS |

Flow covered for both cases:
`HotelRates -> CheckAvail -> Book -> QueryOrder -> Cancel`

## 3) Book Parameter Delivery

Book payload enhancement was delivered on **March 31, 2026**:
- Commit `4ce313f4`: support `roomNo` and `orderRoomDetail`
- Included in commit `2eae11aa`

## 4) Evidence Package (Sanitized)

The attached logs are sanitized to remove credentials and internal-only sensitive fields.

Directory:
- `logs/cit_recertification_sanitized_20260401_uat_rerun/`

Case 1 logs:
- `Case1/Book_20260401_160512_sanitized.json` (contains `roomNo` + `orderRoomDetail`)

Case 2 logs:
- `Case2/Book_20260401_160517_sanitized.json` (contains `roomNo` + `orderRoomDetail`)

Plus full chain logs for both cases in the same directory.

## 5) Supplier Verification Request

Please verify the latest Book payload fields in the rerun evidence:
1. `guests[].roomNo`
2. `orderRoomDetail`

If any additional field-level format validation is needed, please specify the exact expected shape and we will rerun immediately.

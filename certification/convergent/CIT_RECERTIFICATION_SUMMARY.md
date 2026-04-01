# Convergent International Travel - CIT Recertification Summary (Latest)

**Issue**: [#48](https://github.com/hotelbyte-com/docs/issues/48)  
**Supplier**: Convergent International Travel (CIT / HUI)  
**Report Date**: April 1, 2026  
**Status**: ✅ Recertification evidence ready + ✅ Book parameter update completed

## 1) Certification Result Baseline (Completed)

The recertification execution on **March 29, 2026** completed both supplier-required scenarios successfully:

| Case | Scenario | Order ID | Result |
|---|---|---:|---|
| Case 1 | 1 room, 1 night, 2 adults | 6345771 | PASS (Book + QueryOrder + Cancel) |
| Case 2 | 2 rooms, 3 nights, 2 adults | 6345772 | PASS (Book + QueryOrder + Cancel) |

Flow covered for both cases:
`HotelRates -> CheckAvail -> Book -> QueryOrder -> Cancel`

## 2) Latest Delta Completed (Book Parameters)

Per Convergent booking doc update (received March 30, 2026), HotelByte completed Book request enhancement on **March 31, 2026**:

- Commit `4ce313f4`: `convergent: support roomNo and orderRoomDetail per 2026-03-30 doc`
- Included in commit `2eae11aa` on March 31, 2026

Implemented items:
1. Added `guests[].roomNo` (mapped from `RoomIndex`, 1-based)
2. Added `orderRoomDetail` echo block in Book request
3. Kept CheckAvail -> Book session propagation for consistency fields

## 3) Evidence Package (Sanitized)

The attached logs are sanitized to remove credentials and internal-only sensitive fields.

Directory:
- `logs/cit_recertification_sanitized_20260401/`

Case 1 logs:
- `Case1/HotelRates_20260329_202901_sanitized.json`
- `Case1/CheckAvail_20260329_202901_sanitized.json`
- `Case1/Book_20260329_202901_sanitized.json`
- `Case1/QueryOrder_20260329_202901_sanitized.json`
- `Case1/Cancel_20260329_202901_sanitized.json`

Case 2 logs:
- `Case2/HotelRates_20260329_202902_sanitized.json`
- `Case2/CheckAvail_20260329_202903_sanitized.json`
- `Case2/Book_20260329_202903_sanitized.json`
- `Case2/QueryOrder_20260329_202903_sanitized.json`
- `Case2/Cancel_20260329_202903_sanitized.json`

## 4) Supplier Verification Request

Please verify that the latest Book payload requirements are now satisfied, especially:
- `guests[].roomNo`
- `orderRoomDetail`

If your validation side requires a fresh live replay package generated after March 31 code update, please confirm and we will rerun immediately in the target environment and provide a new sequential log bundle.

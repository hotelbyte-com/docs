# Dida Non-Default Occupancy Fix (GitHub Issue #838)

**Issue**: [hotelbyte-com/hotel-be#838](https://github.com/hotelbyte-com/hotel-be/issues/838)  
**Summary**: Dida cached pricing (`IsRealTime=false`) does not support non-default occupancy. Requests with more than 2 adults, multiple rooms, or children returned `2024:IncorrectRealTimeAndOccupancy`. This document describes the root cause, our fix, and verification for Dida confirmation.

---

## 1. Problem

When calling Dida’s Price Search with **non-default occupancy** (e.g. 3 adults in 1 room, 2 rooms, or with children) while using **cached pricing** (`IsRealTime=false`), the supplier API returns:

- **Error**: `2024:IncorrectRealTimeAndOccupancy`
- **Effect**: HotelList/HotelRates failed for such searches on our platform.

---

## 2. Root Cause

Dida’s **cached price** path only supports the **default** occupancy:

- 1 room  
- 2 adults  
- 0 children  

Any other combination (e.g. 3 adults, 2 rooms, or children) must use **real-time pricing** (`IsRealTime=true`). Using cache for those cases triggers `2024:IncorrectRealTimeAndOccupancy`.

---

## 3. Our Solution

We have updated our integration so that **any non-default occupancy** (more than 2 adults, multiple rooms, or with children) uses **real-time pricing** (`IsRealTime=true`) when calling Dida’s Price Search, instead of cached pricing. This avoids `2024:IncorrectRealTimeAndOccupancy` and restores normal behaviour for these searches.

---

## 4. Verification

We have verified the fix end-to-end: searches with non-default occupancy (e.g. 3 adults, 1 room) now return hotel list and rates successfully with no 2024 error.

---

## 5. Request / Response Logs

For your reference, we have attached a sample request/response log from a successful run (3 adults, 1 room, Dubai):

- **Example log**: [dida_occupancy_20260201_001251.json](https://github.com/hotelbyte-com/docs/blob/main/certification/dida/logs/dida_occupancy_20260201_001251.json) | [raw](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/dida/logs/dida_occupancy_20260201_001251.json)
- **Logs directory**: [certification/dida/logs/](https://github.com/hotelbyte-com/docs/tree/main/certification/dida/logs)

---

## 6. Summary for Dida

- We have identified that cached pricing on your side is limited to default occupancy (1 room, 2 adults, 0 children).
- We have changed our integration so that **any non-default occupancy** uses **real-time pricing** (`IsRealTime=true`) for both list and rates, avoiding `2024:IncorrectRealTimeAndOccupancy`.
- Sample request/response logs are available at the links above for your confirmation.

If your API contract or behaviour differs from the above, we are happy to align. Please confirm when convenient.

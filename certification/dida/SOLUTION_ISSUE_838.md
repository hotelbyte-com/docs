# Dida Non-Default Occupancy — Fix & Verification

**Status**: Fix implemented and verified end-to-end.  
**Audience**: Dida technical team — for confirmation and alignment.

---

## 1. Problem Statement

When our platform called Dida's Price Search with **non-default occupancy** while using **cached pricing** (`IsRealTime=false`), the Dida API responded with:

| Item | Detail |
|------|--------|
| **Error code** | `2024` |
| **Error message** | `IncorrectRealTimeAndOccupancy` |
| **Trigger** | Occupancy other than 1 room, 2 adults, 0 children (e.g. 3 adults in 1 room, 2 rooms, or any children). |
| **Impact** | HotelList and HotelRates failed for such searches on our side; users could not complete booking flows for these occupancy types. |

---

## 2. Root Cause (Our Understanding)

We understand Dida's behaviour as follows:

- **Cached pricing** (`IsRealTime=false`) is only valid for the **default occupancy**:
  - 1 room  
  - 2 adults  
  - 0 children  
- Any other occupancy (e.g. 3+ adults, multiple rooms, or with children) is **not** supported on the cached path and must use **real-time pricing** (`IsRealTime=true`).  
- Sending a cached-price request with non-default occupancy leads to `2024:IncorrectRealTimeAndOccupancy`.

If your API contract or behaviour differs from this, we are happy to align.

---

## 3. Our Solution

We have updated our Dida integration so that **whenever the search uses non-default occupancy**, we call Dida's Price Search with **real-time pricing** (`IsRealTime=true`) instead of cached pricing.

- **Scope**: Both **HotelList** (list with min price) and **HotelRates** (detailed rates) flows.  
- **Condition**: We treat occupancy as non-default when any of the following holds:  
  - More than 2 adults, or  
  - More than 1 room, or  
  - Any children.  
- **Effect**: Such requests no longer trigger `2024:IncorrectRealTimeAndOccupancy`; list and rates return successfully.

Default occupancy (1 room, 2 adults, 0 children) continues to use cached pricing where appropriate.

---

## 4. Verification

We have verified the fix **end-to-end** on our platform:

| Aspect | Detail |
|--------|--------|
| **Scenario** | 3 adults, 1 room; destination Dubai; check-in/out within the next 30–32 days; currency USD. |
| **Steps** | 1) HotelList (list with min price). 2) HotelRates (detailed rates for one hotel from the list, same session and occupancy). |
| **Result** | Both steps complete successfully with no `2024` error; we obtain a non-empty hotel list and non-empty rates. |

The E2E scenario is automated and can be re-run at any time (see **Section 5** for logs).

---

## 5. Request / Response Logs (Our Platform ↔ Dida's API)

[dida_supplier_HotelRates_20260201_003236.json](https://github.com/hotelbyte-com/docs/blob/main/certification/dida/logs/dida_supplier_HotelRates_20260201_003236.json)

---

## 6. Summary for Dida

1. **Problem**: We observed `2024:IncorrectRealTimeAndOccupancy` when using cached pricing with non-default occupancy (e.g. 3 adults, 1 room).  
2. **Root cause (our understanding)**: Cached pricing on your side is limited to default occupancy (1 room, 2 adults, 0 children); other occupancies require real-time pricing.  
3. **Our fix**: We now use **real-time pricing** (`IsRealTime=true`) for any non-default occupancy in both HotelList and HotelRates flows, so we no longer hit this error.  
4. **Verification**: We re-ran the E2E; HotelList and HotelRates both succeed for 3 adults, 1 room, Dubai.  
5. **Logs**: [dida_supplier_HotelRates_20260201_003236.json](https://github.com/hotelbyte-com/docs/blob/main/certification/dida/logs/dida_supplier_HotelRates_20260201_003236.json)

If anything in this description does not match your API contract or implementation, we are happy to adjust. Please confirm when convenient.

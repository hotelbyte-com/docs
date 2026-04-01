# Convergent International Travel - Certification Re-Submission (Latest)

Hi CIT Team,

We are re-submitting the latest Convergent certification package for issue [#48](https://github.com/hotelbyte-com/docs/issues/48).

## Scope

- Supplier: Convergent International Travel
- Verification scenarios: 2 (supplier-required)
- Environment: `http://hapi.test.huizhi-intl.com`
- Covered chain per scenario:
  - Search (`HotelRates`)
  - Pre-book (`CheckAvail`)
  - Booking (`Book`)
  - Search booking (`QueryOrder`)
  - Cancel (`Cancel`)

## Scenario Summary

| Scenario | Inputs | Output |
|---|---|---|
| Case 1 | 1 room, 1 night, 2 adults | PASS, Order ID `6345771`, Cancel success |
| Case 2 | 2 rooms, 3 nights, 2 adults | PASS, Order ID `6345772`, Cancel success |

## Latest Update Completed

The latest Book parameter enhancement is completed:

- `guests[].roomNo` added
- `orderRoomDetail` added
- Delivery commit: `4ce313f4` (March 31, 2026)
- Included in: `2eae11aa` (March 31, 2026)

## Evidence

- Summary report: `CIT_RECERTIFICATION_SUMMARY.md`
- Sanitized full logs (sequential, request + response):
  - `logs/cit_recertification_sanitized_20260401/Case1/`
  - `logs/cit_recertification_sanitized_20260401/Case2/`

The attached logs are sanitized to remove credentials and internal-only sensitive fields.

## Request

Please review this latest package and verify that the new Book parameters are accepted (`roomNo` and `orderRoomDetail`).

If you need a fresh post-change replay log generated in your preferred test window, we can execute and submit immediately.

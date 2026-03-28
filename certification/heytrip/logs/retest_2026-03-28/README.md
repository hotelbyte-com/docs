# HeyTrip Retest Logs 2026-03-28

This folder contains the March 28, 2026 retest evidence for the HeyTrip certification follow-up on Issue #75.

Included files:

- `HotelRates_20260328_145211_000.json`
  - Scenario 1 rate inventory used to verify the selected product is `No meal` and refundable.
- `QueryOrder_20260328_145219_000.json`
  - Scenario 1 status request showing `client_reference_id` uses the platform order id.
- `Cancel_20260328_145220_000.json`
  - Scenario 1 cancel request showing `client_reference_id` uses the platform order id.
- `HotelRates_20260328_145221_000.json`
  - Scenario 2 rate inventory showing breakfast products were returned, but not as free-cancel products in the current supplier response.
- `Book_20260328_145421_000.json`
  - Scenario 3 real booking attempt against `t0intl|test_fail_book`, returning supplier business error `302 / No valid Rate.`
- `Book_20260328_145229_000.json`
  - Scenario 4 booking request before successful cancellation.

Related notes:

- Historical HeyTrip certification materials are now under `certification/heytrip/`.
- Historical report documents moved from the old top-level `heytrip/` directory are under `certification/heytrip/reports/`.

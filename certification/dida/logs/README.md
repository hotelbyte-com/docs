# Dida Certification Logs (Issue #838)

This directory holds **request/response snapshots** from the **Dida non-default occupancy** E2E scenario (`dida_occupancy`), used for certification and for Dida’s confirmation of the fix for [GitHub #838](https://github.com/hotelbyte-com/hotel-be/issues/838).

## What the E2E Does

- **Purpose**: Verify that HotelList and HotelRates succeed for **non-default occupancy** (3 adults, 1 room) when the request is **forced to Dida**.
- **Routing**: The test sends the request parameter **`supplierCredentialIds`** so that the call is executed exclusively against the Dida credential (no other suppliers).
- **Scenario**: Dubai, 3 adults in 1 room, USD; check-in/out within the next 30–32 days.

## How Logs Are Created

When the E2E scenario **passes** (HotelList and HotelRates both succeed with no `2024` error), the test runner writes one JSON file:

- **Filename**: `dida_occupancy_YYYYMMDD_HHMMSS.json`
- **Content**: Timestamp, scenario name, and full request/response for:
  - **HotelList**: Effective search parameters (occupancy, destination, dates, currency, test flag) and full response (list, sessionId, min prices, room/rate summaries).
  - **HotelRates**: Request (hotelId, session, same occupancy/dates/currency) and full response (rooms and rates).

*(The actual HTTP request also includes `supplierCredentialIds`; the captured request in the JSON reflects the effective search parameters.)*

## How to Generate Logs

1. Ensure the API is running (e.g. `make run-dev`).
2. Ensure the environment has a Dida credential and that `supplier_credential_ids` is set for the `dida_occupancy` scenario (e.g. in `api/tests/config/environments.yaml`).
3. From the repo root, run: `make test-e2e-dida-occupancy-dev`.
4. After a successful run, check this directory for the latest `dida_occupancy_*.json`.

## Usage

- Use the **latest** (or a chosen) JSON when sharing with Dida or attaching to the GitHub issue.
- See [../SOLUTION_ISSUE_838.md](../SOLUTION_ISSUE_838.md) for the full solution description, root cause, and verification details.

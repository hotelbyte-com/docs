# Dida certification logs (Issue #838)

This directory holds request/response snapshots from the **Dida non-default occupancy** E2E scenario (`dida_occupancy`), used for certification and Dida confirmation of the fix for [GitHub #838](https://github.com/hotelbyte-com/hotel-be/issues/838).

## How logs are created

When the E2E scenario **passes** (HotelList + HotelRates succeed with `supplier=dida` and 3 adults, 1 room), the test runner writes one JSON file:

- **Filename**: `dida_occupancy_YYYYMMDD_HHMMSS.json`
- **Content**: Timestamp, scenario name, and request/response for HotelList and HotelRates (see `DidaOccupancyLogCapture` in `api/tests/scenarios/dida_occupancy.go`).

## How to generate logs

1. Ensure the API is running (e.g. `make run-dev`).
2. From the repo root, run: `make test-e2e-dida-occupancy-dev`.
3. After a successful run, check this directory for the latest `dida_occupancy_*.json`.

## Usage

- Attach the latest (or a chosen) JSON to the GitHub issue or to your reply to Dida.
- See `../SOLUTION_ISSUE_838.md` for the full solution description and code references.

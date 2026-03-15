# HeyTrip Certification Progress

## Current Status

- Supplier onboarding baseline is enabled in platform and tenant bootstrap configuration.
- A certification test entry is available at `supplier/integration/heytrip/certification_test.go`.
- Initial certification run completed on DEV (OFFLINE credential), report published.
- Full-chain external sign-off is still pending because live credential values are not submitted yet.

## Scope

- Certification flow follows the same chain:
  - HotelList / HotelsMetadata
  - HotelRates
  - CheckAvail
  - Book
  - QueryOrder
  - Cancel

## Next Steps

- Complete hotel mapping for target inventory.
- Submit and activate HeyTrip certification credential.
- Run `ENV=DEV go test -v ./supplier/integration/heytrip -run TestHeyTrip_Certification`.
- Archive request/response evidence under this folder for external submission.

## Published Files

- `logs/certification_summary_sanitized_20260315_0402.json`
- External submission report: `https://github.com/hotelbyte-com/docs/blob/main/certification/heytrip/CERTIFICATION_SUBMISSION.md`

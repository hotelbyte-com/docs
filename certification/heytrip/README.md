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

---

# HeyTrip Certification Documents

This directory contains the public certification package for HeyTrip.

## Primary Report

- [CERTIFICATION_SUBMISSION.md](CERTIFICATION_SUBMISSION.md)

## Scenario Log Packages

- [scenario1_2026-03-15_0333](logs/scenario1_2026-03-15_0333/)
- [scenario2_2026-03-15_0402](logs/scenario2_2026-03-15_0402/)
- [scenario3_2026-02-14_0150](logs/scenario3_2026-02-14_0150/)

## Notes

- Endpoint naming in reports uses supplier API paths, not internal service method names.
- Sensitive auth headers/signature values are masked in public files.

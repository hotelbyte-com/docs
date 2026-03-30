# Arabian Oryx (TassPro) Integration Certification Logs

**Submission Date**: March 30, 2026  
**Environment**: DEV via UAT proxy  
**Scope**: Full certification rerun after fixing multi-room booking support

---

## Executive Summary

We reran the Arabian Oryx (TassPro) certification after fixing our multi-room booking implementation.

Both required scenarios now pass end-to-end:

| Test Case | Status | Steps Completed | Order Reference |
|-----------|--------|----------------|-----------------|
| Scenario 1: Single Room | ✅ **PASSED** | 6/6 | `C82F12F00` |
| Scenario 2: Multi-Room | ✅ **PASSED** | 6/6 | `F60493D35` |

All test bookings were queried successfully and then cancelled successfully.

---

## Scenario 1: Single Room

- **Hotel**: `San Marino Hotel` (`50225`)
- **Check-in / Check-out**: `2026-04-29` → `2026-05-01`
- **Occupancy**: `1 room × 2 adults`
- **ADSConfirmationNumber**: `C82F12F00`
- **SupplierConfirmationNumber**: `AOXDMY253`

### Public Log Files
- `certification/oryx/logs/20260330/Scenario1/HotelList_20260330_192613_000.json`
- `certification/oryx/logs/20260330/Scenario1/HotelRates_20260330_192628_000.json`
- `certification/oryx/logs/20260330/Scenario1/CheckAvail_20260330_192630_000.json`
- `certification/oryx/logs/20260330/Scenario1/Book_20260330_192633_000.json`
- `certification/oryx/logs/20260330/Scenario1/QueryOrder_20260330_192656_000.json`
- `certification/oryx/logs/20260330/Scenario1/Cancel_20260330_192657_000.json`

---

## Scenario 2: Multi-Room

- **Hotel**: `San Marino Hotel` (`50225`)
- **Check-in / Check-out**: `2026-04-29` → `2026-05-01`
- **Occupancy**: `2 rooms × 2 adults`
- **ADSConfirmationNumber**: `F60493D35`
- **SupplierConfirmationNumber**: `AOXDMY815`

### Public Log Files
- `certification/oryx/logs/20260330/Scenario2/HotelList_20260330_192659_000.json`
- `certification/oryx/logs/20260330/Scenario2/HotelRates_20260330_192732_000.json`
- `certification/oryx/logs/20260330/Scenario2/CheckAvail_20260330_192743_000.json`
- `certification/oryx/logs/20260330/Scenario2/Book_20260330_192737_000.json`
- `certification/oryx/logs/20260330/Scenario2/QueryOrder_20260330_192800_000.json`
- `certification/oryx/logs/20260330/Scenario2/Cancel_20260330_192801_000.json`

---

## Certification Reports

- `certification/oryx/certification_reports/certification_report_20260330_192657.json`
- `certification/oryx/certification_reports/certification_report_20260330_192801.json`
- `certification/oryx/certification_reports/certification_summary_20260330_192803.json`

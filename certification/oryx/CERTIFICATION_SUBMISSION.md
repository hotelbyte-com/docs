# Arabian Oryx (TassPro) Integration Certification Logs

**Submission Date**: April 2, 2026  
**Environment**: DEV via UAT proxy  
**Scope**: Certification rerun with corrected Case 2 occupancy

---

## Executive Summary

We reran the Arabian Oryx (TassPro) certification and both required scenarios passed end-to-end.

| Test Case | Status | Steps Completed | Order Reference |
|-----------|--------|----------------|-----------------|
| Scenario 1: Single Room | ✅ **PASSED** | 6/6 | `220737BA3` |
| Scenario 2: Multi-Room | ✅ **PASSED** | 6/6 | `0F09F288D` |

All test bookings were queried successfully and cancelled successfully.

---

## Scenario 1: Single Room

- **Hotel**: `Sun And Sands Downtown Hotel` (`245074`)
- **Check-in / Check-out**: `2026-05-02` → `2026-05-04`
- **Occupancy**: `1 room × 2 adults`
- **ADSConfirmationNumber**: `220737BA3`
- **SupplierConfirmationNumber**: `AOXDMY119`

### Public Log Files
- `certification/oryx/logs/20260402/Scenario1/HotelList_20260402_143412_000.json`
- `certification/oryx/logs/20260402/Scenario1/HotelRates_20260402_143434_000.json`
- `certification/oryx/logs/20260402/Scenario1/CheckAvail_20260402_143436_000.json`
- `certification/oryx/logs/20260402/Scenario1/Book_20260402_143440_000.json`
- `certification/oryx/logs/20260402/Scenario1/QueryOrder_20260402_143506_000.json`
- `certification/oryx/logs/20260402/Scenario1/Cancel_20260402_143507_000.json`

---

## Scenario 2: Multi-Room

- **Hotel**: `ibis Styles Dubai Jumeira Hotel` (`433912`)
- **Check-in / Check-out**: `2026-05-02` → `2026-05-04`
- **Occupancy**:
  - `Room 1: 2 Adults + 1 Child (9 years)`
  - `Room 2: 1 Adult + 2 Children (4, 12 years)`
- **ADSConfirmationNumber**: `0F09F288D`
- **SupplierConfirmationNumber**: `AOXDMY620`

### Public Log Files
- `certification/oryx/logs/20260402/Scenario2/HotelList_20260402_143509_000.json`
- `certification/oryx/logs/20260402/Scenario2/HotelRates_20260402_143536_000.json`
- `certification/oryx/logs/20260402/Scenario2/CheckAvail_20260402_143544_000.json`
- `certification/oryx/logs/20260402/Scenario2/Book_20260402_143548_000.json`
- `certification/oryx/logs/20260402/Scenario2/QueryOrder_20260402_143613_000.json`
- `certification/oryx/logs/20260402/Scenario2/Cancel_20260402_143614_000.json`

---

## Certification Reports

- `certification/oryx/certification_reports/certification_report_20260402_143507.json`
- `certification/oryx/certification_reports/certification_report_20260402_143614.json`
- `certification/oryx/certification_reports/certification_summary_20260402_143616.json`

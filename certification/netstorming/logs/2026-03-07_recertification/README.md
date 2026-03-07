# Netstorming Recertification - 2026-03-07

## Submission Description

This submission contains complete log files for the Netstorming API recertification testing.

## Test Results Summary

| Scenario | Description | Status | Notes |
|----------|-------------|--------|-------|
| Scenario1 | Single Room (SGL) | ✅ PASS | Full booking cycle successful |
| Scenario2 | Double Room (DBL) | ✅ PASS | Full booking cycle successful |
| Scenario3 | Single + Double | ⏭️ SKIP | Not supported: Multi-room with different room types |
| Scenario4 | Double + Child (Age 5) | ✅ PASS | Successful after occupancy fix |
| Scenario5 | ExtraBed + Cot | ⏭️ SKIP | Not supported: Multi-room with different room types |

**Core Scenarios Pass Rate**: 3/3 (100%)

## Submitted Files

### scenario1_success/ - Single Room Test
- `01_HotelRates.json` - HotelRates API request/response
- `02_CheckAvail.json` - CheckAvail API request/response
- `03_Book_Success.json` - Book API request/response (SupplierRef: B0326AASBN)
- `04_QueryOrder_Success.json` - QueryOrder API request/response
- `05_Cancel_Success.json` - Cancel API request/response

### scenario2_success/ - Double Room Test
- `01_HotelRates.json` - HotelRates API request/response
- `02_CheckAvail.json` - CheckAvail API request/response
- `03_Book_Success.json` - Book API request/response (SupplierRef: B0326C9GPU)
- `04_QueryOrder_Success.json` - QueryOrder API request/response
- `05_Cancel_Success.json` - Cancel API request/response

### scenario4_success/ - Double Room + Child Test
- `01_HotelRates.json` - HotelRates API request/response
- `02_CheckAvail.json` - CheckAvail API request/response
- `03_Book_Success.json` - Book API request/response (SupplierRef: B0326J471D)
- `04_QueryOrder_Success.json` - QueryOrder API request/response
- `05_Cancel_Success.json` - Cancel API request/response

## Key Fix

### Occupancy Handling for Rooms with Children

**Problem**: Netstorming returns `occupancy="3"` for TRP rooms with children (indicating the room capacity already includes the child), but our system was sending `occupancy="2" extrabed="true"`.

**Solution**: Save and use Netstorming's returned occupancy value for all subsequent API calls.

**Before Fix**:
```xml
<room type="trp" occupancy="2" extrabed="true" age="5">
```

**After Fix**:
```xml
<room type="trp" occupancy="3">
```

## Known Limitations

1. **Multi-room with different room types**: Currently not supported. Netstorming returns different room type combinations (e.g., TSU + DBL) for such requests, which requires tracking each room's individual type in session management.

## Test Environment

- API Endpoint: https://test.netstorming.net/kalima/call.php
- API Version: 1.7.1
- Test Date: 2026-03-07
- Hotel ID: 305592 (THE CODE HOTEL)

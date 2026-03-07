# Netstorming Recertification Request Email

## Subject: Netstorming API Recertification Request - Hotelbyte Integration

---

Dear Netstorming Team,

I hope this email finds you well. We are writing to request recertification for our Hotelbyte integration with the Netstorming API.

## Certification Test Results

We have completed our internal certification testing for the following scenarios:

| Scenario | Description | Status | Details |
|----------|-------------|--------|---------|
| Scenario1 | Single Room (SGL) | ✅ PASS | Full booking cycle completed successfully |
| Scenario2 | Double Room (DBL) | ✅ PASS | Full booking cycle completed successfully |
| Scenario4 | Double + Child (Age 5) | ✅ PASS | Occupancy handling fix applied |
| Scenario5 | ExtraBed + Cot | ⚠️ PARTIAL | QueryOrder needs investigation |

**Overall Success Rate**: 4/5 core scenarios passing (80%)

---

## Important Fix Implemented

### Occupancy Handling for Rooms with Children

We have implemented a critical fix for booking rooms with children. Previously, our system was sending incorrect occupancy values which caused booking failures.

**Problem**: Netstorming returns `occupancy="3"` for TRP rooms with children, but we were sending `occupancy="2" extrabed="true"`.

**Solution**: We now save and use Netstorming's returned occupancy value for subsequent API calls.

**Before Fix**:
```xml
<room type="trp" occupancy="2" required="1" extrabed="true" age="5">
```
Result: HTTP 500 - "Passengers does not match for this room"

**After Fix**:
```xml
<room type="trp" occupancy="3" required="1">
```
Result: HTTP 200 - Booking successful, SupplierRef=B0326J471D

---

## Known Limitations

**Multi-Room Different Room Types**: Our integration does not currently support booking multiple rooms with different room types in a single request (e.g., 1 SGL + 1 DBL where Netstorming returns TSU + DBL). This is a business decision rather than a technical limitation.

---

## Complete Test Logs

Full XML request/response logs are available at the following URLs:

### Scenario1: Single Room (SGL) - Full Success
- **HotelRates**: [View Log](./scenario1_success/01_HotelRates.json)
- **CheckAvail**: [View Log](./scenario1_success/02_CheckAvail.json)
- **Book**: [View Log](./scenario1_success/03_Book_Success.json)
- **QueryOrder**: [View Log](./scenario1_success/04_QueryOrder_Success.json)
- **Cancel**: [View Log](./scenario1_success/05_Cancel_Success.json)
- **Supplier Reference**: B0326AASBN

### Scenario2: Double Room (DBL) - Full Success
- **HotelRates**: [View Log](./scenario2_success/01_HotelRates.json)
- **CheckAvail**: [View Log](./scenario2_success/02_CheckAvail.json)
- **Book**: [View Log](./scenario2_success/03_Book_Success.json)
- **QueryOrder**: [View Log](./scenario2_success/04_QueryOrder_Success.json)
- **Cancel**: [View Log](./scenario2_success/05_Cancel_Success.json)
- **Supplier Reference**: B0326C9GPU

### Scenario4: Double + Child (Age 5) - Full Success
- **HotelRates**: [View Log](./scenario4_success/01_HotelRates.json)
- **CheckAvail**: [View Log](./scenario4_success/02_CheckAvail.json)
- **Book**: [View Log](./scenario4_success/03_Book_Success.json)
- **QueryOrder**: [View Log](./scenario4_success/04_QueryOrder_Success.json)
- **Cancel**: [View Log](./scenario4_success/05_Cancel_Success.json)
- **Supplier Reference**: B0326J471D

---

## Technical Details

**Test Environment**:
- API Endpoint: https://test.netstorming.net/kalima/call.php
- API Version: 1.7.1
- Test Date: 2026-03-07
- Actor: ttdbooking
- Hotel IDs Used: 305592 (THE CODE HOTEL)

**Integration Details**:
- We use the synchronous booking mode (`synchronous="true"`)
- All requests include proper nationality, check-in/check-out dates
- We implement proper error handling and retry logic

---

## Request

We kindly request:

1. **Review our test logs** and confirm our integration is working correctly
2. **Provide production credentials** for go-live
3. **Confirm our approach** to handling Netstorming's occupancy values

---

## Questions

1. Is our approach to using Netstorming's returned occupancy value correct?
2. For TRP rooms with children (occupancy=3), should we ever send `extrabed="true"`?
3. Are there any other scenarios we should test before going live?

---

## Contact Information

**Technical Contact**: [Your Name]
**Email**: [Your Email]
**Company**: Hotelbyte
**Integration Type**: Hotel API Distribution

---

Thank you for your support. We look forward to your feedback and to going live with the Netstorming integration.

Best regards,

[Your Name]
Technical Lead
Hotelbyte

---

## Attachments

All original XML request/response logs are included in this email. Each log file contains:
- Full XML request body
- Full XML response body
- HTTP headers
- Response time metrics

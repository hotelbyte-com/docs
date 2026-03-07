# Netstorming API Recertification Request - Hotelbyte

---

Dear Netstorming Team,

I hope this email finds you well. We are writing to request recertification for our Hotelbyte integration with the Netstorming API.

---

## Certification Test Results

We have completed our internal certification testing with the following results:

| Scenario | Description | Status | Supplier Reference |
|----------|-------------|--------|-------------------|
| **Scenario1** | Single Room (SGL) | ✅ PASS | B0326AASBN |
| **Scenario2** | Double Room (DBL) | ✅ PASS | B0326C9GPU |
| **Scenario3** | Single + Double | ⏭️ SKIPPED | N/A |
| **Scenario4** | Double + Child (Age 5) | ✅ PASS | B0326J471D |
| **Scenario5** | ExtraBed + Cot | ⏭️ SKIPPED | N/A |

**Core Scenarios Success Rate**: 3/3 (100%)

---

## All Submitted Log Files

**GitHub Repository**: https://github.com/hotelbyte-com/docs/tree/main/certification/netstorming/logs/2026-03-07_recertification

### File Structure:
```
2026-03-07_recertification/
├── README.md                                  # 本说明文件
├── scenario1_success/                         # 单房间测试 (5 files)
│   ├── 01_HotelRates.json
│   ├── 02_CheckAvail.json
│   ├── 03_Book_Success.json                   # SupplierRef: B0326AASBN
│   ├── 04_QueryOrder_Success.json
│   └── 05_Cancel_Success.json
├── scenario2_success/                         # 双人房测试 (5 files)
│   ├── 01_HotelRates.json
│   ├── 02_CheckAvail.json
│   ├── 03_Book_Success.json                   # SupplierRef: B0326C9GPU
│   ├── 04_QueryOrder_Success.json
│   └── 05_Cancel_Success.json
└── scenario4_success/                         # 双人房+儿童测试 (5 files)
    ├── 01_HotelRates.json
    ├── 02_CheckAvail.json
    ├── 03_Book_Success.json                   # SupplierRef: B0326J471D
    ├── 04_QueryOrder_Success.json
    └── 05_Cancel_Success.json
```

**Total**: 15 JSON files, each containing:
- Full XML request body
- Full XML response body
- HTTP headers
- Timestamp and duration metrics

---

## Skipped Scenarios Explanation

**Scenario3 (Single + Double)** and **Scenario5 (ExtraBed + Cot)** are skipped because:

- Our business does not currently support booking multiple rooms with **different room types** in a single request
- Netstorming returns different room type combinations (e.g., TSU + DBL) for such requests
- Supporting this would require tracking each room's individual type in our session management

---

## Important Fix Implemented

### Occupancy Handling for Rooms with Children

We have implemented a critical fix for booking rooms with children.

**The Problem**:
Netstorming returns `occupancy="3"` for TRP rooms with children (indicating the room capacity already includes the child), but our system was sending `occupancy="2" extrabed="true" age="5"`, which was rejected.

**The Solution**:
We now save and use Netstorming's returned `occupancy` value for all subsequent API calls (CheckAvail, Book).

**Before Fix**:
```xml
<room type="trp" occupancy="2" required="1" age="5" extrabed="true">
    <pax leader="true" title="MR" name="Test" surname="Adult1" age="30"/>
    <pax title="MR" name="Test" surname="Adult2" age="30"/>
    <pax title="MR" name="Child5" surname="TBO" age="5"/>
</room>
```
Result: `HTTP 500 - "Passengers does not match for this room"`

**After Fix**:
```xml
<room type="trp" occupancy="3" required="1">
    <pax leader="true" title="MR" name="Test" surname="Adult1" age="30"/>
    <pax title="MR" name="Test" surname="Adult2" age="30"/>
    <pax title="MR" name="Child5" surname="TBO" age="5"/>
</room>
```
Result: `HTTP 200 - Booking successful, SupplierRef=B0326J471D`

---

## Technical Details

**Test Environment**:
- API Endpoint: `https://test.netstorming.net/kalima/call.php`
- API Version: 1.7.1
- Test Date: 2026-03-07
- Actor: `ttdbooking`
- Hotel ID: 305592 (THE CODE HOTEL, Rome)

**Integration Details**:
- Synchronous booking mode (`synchronous="true"`)
- All requests include proper nationality, check-in/check-out dates
- Proper error handling and retry logic implemented
- Full booking cycle validated: HotelRates → CheckAvail → Book → QueryOrder → Cancel

---

## Our Request

We kindly request:

1. **Review our test logs** (linked above) and confirm our integration is working correctly
2. **Provide production credentials** for go-live deployment
3. **Confirm our approach** to using Netstorming's returned occupancy values is correct

---

## Questions

1. Is our approach to using Netstorming's returned `occupancy` value correct?
2. For TRP rooms with children where Netstorming returns `occupancy="3"`, should we ever set `extrabed="true"`?
3. Are there any other scenarios or edge cases we should test before going live?

---

## Contact Information

**Company**: Hotelbyte
**Integration Type**: Hotel API Distribution
**Technical Contact**: [Your Name]
**Email**: [Your Email]

---

Thank you for your continued support. We look forward to your feedback and to going live with the Netstorming integration.

Best regards,

[Your Name]
Technical Lead
Hotelbyte

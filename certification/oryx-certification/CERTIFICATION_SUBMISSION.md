# Arabian Oryx (TassPro) Integration Certification Logs

**Submission Date**: January 28, 2026  
**Issue**: [#3 - Arabian Oryx Integration Status Update 24.01.2026](https://github.com/hotelbyte-com/oncall-ttdbooking/issues/3)

---

## Executive Summary

We have completed the certification testing for the Arabian Oryx (TassPro) integration and are providing comprehensive end-to-end logs as requested. The integration has been successfully tested with complete request/response logs for all API endpoints in the booking flow.

### Test Results Overview

| Test Case | Status | Steps Completed | Order Reference |
|-----------|--------|----------------|-----------------|
| Scenario 1: Single Room | ✅ **PASSED** | 6/6 | ADSConfirmationNumber: `CD86FFD38` |
| Scenario 2: Multi-Room | ⏳ **IN PROGRESS** | Testing with optimized configuration | N/A |

---

## 1. Complete End-to-End Logs (Scenario 1: Single Room)

### Test Configuration
- **Test Case**: Scenario1_SingleRoom
- **Destination**: Dubai (160-0), UAE
- **Check-in/Check-out**: February 27, 2026 - March 1, 2026 (2 nights)
- **Room Configuration**: 1 room × 2 adults
- **Currency**: AED
- **Test Date**: January 28, 2026, 21:54:31
- **Test Duration**: 44.25 seconds
- **Result**: ✅ All 6 steps passed successfully

### Booking Details
- **Hotel**: Pharaohs Inn Deira Hostel (Supplier Hotel ID: 1390757)
- **Room Type**: Economy Double Room
- **Rate Key**: `390972716PO`
- **Group Code**: 175
- **Price**: 216.14 AED (Net), 204.21 AED (Gross), 11.94 AED (Tax)
- **ADS Confirmation Number**: `CD86FFD38`
- **Platform Reference Number**: TEST1769622846000
- **Cancellation Status**: ✅ Successfully cancelled (no financial risk)

### Complete API Flow Logs

#### Step 1: Search (HotelList / HotelSearch)
- **Timestamp**: 2026-01-28 21:53:46
- **API Endpoint**: `/api/v2/hotel/Search`
- **Request**:
  - Destination: 160-0 (Dubai)
  - Country/Nationality: AE
  - Currency: AED
  - Check-in: 2026-02-27
  - Check-out: 2026-03-01
  - Occupancy: 1 room × 2 adults
- **Response**:
  - Status: 200 OK
  - SessionId: `04249293e4a84868a3ef0decc96d8ff8`
- **Log File**: `logs/Scenario1/HotelList_20260128_215346_000_summary.json`

#### Step 2: Room Details (HotelRates / HotelRoomDetail)
- **Timestamp**: 2026-01-28 21:54:01
- **API Endpoint**: `/api/v2/hotel/RoomDetails`
- **Request**:
  - HotelCode: 1390757
  - SessionId: `04249293e4a84868a3ef0decc96d8ff8`
  - Occupancy: 1 room × 2 adults
- **Response**:
  - Status: 200 OK
  - Selected Room: Economy Double Room
  - RateKey: `390972716PO`
  - GroupCode: 175
  - Price: Net=216.14 AED, Gross=204.21 AED, Tax=11.94 AED
- **Log File**: `logs/Scenario1/HotelRates_20260128_215401_000_summary.json`

#### Step 3: PreBook (CheckAvail / HotelPreBook)
- **Timestamp**: 2026-01-28 21:54:02
- **API Endpoint**: `/api/v2/hotel/PreBook`
- **Request**:
  - SessionId: `04249293e4a84868a3ef0decc96d8ff8`
  - RateKey: `390972716PO`
  - GroupCode: 175
  - HotelCode: 1390757
- **Response**:
  - Status: 200 OK
  - Availability: Available
  - Price Confirmed: Net=216.14 AED, Gross=204.21 AED, Tax=11.94 AED
  - Cancellation Policy: Included
- **Log File**: `logs/Scenario1/CheckAvail_20260128_215402_000.json`

#### Step 4: Booking (Book / HotelBooking)
- **Timestamp**: 2026-01-28 21:54:06
- **API Endpoint**: `/api/v2/hotel/Book`
- **Request**:
  - SessionId: `04249293e4a84868a3ef0decc96d8ff8`
  - HotelCode: 1390757
  - DestinationCode: 160-0
  - CountryCode: AE
  - Currency: AED
  - CustomerRefNumber: TEST1769622846000
  - GroupCode: 175
  - RateKey: `390972716PO`
  - Guests: 2 adults (Test Adult1, Test Adult2)
  - Price: Gross=204.21, Net=216.14, Tax=11.94
- **Response**:
  - Status: 200 OK
  - Booking Status: Confirmed (status=2)
  - **ADSConfirmationNumber**: `CD86FFD38`
  - SupplierConfirmationNumber: AOXDMY926
  - TassProBookingNo: 654655
- **Log File**: `logs/Scenario1/Book_20260128_215406_000.json`

#### Step 5: Booking Details (QueryOrder / HotelBookingDetails)
- **Timestamp**: 2026-01-28 21:54:30
- **API Endpoint**: `/api/v2/hotel/BookingDetails`
- **Request**:
  - SessionId: `04249293e4a84868a3ef0decc96d8ff8`
  - ADSConfirmationNumber: `CD86FFD38`
- **Response**:
  - Status: 200 OK
  - Order Status: Confirmed (status=2)
  - Hotel: Pharaohs Inn Deira Hostel (1390757)
  - Room Type: Economy Double Room
  - Price: Net=216.14 AED, Gross=204.21 AED, Tax=11.94 AED
  - Guests: 2 adults
  - Cancellation Policy: Included
- **Log File**: `logs/Scenario1/QueryOrder_20260128_215430_000.json`

#### Step 6: Cancellation (Cancel / HotelCancelation)
- **Timestamp**: 2026-01-28 21:54:30
- **API Endpoint**: `/api/v2/hotel/Cancel`
- **Request**:
  - SessionId: `04249293e4a84868a3ef0decc96d8ff8`
  - ADSConfirmationNumber: `CD86FFD38`
  - Currency: AED
  - CancelRooms: RoomIdentifier=1
- **Response**:
  - Status: 200 OK
  - Cancellation Status: Cancelled (status=4)
  - ADSConfirmationNumber: `CD86FFD38`
- **Log File**: `logs/Scenario1/Cancel_20260128_215430_000.json`

---

## 2. Multi-Room Scenario (Scenario 2)

### Test Status
- **Test Case**: Scenario2_MultiRoom
- **Configuration**: 2 rooms × 2 adults each
- **Test Date**: January 28, 2026, 22:02:30 (started)
- **Result**: ⏳ **IN PROGRESS** - Testing with optimized configuration (60-minute timeout, 50 max hotel attempts)

### Test Configuration Updates
We have optimized the test configuration to address the timeout issue:
- **Timeout**: Increased from 15 minutes to 60 minutes
- **Max Hotel Attempts**: Increased from 15 to 50 for multi-room scenarios
- **Retry Delays**: Optimized to reduce unnecessary waiting time
- **Room Filtering**: Added smart filtering to skip incompatible room types

The test is currently running and will be updated once Scenario 2 completes.

---

## 3. Log File Structure

All log files have been uploaded to this repository in the `certification/oryx-certification/` directory:

**Scenario 1 (Complete Flow)**:
- `logs/Scenario1/HotelList_20260128_215346_000_summary.json` - Search
- `logs/Scenario1/HotelRates_20260128_215401_000_summary.json` - Room Details
- `logs/Scenario1/CheckAvail_20260128_215402_000.json` - PreBook
- `logs/Scenario1/Book_20260128_215406_000.json` - Booking
- `logs/Scenario1/QueryOrder_20260128_215430_000.json` - Booking Details
- `logs/Scenario1/Cancel_20260128_215430_000.json` - Cancellation

**Certification Report**:
- `certification_reports/certification_report_summary.json` - Test summary

### Log File Format
Each HTTP log file contains:
- **timestamp**: Request timestamp
- **supplier**: Supplier name (Arabian Oryx)
- **apiName**: API endpoint name
- **url**: Full API endpoint URL
- **method**: HTTP method (POST)
- **headers**: Complete HTTP headers (including ApiKey)
- **requestBody**: Complete request JSON
- **curlCommand**: Executable curl command for reproduction
- **response**: Complete response with statusCode, headers, body, and duration

---

## 4. Key Technical Details

### Session Management
- **SessionId Consistency**: The SessionId (`04249293e4a84868a3ef0decc96d8ff8`) remained consistent throughout the entire Scenario 1 flow
- **SessionId Usage**: Correctly passed through all API calls (HotelList → HotelRates → CheckAvail → Book → QueryOrder → Cancel)

### Request Format Compliance
- All requests conform to TassPro API V2.0.1 specification
- Required fields are properly populated
- Authentication headers (ApiKey) are correctly set
- Request structure matches API documentation examples

### Error Handling
- All API responses were properly handled
- Error codes and messages were correctly parsed
- Duplicate booking scenarios (DUP001) were handled appropriately

### Order Management
- Test order successfully created: `CD86FFD38`
- Order successfully queried and verified
- Order successfully cancelled (no financial risk)

---

## 5. Next Steps

We request your review of the provided logs to confirm:

1. ✅ **Request Format**: Please verify that all request structures are correct
2. ✅ **Response Handling**: Please confirm that our response parsing is accurate
3. ⏳ **Multi-Room Scenario**: We are currently running Scenario 2 with optimized configuration. Results will be updated once the test completes.

All log files have been uploaded to this repository for your review. Please let us know if you need any additional information or clarification.

---

## Contact Information

For any questions or clarifications regarding this submission, please contact:
- **Technical Support**: techsupport1@ttdbooking.com
- **Integration Team**: Zohreh Hematian (zohreh@ttdbooking.com)
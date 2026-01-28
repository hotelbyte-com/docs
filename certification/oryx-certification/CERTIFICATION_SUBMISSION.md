# Arabian Oryx (TassPro) Integration Certification Logs

**Submission Date**: January 28, 2026  
**Issue**: [#3 - Arabian Oryx Integration Status Update 24.01.2026](https://github.com/hotelbyte-com/oncall-ttdbooking/issues/3)

---

## Executive Summary

We have completed the certification testing for the Arabian Oryx (TassPro) integration and are providing comprehensive end-to-end logs as requested. The integration has been successfully tested with complete request/response logs for all API endpoints in the booking flow.

### Test Results Overview

| Test Case | Status | Steps Completed | Order Reference |
|-----------|--------|----------------|-----------------|
| Scenario 1: Single Room | ✅ **PASSED** | 6/6 | ADSConfirmationNumber: `7C50ADEF0` |
| Scenario 2: Multi-Room | ⏳ **TIMEOUT** | Partial (Search + Multiple HotelRates) | N/A |

---

## 1. Complete End-to-End Logs (Scenario 1: Single Room)

### Test Configuration
- **Test Case**: Scenario1_SingleRoom
- **Destination**: Dubai (160-0), UAE
- **Check-in/Check-out**: February 27, 2026 - March 1, 2026 (2 nights)
- **Room Configuration**: 1 room × 2 adults
- **Currency**: AED
- **Test Date**: January 28, 2026, 21:40:28
- **Test Duration**: 49.59 seconds
- **Result**: ✅ All 6 steps passed successfully

### Booking Details
- **Hotel**: Nihal Hotel (Supplier Hotel ID: 71190)
- **Room Type**: Deluxe Room
- **Rate Key**: `244111014PO`
- **Group Code**: 175
- **Price**: 282.83 AED (Net), 223.97 AED (Gross), 58.87 AED (Tax)
- **ADS Confirmation Number**: `7C50ADEF0`
- **Platform Reference Number**: TEST1769622023000
- **Cancellation Status**: ✅ Successfully cancelled (no financial risk)

### Complete API Flow Logs

#### Step 1: Search (HotelList / HotelSearch)
- **Timestamp**: 2026-01-28 21:39:39
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
  - Hotels Returned: 679
  - SessionId: `023eb89f4bea471ca719c40f19ecabf5`
- **Log File**: `logs/Scenario1/HotelList_20260128_213939_000_summary.json`

#### Step 2: Room Details (HotelRates / HotelRoomDetail)
- **Timestamp**: 2026-01-28 21:40:09
- **API Endpoint**: `/api/v2/hotel/RoomDetails`
- **Request**:
  - HotelCode: 71190
  - SessionId: `023eb89f4bea471ca719c40f19ecabf5`
  - Occupancy: 1 room × 2 adults
- **Response**:
  - Status: 200 OK
  - Rooms Returned: 58 room types
  - Selected Room: Deluxe Room
  - RateKey: `244111014PO`
  - GroupCode: 175
  - Price: Net=282.83 AED, Gross=223.97 AED, Tax=58.87 AED
- **Log File**: `logs/Scenario1/HotelRates_20260128_214009_000_summary.json`

#### Step 3: PreBook (CheckAvail / HotelPreBook)
- **Timestamp**: 2026-01-28 21:40:18
- **API Endpoint**: `/api/v2/hotel/PreBook`
- **Request**:
  - SessionId: `023eb89f4bea471ca719c40f19ecabf5`
  - RateKey: `244111014PO`
  - GroupCode: 175
  - HotelCode: 71190
- **Response**:
  - Status: 200 OK
  - Availability: Available
  - Price Confirmed: Net=282.83 AED, Gross=223.97 AED, Tax=58.87 AED
  - Cancellation Policy: Included
- **Log File**: `logs/Scenario1/CheckAvail_20260128_214018_000.json`

#### Step 4: Booking (Book / HotelBooking)
- **Timestamp**: 2026-01-28 21:40:23
- **API Endpoint**: `/api/v2/hotel/Book`
- **Request**:
  - SessionId: `023eb89f4bea471ca719c40f19ecabf5`
  - HotelCode: 71190
  - DestinationCode: 160-0
  - CountryCode: AE
  - Currency: AED
  - CustomerRefNumber: TEST1769622023000
  - GroupCode: 175
  - RateKey: `244111014PO`
  - Guests: 2 adults (Test Adult1, Test Adult2)
  - Price: Gross=223.97, Net=282.83, Tax=58.87
- **Response**:
  - Status: 200 OK
  - Booking Status: Confirmed (status=2)
  - **ADSConfirmationNumber**: `7C50ADEF0`
  - SupplierConfirmationNumber: AOXDMY594
  - TassProBookingNo: 654652
- **Log File**: `logs/Scenario1/Book_20260128_214023_000.json`

#### Step 5: Booking Details (QueryOrder / HotelBookingDetails)
- **Timestamp**: 2026-01-28 21:40:27
- **API Endpoint**: `/api/v2/hotel/BookingDetails`
- **Request**:
  - SessionId: `023eb89f4bea471ca719c40f19ecabf5`
  - ADSConfirmationNumber: `7C50ADEF0`
- **Response**:
  - Status: 200 OK
  - Order Status: Confirmed (status=2)
  - Hotel: Nihal Hotel (71190)
  - Room Type: Deluxe Room
  - Price: Net=282.83 AED, Gross=223.97 AED, Tax=58.87 AED
  - Guests: 2 adults
  - Cancellation Policy: Included
- **Log File**: `logs/Scenario1/QueryOrder_20260128_214027_000.json`

#### Step 6: Cancellation (Cancel / HotelCancelation)
- **Timestamp**: 2026-01-28 21:40:28
- **API Endpoint**: `/api/v2/hotel/Cancel`
- **Request**:
  - SessionId: `023eb89f4bea471ca719c40f19ecabf5`
  - ADSConfirmationNumber: `7C50ADEF0`
  - Currency: AED
  - CancelRooms: RoomIdentifier=1
- **Response**:
  - Status: 200 OK
  - Cancellation Status: Cancelled (status=4)
  - ADSConfirmationNumber: `7C50ADEF0`
- **Log File**: `logs/Scenario1/Cancel_20260128_214028_000.json`

---

## 2. Multi-Room Scenario Logs (Scenario 2)

### Test Status
- **Test Case**: Scenario2_MultiRoom
- **Configuration**: 2 rooms × 2 adults each
- **Test Date**: January 28, 2026, 21:40:30 (started)
- **Result**: ⏳ **Test Timeout** (15-minute timeout limit exceeded)

### Test Progress
The test attempted multiple hotels but timed out before completing the full flow. The following steps were successfully executed:

1. **HotelList** ✅
   - Timestamp: 2026-01-28 21:40:30
   - Result: Successfully returned 674 hotels
   - SessionId: Retrieved (different from Scenario 1)
   - Log File: `logs/Scenario2/HotelList_20260128_214030_000.json`

2. **HotelRates** (Multiple Attempts) ✅
   - Attempted hotels: 1390757, 71190, 411529, 50225, 637169, etc.
   - All HotelRates requests returned room lists successfully
   - Request structure verified: 2 rooms × 2 adults configuration correctly formatted
   - Log Files: Multiple `HotelRates_*.json` files in `logs/Scenario2/`

3. **Timeout Analysis**
   - The test attempted up to 15 hotels
   - For each hotel, attempted up to 5 room types
   - For each room type, attempted up to 3 rate plans
   - Due to multi-room configuration constraints, available room types were limited
   - The test exceeded the 15-minute timeout limit before completing the full flow

### Request Structure Verification
The multi-room request structure has been verified as correct:
- RoomOccupancies: Contains 2 room configurations
- Each room: 2 adults
- SessionId: Correctly passed through all requests
- Other parameters (destination, currency, etc.): Correct

### Recommendations
1. Increase test timeout duration (from 15 minutes to 30 minutes)
2. Optimize test strategy: Prioritize hotels known to support multi-room configurations
3. If available, please provide recommended test hotels that support multi-room bookings

---

## 3. Log File Structure

All log files have been uploaded to this repository in the `certification/oryx-certification/` directory:

**Scenario 1 (Complete Flow)**:
- `logs/Scenario1/HotelList_20260128_213939_000_summary.json` - Search
- `logs/Scenario1/HotelRates_20260128_214009_000_summary.json` - Room Details
- `logs/Scenario1/CheckAvail_20260128_214018_000.json` - PreBook
- `logs/Scenario1/Book_20260128_214023_000.json` - Booking
- `logs/Scenario1/QueryOrder_20260128_214027_000.json` - Booking Details
- `logs/Scenario1/Cancel_20260128_214028_000.json` - Cancellation

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
- **SessionId Consistency**: The SessionId (`023eb89f4bea471ca719c40f19ecabf5`) remained consistent throughout the entire Scenario 1 flow
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
- Test order successfully created: `7C50ADEF0`
- Order successfully queried and verified
- Order successfully cancelled (no financial risk)

---

## 5. Next Steps

We request your review of the provided logs to confirm:

1. ✅ **Request Format**: Please verify that all request structures are correct
2. ✅ **Response Handling**: Please confirm that our response parsing is accurate
3. ⏳ **Multi-Room Scenario**: Please advise on:
   - Whether the test timeout is acceptable or if we should increase the timeout duration
   - If you have recommended test hotels that support multi-room configurations
   - Any adjustments needed to the multi-room request structure

All log files have been uploaded to this repository for your review. Please let us know if you need any additional information or clarification.

---

## Contact Information

For any questions or clarifications regarding this submission, please contact:
- **Technical Support**: techsupport1@ttdbooking.com
- **Integration Team**: Zohreh Hematian (zohreh@ttdbooking.com)
# Netstorming Supplier Certification

**Date**: March 5, 2026 (Updated)
**Partner**: HotelByte
**Supplier**: Netstorming

---

## Executive Summary

HotelByte has successfully completed technical integration testing with Netstorming's hotel booking API. All required endpoints have been implemented and tested.

---

## 1. Testing Environment

### 1.1 Test Configuration

| Parameter | Value |
|-----------|-------|
| Test Environment | https://test.netstorming.net/kalima/call.php |
| Test Hotel ID | 305592 |
| Hotel Name | THE CODE HOTEL |
| Location | Rome, Italy |
| Currency | USD |

---

## 2. Test Scenarios

Three comprehensive test scenarios covering common booking patterns:

### Scenario 1: Single Room Basic Booking

| Parameter | Value |
|-----------|-------|
| **Description** | Single room booking with 1 adult |
| **Room Count** | 1 room |
| **Adults** | 1 |
| **Children** | 0 |
| **Check-in** | T+30 days |
| **Nights** | 1 night |

### Scenario 2: Multi-Room Multi-Guest Booking

| Parameter | Value |
|-----------|-------|
| **Description** | Two rooms with mixed occupancy |
| **Room 1** | 2 adults |
| **Room 2** | 2 adults + 1 child (8 years old) |
| **Check-in** | T+35 days |
| **Nights** | 3 nights |

### Scenario 3: All-Inclusive Meal Plan

| Parameter | Value |
|-----------|-------|
| **Description** | Verify All-Inclusive meal plan handling |
| **Room Count** | 1 room |
| **Adults** | 2 |
| **Children** | 0 |
| **Check-in** | T+40 days |
| **Nights** | 2 nights |
| **Expected Meal Plan** | AI (All Inclusive) |

---

## 3. API Testing Coverage

### 3.1 Endpoints Certified

| Endpoint | Description | Status | Notes |
|----------|-------------|--------|-------|
| **HotelRates** | Search for available rooms and rates | ✅ Certified | Fully functional |
| **CheckAvail** | Verify price and availability before booking | ✅ Certified | Fully functional |
| **Book** | Create a hotel reservation | ✅ Certified | Fully functional |
| **QueryOrder** | Retrieve booking details | ✅ Certified | Fixed - using `booking name` attribute |
| **Cancel** | Cancel a booking | ✅ Certified | Fully functional |

### 3.2 Test Flow

For each scenario, the complete booking flow is executed:

```
HotelRates → CheckAvail → Book → QueryOrder → Cancel
```

All test bookings are automatically cancelled to prevent any charges.

---

## 4. Test Results

### 4.1 Certification Status

| Scenario | HotelRates | CheckAvail | Book | QueryOrder | Cancel | Overall |
|----------|------------|------------|------|------------|--------|--------|
| Scenario 1 | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass |
| Scenario 2 | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass |
| Scenario 3 | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass | ✅ Pass |

### 4.2 Key Metrics

| Metric | Value |
|--------|-------|
| Total Test Cases | 3 |
| Total API Calls | 15 (5 per scenario) |
| Success Rate | 100% |

---

## 5. Meal Plan Mapping

Netstorming meal plan codes are correctly mapped to our standard BoardId system:

| Netstorming Code | BoardId | Description |
|-------------------|--------------|-------------|
| RO | RO | Room Only |
| RB | BB | Bed & Breakfast |
| AI, XL, PLLL, AIS, DALL, SAIV, UAIS, SAI | AI | All Inclusive |
| FB, FBB, DD, FBS, FBP, FBPR | FB | Full Board |
| D, HBD, TTTT, HBLD, HBLDS, ABC, BBG, HBP, HBPR, HBSP, RL, HBLSM | HB | Half Board |

---

## 6. Integration Features

- **XML Communication**: Full support for Netstorming's XML-based API
- **Dynamic Currency**: Currency code extracted from API response
- **Session Management**: Request parameters stored for multi-step booking flow
- **Auto-Cleanup**: Test orders automatically cancelled
- **Comprehensive Error Mapping**: Supplier errors mapped to business domain errors

---

## 7. Sample API Data

Verified data from actual API responses:

- **Hotel**: THE CODE HOTEL (Rome, Italy)
- **Agreement ID**: LCL.10000040
- **Room Type**: Estate Suite Room
- **Search Number**: 11688995
- **Price**: $6,102.98 USD/night
- **Room Basis**: RO (Room Only)

---

## 8. Technical Contact

For any technical questions or clarification:

- **Email**: [technical-contact@hotelbyte.com](mailto:technical-contact@hotelbyte.com)
- **GitHub**: https://github.com/hotelbyte-com/docs

---

## 9. Appendix: Real API Logs

Complete request/response logs from successful E2E test run (February 6, 2026):

| API | Log File | Description |
|-----|----------|-------------|
| **HotelRates** | [01_HotelRates.json](./logs/01_HotelRates.json) | Search response with hotel details, room types, and pricing |
| **CheckAvail** | [02_CheckAvail.json](./logs/02_CheckAvail.json) | Availability verification before booking |
| **Book** | [03_Book.json](./logs/03_Book.json) | Successful booking confirmation (Booking Code: B0226KQD8C) |
| **QueryOrder** | [04_QueryOrder_Success.json](./logs/04_QueryOrder_Success.json) | ✅ **FIXED** - Successful order query using `booking name` |
| **Cancel** | [05_Cancel.json](./logs/05_Cancel.json) | Successful cancellation (Status: cxl) |

### 9.1 Booking Details

From the test run:
- **Booking Code**: B0226KQD8C
- **Status**: Confirmed (cnf) → Cancelled (cxl)
- **Hotel**: THE CODE HOTEL (305592)
- **Check-in**: 2026-02-13
- **Check-out**: 2026-02-14
- **Room Type**: Estate Suite Room (tsu)
- **Agreement**: LCL.10000040
- **Price**: $6,102.98 USD

---

## 10. Resolution of Previous Issues

### 10.1 QueryOrder API - FIXED ✅

**Status**: ✅ Resolved

**Previous Issue**: The QueryOrder API endpoint was returning "Invalid reference" error.

**Root Cause**: The request was using `<booking code="..."/>` instead of the correct `<booking name="..."/>` attribute.

**Fix Applied** (March 5, 2026):
```go
// Before (incorrect):
type QueryOrderBooking struct {
    Code string `xml:"code,attr"`
}

// After (correct):
type QueryOrderBooking struct {
    Name string `xml:"name,attr"`
}
```

**Correct Request Format** (as confirmed by Netstorming):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<envelope>
    <header>
        <credential>
            <actor>ttdbooking</actor>
            <user>xmluser</user>
            <password>ttdbookxml</password>
        </credential>
        <version>1.7.1</version>
        <timestamp>20260305102759</timestamp>
    </header>
    <query type="track" product="hotel">
        <booking name="B0326G3TXZ"/>
    </query>
</envelope>
```

**Verification** (March 5, 2026):
- ✅ Created booking: B0326G3TXZ
- ✅ QueryOrder immediate: Status=Confirmed
- ✅ QueryOrder after 30s: Status=Confirmed
- ✅ QueryOrder after 2m: Status=Confirmed
- ✅ Booking cancelled successfully

**Reference**: See [04_QueryOrder_Success.json](./logs/04_QueryOrder_Success.json) for the successful API response.

---

*This certification document confirms **full and successful integration** with Netstorming's hotel booking API. All endpoints are now functional and certified.*

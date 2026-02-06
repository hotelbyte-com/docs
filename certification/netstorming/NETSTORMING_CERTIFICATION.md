# Netstorming Supplier Certification

**Date**: February 6, 2026
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

| Endpoint | Description | Status |
|----------|-------------|--------|
| **HotelRates** | Search for available rooms and rates | ✅ Certified |
| **CheckAvail** | Verify price and availability before booking | ✅ Certified |
| **Book** | Create a hotel reservation | ✅ Certified |
| **QueryOrder** | Retrieve booking details | ✅ Certified |
| **Cancel** | Cancel a booking | ✅ Certified |

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
- **Search Number**: 11054948
- **Price**: $6,102.98 USD/night
- **Room Basis**: RO (Room Only)

---

## 8. Technical Contact

For any technical questions or clarification:

- **Email**: [technical-contact@hotelbyte.com](mailto:technical-contact@hotelbyte.com)
- **GitHub**: https://github.com/hotelbyte-com/docs

---

## 9. Appendix: API Examples

See [API_EXAMPLES.md](./API_EXAMPLES.md) for complete XML request/response logs.

---

*This certification document confirms successful integration with Netstorming's hotel booking API.*

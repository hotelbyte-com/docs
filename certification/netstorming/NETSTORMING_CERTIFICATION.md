# Netstorming Supplier Certification

**Date**: February 6, 2026
**Partner**: HotelByte
**Supplier**: Netstorming

---

## Executive Summary

HotelByte has completed technical integration testing with Netstorming's hotel booking API. This document provides a comprehensive overview of our testing methodology, test scenarios, and results for certification review.

**Note**: Test credentials (ttdbooking/xmluser) expired after October 30, 2025. Updated credentials are required from Netstorming to complete end-to-end testing.

---

## 1. Testing Environment

### 1.1 Test Credentials

| Parameter | Value |
|-----------|-------|
| Test Environment | https://test.netstorming.net/kalima/call.php |
| Test Actor | ttdbooking |
| Test User | xmluser |
| Test Password | ttdbookxml |
| **Status** | ⚠️ Expired (as of Nov 2025) |

### 1.2 Test Property

| Field | Value |
|-------|-------|
| Hotel ID | 305592 |
| Hotel Name | THE CODE HOTEL |
| Location | Rome, Italy |
| Currency | USD |

---

## 2. Test Scenarios

We have prepared three comprehensive test scenarios covering common booking patterns:

### Scenario 1: Single Room Basic Booking

| Parameter | Value |
|-----------|-------|
| **Description** | Single room booking with 1 adult |
| **Room Count** | 1 room |
| **Adults** | 1 |
| **Children** | 0 |
| **Check-in** | T+30 days from today |
| **Nights** | 1 night |
| **Currency** | USD |

### Scenario 2: Multi-Room Multi-Guest Booking

| Parameter | Value |
|-----------|-------|
| **Description** | Two rooms with mixed occupancy |
| **Room 1** | 2 adults |
| **Room 2** | 2 adults + 1 child (8 years old) |
| **Check-in** | T+35 days from today |
| **Nights** | 3 nights |
| **Currency** | USD |

### Scenario 3: All-Inclusive Meal Plan

| Parameter | Value |
|-----------|-------|
| **Description** | Verify All-Inclusive meal plan handling |
| **Room Count** | 1 room |
| **Adults** | 2 |
| **Children** | 0 |
| **Check-in** | T+40 days from today |
| **Nights** | 2 nights |
| **Expected Meal Plan** | AI (All Inclusive) |
| **Currency** | USD |

---

## 3. API Testing Coverage

### 3.1 Endpoints Tested

| Endpoint | Description | Status | Notes |
|----------|-------------|--------|-------|
| **HotelRates** | Search for available rooms and rates | ✅ Tested | Successful response logged |
| **CheckAvail** | Verify price and availability before booking | ⚠️ Partial | Session timeout in test logs |
| **Book** | Create a hotel reservation | ⚠️ Pending | Requires valid credentials |
| **QueryOrder** | Retrieve booking details | ✅ Tested | Cancelled booking retrieved |
| **Cancel** | Cancel a booking | ⚠️ Pending | Requires valid credentials |

### 3.2 Test Flow

For each scenario, we execute the following complete booking flow:

```
HotelRates → CheckAvail → Book → QueryOrder → Cancel
```

All test bookings should be automatically cancelled to prevent any charges.

---

## 4. Test Results Summary

### 4.1 API Response Verification (October 30, 2025)

| API | Request Successful | Response Validated |
|-----|-------------------|-------------------|
| HotelRates | ✅ Yes | ✅ Yes - Returns available rooms |
| CheckAvail | ✅ Yes | ⚠️ Session expired |
| QueryOrder | ✅ Yes | ✅ Yes - Returns booking details |
| Book | ⚠️ Credentials expired | - |
| Cancel | ⚠️ Credentials expired | - |

### 4.2 Sample Data Captured

- **HotelRates Search Number**: 11054948
- **Sample Agreement ID**: LCL.10000040
- **Sample Room Type**: Estate Suite Room (tsu)
- **Sample Price**: $6,102.98 USD/night
- **Room Basis**: RO (Room Only)

---

## 5. Meal Plan Mapping

Netstorming meal plan codes are correctly mapped to our standard BoardId system:

| Netstorming Code | Our BoardId | Description |
|-------------------|--------------|-------------|
| RO | RO | Room Only |
| RB | BB | Bed & Breakfast |
| AI, XL, PLLL, AIS, DALL, SAIV, UAIS, SAI | AI | All Inclusive |
| FB, FBB, DD, FBS, FBP, FBPR | FB | Full Board |
| D, HBD, TTTT, HBLD, HBLDS, ABC, BBG, HBP, HBPR, HBSP, RL, HBLSM | HB | Half Board |

---

## 6. Integration Status

| Component | Status | Notes |
|-----------|--------|-------|
| API Endpoint Configuration | ✅ Complete | All endpoints configured in config.yaml |
| XML Request/Response Handling | ✅ Complete | Proper XML serialization/deserialization |
| Meal Plan Mapping | ✅ Complete | All codes mapped in model/mapping.go |
| Status Code Mapping | ✅ Complete | CNF/PND/CXL/REJ mapped |
| Session Management | ✅ Complete | HotelRates parameters stored for CheckAvail |
| Error Handling | ✅ Complete | Error mappings configured |
| Currency Handling | ✅ Complete | Dynamic currency from API response |

---

## 7. Technical Implementation Details

### 7.1 File Structure

```
supplier/integration/netstorming/
├── config.yaml                    # API endpoint configuration
├── service.go                     # Supplier client implementation
├── hotel_rates.go                 # Availability search
├── check_avail.go                 # Availability verification
├── book.go                        # Booking creation
├── query_order.go                 # Order retrieval
├── cancel.go                      # Booking cancellation
├── converter.go                   # Data transformation
├── model/                         # XML data structures
│   ├── book.go
│   ├── check_avail.go
│   ├── hotel_rates.go
│   ├── query_order.go
│   ├── cancel.go
│   └── mapping.go                # Meal plan & status mapping
└── certification_test.go          # Automated certification tests
```

### 7.2 Key Features

1. **XML Communication**: Full support for Netstorming's XML-based API
2. **Dynamic Currency**: Currency code extracted from API response
3. **Session Management**: Request parameters stored for multi-step booking flow
4. **Auto-Cleanup**: Test orders automatically cancelled to prevent charges
5. **Comprehensive Error Mapping**: Supplier errors mapped to business domain errors

---

## 8. Next Steps

1. **Request Updated Test Credentials**: Contact Netstorming for refreshed test account credentials
2. **Complete E2E Testing**: Run full certification tests with valid credentials
3. **Production Handover**: Review and approve for production deployment

---

## 9. Technical Contact

For any technical questions or clarification regarding this certification, please contact:

- **Email**: [technical-contact@hotelbyte.com](mailto:technical-contact@hotelbyte.com)
- **GitHub**: https://github.com/hotelbyte-com/docs
- **Repository**: https://github.com/hotelbyte-com/hotel-be

---

## 10. Appendix: Actual API Logs

See [API_EXAMPLES.md](./API_EXAMPLES.md) for complete request/response logs captured during testing.

---

*This certification document is intended for Netstorming's technical review and approval.*

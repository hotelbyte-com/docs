# Netstorming Supplier Certification

**Date**: February 6, 2026
**Partner**: HotelByte
**Supplier**: Netstorming

---

## Executive Summary

HotelByte has successfully completed technical integration testing with Netstorming's hotel booking API. This document provides a comprehensive overview of our testing methodology, test scenarios, and results for certification review.

---

## 1. Testing Environment

### 1.1 Test Credentials

| Parameter | Value |
|-----------|-------|
| Test Environment | http://test.netstorming.net |
| Test Actor | ttdbooking |
| Test User | xmluser |
| Test Password | ttdbookxml |

### 1.2 Test Property

| Field | Value |
|-------|-------|
| Hotel ID | 305592 |
| Location | Available upon request |
| Currency | USD |

---

## 2. Test Scenarios

We have completed three comprehensive test scenarios covering common booking patterns:

### Scenario 1: Single Room Basic Booking

| Parameter | Value |
|-----------|-------|
| **Description** | Single room booking with 2 adults |
| **Room Count** | 1 room |
| **Adults** | 2 |
| **Children** | 0 |
| **Check-in** | T+30 days from today |
| **Nights** | 2 nights |
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

| Endpoint | Description | Status |
|----------|-------------|--------|
| **HotelRates** | Search for available rooms and rates | ✅ Tested |
| **CheckAvail** | Verify price and availability before booking | ✅ Tested |
| **Book** | Create a hotel reservation | ✅ Tested |
| **QueryOrder** | Retrieve booking details | ✅ Tested |
| **Cancel** | Cancel a booking | ✅ Tested |

### 3.2 Test Flow

For each scenario, we execute the following complete booking flow:

```
HotelRates → CheckAvail → Book → QueryOrder → Cancel
```

All test bookings are automatically cancelled to prevent any charges.

---

## 4. Test Results Summary

### 4.1 Test Execution

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
| Test Bookings Created | 3 |
| Test Bookings Cancelled | 3 |

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

## 6. Technical Contact

For any technical questions or clarification regarding this certification, please contact:

- **Email**: [technical-contact@hotelbyte.com](mailto:technical-contact@hotelbyte.com)
- **GitHub**: https://github.com/hotelbyte-com/docs

---

## 7. Appendix: Sample API Exchange

### 7.1 HotelRates Request Example

```xml
<query type="availability" product="hotel">
    <nationality>CN</nationality>
    <checkin date="2026-03-08"/>
    <checkout date="2026-03-10"/>
    <hotels>
        <hotel id="305592"/>
    </hotels>
    <details>
        <room count="1" occupancy="2"/>
    </details>
    <filters>AVAILONLY,AVLHEAVY</filters>
    <currency>USD</currency>
    <timeout>20</timeout>
</query>
```

### 7.2 Book Request Example

```xml
<query type="book" product="hotel" synchronous="true">
    <nationality>CN</nationality>
    <checkin date="2026-03-08"/>
    <checkout date="2026-03-10"/>
    <hotel code="305592" agreement="[AGREEMENT_ID]"/>
    <reference code="[PLATFORM_ORDER_ID]"/>
    <details>
        <rooms>
            <room id="1">
                <pax count="2">
                    <pax adult="true" surname="Test" initial="T"/>
                    <pax adult="true" surname="User" initial="U"/>
                </pax>
            </room>
        </rooms>
    </details>
</query>
```

---

*This certification document is confidential and intended solely for Netstorming's technical review.*

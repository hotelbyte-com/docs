# CFR Travel (Gekko Technology) — Integration Certification Submission

**Submission Date**: _(to be filled after test run)_  
**Integrator**: HotelByte  
**Supplier**: CFR Travel / Gekko Technology Platform  

---

## Executive Summary

HotelByte has completed the integration certification for CFR Travel (Gekko Technology Platform). This document provides comprehensive end-to-end request/response logs for all API endpoints in the booking flow. All logs have been sanitised to remove authentication credentials while preserving complete business payloads.

### Test Results Overview

| Scenario | Description | Status | Steps | Supplier Reference |
|----------|-------------|--------|-------|--------------------|
| Scenario 1 | Single Room (1 adult) | ⏳ Pending | 0/6 | — |
| Scenario 2 | Multi Room (2 rooms × 2 adults) | ⏳ Pending | 0/6 | — |

> **Note**: This report will be updated with actual test results after running `TestCFR_Certification`.

---

## 1. Integration Scope

- **Supplier API**: CFR Travel Accommodation Shop API v1
- **Base URL**: `https://api.gekko-technology.com/accommodations/shop-api`
- **Authentication**: HTTP Basic Auth
- **Supported Flow**: Availability → Prebook → Book → BookingDetails → Cancel

### API Endpoints Used

| Step | Supplier API | Method | Endpoint |
|------|-------------|--------|----------|
| 1. Search | Availability | POST | `/v1/availability` |
| 2. Room Details | Availability (rates) | POST | `/v1/availability` |
| 3. Prebook | Prebook | POST | `/v1/prebook` |
| 4. Book | Book | POST | `/v1/book` |
| 5. Booking Details | BookingDetails | GET | `/v1/bookingDetails` |
| 6. Cancel | Cancel | POST | `/v1/cancel` |

---

## 2. Test Environment

- **API Environment**: UAT (gekko-technology.com)
- **Test Hotels**: Infinite test hotels (`FR308898`, `FR619961`, `FR619962`, `FR619963`, `FR619964`)
- **Currency**: USD
- **Destination**: France (FR)
- **Check-in**: 30 days from test date
- **Check-out**: 32 days from test date

---

## 3. Scenario 1: Single Room (1 adult)

### Test Configuration
- **Occupancy**: 1 room × 1 adult
- **Nationality**: FR
- **Guest**: Jean Dupont
- **Result**: ⏳ Pending

### Complete API Flow Logs

#### Step 1: Availability (HotelList)
- **Endpoint**: `POST /v1/availability`
- **Request Log**: [s1_rq_availability.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rq_availability.json)
- **Response Log**: [s1_rs_availability.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rs_availability.json)

#### Step 2: Availability Rates (HotelRates)
- **Endpoint**: `POST /v1/availability`
- **Request Log**: [s1_rq_availability_rates.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rq_availability_rates.json)
- **Response Log**: [s1_rs_availability_rates.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rs_availability_rates.json)

#### Step 3: Prebook (CheckAvail)
- **Endpoint**: `POST /v1/prebook`
- **Request Log**: [s1_rq_prebook.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rq_prebook.json)
- **Response Log**: [s1_rs_prebook.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rs_prebook.json)

#### Step 4: Book
- **Endpoint**: `POST /v1/book`
- **Supplier Reference**: _(to be filled)_
- **Platform Reference**: _(to be filled)_
- **Request Log**: [s1_rq_book.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rq_book.json)
- **Response Log**: [s1_rs_book.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rs_book.json)

#### Step 5: Booking Details (QueryOrder)
- **Endpoint**: `GET /v1/bookingDetails`
- **Request Log**: [s1_rq_bookingDetails.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rq_bookingDetails.json)
- **Response Log**: [s1_rs_bookingDetails.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rs_bookingDetails.json)

#### Step 6: Cancel
- **Endpoint**: `POST /v1/cancel`
- **Cancellation Status**: _(to be filled)_
- **Request Log**: [s1_rq_cancel.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rq_cancel.json)
- **Response Log**: [s1_rs_cancel.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario1/s1_rs_cancel.json)

---

## 4. Scenario 2: Multi Room (2 rooms × 2 adults)

### Test Configuration
- **Occupancy**: 2 rooms × 2 adults each
- **Nationality**: FR
- **Guests**: Pierre Martin, Marie Martin (Room 1); Luc Bernard, Sophie Bernard (Room 2)
- **Result**: ⏳ Pending

### Complete API Flow Logs

#### Step 1: Availability (HotelList)
- **Endpoint**: `POST /v1/availability`
- **Request Log**: [s2_rq_availability.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rq_availability.json)
- **Response Log**: [s2_rs_availability.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rs_availability.json)

#### Step 2: Availability Rates (HotelRates)
- **Endpoint**: `POST /v1/availability`
- **Request Log**: [s2_rq_availability_rates.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rq_availability_rates.json)
- **Response Log**: [s2_rs_availability_rates.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rs_availability_rates.json)

#### Step 3: Prebook (CheckAvail)
- **Endpoint**: `POST /v1/prebook`
- **Request Log**: [s2_rq_prebook.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rq_prebook.json)
- **Response Log**: [s2_rs_prebook.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rs_prebook.json)

#### Step 4: Book
- **Endpoint**: `POST /v1/book`
- **Supplier Reference**: _(to be filled)_
- **Platform Reference**: _(to be filled)_
- **Request Log**: [s2_rq_book.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rq_book.json)
- **Response Log**: [s2_rs_book.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rs_book.json)

#### Step 5: Booking Details (QueryOrder)
- **Endpoint**: `GET /v1/bookingDetails`
- **Request Log**: [s2_rq_bookingDetails.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rq_bookingDetails.json)
- **Response Log**: [s2_rs_bookingDetails.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rs_bookingDetails.json)

#### Step 6: Cancel
- **Endpoint**: `POST /v1/cancel`
- **Cancellation Status**: _(to be filled)_
- **Request Log**: [s2_rq_cancel.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rq_cancel.json)
- **Response Log**: [s2_rs_cancel.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/cfr/logs/scenario2/s2_rs_cancel.json)

---

## 5. Log File Format

Each public log file contains either the sanitised **request** or **response** from a single API call.

### Request File (`*_rq_*.json`)
```json
{
  "timestamp": "2026-01-01T12:00:00Z",
  "supplier": "CFRTravel",
  "apiName": "availability",
  "url": "https://api.gekko-technology.com/accommodations/shop-api/v1/availability",
  "method": "POST",
  "headers": { "Content-Type": ["application/json"], "Authorization": "***REDACTED***" },
  "requestBody": { ... }
}
```

### Response File (`*_rs_*.json`)
```json
{
  "timestamp": "2026-01-01T12:00:00Z",
  "supplier": "CFRTravel",
  "apiName": "availability",
  "statusCode": 200,
  "headers": { ... },
  "body": { ... },
  "contentType": "application/json",
  "duration": 1234567000
}
```

---

## 6. Data Processing Statement

- All request/response payloads are complete and unmodified (business data).
- Authentication headers (`Authorization`, `X-Caller`) have been redacted.
- No user personal data is present in test scenarios (test names and emails used).
- Internal identifiers (`logId`, `curlCommand`) have been removed.
- Logs are generated automatically by our middleware logging layer and exported via `TestCFR_Certification`.

---

## 7. Contact Information

For questions or clarifications regarding this submission:
- **Technical Support**: techsupport1@ttdbooking.com
- **Integration Team**: Available via GitHub issue comments

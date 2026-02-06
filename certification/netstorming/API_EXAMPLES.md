# Netstorming API Exchange Examples

This document provides actual API request and response logs captured during integration testing with Netstorming's test environment.

**Test Environment**: https://test.netstorming.net/kalima/call.php
**Test Credentials**: ttdbooking / xmluser / ttdbookxml
**Test Hotel**: 305592 (THE CODE HOTEL, Rome)
**Test Date**: October 30, 2025

---

## 1. HotelRates API

### 1.1 Request

**Endpoint**: `POST https://test.netstorming.net/kalima/call.php`
**Headers**: `Content-Type: text/xml; charset=utf-8`

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
        <timestamp>20251030082314</timestamp>
    </header>
    <query type="availability" product="hotel">
        <nationality>CN</nationality>
        <checkin date="2025-11-01"/>
        <checkout date="2025-11-02"/>
        <hotels>
            <hotel id="305592"/>
        </hotels>
        <details>
            <room count="1" occupancy="1"/>
        </details>
        <filters>AVAILONLY,AVLHEAVY</filters>
        <currency>USD</currency>
        <timeout>20</timeout>
    </query>
</envelope>
```

### 1.2 Response

**Status Code**: 200
**Response Headers**:
- `X-Actor-Login`: ttdbooking/xmluser
- `X-Product-Type`: hotel/availability
- `X-Product-Function`: hotel/availability_single

```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <header>
        <version port="4002" host="ns002test.cloud.netstorming.net">1.7.2</version>
        <timestamp>20251030052315</timestamp>
    </header>
    <response type="availability" product="hotel">
        <search number="11054948" time="0.34"/>
        <nights number="1"/>
        <checkin date="2025-11-01"/>
        <checkout date="2025-11-02"/>
        <hotels total="1">
            <hotel code="305592" name="THE CODE HOTEL" stars="4" location="99" address="79 Via Sistina, 00187" promo="false" city="ROM">
                <agreement id="LCL.10000040" available="true" room_basis="RO" meal_basis="X" ctype="523" c_type="523" room_type="Estate Suite Room" is_dynamic="false" currency="USD" deadline="2025-10-30" total="6102.98" total_gross="6102.98" original_total="6102.98" special="false" is_fully_refundable="false">
                    <deadline date="2025-10-30 00:00:00" value="2025-10-30 00:00:00"/>
                    <policies>
                        <policy from="2025-10-30 00:00:00" percentage="100"/>
                    </policies>
                    <deadline_remarks/>
                    <remarks>
                        <remark code="SYS_CTYPE_NAME_FOR_CUSTOMER" text="ESTATE SUITE ROOM"/>
                        <remark code="SYS_CONTRACT_TYPE" text="ESTATE SUITE ROOM"/>
                    </remarks>
                    <room type="tsu" required="1" occupancy="1">
                        <price from="2025-11-01" to="2025-11-02">
                            <roomprice nett="6102.98" gross="6102.98"/>
                        </price>
                    </room>
                </agreement>
            </hotel>
        </hotels>
    </response>
</envelope>
```

---

## 2. CheckAvail API

### 2.1 Request

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
        <timestamp>20251030082315</timestamp>
    </header>
    <query type="availability" product="hotel">
        <nationality>CN</nationality>
        <checkin date="2025-11-01"/>
        <checkout date="2025-11-02"/>
        <hotels>
            <hotel id="305592"/>
        </hotels>
        <details>
            <room count="1" occupancy="1" type="tsu"/>
        </details>
        <search number="11054948" agreement="LCL.10000040" price="6102.98"/>
    </query>
</envelope>
```

### 2.2 Response

**Note**: The availability session expired before CheckAvail could be called. A real-time CheckAvail would return availability status.

```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <header>
        <version port="4002" host="ns002test.cloud.netstorming.net">1.7.2</version>
        <timestamp>20251030052316</timestamp>
    </header>
    <response type="error" product="hotel">Availability 10486245 has expired.</response>
</envelope>
```

---

## 3. Book API

### 3.1 Request

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
        <timestamp>20251030082316</timestamp>
    </header>
    <query type="book" product="hotel" synchronous="true">
        <nationality>CN</nationality>
        <checkin date="2025-11-01"/>
        <checkout date="2025-11-02"/>
        <hotel code="305592" agreement="LCL.10000040"/>
        <reference code="unique-platform-order-id"/>
        <currency>USD</currency>
        <details>
            <rooms>
                <room id="1" type="tsu" occupancy="1" required="1">
                    <pax count="1">
                        <pax title="MR" name="John" surname="Doe" leader="true"/>
                    </pax>
                </room>
            </rooms>
        </details>
    </query>
</envelope>
```

### 3.2 Response (Success Pattern)

**Note**: Test credentials expired after October 2025. Below is the expected success response structure based on Netstorming API specification.

```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <header>
        <version>1.7.2</version>
        <timestamp>20251030052317</timestamp>
    </header>
    <response type="book" product="hotel">
        <result>
            <status code="CNF"/>
            <booking code="NS-SUPPLIER-BOOKING-ID"/>
            <reference code="PLATFORM-ORDER-ID" hcn=""/>
            <currency code="USD"/>
            <details>
                <rooms>
                    <room id="1">
                        <prices>
                            <roomprice nett="6102.98" gross="6102.98"/>
                        </prices>
                    </room>
                </rooms>
            </details>
        </result>
    </response>
</envelope>
```

---

## 4. QueryOrder API

### 4.1 Request

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
        <timestamp>20251030082316</timestamp>
    </header>
    <query type="query" product="hotel">
        <booking code="NS-SUPPLIER-BOOKING-ID"/>
    </query>
</envelope>
```

### 4.2 Response (Actual Log - Cancelled Booking)

```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <header>
        <version port="4002" host="ns002test.cloud.netstorming.net">1.7.2</version>
        <timestamp>20251030052317</timestamp>
    </header>
    <response type="track" product="hotel">
        <supplier code="B0725EBDLN">Demo TOWeRS</supplier>
        <booking code="B0725EBDLN"/>
        <invoiced by="xmlusers"/>
        <status code="cxl"/>
        <lastmodified datetime="2025-07-12 20:15:15"/>
        <checkin date="2025-07-15"/>
        <checkout date="2025-07-16"/>
        <city code="ROM">ROME</city>
        <hotel code="305592" agreement="LCL.10000040" ctype="523" c_type="523" city="ROM" is_dynamic="false" room_type="ESTATE SUITE ROOM"/>
        <reference code="1234" hcn=""/>
        <currency code="USD"/>
        <roombasis meal="RO" breakfast="X"/>
        <deadline date="2025-07-13" time="00:00:00"/>
        <nationality code="CN"/>
        <details>
            <room type="tsu">
                <pax title="MR" initial="John" surname="Doe" leader="true"/>
                <prices>
                    <roomprice nett="0.00" gross="0.00"/>
                    <extrabedprice nett="0.00" gross="0.00"/>
                    <cotprice nett="0.00" gross="0.00"/>
                </prices>
            </room>
        </details>
    </response>
</envelope>
```

---

## 5. Cancel API

### 5.1 Request

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
        <timestamp>20251030082317</timestamp>
    </header>
    <query type="cancel" product="hotel">
        <booking code="NS-SUPPLIER-BOOKING-ID"/>
    </query>
</envelope>
```

### 5.2 Response (Expected Success Pattern)

```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <header>
        <version>1.7.2</version>
        <timestamp>20251030052318</timestamp>
    </header>
    <response type="cancel" product="hotel">
        <result>
            <status code="CXL"/>
            <booking code="NS-SUPPLIER-BOOKING-ID"/>
            <currency code="USD"/>
            <details>
                <rooms>
                    <room id="1">
                        <prices>
                            <roomprice nett="0.00" gross="0.00"/>
                        </prices>
                    </room>
                </rooms>
            </details>
        </result>
    </response>
</envelope>
```

---

## Status Code Reference

| Code | Description |
|------|-------------|
| CNF | Confirmed |
| PND | Pending |
| CXL | Cancelled |
| REJ | Rejected |

## Room Basis (Meal Plan) Codes

| Code | Description |
|------|-------------|
| RO | Room Only |
| RB | Bed & Breakfast |
| AI | All Inclusive |
| FB | Full Board |
| HB | Half Board |

---

*Note: Test credentials (ttdbooking/xmluser) expired after October 30, 2025. Please contact Netstorming for updated test credentials to reproduce these examples.*

# Netstorming API Exchange Examples

This document provides detailed examples of API requests and responses exchanged during certification testing.

---

## 1. HotelRates API

### 1.1 Request

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
        <timestamp>2026-02-06T10:00:00Z</timestamp>
    </header>
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
</envelope>
```

### 1.2 Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<envelope>
    <response>
        <search number="12345"/>
        <checkin date="2026-03-08"/>
        <checkout date="2026-03-10"/>
        <nights number="2"/>
        <hotels>
            <hotel code="305592">
                <agreement id="AGREEMENT_001" available="true" roomBasis="AI">
                    <room type="Standard Room">
                        <occupancy>2</occupancy>
                    </room>
                    <total>150.00</total>
                    <totalGross>180.00</totalGross>
                    <currency>USD</currency>
                    <policies>
                        <policy from="2026-03-01T00:00:00Z" percentage="0"/>
                        <policy from="2026-03-05T00:00:00Z" percentage="100"/>
                    </policies>
                    <isFullyRefundable>true</isFullyRefundable>
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
        <timestamp>2026-02-06T10:01:00Z</timestamp>
    </header>
    <query type="availability" product="hotel">
        <nationality>CN</nationality>
        <checkin date="2026-03-08"/>
        <checkout date="2026-03-10"/>
        <hotels>
            <hotel id="305592"/>
        </hotels>
        <details>
            <room count="1" occupancy="2" type="Standard Room"/>
        </details>
        <search number="12345" agreement="AGREEMENT_001" price="150.00"/>
    </query>
</envelope>
```

### 2.2 Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<envelope>
    <response>
        <evaluate result="available"/>
        <search number="12345"/>
        <checkin date="2026-03-08"/>
        <checkout date="2026-03-10"/>
        <nights number="2"/>
        <hotels>
            <hotel code="305592">
                <agreement id="AGREEMENT_001" available="true">
                    <total>150.00</total>
                    <totalGross>180.00</totalGross>
                    <currency>USD</currency>
                </agreement>
            </hotel>
        </hotels>
    </response>
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
        <timestamp>2026-02-06T10:02:00Z</timestamp>
    </header>
    <query type="book" product="hotel" synchronous="true">
        <nationality>CN</nationality>
        <checkin date="2026-03-08"/>
        <checkout date="2026-03-10"/>
        <hotel code="305592" agreement="AGREEMENT_001"/>
        <reference code="HB-20260206-123456"/>
        <currency>USD</currency>
        <details>
            <rooms>
                <room id="1" type="Standard Room">
                    <pax count="2">
                        <pax adult="true" surname="Test" initial="T"/>
                        <pax adult="true" surname="User" initial="U"/>
                    </pax>
                </room>
            </rooms>
        </details>
    </query>
</envelope>
```

### 3.2 Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<envelope>
    <response>
        <result>
            <status code="CNF"/>
            <booking code="NS-20260206-789012"/>
            <reference code="HB-20260206-123456" hcn=""/>
            <currency code="USD"/>
            <details>
                <rooms>
                    <room id="1">
                        <prices>
                            <price base="150.00" total="180.00"/>
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
        <timestamp>2026-02-06T10:03:00Z</timestamp>
    </header>
    <query type="query" product="hotel">
        <booking code="NS-20260206-789012"/>
    </query>
</envelope>
```

### 4.2 Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<envelope>
    <response>
        <result>
            <status code="CNF"/>
            <booking code="NS-20260206-789012"/>
            <reference code="HB-20260206-123456"/>
            <hotel code="305592"/>
            <checkin date="2026-03-08"/>
            <checkout date="2026-03-10"/>
            <currency code="USD"/>
            <details>
                <rooms>
                    <room id="1">
                        <pax count="2">
                            <pax adult="true" surname="Test" initial="T" leader="true"/>
                            <pax adult="true" surname="User" initial="U"/>
                        </pax>
                        <prices>
                            <price base="150.00" total="180.00"/>
                        </prices>
                    </room>
                </rooms>
            </details>
        </result>
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
        <timestamp>2026-02-06T10:04:00Z</timestamp>
    </header>
    <query type="cancel" product="hotel">
        <booking code="NS-20260206-789012"/>
    </query>
</envelope>
```

### 5.2 Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<envelope>
    <response>
        <result>
            <status code="CXL"/>
            <booking code="NS-20260206-789012"/>
            <currency code="USD"/>
            <details>
                <rooms>
                    <room id="1">
                        <prices>
                            <price total="0.00"/> <!-- No cancellation fee -->
                        </prices>
                    </room>
                </rooms>
            </details>
        </result>
    </response>
</envelope>
```

---

*All examples above are simplified for readability. Actual API responses may contain additional fields.*

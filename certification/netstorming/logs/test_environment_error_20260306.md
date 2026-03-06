# Netstorming Test Environment Error Logs

**Date:** 2026-03-06
**Test Hotel:** 305592
**Test Dates:** 2026-04-05 to 2026-04-06
**Environment:** test.netstorming.net

---

## Issue Summary

The test environment is returning database errors for CheckAvail requests,
which causes Book requests to fail with "Missing hotel agreement".

---

## 1. CheckAvail Request

**Endpoint:** `https://test.netstorming.net/kalima/call.php`
**Method:** POST
**Timestamp:** 2026-03-06T10:12:25Z

**Request XML:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
  <header>
    <version>1.7.1</version>
    <timestamp>20260306101225</timestamp>
    <actor>ttdbooking</actor>
    <user>xmluser</user>
  </header>
  <query type="availability" product="hotel">
    <nationality>CN</nationality>
    <checkin>
      <date>2026-04-05</date>
    </checkin>
    <checkout>
      <date>2026-04-06</date>
    </checkout>
    <hotels>
      <hotel id="305592"/>
    </hotels>
    <details>
      <room occupancy="2" required="1"/>
    </details>
  </query>
</envelope>
```

**Response XML:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
  <header>
    <version port="4002" host="ns002test.cloud.netstorming.net">1.7.2</version>
    <timestamp>20260306071226</timestamp>
  </header>
  <response type="error" product="hotel">
    You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '' at line 1
  </response>
</envelope>
```

**Response Headers:**
```
X-Product-Type: hotel/error
X-Product-Function: hotel/availability_single
Content-Type: text/xml;charset=UTF-8
```

---

## 2. Book Request (Following Failed CheckAvail)

**Endpoint:** `https://test.netstorming.net/kalima/call.php`
**Method:** POST
**Timestamp:** 2026-03-06T10:12:27Z

**Request XML:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
  <header>
    <version>1.7.1</version>
    <timestamp>20260306101227</timestamp>
    <actor>ttdbooking</actor>
    <user>xmluser</user>
  </header>
  <query type="book" product="hotel">
    <synchronous value="true"/>
    <nationality>CN</nationality>
    <checkin>
      <date>2026-04-05</date>
    </checkin>
    <checkout>
      <date>2026-04-06</date>
    </checkout>
    <availonly value="true"/>
    <hotel code="305592" agreement=""/>
    <reference code="999903"/>
    <details>
      <rooms>
        <room occupancy="2" required="1">
          <paxs>
            <pax title="MR" name="Guest" surname="One" leader="true"/>
            <pax title="MR" name="Guest" surname="Two"/>
          </paxs>
        </room>
      </rooms>
    </details>
  </query>
</envelope>
```

**Response XML:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
  <header>
    <version port="4002" host="ns002test.cloud.netstorming.net">1.7.2</version>
    <timestamp>20260306071227</timestamp>
  </header>
  <response type="error" product="hotel">
    Missing hotel agreement
  </response>
</envelope>
```

**Response Headers:**
```
X-Product-Type: hotel/error
X-Product-Function: hotel/book
Content-Type: text/xml;charset=UTF-8
```

---

## Analysis

1. **CheckAvail** returns a SQL syntax error from the test database
2. Because CheckAvail fails, the `agreement` parameter is not populated
3. **Book** fails with "Missing hotel agreement" (note: `agreement=""` in request)

This appears to be a backend database issue on `test.netstorming.net` (host: `ns002test.cloud.netstorming.net`).

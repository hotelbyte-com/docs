# HeyTrip Certification Submission

## Scope

- Supplier: **HeyTrip**
- Test date: **2026-03-15**
- Environment: **DEV (offline credential)**
- Scenario: **Single-room booking lifecycle**

## Scenario → Supplier API → Request Log

| Scenario Step | Supplier API Endpoint | Method | Full Request/Response Log |
|---|---|---|---|
| 1. Static catalog load | `/v3/api/Static/catalog/hotels` | GET | [01_Static_HotelsCatalog.json](https://github.com/hotelbyte-com/docs/raw/main/certification/heytrip/logs/2026-03-15_certification/01_Static_HotelsCatalog.json) |
| 2. Hotel search | `/v3/api/Search/search` | POST | [02_Search_Search.json](https://github.com/hotelbyte-com/docs/raw/main/certification/heytrip/logs/2026-03-15_certification/02_Search_Search.json) |
| 3. Price check | `/v3/api/Order/checkPrice` | POST | [03_Order_CheckPrice.json](https://github.com/hotelbyte-com/docs/raw/main/certification/heytrip/logs/2026-03-15_certification/03_Order_CheckPrice.json) |
| 4. Booking create | `/v3/api/Order/book` | POST | [04_Order_Book.json](https://github.com/hotelbyte-com/docs/raw/main/certification/heytrip/logs/2026-03-15_certification/04_Order_Book.json) |
| 5. Order status query | `/v3/api/Order/status` | POST | [05_Order_Status.json](https://github.com/hotelbyte-com/docs/raw/main/certification/heytrip/logs/2026-03-15_certification/05_Order_Status.json) |
| 6. Order cancel | `/v3/api/Order/cancel` | POST | [06_Order_Cancel.json](https://github.com/hotelbyte-com/docs/raw/main/certification/heytrip/logs/2026-03-15_certification/06_Order_Cancel.json) |

## Result

- Lifecycle result: **PASS**
- Booking reference used in query/cancel: `V2603150402330402336693024`
- SearchOrders observation: upstream range lookup did not return the newly created order in this run.

## Data Safety

- Full supplier request/response payloads are preserved.
- Sensitive auth fields are masked in public artifacts: `X-Api-Key`, `X-Client-Id`, `Sign`.

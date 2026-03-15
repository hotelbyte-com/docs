# HeyTrip Certification Submission

## Scope

- Supplier: **HeyTrip**
- Verification set: **3 end-to-end booking scenarios**
- Authentication model: **API key header (`X-Api-Key`) + signed request headers (`X-Client-Id`, `Timestamp`, `Sign`)**
- Supplier hosts covered:
  - `https://smbizstatic.heytripgo.com`
  - `https://smbizsearch.heytripgo.com`
  - `https://smbizorder.heytripgo.com`

We have completed validation testing and attached full request/response logs for your review.

## Scenario 1 (Booking Ref: `V2603150333070333079739120`)

| Step | Supplier API Endpoint | Method | Full Request/Response Log |
|---|---|---|---|
| Static catalog | `/v3/api/Static/catalog/hotels` | GET | [01_Static_HotelsCatalog.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario1_2026-03-15_0333/01_Static_HotelsCatalog.json) |
| Search | `/v3/api/Search/search` | POST | [02_Search_Search.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario1_2026-03-15_0333/02_Search_Search.json) |
| Check price | `/v3/api/Order/checkPrice` | POST | [03_Order_CheckPrice.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario1_2026-03-15_0333/03_Order_CheckPrice.json) |
| Book | `/v3/api/Order/book` | POST | [04_Order_Book.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario1_2026-03-15_0333/04_Order_Book.json) |
| Query order | `/v3/api/Order/status` | POST | [05_Order_Status.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario1_2026-03-15_0333/05_Order_Status.json) |
| Cancel order | `/v3/api/Order/cancel` | POST | [06_Order_Cancel.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario1_2026-03-15_0333/06_Order_Cancel.json) |

## Scenario 2 (Booking Ref: `V2603150402330402336693024`)

| Step | Supplier API Endpoint | Method | Full Request/Response Log |
|---|---|---|---|
| Static catalog | `/v3/api/Static/catalog/hotels` | GET | [01_Static_HotelsCatalog.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario2_2026-03-15_0402/01_Static_HotelsCatalog.json) |
| Search | `/v3/api/Search/search` | POST | [02_Search_Search.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario2_2026-03-15_0402/02_Search_Search.json) |
| Check price | `/v3/api/Order/checkPrice` | POST | [03_Order_CheckPrice.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario2_2026-03-15_0402/03_Order_CheckPrice.json) |
| Book | `/v3/api/Order/book` | POST | [04_Order_Book.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario2_2026-03-15_0402/04_Order_Book.json) |
| Query order | `/v3/api/Order/status` | POST | [05_Order_Status.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario2_2026-03-15_0402/05_Order_Status.json) |
| Cancel order | `/v3/api/Order/cancel` | POST | [06_Order_Cancel.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario2_2026-03-15_0402/06_Order_Cancel.json) |

## Scenario 3 (Booking Ref: `V2602140150480150485153807`)

| Step | Supplier API Endpoint | Method | Full Request/Response Log |
|---|---|---|---|
| Static catalog | `/v3/api/Static/catalog/hotels` | GET | [01_Static_HotelsCatalog.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario3_2026-02-14_0150/01_Static_HotelsCatalog.json) |
| Search | `/v3/api/Search/search` | POST | [02_Search_Search.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario3_2026-02-14_0150/02_Search_Search.json) |
| Check price | `/v3/api/Order/checkPrice` | POST | [03_Order_CheckPrice.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario3_2026-02-14_0150/03_Order_CheckPrice.json) |
| Book | `/v3/api/Order/book` | POST | [04_Order_Book.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario3_2026-02-14_0150/04_Order_Book.json) |
| Query order | `/v3/api/Order/status` | POST | [05_Order_Status.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario3_2026-02-14_0150/05_Order_Status.json) |
| Cancel order | `/v3/api/Order/cancel` | POST | [06_Order_Cancel.json](https://raw.githubusercontent.com/hotelbyte-com/docs/main/certification/heytrip/logs/scenario3_2026-02-14_0150/06_Order_Cancel.json) |

Please review and confirm.

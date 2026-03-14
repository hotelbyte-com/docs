# HeyTrip Certification Logs (2026-03-15)

## Scenario

- Single-room booking lifecycle on HeyTrip test environment routing.
- Currency: USD
- Check-in/check-out: 2026-04-14 / 2026-04-16

## Scenario → Supplier API → Log

1. Static catalog  
   - API: `GET /v3/api/Static/catalog/hotels`  
   - Log: [01_Static_HotelsCatalog.json](01_Static_HotelsCatalog.json)
2. Search rates  
   - API: `POST /v3/api/Search/search`  
   - Log: [02_Search_Search.json](02_Search_Search.json)
3. Check price  
   - API: `POST /v3/api/Order/checkPrice`  
   - Log: [03_Order_CheckPrice.json](03_Order_CheckPrice.json)
4. Book  
   - API: `POST /v3/api/Order/book`  
   - Log: [04_Order_Book.json](04_Order_Book.json)
5. Query status  
   - API: `POST /v3/api/Order/status`  
   - Log: [05_Order_Status.json](05_Order_Status.json)
6. Cancel  
   - API: `POST /v3/api/Order/cancel`  
   - Log: [06_Order_Cancel.json](06_Order_Cancel.json)

## Data Handling

- Files keep full request/response payloads for certification traceability.
- Authentication-sensitive fields are masked for public sharing.

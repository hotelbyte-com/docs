# Scenario 1

- Booking reference: `V2603150333070333079739120`
- Supplier API flow:
  1. `GET /v3/api/Static/catalog/hotels`
  2. `POST /v3/api/Search/search`
  3. `POST /v3/api/Order/checkPrice`
  4. `POST /v3/api/Order/book`
  5. `POST /v3/api/Order/status`
  6. `POST /v3/api/Order/cancel`

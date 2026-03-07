# Scenario4 Fix: Occupancy Mismatch with Children

## Problem Description

When booking a TRP (Triple Room) with 2 adults + 1 child (age 5), the Book API was failing with:
```
Passengers does not match for this room
```

## Root Cause

**Netstorming's Occupancy Behavior**: For TRP rooms with children, Netstorming returns `occupancy="3"` in the HotelRates response, meaning the room capacity already includes the child.

**Previous Implementation Bug**: Our code was:
1. Not saving Netstorming's returned occupancy value
2. Calculating occupancy from adult count only (`occupancy="2"`)
3. Setting `extrabed="true" age="5"` for the child

This mismatch caused Netstorming to reject the booking:
- **Request sent**: `<room type="trp" occupancy="2" extrabed="true" age="5">`
- **Netstorming returned**: `<room type="trp" occupancy="3">` (child already included)
- **Error**: "Passengers does not match for this room"

## Solution Implemented

### 1. Added SessionOccupancyKey (model/session.go)
```go
const SessionOccupancyKey = SupplierPrefix + ":occupancy"
```

### 2. Save Netstorming's Occupancy (converter.go)
```go
SessionParams: map[string]any{
    model.SessionNumberKey:    number,
    model.SessionAgreementKey: agreement.ID,
    model.SessionPriceKey:     fmt.Sprintf("%f", agreement.Total),
    model.SessionRoomTypeKey:  agreement.Room[0].Type,
    model.SessionOccupancyKey: agreement.Room[0].Occupancy, // NEW!
},
```

### 3. Use Saved Occupancy in CheckAvail/Book
- CheckAvail: Use saved occupancy instead of calculated adult count
- Book: Use saved occupancy and **only set extrabed when total guests > saved occupancy**

### 4. Conditional Extrabed Logic (room_converter.go)
```go
// If Netstorming's occupancy already includes the child, don't set extrabed
useExtrabed := int64(totalPax) > occupancy
```

## Verification

### Before Fix
```xml
<room type="trp" occupancy="2" required="1" age="5" extrabed="true">
```
Result: **HTTP 500** - "Passengers does not match for this room"

### After Fix
```xml
<room type="trp" occupancy="3" required="1">
```
Result: **HTTP 200** - Booking successful, SupplierRef=B0326J471D

## Test Results

**Scenario4_DoubleWithChild_Age5**: ✅ PASS (5/5 steps)
- HotelRates ✅
- CheckAvail ✅
- Book ✅ (SupplierRef=B0326J471D)
- QueryOrder ✅
- Cancel ✅

## Files Modified

1. `supplier/integration/netstorming/model/session.go` - Added SessionOccupancyKey
2. `supplier/integration/netstorming/converter.go` - Save Netstorming's occupancy
3. `supplier/integration/netstorming/serviceutil.go` - Use saved occupancy in genRoomDetail()
4. `supplier/integration/netstorming/check_avail.go` - Pass sessionOccupancy to genRoomDetail()
5. `supplier/integration/netstorming/book.go` - Pass sessionOccupancy to genRoomsFromBookReq()
6. `supplier/integration/netstorming/room_converter.go` - Conditional extrabed logic

## Important Notes

1. **Netstorming's occupancy includes children**: Always use Netstorming's returned occupancy, never calculate it yourself
2. **Extrabed is conditional**: Only set `extrabed="true"` when total guests > Netstorming's returned occupancy
3. **Cot vs ExtraBed**: Use `Cot=true` for infants (age=0), `ExtraBed=true` for children (age 1-17)

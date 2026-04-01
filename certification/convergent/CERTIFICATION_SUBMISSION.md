# Convergent International Travel - Certification Re-Submission (Corrected)

Hi CIT Team,

We are submitting a corrected latest package for issue [#48](https://github.com/hotelbyte-com/docs/issues/48).

## Correction

You are right that earlier logs did not show the new Book fields.  
We reran certification on **April 1, 2026** and now provide fresh raw logs that include:
- `guests[].roomNo`
- `orderRoomDetail`

## Rerun Scenario Results

| Scenario | Inputs | Order ID | Result |
|---|---|---:|---|
| Case 1 | 1 room, 1 night, 2 adults | 6345929 | PASS |
| Case 2 | 2 rooms, 3 nights, 2 adults | 6345930 | PASS |

## Delivery References

- Commit `4ce313f4` (March 31, 2026): add Book fields `roomNo`, `orderRoomDetail`
- Commit `2eae11aa` (March 31, 2026): includes the above change

## Evidence

- Summary: `CIT_RECERTIFICATION_SUMMARY.md`
- Sanitized rerun logs:
  - `logs/cit_recertification_sanitized_20260401_uat_rerun/Case1/Book_20260401_160512_sanitized.json`
  - `logs/cit_recertification_sanitized_20260401_uat_rerun/Case2/Book_20260401_160517_sanitized.json`
  - full chain logs in same directory

Please verify the two new Book fields from these rerun logs.

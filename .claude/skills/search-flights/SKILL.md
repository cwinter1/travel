---
name: search-flights
description: Live-price flights for the Winter family for a given destination and date (or date range), using the family's real passenger mix and home airport. Use when Chris just wants flight prices/options — not a full trip budget — e.g. "what do flights to X cost in April", "compare flight prices across these three weeks", "find a nonstop to Y". For a full trip (flights + lodging + activities + budget write-up), use /trip-search instead.
---

# Search flights

Narrow, flights-only version of the family's trip research process. Use this
when the ask is just "what would flights cost," not a full trip plan.

## Before starting

Read `Family.md` (repo root) first. It has the passenger roster and
birthdates (needed to compute the correct fare-class mix), the home airport
(Tel Aviv / TLV — assume unless told otherwise), and the currency rule
(always convert and present final prices in ILS).

## Inputs to collect from Chris

- Destination (or a short list of candidate destinations)
- Departure date, or a date range / list of candidate weeks
- Trip length (to derive the return date) if not given directly
- Any hard constraints: nonstop only, specific airline preference, cabin class, etc.

## Steps

1. **Compute each traveler's age as of the departure date**, not today.
   Fare classes can key off age (e.g. Kiwi.com treats 12+ as "adult") — get
   the passenger mix right before searching, since it changes the price.
2. **Search live prices.** Use the Kiwi.com connector first (confirmed
   working — see `Family.md`'s "Tools confirmed working" section). If it's
   unavailable or doesn't cover the route, fall back to Expedia or
   lastminute.com.
3. **If multiple candidate dates/weeks were given**, search each one and
   present them side by side rather than picking one silently — let Chris
   see the comparison.
4. **Report each option** with: airline, flight numbers, outbound and return
   times, nonstop vs. connections, and total price for the full family —
   converted to ILS regardless of the currency the tool returned.
5. **Label the check date** ("live-checked [date]") since fares move — don't
   let a price go stale without that caveat.
6. **Do not write a trip file.** This skill only reports flight prices in
   the conversation. If Chris wants the result saved, that's `/trip-search`'s
   job (or ask before writing anything to `trips/`).

## Out of scope

- Does not search or price lodging, activities, transfers, or meals — flights only
- Does not book or hold anything — pricing/research only
- Does not write to `trips/` or any file — output stays in the conversation
  unless Chris explicitly asks to save it
- Does not use points/miles/award-flight search — cash fares only, per `Family.md`
- Does not assume a budget — if Chris wants a budget check, that's part of
  `/trip-search`, not this skill

## Example usage

```
/search-flights
/search-flights TLV to Larnaca, April 10-17 2027
/search-flights compare Feb 1-7 vs Feb 8-14 2027 to Sofia
```

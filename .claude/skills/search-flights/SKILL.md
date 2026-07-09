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

## When to buy (booking-window guidance)

If Chris asks *when* to book rather than *what it costs right now*, note
upfront that **no historical-fare or price-trend connector is available in
this environment** — Kiwi.com/Expedia/lastminute.com only return live,
current-day quotes. Any "best time to buy" answer is general published
aviation-industry research, not a historical data pull for the specific
route, and should be labeled that way rather than presented as a live
analysis.

**Researched (2026-07) and ruled out as a live source:** Amadeus's
Flight Price Analysis API (`/v1/analytics/itinerary-price-metrics`) is the
one purpose-built API that returns a real historical fare distribution
(quartiles) for a route — but its Self-Service developer portal is being
decommissioned July 17, 2026 and new registrations are already paused, so
it isn't viable to stand up. Kiwi.com's own API has no historical-price
endpoint (live search only). Other "flight data" APIs found in that search
(FlightLabs, FlightAPI.io, Aviationstack, Flightradar24) are primarily
flight tracking/schedule-status history, not fare-price history — don't
be misled by "historical" in their marketing. Re-check this landscape
periodically; a comparable self-serve fare-history API may become
available later.

**Practical substitute: build a route-specific price history manually.**
Since no live historical source exists, log every `/search-flights` result
for a given trip in a dated table (see the Bulgaria trip file for the
pattern) instead of relying on a single check. Suggested cadence: one
check now, one check monthly until ~120 days out, then weekly through the
sweet-spot window — enough points to see a real trend for that specific
route rather than guessing from general research.

General patterns to draw on:
- **Domestic / short-haul leisure**: book ~1–3 months out; prices often
  sweet-spot around 4–6 weeks before departure.
- **International leisure** (the family's typical case, flying out of TLV):
  book ~2–5 months out; sweet spot commonly cited around **90–120 days**
  before departure.
- **Peak season / school-holiday travel** (which most family trips are,
  being scheduled around school breaks): book earlier than the general
  sweet spot — 4–6+ months out — since peak-period seat maps fill and
  cheap fare buckets close faster.
- **Prices typically rise sharply inside ~21 days** of departure as airlines
  shift into last-minute/business-fare pricing.
- **Carrier mix matters.** Legacy carriers (e.g. El Al) often open booking
  further out (~10–11 months) with fares that drift gradually; low-cost
  carriers (e.g. Wizz Air) tend to release schedules later (~6–9 months
  out) with more volatile, demand-driven pricing. A round trip combining
  both (as the Bulgaria trip does) may not have a single unified "best
  window" — check each leg's typical release pattern.
- **Don't commit on a single guess.** Recommend Chris re-run `/search-flights`
  for the same route every few weeks starting near the sweet-spot window,
  and book when a check comes back meaningfully cheaper than the prior one
  — that's a real (if manual) substitute for historical trend data.

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

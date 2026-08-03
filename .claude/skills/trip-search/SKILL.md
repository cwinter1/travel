---
name: trip-search
description: Research and price a new family trip for the Winter family — live-prices flights and lodging across candidate weeks, itemizes the full trip budget (flights, lodging, activities, transfer, meals), and writes the result to trips/. Use when Chris asks to plan, research, or price a new family trip, vacation, or activity trip (e.g. "plan a trip to X", "find flights and hotels for Y", "research a ski trip in Z"), or to re-check/update an existing trip's prices.
---

# Trip search

Runs the family's standard trip-research process end to end: from a destination
and date range to a fully itemized, budget-checked trip file.

## Before starting

Read `Family.md` (repo root) in full before doing anything else. It holds the
family roster, birthdates, home airport, currency, and durable preferences
(self-catering, cash bookings only, beginner-level activity planning). Nothing
in this skill overrides it — if this file and `Family.md` conflict, `Family.md`
wins.

## Inputs to collect from Chris

Ask for whatever isn't already given:
- Destination(s) under consideration
- Date range (or a fixed window) and trip length
- Budget (ask fresh every time — do not carry over a previous trip's budget)
- Trip-specific must-haves (e.g. "3 days of ski lessons," "beginner-friendly")

## Steps

1. **Compute ages at the trip's start date**, not today, for every family
   member in `Family.md`. Age brackets (lift passes, ski-lesson tiers, flight
   fare classes, hotel child-occupancy rules) key off age *during travel* —
   a child can cross a bracket between now and departure. Flag any such
   crossing explicitly.
2. **If the date range spans multiple candidate weeks**, live-price flights
   and lodging for each week in parallel, using the real passenger mix from
   step 1. Use the Kiwi.com and Booking.com connectors (confirmed working —
   see `Family.md`'s "Tools confirmed working" section); try Expedia /
   lastminute.com as fallbacks if either is unavailable. Pick the cheapest
   combined week unless another week has a hard reason to win (birthday,
   snow/weather conditions, school holidays) — state the reason if so.
3. **Itemize the full budget**, not just flights and lodging: activity costs
   (lessons, passes, rentals), local transfer, and a meals/groceries estimate
   all belong in the same table, converted to ILS.
4. **Mark each number's source.** Live-quoted prices (via a connector, with
   the check date) are "live-checked." Anything else is a "published rate,
   not live-checked." Never blur the two — if no connector covers a category
   (e.g. a resort-specific lesson/lift-pass site), say so explicitly rather
   than presenting an estimate as a live quote.
5. **Compare the total to budget.** State clearly whether it's within budget
   or by how much it's over/under.
6. **If over budget, give concrete levers** to close the gap (fewer lesson
   days, skip rental gear already owned, tighter grocery budget, earlier
   booking for pre-sale pricing, etc.) — not just a restated shortfall.
7. **Write the trip to its own file** under `trips/` (create the directory if
   it doesn't exist), named `trips/<YYYY-MM>-<destination>.md`. Follow the
   structure of the existing Bulgaria trip file — trip summary, "why this
   week" comparison table (if applicable), flights, lodging, activity costs,
   transfer, meals, total-vs-budget table, ways to close the gap, and a
   "still to do" list for anything unresolved. Keep `Family.md` itself
   untouched — trip specifics never belong there.

## Out of scope

- Does not actually book or pay for anything — research and price only
- Does not use points/miles/award-flight optimization or credit-card travel
  hacking — cash bookings only, per `Family.md`
- Does not assume a budget from a prior trip — always ask fresh
- Does not overwrite an existing trip file's history — for a re-check/update,
  append a new dated price-check section rather than silently replacing old
  numbers
- Does not modify `Family.md`'s durable content (roster, preferences,
  process) — only reads it

## Example usage

```
/trip-search
/trip-search plan a 6-day trip to Cyprus in April 2027, budget 8000 ILS
```

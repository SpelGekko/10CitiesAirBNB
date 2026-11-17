# Europe's Airbnb rules: one ruler for ten cities

The analysis behind the video [One Airbnb Host Runs 583 Homes in Paris. I Checked All 10 Big Cities.](https://youtu.be/8Z9CpObP-GE)

Every big European city regulates Airbnb its own way. Amsterdam allows an entire home to be
rented to tourists 30 nights a year. Paris, London, Vienna and Berlin allow 90. Barcelona is
ending its tourist-flat licences in 2028, and Lisbon, Rome, Brussels and Prague have no night cap.
So the notebook measures **every listing in ten cities with one ruler** (Inside Airbnb, September
2026 snapshots, 299,434 listings) and then checks each city against its own rule.

## The ruler

- **Entire homes only** for nights and hosts: that's where the rules bite
- **Nights booked** = Inside Airbnb's estimate for the last 12 months (`estimated_occupancy_l365d`):
  reviews ÷ a 50% review rate × the average stay, capped at 70% of the year. Every result also has
  a **careful** version (70% review rate, 2-night stays) that comes out much lower
- **Short-stay homes** = entire homes that take stays under 31 nights; the night caps only apply
  to those
- **Active** = at least one review in the last 12 months

## What it found

| City | Cap (nights) | Median nights | Short-stay homes over own cap | Earnings made over the cap | Homes run by 10+ hosts | No review in 12 months |
|---|---|---|---|---|---|---|
| Amsterdam | 30 | **31** | 32% | 88% | 1% | 38% |
| Paris | 90 | 94 | 31% | 85% | 20% | 47% |
| London | 90 | 60 | 21% | 75% | 29% | **49%** |
| Vienna | 90 | 79 | 31% | 79% | 38% | 35% |
| Berlin | 90 | **176** | **52%** | **91%** | 18% | 44% |
| Barcelona | none | 162 | | | **56%** | 35% |
| Lisbon | none | 84 | | | 31% | 24% |
| Rome | none | 96 | | | 18% | 26% |
| Brussels | none | 126 | | | 12% | 24% |
| Prague | none | 120 | | | 44% | 20% |

Median nights are for active entire homes; "over own cap" is for short-stay entire homes.

More findings in the notebook:

- **Amsterdam is the odd one out.** It's the only city where the typical home is rented out about
  as many nights as the cap allows. With the careful estimate it's still last (14 nights, against
  89 in Barcelona).
- **Locals or pros?** Hosts with one home go over the cap far less often than hosts with several.
  London: 13% for single-home hosts vs 26–29% for hosts with more.
- **Who runs them?** One host runs 583 entire homes in Paris. Most big hosts are management
  companies, so "runs", not "owns". No host is named anywhere.
- **Where they are.** In Paris, the busiest square kilometre holds 3,494 entire homes. In Prague,
  7 squares hold half of the city's entire homes.
- **The 31-night trick.** Where short stays get hard, the month-long let takes over. 31% of entire
  homes in Barcelona and Berlin only take stays of 31 nights or more. In Paris, 7,549 homes (11%)
  are offered on a "mobility lease only".
- **The guest's view.** In every city, homes from single-home hosts are rated 4.9+ more often than
  homes from hosts with 10+: from about twice as often (Lisbon, Prague) to 4.5 times (Brussels).

## Running it

```
pip install -r requirements.txt
```

Open `europe_airbnb.ipynb` and run all cells. It has three parts:

1. **Download** the ten detailed listings files from Inside Airbnb (about 170 MB). Files that are
   already there are skipped. Inside Airbnb keeps the latest snapshots on its download page and
   moves older ones to an archive you have to ask for. If a link has stopped working, use the
   newest snapshot for that city: the numbers will shift a little, but the method stays the same.
2. **Prepare** one table with the ruler applied to every listing (about 15 seconds).
3. **Analyse**: every number and chart above.

Data quirks handled in the prepare step:

- Prices and earnings carry a "$" in every city but are in local currency: London (GBP) and
  Prague (CZK) are converted at ECB reference rates around the snapshot date.
- The licence field is empty for London, Vienna, Brussels and Prague (Inside Airbnb doesn't
  collect it there), so registration is only checked in the other six, and only for the right format.
- Berlin: about a third of entire homes show Airbnb's host block (name or company and the listing
  address) instead of a number. That's counted as its own class; the text itself is never shown.
- Barcelona: Catalan tourist licences (HUTB-…) and Spain's national numbers (tourist vs
  non-tourist / seasonal) are told apart. Paris: "mobility lease only" is its own class.

## Limits

Nights and earnings are estimated from reviews, not real bookings. Prices are the asking price on
the day the data was collected. There's one snapshot per city. Permits, planning permission and
second-home rules aren't in the data, so "over the cap" doesn't always mean breaking the rules.
Airbnb only. Ten cities show a pattern, not proof of what caused it.

## Data and credits

- Listings: [Inside Airbnb](https://insideairbnb.com), September 2026 snapshots, CC BY 4.0
- The rules: [Amsterdam](https://www.amsterdam.nl/en/housing/holiday-rentals/) ·
  [Paris](https://www.paris.fr/en/pages/furnished-vacation-rentals-rules-to-follow-34993) ·
  [London](https://www.london.gov.uk/programmes-strategies/housing-and-land/buying-and-owning-home/guidance-short-term-and-holiday-lets-london) ·
  [Vienna](https://www.wien.gv.at/wohnen/vermietung-wohnungen-tourismus) ·
  [Berlin](https://www.berlin.de/sen/wohnen/rechtliches/zweckentfremdungsverbot/) ·
  [Barcelona](https://www.barcelona.cat/infobarcelona/en/barcelona-will-not-renew-holiday-flat-permits_1414584.html) ·
  [Lisbon](https://www.lisboa.pt/en/themes/urban-planning/studies-plans-and-programs/studies-and-plans/local-accommodation)

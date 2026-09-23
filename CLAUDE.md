# CLAUDE.md

Two-page reference for Hei's Bangkok team offsite (Sep 2026). Hosted on GitHub Pages. Primary reader is Kenny (the boss), mostly on mobile.

Eight people on the trip. Three fly: Tisha, Angru and Jordi. Kenny, Mathew, Cecilia, Dayvin and Wilson are already in Bangkok on the LIXIL project budget. Core travel costs are per person x 3, because only three fly. Everything social, meals and activities, is per person x 8. Every total on both pages is written out by hand, there is no arithmetic in JS any more.

## Structure

Two pages, each a standalone file with inline CSS, HTML and JS. No build step, no package manager, no dependencies beyond Google Fonts and an Unsplash hero image.

- `index.html` is the final shared itinerary for all eight: timed Friday and Saturday schedules, Sunday chill day, and what to wear. No options or decisions left on it.
- `costing.html` is the cost record: what was actually booked for the three who fly, plus per-person estimates for the weekend social spend. It is static, there is no configurator and no JS beyond the shared scroll reveal.
- `og.jpg` is shared by both: a 1200x630 crop of the hero image, used as the social preview for link unfurls (Slack, WhatsApp, Teams).

Both pages use the same hero image, palette, type and section rhythm, and both carry "Internal" at the top right of the hero. All style rules below apply to both.

## Nav lives in both files, keep it in sync

Each page carries its own copy of the same nav: a `.nav` block above the hero with "Weekend" and "Costing". The current page gets `class="on"` (blue) and `aria-current="page"`. The `.nav` CSS block is duplicated in both files and marked with a "keep in sync" comment. Any change to the nav markup or CSS must be made in both, or the two pages drift.

## Head tags

Each page has its own `<title>`, description and og tags. Keep `og:title` and `og:description` in sync with `<title>` and `<meta name="description">` on each page.

- `index.html`: "Heitreat 01 · The weekend", `og:url` the site root. The description summarises the final plan (Friday rooftop, dinner and bars, Saturday cooking class, Pastel and a night out, Sunday chill day), so update it if the plan changes.
- `costing.html`: "Heitreat 01 · Costing", `og:url` ending in `/costing.html`. Its description covers booked flights and rooms plus weekend spend estimates.

`og:image` is an absolute URL (`https://jordialbanell.github.io/bkk-retreat/og.jpg`) on both, which is required, relative paths do not resolve for link previews. If the hero image or the site URL ever changes, regenerate `og.jpg` and update the absolute URL in both files.

## costing.html: a record, not a calculator

The page used to be a decision tool with an interactive configurator, a `FLIGHTS`/`ROOMS` data model, flight and hotel option tables and Option A/B scenario cards. Bookings are made, so all of that is gone. There are no ranges to recompute and no duplicated price source to keep in sync. What is left is two static tables:

- **Core cost**, booked and confirmed: Scoot return S$316 pp, Ibis Sukhumvit 24 S$303 per room for 3 nights Thu-Sun on flexible terms, core total S$619 pp and S$1,857 for three. The three columns are item, per person, for three. The per-person and for-three figures are written out, so if one moves, update the row and the `tr.total` line together.
- **Weekend spend**, estimates for all eight: Supanniga, Pastel, the Tingly cooking class (price at booking), Fri and Sat bar rounds (pay as you go), Sunday (own spend). The total row is labelled "Both dinners" and covers only the two priced rows, Supanniga S$42-55 plus Pastel S$60, so S$102-115 pp. The note under the table says the rest comes on top. When Tingly's price is known, add it to the total and relabel the row to say what it includes.

Flights are per person, the hotel is per room but there is one room each, so both sit in the per-person column. The row says "one room each" so the column heading stays honest.

`tr.total` is the heavier summary row, and `td.num`/`th.num` carry `padding-left` so two numeric columns cannot collide on a phone. The `.note-sm` paragraph under each table carries the caveats. The total row's figure is `white-space:nowrap` so a range like S$102-115 does not break at the hyphen on a phone.

## index.html: the itinerary

Sections in order: Friday, Saturday, Sunday, Dress. Friday and Saturday are timed schedules in `.plan`, time on the left, content on the right. "Travel times are estimates." sits once under the Friday heading.

Two row types inside a schedule:

- Venue entries are a plain `.plan > li`: `h3` (e.g. "Drinks · Aether rooftop"), optional `.desc`, `.price`, `.detail` note, then a `.link`.
- Transit legs and other one-line rows are `li.go` with a `p.go-text` ("Hotel to Tichuca · Grab, about 15 min"). They are compact and grey so the venues stand out. Shower and change on Saturday uses the same row.

Times use the `7:30-9:15pm` form, open-ended ones `11:45pm-late`, and after-midnight rows just say "Late". If a slot moves, check the transit rows either side still add up.

Sunday reuses `li.go` inside `ul.plan.groups`, where the left column is a category (Shopping, Massage, Sightseeing) in blue and the text is dark, not grey. The flights-back line is the lede above it. Dress is a normal `.plan` with the day in the left column and a `.desc`, closed by a `.note` saying only Tichuca and Sing Sing have confirmed codes. If a venue changes, recheck the dress copy, it names venues.

The Tingly cooking class shows "Price confirmed at booking". When the price is known, update the entry and the costing.html weekend table together.

## index.html: Saturday dinner

Pastel is the 7:45-10:00pm entry in the Saturday schedule, a Mediterranean rooftop that turns into a DJ party later. Budget is confirmed at S$60 pp capped, about S$480 for 8, which assumes the planned shared order and one signature cocktail each ordered centrally, with 10% service and 7% VAT included.

The planned order is a list inside a `<details class="order">` within the Pastel entry, collapsed by default so the entry stays short on a phone. If a dish moves, the S$58 pp working figure in the detail line moves with it, and the S$60 cap has to be rechecked.

## index.html: the disclosure pattern

`<details class="order">` is the only `<details>` in the project. The default triangle is removed twice over, `list-style:none` on the summary for modern browsers and `::-webkit-details-marker{display:none}` for Safari, and replaced with a typographic `+` that becomes `-` when open, so the control stays inside the palette instead of adding a marker glyph. The summary carries `padding:8px 0` purely to give a 35px tap target on a phone. Reuse this pattern rather than inventing a second disclosure style.

Watch out when adding anything with an `<li>` inside a `.plan` entry: the schedule row rules, including `li.go`, are scoped `.plan > li` for exactly this reason. An unscoped `.plan li` also matches the order list nested inside the Pastel entry and forces its items into the 140px time column. The column is 140px so the longest time, `7:45-10:00pm`, fits on one line at desktop size.

## Writing style

- No em dashes anywhere. No en dashes either: one dash convention, the plain hyphen, for every range and date span
  (`25-27 Sep`, `Fri-Sun`, `S$50-75`). The only non-ASCII characters are the mid-dot separator `·` and the `m²` in
  costing.html's hotel copy. Check with `grep -n '[^\x00-\x7F]' index.html costing.html` after editing.
- Sentence case headings.
- No corporate filler. Factual, direct tone.
- These are reference pages, not a pitch. Kenny has already approved the trip.

## Design

- Palette: black, white, electric blue `#0038FF` (`--blue`). Do not add new colors.
- Display font: Inter Tight (`--display`).
- Swiss editorial look. No new decorative elements.

## Responsive

Must stay usable at 480px and below. Test any layout change against the mobile breakpoints at the bottom of each file's CSS: `@media (max-width:820px)` and `@media (max-width:480px)`.

- costing.html: the 480px query only drops the table and caption font sizes. The tables are plain and reflow on their own, but `td.num` needs its `padding-left` or two numeric columns run into each other on the total row at phone width.
- index.html: at 480px `.plan > li` collapses to one column, time above content, and `li.go` rows tighten further. Long link labels wrap inside the entry because `.link` carries `max-width:100%`, which `overflow-wrap` alone cannot do on an inline-block. Keep link labels short (e.g. "Tingly cooking school" rather than the full domain).

## Deploy

```
git add . && git commit -m "message" && git push
```

Pages rebuilds automatically in about a minute.

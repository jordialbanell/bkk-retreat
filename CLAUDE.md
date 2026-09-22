# CLAUDE.md

Two-page reference for Hei's Bangkok team offsite (Sep 2026). Hosted on GitHub Pages. Primary reader is Kenny (the boss), mostly on mobile.

Eight people on the trip. Three fly: Tisha, Angru and Jordi. Kenny, Mathew, Cecilia, Dayvin and Wilson are already in Bangkok on the LIXIL project budget. Core travel costs are per person x 3, because only three fly. Everything social, meals and activities, is per person x 8. Every total on both pages is written out by hand, there is no arithmetic in JS any more.

## Structure

Two pages, each a standalone file with inline CSS, HTML and JS. No build step, no package manager, no dependencies beyond Google Fonts and an Unsplash hero image.

- `index.html` is the weekend plan: Friday (confirmed), Saturday (six activity options, then dinner), Sunday (free). It currently holds options 1-6 awaiting Kenny's pick. Once he picks, replace the six option cards with the final schedule, in the same `.plan` list style Friday and Saturday dinner use.
- `costing.html` is the cost record: what was actually booked for the three who fly, plus per-person estimates for the weekend social spend. It is static, there is no configurator and no JS beyond the shared scroll reveal.
- `og.jpg` is shared by both: a 1200x630 crop of the hero image, used as the social preview for link unfurls (Slack, WhatsApp, Teams).

Both pages use the same hero image, palette, type and section rhythm, and both carry "Internal" at the top right of the hero. All style rules below apply to both.

## Nav lives in both files, keep it in sync

Each page carries its own copy of the same nav: a `.nav` block above the hero with "Weekend" and "Costing". The current page gets `class="on"` (blue) and `aria-current="page"`. The `.nav` CSS block is duplicated in both files and marked with a "keep in sync" comment. Any change to the nav markup or CSS must be made in both, or the two pages drift.

## Head tags

Each page has its own `<title>`, description and og tags. Keep `og:title` and `og:description` in sync with `<title>` and `<meta name="description">` on each page.

- `index.html`: "Heitreat 01 · The weekend", `og:url` the site root.
- `costing.html`: "Heitreat 01 · Costing", `og:url` ending in `/costing.html`. Its description covers booked flights and rooms plus weekend spend estimates.

`og:image` is an absolute URL (`https://jordialbanell.github.io/bkk-retreat/og.jpg`) on both, which is required, relative paths do not resolve for link previews. If the hero image or the site URL ever changes, regenerate `og.jpg` and update the absolute URL in both files.

## costing.html: a record, not a calculator

The page used to be a decision tool with an interactive configurator, a `FLIGHTS`/`ROOMS` data model, flight and hotel option tables and Option A/B scenario cards. Bookings are made, so all of that is gone. There are no ranges to recompute and no duplicated price source to keep in sync. What is left is two static tables:

- **Core cost**, booked and confirmed: Scoot return S$316 pp, Ibis Sukhumvit 24 S$303 per room for 3 nights Thu-Sun on flexible terms, core total S$619 pp and S$1,857 for three. The three columns are item, per person, for three. The per-person and for-three figures are written out, so if one moves, update the row and the `tr.total` line together.
- **Weekend spend**, estimates for all eight: Friday dinner, Friday drinks, Saturday activity, Saturday dinner, Sunday. The total row is the sum of the lows and the sum of the highs, currently S$139-214 pp. Recompute it whenever a row changes, and remember the Saturday activity range (S$21-77) is the cheapest and priciest option on the Weekend page, so it moves when those options move.

Flights are per person, the hotel is per room but there is one room each, so both sit in the per-person column. The row says "one room each" so the column heading stays honest.

`tr.total` is the heavier summary row, and `td.num`/`th.num` carry `padding-left` so two numeric columns cannot collide on a phone. The `.note-sm` paragraph under each table carries the caveats, and `.note-sm a` is the only inline prose link style on either page.

## index.html: activity totals are hardcoded

Each option card states a price per person and a total for eight. The totals are written out, not computed. If a per-person price changes, recompute `pp x 8` by hand and update both numbers on the card. The rainy-season note under the cards names option numbers, not venues, so it has to be rechecked whenever an option changes indoor/outdoor.

Option 2 is the exception to one card, one price: it holds two cooking schools as `.variant` blocks inside a single card, Bangkok Thai Cooking Academy (S$50 pp, S$400 for 8) and Pink Chili (S$46 pp, S$368 for 8), each with its own timing, tag, link and hardcoded `pp x 8` total. It stays numbered 2 so Kenny can still answer with a number. The THB figures are the quoted source prices; the SGD ones are converted, so move both together. If one school confirms, collapse the card back to a single option in the shape of cards 1 and 3-6.

## index.html: Saturday dinner

Saturday dinner is Pastel, a Mediterranean rooftop that turns into a DJ party later. It sits in a `.plan` entry below the options, in the `.evening` block. Budget is confirmed at S$60 pp capped, about S$480 for 8, which assumes the planned shared order and one signature cocktail each ordered centrally, with 10% service and 7% VAT included. The time is still "From evening / time TBC", so replace that when the booking is made.

The planned order is a list inside a `<details class="order">`, collapsed by default so the entry stays short on a phone. If a dish moves, the S$58 pp working figure in the detail line moves with it, and the S$60 cap has to be rechecked.

## index.html: the disclosure pattern

`<details class="order">` is the only `<details>` in the project. The default triangle is removed twice over, `list-style:none` on the summary for modern browsers and `::-webkit-details-marker{display:none}` for Safari, and replaced with a typographic `+` that becomes `-` when open, so the control stays inside the palette instead of adding a marker glyph. The summary carries `padding:8px 0` purely to give a 35px tap target on a phone. Reuse this pattern rather than inventing a second disclosure style.

Watch out when adding anything with an `<li>` inside a `.plan` entry: the schedule row rules are scoped `.plan > li` for exactly this reason. An unscoped `.plan li` also matches list items nested inside an entry and forces them into the 130px time column.

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
- index.html: there is one `min-width` query, `@media (min-width:672px)`, which is the width at which the `.options` grid reaches 2 columns. From there option 2 (`.opt.wide`) spans a full grid row and sets its two schools side by side, so the double-height card stays close to its neighbours. Below it, the card is a normal single column with the two schools stacked. If the `305px` grid floor below ever changes, recompute this `672px` (`2 x floor + 14px gap + 48px padding`) to match.
- index.html: the `.options` grid is otherwise not breakpoint-driven. It uses `repeat(auto-fit,minmax(min(305px,100%),1fr))`, which yields 3 columns on desktop, 2 in the tablet band and 1 on phones. The 305px floor exists because the longest link label (`sompongthaicookingschool.com`) renders 257px wide and does not scale with the viewport. If you add a longer label, either shorten it or raise that floor. `.link` carries `max-width:100%` so a long label wraps inside its card instead of overflowing it, which `overflow-wrap` alone cannot do on an inline-block.

## Deploy

```
git add . && git commit -m "message" && git push
```

Pages rebuilds automatically in about a minute.

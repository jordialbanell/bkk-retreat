# CLAUDE.md

Two-page reference for Hei's Bangkok team offsite (Sep 2026). Hosted on GitHub Pages. Primary reader is Kenny (the boss), mostly on mobile.

Eight people on the trip. Three fly: Tisha, Angru and Jordi. Kenny, Mathew, Cecilia, Dayvin and Wilson are already in Bangkok. Costing totals are per person x 3 (`pp*3` in the JS). Weekend-plan totals are per person x 8, because activities and meals are for everyone.

## Structure

Two pages, each a standalone file with inline CSS, HTML and JS. No build step, no package manager, no dependencies beyond Google Fonts and an Unsplash hero image.

- `index.html` is the weekend plan: Friday (confirmed), Saturday (six activity options), Sunday (free). It currently holds options 1-6 awaiting Kenny's pick. Once he picks, replace the six option cards with the final schedule, in the same `.plan` list style Friday uses.
- `costing.html` is the frozen costing reference: flights, rooms, the interactive configurator and the scenario cards. It was the original single-page site. Nothing on it is expected to change unless prices move.
- `og.jpg` is shared by both: a 1200x630 crop of the hero image, used as the social preview for link unfurls (Slack, WhatsApp, Teams).

Both pages use the same hero image, palette, type and section rhythm. All style rules below apply to both.

## Nav lives in both files, keep it in sync

Each page carries its own copy of the same nav: a `.nav` block above the hero with "Weekend" and "Costing". The current page gets `class="on"` (blue) and `aria-current="page"`. The `.nav` CSS block is duplicated in both files and marked with a "keep in sync" comment. Any change to the nav markup or CSS must be made in both, or the two pages drift.

## Head tags

Each page has its own `<title>`, description and og tags. Keep `og:title` and `og:description` in sync with `<title>` and `<meta name="description">` on each page.

- `index.html`: "Heitreat 01 · The weekend", `og:url` the site root.
- `costing.html`: "Heitreat 01 · Costing", `og:url` ending in `/costing.html`.

`og:image` is an absolute URL (`https://jordialbanell.github.io/bkk-retreat/og.jpg`) on both, which is required, relative paths do not resolve for link previews. If the hero image or the site URL ever changes, regenerate `og.jpg` and update the absolute URL in both files.

## costing.html: pricing lives in two places, keep them in sync

1. The `FLIGHTS` and `ROOMS` objects in the script block drive the interactive configurator.
2. The static flight, hotel and add-on tables in the HTML.

Any price change must update both. They are separate sources of the same numbers.

`FLIGHTS` is keyed by Sunday return time (`scoot_eve`, `scoot_late`, `sq`), not by number of nights: Friday and Thursday departures price the same, so the return flight is the only thing that moves the fare. `ROOMS` is still keyed by nights (2 or 3), then breakfast, then booking terms. The add-ons table is static, it is not wired to the configurator.

## costing.html: scenario cards are hardcoded, recalculate them

The Option A / Option B cards show hardcoded ranges: S$1,515-1,968 and S$1,794-2,319. If prices change, recompute:

```
range low  = (cheapest flight + cheapest room) x 3
range high = (priciest flight + priciest room) x 3
```

per scenario (Option A = 2 nights, Option B = 3 nights). Since flights no longer vary by nights, the cheapest and priciest flight are the same in both scenarios, only the room rates differ. Current check: A low = (310 + 195) x 3 = 1,515. B high = (413 + 360) x 3 = 2,319.

The Option B card also states the like-for-like gap to Option A (currently about S$350, the 3-night minus 2-night room difference x 3). Recheck it when room rates move.

## index.html: activity totals are hardcoded

Each option card states a price per person and a total for eight. The totals are written out, not computed. If a per-person price changes, recompute `pp x 8` by hand and update both numbers on the card. The rainy-season note under the cards names which options are outdoors, so it has to be rechecked if the options change.

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

- costing.html: the 480px query flips `.seg` to a row, so configurator controls with more than two options need an explicit override (the flights control has one).
- index.html: the `.options` grid is deliberately not breakpoint-driven. It uses `repeat(auto-fit,minmax(min(305px,100%),1fr))`, which yields 3 columns on desktop, 2 in the tablet band and 1 on phones. The 305px floor exists because the longest link label (`sompongthaicookingschool.com`) renders 257px wide and does not scale with the viewport. If you add a longer label, either shorten it or raise that floor. `.link` carries `max-width:100%` so a long label wraps inside its card instead of overflowing it, which `overflow-wrap` alone cannot do on an inline-block.

## Deploy

```
git add . && git commit -m "message" && git push
```

Pages rebuilds automatically in about a minute.

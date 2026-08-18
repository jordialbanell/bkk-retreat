# CLAUDE.md

Single-page costing reference for Hei's Bangkok team offsite (Sep 2026). Hosted on GitHub Pages. Primary reader is Kenny (the boss), mostly on mobile.

Three people fly: Tisha, Angru and Jordi. Kenny, Mathew, Cecilia, Dayvin and Wilson are already in Bangkok. Every total on the page is per person x 3 (`pp*3` in the JS).

## Structure

`index.html` is the entire site: inline CSS, HTML, and JS in one file. No build step, no package manager, no dependencies beyond Google Fonts and an Unsplash hero image.

`og.jpg` is the only other file: a 1200x630 crop of the hero image, used as the social preview for link unfurls (Slack, WhatsApp, Teams). The `og:image` tag in the head points at it by absolute URL (`https://jordialbanell.github.io/bkk-retreat/og.jpg`), which is required, relative paths do not resolve for link previews. Keep `og:title` and `og:description` in sync with `<title>` and `<meta name="description">`. If the hero image or the site URL ever changes, regenerate `og.jpg` and update the absolute URL.

## Pricing lives in two places, keep them in sync

1. The `FLIGHTS` and `ROOMS` objects in the script block drive the interactive configurator.
2. The static flight, hotel and add-on tables in the HTML.

Any price change must update both. They are separate sources of the same numbers.

`FLIGHTS` is keyed by Sunday return time (`scoot_eve`, `scoot_late`, `sq`), not by number of nights: Friday and Thursday departures price the same, so the return flight is the only thing that moves the fare. `ROOMS` is still keyed by nights (2 or 3), then breakfast, then booking terms. The add-ons table is static, it is not wired to the configurator.

## Scenario cards are hardcoded, recalculate them

The Option A / Option B cards show hardcoded ranges: S$1,515-1,968 and S$1,794-2,319. If prices change, recompute:

```
range low  = (cheapest flight + cheapest room) x 3
range high = (priciest flight + priciest room) x 3
```

per scenario (Option A = 2 nights, Option B = 3 nights). Since flights no longer vary by nights, the cheapest and priciest flight are the same in both scenarios, only the room rates differ. Current check: A low = (310 + 195) x 3 = 1,515. B high = (413 + 360) x 3 = 2,319.

The Option B card also states the like-for-like gap to Option A (currently about S$350, the 3-night minus 2-night room difference x 3). Recheck it when room rates move.

## Writing style

- No em dashes anywhere. No en dashes either: one dash convention, the plain hyphen, for every range and date span
  (`25-27 Sep`, `Fri-Sun`, `S$50-75`). The only non-ASCII characters in `index.html` are the mid-dot separator `·`
  and the `m²` in the hotel copy. Check with `grep -n '[^\x00-\x7F]' index.html` after editing.
- Sentence case headings.
- No corporate filler. Factual, direct tone.
- This is a costing reference, not a pitch. Kenny has already approved the trip.

## Design

- Palette: black, white, electric blue `#0038FF` (`--blue`). Do not add new colors.
- Display font: Inter Tight (`--display`).
- Swiss editorial look. No new decorative elements.

## Responsive

Must stay usable at 480px and below. Test any layout change against the mobile breakpoints at the bottom of the CSS: `@media (max-width:820px)` and `@media (max-width:480px)`. Note that the 480px query flips `.seg` to a row, so configurator controls with more than two options need an explicit override (the flights control has one).

## Deploy

```
git add . && git commit -m "message" && git push
```

Pages rebuilds automatically in about a minute.

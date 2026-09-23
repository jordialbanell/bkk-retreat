# CLAUDE.md

Two-page reference for Hei's Bangkok team offsite (Sep 2026). Hosted on GitHub Pages. `index.html` is the itinerary shared with all eight, read mostly on mobile. `costing.html` is unlisted, for Kenny and whoever else is given the link.

Eight people on the trip. Three fly: Tisha, Angru and Jordi. Kenny, Mathew, Cecilia, Dayvin and Wilson are already in Bangkok on the LIXIL project budget. Core travel costs are per person x 3, because only three fly. Everything social, meals and activities, is per person x 8. Every total on both pages is written out by hand, there is no arithmetic in JS any more.

## Structure

Two pages, each a standalone file with inline CSS, HTML and JS. No build step, no package manager, no dependencies beyond Google Fonts and an Unsplash hero image.

- `index.html` is the final shared itinerary for all eight: timed Friday and Saturday schedules, Sunday chill day, and what to wear, plus an ask bar docked to the bottom of the viewport. No options or decisions left on it, and no prices.
- `costing.html` is the cost record: what was actually booked for the three who fly, plus per-person estimates for the weekend social spend. It is unlisted: nothing links to it, and it is reached at `/costing` (GitHub Pages serves `costing.html` there) by anyone given the link. A grey `.unlisted` line at the top of its first section says so. It is static, there is no configurator and no JS beyond the shared scroll reveal.
- `og.jpg` is shared by both: a 1200x630 crop of the hero image, used as the social preview for link unfurls (Slack, WhatsApp, Teams).

Both pages use the same hero image, palette, type and section rhythm, and both carry "Internal" at the top right of the hero. All style rules below apply to both.

## No page nav, costing stays unlinked

There is no nav between the pages. The old Weekend/Costing nav was removed from both files so the costing page is not reachable from the itinerary. Do not add a link to `costing.html` from `index.html`, including in the QA answers.

## index.html: jump nav

`index.html` alone has a slim sticky `.jump` bar at the very top, above the hero: "Fri · Sat · Sun · Dress · Ask", anchor links to `#fri`, `#sat`, `#sun` and `#dress`. "Ask" is not a section jump: it carries `data-ask`, and a click handler focuses the docked input and opens its panel (its `href="#ask-input"` is only the no-JS fallback). Kicker type (display font, uppercase, tracked), blue links, white background, hairline bottom border, `position:sticky;top:0;z-index:20`. No active state.

The bar height is `--jump` (44px), border included: the inner `.wrap` is `calc(var(--jump) - 1px)` tall to leave room for the 1px hairline. Every `section[id]` carries `scroll-margin-top:var(--jump)` so anchor jumps land below the bar instead of under it. If the bar height changes, change `--jump`, not the sections. A new jump target needs an `id` on its `<section>`, which picks up the margin automatically. Smooth scrolling comes from `html{scroll-behavior:smooth}`. The bar is in normal flow above the hero, so it never overlaps it; at 320px the five labels fit on one line with the 480px query's tighter gap.

## Head tags

Each page has its own `<title>`, description and og tags. Keep `og:title` and `og:description` in sync with `<title>` and `<meta name="description">` on each page.

- `index.html`: "Heitreat 01 · The weekend", `og:url` the site root. The description summarises the final plan (Friday rooftop, dinner and bars, Saturday cooking class, Pastel and a night out, Sunday chill day), so update it if the plan changes.
- `costing.html`: "Heitreat 01 · Costing", `og:url` ending in `/costing.html`. Its description covers booked flights and rooms plus weekend spend estimates.

`og:image` is an absolute URL (`https://jordialbanell.github.io/bkk-retreat/og.jpg`) on both, which is required, relative paths do not resolve for link previews. If the hero image or the site URL ever changes, regenerate `og.jpg` and update the absolute URL in both files.

## costing.html: a record, not a calculator

All money lives here and only here.

The page used to be a decision tool with an interactive configurator, a `FLIGHTS`/`ROOMS` data model, flight and hotel option tables and Option A/B scenario cards. Bookings are made, so all of that is gone. There are no ranges to recompute and no duplicated price source to keep in sync. What is left is two static tables:

- **Core cost**, booked and confirmed: Scoot return S$316 pp, Ibis Sukhumvit 24 S$303 per room for 3 nights Thu-Sun on flexible terms, core total S$619 pp and S$1,857 for three. The three columns are item, per person, for three. The per-person and for-three figures are written out, so if one moves, update the row and the `tr.total` line together.
- **Weekend spend**, estimates for all eight: Supanniga, Pastel, the Tingly cooking class (price at booking), Fri and Sat bar rounds (pay as you go), Sunday (own spend). The total row is labelled "Both dinners" and covers only the two priced rows, Supanniga S$42-55 plus Pastel S$60, so S$102-115 pp. The note under the table says the rest comes on top. When Tingly's price is known, add it to the total and relabel the row to say what it includes.

Flights are per person, the hotel is per room but there is one room each, so both sit in the per-person column. The row says "one room each" so the column heading stays honest.

`tr.total` is the heavier summary row, and `td.num`/`th.num` carry `padding-left` so two numeric columns cannot collide on a phone. The `.note-sm` paragraph under each table carries the caveats. The total row's figure is `white-space:nowrap` so a range like S$102-115 does not break at the hyphen on a phone.

## index.html: no prices

The itinerary carries no money at all: no S$, no THB, no caps, deposits or "price at booking" notes, in the copy, the QA answers, `<meta name="description">` or `og:description`. Prices belong on costing.html. Check with `grep -n 'S\$\|THB' index.html`, which should return nothing.

## index.html: the itinerary

Sections in order: Friday, Saturday, Sunday, Dress. The ask bar is not a section, it is the fixed dock. Friday and Saturday are timed schedules in `.plan`, time on the left, content on the right. The Friday section opens with a `.base` line, "Base: Ibis Bangkok Sukhumvit 24" plus its Map link, then "Travel times are estimates.", once.

Two row types inside a schedule:

- Venue entries are a plain `.plan > li`: `h3` (e.g. "Drinks · Aether rooftop"), optional `.desc` and `.detail` note, then a `.links` row holding the venue's own link and a Map link.
- Transit legs and other one-line rows are `li.go` with a `p.go-text` ("Hotel to Tichuca · Grab, about 15 min"). They are compact and grey so the venues stand out. Shower and change on Saturday uses the same row.

Times use the `7:30-9:15pm` form, open-ended ones `11:45pm-late`, and after-midnight rows just say "Late". If a slot moves, check the transit rows either side still add up, and update the QA answers (see below).

Sunday reuses `li.go` inside `ul.plan.groups`, where the left column is a category (Shopping, Massage, Sightseeing) in blue and the text is dark, not grey. The flights-back line is the lede above it. Dress is a normal `.plan` with the day in the left column and a `.desc`, closed by a `.note` saying only Tichuca and Sing Sing have confirmed codes. If a venue changes, recheck the dress copy, it names venues.

## index.html: Map links

Every venue entry in the schedule, and the base line, has a "Map" link in the normal `.link` style, next to the venue's own link inside `<div class="links">` (flex, wraps on a phone). The URL is a Google Maps search, no place IDs:

```
https://www.google.com/maps/search/?api=1&amp;query=<venue name>+Bangkok
```

The query is the venue name plus "Bangkok", URL-encoded with `+` for spaces (e.g. `Supanniga+Eating+Room+Thonglor+Bangkok`), and the `&` is written `&amp;` in the HTML. The Ibis line drops the extra "Bangkok" because its name already has it. A new venue entry gets a Map link too.

## index.html: ask dock and the QA array (keep in sync with the schedule)

The ask bar is a `.dock` fixed to the bottom of the viewport (`z-index:30`, above the jump nav's 20), always visible, after the stamp in the markup. Two parts:

- `.dock-bar`: white, hairline top border, `--dock` (65px) tall, holding the 44px `input.ask-input` (16px text so iOS does not zoom on focus) with placeholder "Ask about the weekend".
- `.dock-panel`: rises above the bar, `hidden` until the input is focused, tapped or typed in. It holds a sticky `.dock-head` (kicker "Ask" and a Close button), the five `.chip` buttons, and `#ask-answer` in the hairline style. It collapses on Close, Escape, or a pointerdown anywhere outside the dock. After an answer it scrolls to the bottom so the answer sits next to the input.

On desktop both parts cap their contents at 620px through `.dock-inner`, left-aligned inside `.wrap`. `body` has `padding-bottom:var(--dock)` so the stamp is never hidden behind the bar. If the bar's height changes, change `--dock`.

Panel height is capped at `min(420px, 100dvh - dock - 12px, --vvh - dock - 12px)`. Use `dvh`, never `vh`. On iOS the layout viewport does not shrink when the keyboard opens, so a `visualViewport` listener at the end of the script sets `--vvh` to the visible height and moves the dock up by the hidden amount with `translateY`. Android Chrome gets `interactive-widget=resizes-content` in the viewport meta, so its layout viewport and `dvh` shrink with the keyboard. Keep all three: the dvh cap, the `--vvh` listener and the meta.

The data is the `QA` array in the `<script>` at the bottom of `index.html`, under the `// ----- ask bar -----` comment. Each entry is:

```
{keywords:["saturday night","sat night","wear", ...],
 answer:["First line, one fact.", "Second line, the next fact."]}
```

`answer` is an array of lines, rendered as one `<p>` per line with 6px between them. Keep each line to one fact. If an answer chains two facts, split it. A full-day plan or a drinks run is one fact and stays on one line. Lines are plain text (`textContent`), no HTML.

Matching is on whole words. `words()` lowercases the question, drops a possessive `'s` ("Saturday's" becomes "saturday", "who's going" becomes "who going"), removes other apostrophes, and splits on anything that is not a letter, digit or colon. Keywords go through the same function. A one-word keyword matches only a whole word, so "eat" no longer matches inside "weather". A multi-word keyword matches only as that exact word sequence. Each match adds the keyword's character length, the highest score wins, and ties go to the earlier entry. Dress entries come first on purpose, people ask before they scroll.

Threshold: an entry only counts if it matched at least one keyword of 4+ characters, or at least two shorter ones. A lone "fri", "sat", "who" or "hot" is not enough, which is why phrases like "is it hot", "who going" and "on friday" exist as their own keywords. Below the threshold everywhere, the answer is `FALLBACK`: "I can only answer questions about the weekend. This isn't a real chat, you've been fooled. Ask in the group instead."

Sync rule: the QA answers repeat times, venues and dress rules from the schedule. Any change to a time, venue, transit leg or dress line must be made in the matching answers too, or the ask bar gives the old plan. After editing, run the five chips, "weather" (weather answer), "sweater" (fallback) and a nonsense query, and check each lands on the right answer.

## index.html: Saturday dinner

Pastel is the 7:45-10:00pm entry in the Saturday schedule, a Mediterranean rooftop that turns into a DJ party later. The planned shared order is a list inside a `<details class="order">` within the Pastel entry, collapsed by default so the entry stays short on a phone. It is the plan for the table, with no price framing. The S$60 pp cap it was built to lives only on costing.html; if a dish moves, recheck the cap there.

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

- Both pages: the 480px query gives `.hero-bottom` `padding-top:76px` so the hero text never runs up under the absolutely positioned `.hero-top` line on a short phone screen.
- costing.html: otherwise the 480px query only drops the table and caption font sizes. The tables are plain and reflow on their own, but `td.num` needs its `padding-left` or two numeric columns run into each other on the total row at phone width.
- index.html: at 480px `.plan > li` collapses to one column, time above content, and `li.go` rows tighten further. Long link labels wrap inside the entry because `.link` carries `max-width:100%`, which `overflow-wrap` alone cannot do on an inline-block. Keep link labels short (e.g. "Tingly cooking school" rather than the full domain).
- index.html: the ask input stays 44px tall at every width and the chips wrap inside the dock panel. Test the dock at 320px with a short viewport (about 300px, what is left with a keyboard open): the panel must fit above the bar and scroll. The 480px query tightens the jump nav's gap and type so "Fri · Sat · Sun · Dress · Ask" stays on one line at 320px. Test the jump links at 320px after any change above the schedule.

## Deploy

```
git add . && git commit -m "message" && git push
```

Pages rebuilds automatically in about a minute.

# CLAUDE.md

Guidance for working in this repo. Read before editing.

## What this is

A single self-contained file, **`index.html`** — a personal Vietnam trip itinerary
rendered as one static HTML page. No framework, no build step, no dependencies to
install. Open the file in a browser and it works. `index.html` is the **source of
truth for the schedule** — do not duplicate the day-by-day plan anywhere else
(including here); read it from the file.

Repo: `git@github.com:alshehri149/vitnam.git` · work on `main` · **push only when
the user asks** (they usually say "push again to github").

## Owner context (matters for content)

The trip is for a Muslim traveller and group. **Halal is a hard requirement, not a
preference.** Every food suggestion on the page must be halal (or kosher). Never add
a restaurant you cannot stand behind as halal; when a place has no halal option, say
so plainly rather than inventing one. Jumu'ah / prayer timing is worth flagging when
the dates make it possible or impossible.

## Hard rules

1. **Halal/kosher food only.** See above.
2. **Hotels and Activities stay as separate sections.** The user asked for this
   explicitly and likes it — do not merge them.
3. **Real photos, with attribution.** Place/activity photos come from Wikimedia
   Commons (or Wikipedia REST summaries), not stock. Every real photo needs a CC
   credit in the footer `.credits` block (author + licence + link to the file page).
   Verify a photo shows the right subject by actually viewing it (Read tool) — past
   searches returned literal cats for "Cat Cat" and wartime aerials for Hoa Lo.
   Hotel/landscape header images (the leg `.shot` images and the `#hotels`
   gallery) are **deliberately stock** and are labelled as such in `.credits` —
   do not "fix" them into claimed photographs of a specific named property.
4. **Validate before pushing.** Check the HTML has no unclosed/mismatched tags and
   that every image URL returns 200. Keep the scroll-spy `sections` array (in the
   `<script>`) in sync with the section IDs that actually exist.
5. **Don't silently "correct" the user's numbers.** When the user gives explicit
   clock times (arrival, departure, when to leave for the airport), treat them as
   given and build around them — don't recompute a "better" time.

## Cloning gotcha

A `.claude` folder in the working directory blocks `git clone .` into it. Instead:
`git init` → `git remote add origin <url>` → `git fetch origin` →
`git checkout -b main origin/main`.

## Page structure (`index.html`)

One `<style>` block, then the sections, then one small `<script>`.

- **Design tokens** — CSS custom properties on `:root`: `--ink --paper --card
  --terrace --clay --mist --line --line2`. Fonts: Bebas Neue (headings), IBM Plex
  Mono (labels/eyebrows), IBM Plex Sans (body).
- **Sections in order:** nav → hero (`.hero`, elevation SVG + `.facts`) → `#route`
  (accordion `.leg` blocks) → `#transfers` (`table.moves` + `.hardstop`) →
  `#shopping` → `#activities` (per-city `.citygrp` with `.gal`/`.hcard`) →
  `#hotels` (`.gal`/`.hcard` gallery) → `#notes` (`.notes`/`.note`) → `#weather`
  (`.warn`) → footer (with `.credits`).
- **Reusable patterns:** `.leg`/`.leghead`/`.legbody`/`.legcols`, `ul.beats`,
  `.stays`/`.stay` (halal-eats + where-you-stay strips inside a leg), `.gal`/`.hcard`
  (image cards), `.move` (green transfer callout inside a leg), `table.moves` +
  `.hardstop` (dark callout for the immovable timing), `.citygrp`/`.cityname`/
  `.alsohead`, `.notes`/`.note`.
- **Script:** accordion toggle on `.leghead` (aria-expanded), plus an
  IntersectionObserver scroll-spy driving the nav `.on` state from a `sections`
  array of IDs.

## When editing the itinerary

The trip year is **2026** (25 Aug is a Tuesday, 28 Aug is the only Friday, 1 Sep
is a Tuesday) — don't recompute weekdays from scratch. The outbound flight lands
07:15 on 25 Aug; the return was originally built around an 08:45 departure on
1 Sep, but the user later set their own airport-departure time (04:00) — treat
their explicit clock times as authoritative.

A schedule change ripples. After changing legs, also check and update: the hero
eyebrow + standfirst, the `.facts` grid (nights/bases/flights/longest transfer), the
elevation SVG `aria-label` and `.profile-key`, the `#transfers` table and
`.hardstop`, the `#notes` cards, the `#weather` copy, the footer line and
`.credits`, the `<title>` and `.brand`, and the scroll-spy `sections` array. Grep
old place names across the whole file before declaring done — stale references hide
in the SVG, footer, notes, and weather blocks.

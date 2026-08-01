# O.A.S.I.S. — todo

**O**ffline **A**dvanced **S**ystem for **I**nformation and **S**urvival.
An air-gapped field reference: doctrine, tables and working calculators that keep
working with the cable cut. <https://oasis.labidi.eu>

## Hard rules

- **Zero external requests.** No fonts, no CDN, no analytics, no frameworks.
  If a change adds a network call, the change is wrong.
- **No `innerHTML`.** `app.js` builds everything with `createElement`/`textContent`.
  Authored copy supports `**bold**` and backtick-code via `rich()`.
- **Never change an existing `id`** (chapter, card, tool). Hash routes and the
  search index point at them.
- Content is reference, not advice. Every risky topic carries its limits.

## Files

| File | Role |
| --- | --- |
| `index.html` | Shell only — header, rail, `<main>`. Everything else is rendered. |
| `style.css` | Design system + 5 themes via `[data-theme]` + print stylesheet. |
| `geo.js` | Pure geodesy/astronomy → `window.GEO`. No DOM. |
| `data-knowledge.js` | Chapters → cards. The doctrine. |
| `data-scenarios.js` | `OASIS_BANDS` + `OASIS_SCENARIOS` — before/during/after playbooks. |
| `data-trees.js` | `OASIS_TREES` — interactive decision guides. |
| `data-reference.js` | `OASIS_SOURCES` (attribution registry) + `OASIS_TABLES` + `OASIS_LINKS`. |
| `tools.js` | `OASIS_TOOLS` — declarative calculators. |
| `app.js` | Router, renderer, search, log, card, position, theme, install, offline. |
| `sw.js` | Cache-first service worker, self-healing shell cache. |

## Built

- [x] 12 chapters, 94 cards, **42 playbooks**, **6 decision guides**, 26 tables,
      24 calculators, 52 named source authorities
- [x] **`#/now` — "I need help now"**: 13 one-tap critical links plus interactive
      decision guides for unknown situation, casualty, navigation, meeting people,
      water safety and stay-or-go
- [x] **Playbooks**: before / during / after across 8 bands — everyday, infrastructure,
      natural, CBRN, disease, conflict, isolation, long-term collapse
- [x] **People chapter**: what really happens in disasters, meeting strangers,
      running a group, trade and barter, working across a language barrier
- [x] **Navigation**: WGS84 vs ETRS89 vs UTM, using a compass, making a compass,
      night navigation, navigating under cloud, plus the existing sun/stars work
- [x] **Medical completeness**: penetrating trauma, poisoning and fumes, crush and
      entrapment, drowning, electrical injury, bites and envenomation, abdominal
      injury and hernia, eye injuries, childbirth, dental
- [x] **Communications**: satellite, digital modes, non-electronic methods, running
      a net, repairing a radio, CB on 12/24 V, obtaining a beacon, building a
      crystal radio, master table of 38 channels, global alerting table with BE-Alert
- [x] **Position page** (`#/pos`, or tap the status chip): all formats, copy, log,
      save waypoints, GPX export, and a locally drawn range-ring map
- [x] **Stockpiling, sourced against the national authorities**: what to keep in
      stock, how long for, and the fact that the authorities openly disagree —
      the EU floor of 72 h against Sweden and Norway's week and Germany's ten
      days, presented side by side rather than averaged into a single invented
      number. Includes Germany's quantified 10-day per-person food table, the
      EPA bleach dose table, the 4 h / 48 h / 24 h power-cut food rules with the
      measured refreeze test, and the FDA potassium-iodide schedule by age
- [x] **Emergency card** (`#/card`): fillable, printable, stored only on the device
- [x] Install: `beforeinstallprompt` captured, status-bar button, install card with
      per-platform manual steps and installed-state detection
- [x] 5 themes including red night vision; adjustable text; wake lock; print layouts
- [x] Field log, GPS fix, tool-input persistence, "erase all local data"

### Verified

- Quarter meridian 10 001 965.730 m (truth …729); global UTM forward/inverse
  round-trip closes < 1.5 mm; MGRS of 0°,0° = `31N AA 66021 00000`
- Resection closes to 0.000 mm across 8 geometries including the dateline and
  both polar regions
- **Zero external requests** — a full cold load is exactly 9 same-origin files
  (the document, one stylesheet, seven scripts) and nothing else, measured from
  the resource timing log
- All 24 tools × 7 adversarial input sets (empty, zero, negative, 1e21, garbage,
  whitespace) produce no NaN, no Infinity and no exception
- Structural audit passes: no duplicate ids, every table row matches its column
  count, every chapter/tool/table cross-reference resolves, all `**` balanced,
  every select default is a real option, every source link is https, every
  decision-tree branch resolves to a real node with no loops and no orphans,
  every cited source key exists and every defined source is cited
- No horizontal scroll from 320 px to 1440 px; zero console errors across all 69
  routes (every chapter, playbook, decision guide and standalone page)

### Fixed in the post-build audit

- `+'' === 0` is finite, so a cleared numeric field became a real zero instead of
  the default — produced `NaN` in the pace-count tool
- Full-wave loop applied the velocity factor twice (the 1005/f rule already
  includes end effect): antenna was 5 % short and contradicted its own doctrine card
- Morse letter gaps were 4 units and word gaps 8, instead of 3 and 7; SOS now
  sends as an unbroken prosign
- Coordinate parser dropped the minus sign on unsigned-hemisphere DMS, and a
  place name like "BRUSSELS" donated its S and flipped the latitude
- DDM/DMS printed `60.000'` and `60.00"` instead of carrying to the next unit
- Time fields were persisted, freezing the sun/moon and DTG tools at whenever
  you last opened them
- Worked UTM/MGRS example in the coordinates card was simply wrong for its own
  lat/lon; radiation card's halving thicknesses contradicted the shielding table;
  wind chill table was up to 3 °C out; fallout decay figures did not match the
  t⁻¹·² curve the calculator uses; 12 AWG quoted as 4 mm² at under 3 % drop
- Removed `GEO.intersect`: unused, and its docstring invited exactly the
  back-bearing misuse that had already cost 694 m at 64°N
- `aria-live` on tool output re-announced the entire result on every keystroke
- Canvas kept stale theme colours after a theme switch

### Fixed in the final sweep

- Reference tables rendered no citations at all — `renderTable` ignored a
  `sources` field, so the alerting table made country-by-country claims with
  nothing behind them. Tables now carry source chips like every other block,
  and the alerting table cites NCCN, FEMA, NOAA and the ITU.

### Fixed in the findability sweep

All three were found by searching the way a member of the public would, rather
than the way the author does.

- **Natural questions returned a blank page.** Search demanded every word appear
  in the same entry, so "what should I buy" and "shopping list" found nothing at
  all. A failed strict pass now falls back to a relaxed one ranked by how much
  of the question was covered. Queries that already worked are untouched — the
  fallback only ever runs when the result would otherwise be empty.
- **Typing the name of a card did not return that card.** "burns" gave the
  electrical-injury card, which is tagged critical and merely mentions burns,
  above the card actually titled "Burns". An exact title match now multiplies
  the score; a title that starts with the query gets a smaller bonus.
- **Two-letter words matched inside other words.** "no" hid inside "monoxide"
  and "co" inside "record", quietly poisoning the ranking for "no power". Terms
  of one or two characters now only count as whole words — which also made "co"
  return the carbon monoxide playbook.
- Doctrine cards could never outrank a playbook on a single common word, because
  playbooks carry a 3× boost and ordinary cards 1×. "supplies" returned six
  scenarios and no kit list. Cards the author tagged `priority` now sit at 2×,
  between ordinary reference material and the critical cards.

## Next

- [ ] **Offline map tiles** for a user-chosen region — the one genuine gap. The
      position page draws range rings and waypoints to scale, which is a
      substitute, not a map. Any real solution means bundling tiles, which
      conflicts with the zero-fetch rule unless the user imports them by hand.
- [ ] Ship a reduced WMM/IGRF coefficient set so declination needs no observation
      (the solar method now covers it, but a model would work at night)
- [ ] GPX **import** (export is done)
- [ ] Translations — NL and FR first, to match the other labidi.eu projects
- [ ] Satellite pass prediction from stored TLEs (SGP4 is ~200 lines)
- [ ] Share a waypoint set or the emergency card as a QR code, device to device,
      with no network (needs a QR encoder, ~200 lines)
- [ ] Region selector that surfaces the playbooks and alerting systems for where
      you live, and hides the irrelevant ones
- [ ] More decision guides: chest pain, breathlessness, abdominal pain, fever
- [ ] Playbook: dam failure, train derailment, sinkhole, mass gathering crush

## Standing alone

This project is self-contained and origin-agnostic: every internal link is
relative and every asset is local, so the folder runs from its own domain, from
any sub-path, or straight off a USB stick over `file://`. All environment-specific
values live in the `CONFIG` block at the top of `app.js` — contact address, the
site it belongs to, and the link back to it.

If it ever has to move again: copy the folder, edit `CONFIG`, update the
`canonical` link and `CNAME`, bump `CACHE` in `sw.js`, and leave a redirect plus
a tombstone service worker behind. The tombstone matters — without it a
cache-first worker keeps serving the old copy out of visitors' own storage and
they never see the redirect at all.

## Content backlog

- [ ] Water: improvised filter build with measured flow rates
- [ ] Comms: a worked CHIRP codeplug for the common cheap handhelds
- [ ] Power: measured draw table for real devices rather than nameplate figures
- [ ] Nav: star charts drawn as SVG for both hemispheres
- [ ] Hazards: volcanic ash, landslide, avalanche and tsunami first-hour actions

## Gotchas

- `+'' === 0` and `Number.isFinite(0)` is true, so "empty means default" needs an
  explicit empty check. `tools.js` has one helper, `n()` — use it, never `+x || d`.
- The service worker calls `clients.claim()`, so it serves stale JS on the very
  next reload. During development: unregister + clear caches + `about:blank`
  before every reload, or you will test yesterday's code.
- Colour emoji break the night-vision theme — glyphs must be monochrome text
  symbols that inherit `currentColor`. That includes the moon-phase glyphs.
- Grid tracks need `minmax(0, 1fr)`. A bare `1fr` cannot shrink below its
  content, and one wide table then takes the whole page sideways.
- A back bearing is **not** the forward bearing plus 180° on a sphere. Resection
  must iterate, or meridian convergence puts you hundreds of metres out.
- Any numbers written into a doctrine card or reference table must be generated
  from the same formula the calculator uses. Every hand-typed table in the first
  draft had at least one wrong cell.
- Canvas drawing reads CSS custom properties, so anything drawn must be
  re-drawn when the theme changes (`sec._redraw`).
- Search is substring-based and requires **all** terms to match, so write `keys`
  in the words a frightened person would actually type. "warzone", "shtf",
  "deserted island" and "martial law" all returned nothing until they were added.
  Add plurals too — "rupture" does not match a search for "ruptures".
- Search scores three fields: title, `keys`, body. Whole-word matches beat
  substring matches and an all-terms-whole-word hit gets a 1.6× bonus. Without
  that, "car" matched inside "cargo" and "car crash" returned the aircraft playbook.
- Every playbook needs `sources`; every tree option must point at a node that
  exists and every path must terminate. The structural audit checks both.
- Contact for gaps is `oasis@labidi.eu`, shown in the footer and on About.

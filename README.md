# O.A.S.I.S.

**O**ffline **A**dvanced **S**ystem for **I**nformation and **S**urvival —
an air-gapped field reference that keeps working with the cable cut.

<https://oasis.labidi.eu>

## What it is

A single folder of plain files. No build step, no framework, no fonts, no CDN,
no analytics, no cookies, no accounts, and **no outbound request of any kind**.
Open it once and it works forever, offline, on any device.

- **Playbooks** — before / during / after for every major emergency, from a
  blackout to a nuclear detonation.
- **Decision guides** — interactive triage for people who do not know what is
  happening or what to do.
- **Doctrine** — first response, medical, water, shelter and fire, food,
  navigation, communications, signals, power, hazards, people, kit.
- **Calculators** — coordinates, resection, sun and moon, antenna cutting,
  battery runtime, water dosing, radiation decay and more.
- Every claim is attributed to a named authority (WHO, CDC, FEMA, ICRC, IAEA,
  NOAA, USGS, Sphere, ERC, ITU, UNHCR and others).

## Hard rules

1. **Zero external requests.** If a change adds a network call, the change is wrong.
2. **No `innerHTML`.** Everything is `createElement` + `textContent`.
3. **Never change an existing `id`** — hash routes and the search index point at them.
4. Numbers printed in cards and tables must come from the same formula the
   calculators use.

## Files

| File | Role |
| --- | --- |
| `index.html` | Shell only. Everything else is rendered. |
| `style.css` | Design system, 5 themes, print layouts. |
| `geo.js` | Pure geodesy and astronomy → `window.GEO`. No DOM. |
| `data-knowledge.js` | Chapters → cards. The doctrine. |
| `data-scenarios.js` | Before/during/after playbooks. |
| `data-trees.js` | Interactive decision guides. |
| `data-reference.js` | Source registry, lookup tables, mirrorable links. |
| `tools.js` | Declarative calculators. |
| `app.js` | Router, renderer, search, log, card, position, install, offline. |
| `sw.js` | Cache-first service worker with a self-healing shell cache. |

Configuration lives in one `CONFIG` block at the top of `app.js`.

## Running it locally

```powershell
python -m http.server 8811 --bind 127.0.0.1
```

Then open <http://127.0.0.1:8811/>.

> During development the service worker calls `clients.claim()`, so it serves
> stale JavaScript on the very next reload. Unregister it and clear the caches
> before each reload, or you will test yesterday's code.

## Limits

This is general reference assembled from published civilian guidance and open
military doctrine. It is not medical, legal or engineering advice for your
situation, guidance varies by country and by year, and training beats reading.
Where emergency services exist, call them first.

## Contributing

Something missing, wrong, out of date for your country, or hard to find?

**<oasis@labidi.eu>**

The single most useful thing you can send is the word you searched for that
found nothing.

Backlog and known gaps are in [`todo.md`](todo.md).

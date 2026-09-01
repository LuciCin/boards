# boards

Live football standings boards — **Portland Hearts of Pine** (USL League One), **Arsenal** and **Manchester City** (Premier League) — rendered as images from live data every two hours and published here automatically.

**Live site:** https://lucicin.github.io/boards/ · deep links per club: [#hearts](https://lucicin.github.io/boards/#hearts) · [#arsenal](https://lucicin.github.io/boards/#arsenal) · [#mancity](https://lucicin.github.io/boards/#mancity)

This is the companion site to a physical build: a battery-powered e-ink display that shows the same boards, wakes on a nudge (or a timer), and pulls fresh renders. What the little screen shows, this site shows in color.

## How it works

```
ESPN public API  →  Raspberry Pi (Python/Pillow renderer, every 2h)
                          →  git push to `live` (scoped deploy key)  →  GitHub Pages
```

- A Raspberry Pi fetches live standings, schedules and results, renders every board as a PNG/GIF (grayscale for the e-ink device, color for this site), and pushes the color set here.
- The site itself is one static `index.html` — a hash-routed landing page of club cards, each opening that club's boards: standings, next match, recent results, and an animated season sparkline that morphs into the table.
- Adding a club is a ~6-line "team pack" in the renderer (league + team ids, labels, tiebreaker); every view, the combined upcoming list, and this site's registry pick it up from there.

## Repo shape

- **`main` — the source branch.** The site page, this README, and the workflows. Normal git: branch, PR, merge; the publisher picks up merged site changes on its next cycle.
- **`live` — what Pages serves.** `main`'s site files plus the latest renders, rebuilt as a single force-pushed commit each publish so image history never bloats the repo. Generated — don't edit it.
- **`archive` — the history that matters.** A weekly GitHub Action snapshots the boards into `snapshots/<date>/`, so season results survive the publish overwrite.

## Notes

- Unofficial, non-commercial fan project — not affiliated with the clubs, the leagues, or ESPN. Crest artwork belongs to the clubs.
- The renderer and device firmware live in a private repo; this repo is only the publish target.

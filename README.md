# boards

Live football standings boards — **Portland Hearts of Pine** (USL League One), **Arsenal** and **Manchester City** (Premier League) — rendered as images from live data every two hours and published here automatically.

**Live site:** https://lucicin.github.io/boards/ · deep links per club: [#hearts](https://lucicin.github.io/boards/#hearts) · [#arsenal](https://lucicin.github.io/boards/#arsenal) · [#mancity](https://lucicin.github.io/boards/#mancity)

This is the companion site to a physical build: a battery-powered e-ink display that shows the same boards, wakes on a nudge (or a timer), and pulls fresh renders. What the little screen shows, this site shows in color.

## How it works

```
ESPN public API  →  Raspberry Pi (Python/Pillow renderer, every 2h)
                          →  git push (scoped deploy key)  →  GitHub Pages (this repo)
```

- A Raspberry Pi fetches live standings, schedules and results, renders every board as a PNG/GIF (grayscale for the e-ink device, color for this site), and pushes the color set here.
- The site itself is one static `index.html` — a hash-routed landing page of club cards, each opening that club's boards: standings, next match, recent results, and an animated season sparkline that morphs into the table.
- Adding a club is a ~6-line "team pack" in the renderer (league + team ids, labels, tiebreaker); every view, the combined upcoming list, and this site's registry pick it up from there.

## Repo shape (deliberately odd)

- **`main` holds exactly one commit.** Each publish amends and force-pushes it, so the repo never accumulates gigabytes of image history. Don't PR against `main` — the next render will overwrite it.
- **`archive` holds the history that matters.** A weekly GitHub Action snapshots the boards into `snapshots/<date>/` there, so season results survive the overwrite.

## Notes

- Unofficial, non-commercial fan project — not affiliated with the clubs, the leagues, or ESPN. Crest artwork belongs to the clubs.
- The renderer and device firmware live in a private repo; this repo is only the publish target.

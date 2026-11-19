# Scripts documentation

This project reads your own [Letterboxd data export](https://letterboxd.com/settings/data/)
(a `letterboxd-*.zip` backup) and produces graphs, tag statistics, sneak-vs-release
analysis, a backdated git history and a static highlights page. No HTML scraping,
no Letterboxd API — the only third-party API is TMDB (for German cinema release
dates, via the key in the project's `.env`).

## Getting a fresh data export

Every script below consumes the same input: a Letterboxd data export zip.

1. Go to <https://letterboxd.com/settings/data/>
2. Click **Export your data** (a `letterboxd-<username>-<timestamp>-utc.zip` file
   downloads)
3. Drop the zip anywhere in this project (`/home/saltmeyer/opencode/letterboxd/`)
   or pass its path explicitly with `--export` / as an argument

The scripts automatically pick the **newest** zip in the project. Re-download the
export whenever your diary changes (new films, edited ratings, tag changes).

## The scripts

| Script | What it does | Quick start |
| --- | --- | --- |
| [`graph.py`](graph.md) | GitHub-style contribution graph of films/day | `python3 graph.py --all` |
| [`tags.py`](tags.md) | Diary tag rankings, trends & co-occurrence | `python3 tags.py` |
| [`sneak-gap.py`](sneak-gap.md) | Sneak/OV-sneak vs. German cinema release | `python3 sneak-gap.py` |
| [`build.py`](build.md) | Backdate one git commit per film | `python3 build.py` |
| [`generate_site.py`](generate-site.md) | Build the highlights `index.html` | `python3 generate_site.py` |

## How they fit together

```
letterboxd-*.zip
   │
   ├─> graphs/graph.py        → contribution graph (terminal / SVG)
   ├─> tags/tags.py           → tag stats (terminal / JSON / SVG)
   ├─> sneak-preview/sneak-gap.py → sneak table + CSV + Markdown
   │
   ├─> diary/build.py         → diary.json + films/*.md + one git commit per film
   │        │
   │        ▼
   │   diary/generate_site.py → diary/site/index.html  →  sirconanad.github.io
   │
   └─> sneak-preview/sneak-gap.csv  ─┘
```

## Prerequisites

- Python 3.10+
- A `letterboxd-*.zip` export in the project (see above)
- TMDB key in `/home/saltmeyer/opencode/letterboxd/.env` — only needed for
  `sneak-gap.py`

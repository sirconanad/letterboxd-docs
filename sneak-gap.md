# `sneak-gap.py` — sneak preview vs. German cinema release

Measures how far in advance each **sneak** / **ov sneak** screening happened
compared to the film's German theatrical release (Kinostart). Every sneak
watch is listed with the gap in days: positive = seen before the German
release, negative = seen after. This is the only script that uses an external
API (TMDB), for the German release dates.

**Location:** `sneak-preview/sneak-gap.py` (data/results live alongside it)

## TMDB key

The German premiere dates come from TheMovieDB. The API key is read from the
project's `.env` (already set for you):

```
<project-root>/.env     # a file named .env in the project root
TMDB_API_KEY=your_key
```

It can also be supplied via the `TMDB_API_KEY` environment variable or
`--api-key`. No key → the script prints an explanatory error.

## Usage

```sh
cd sneak-preview    # from the project root

python3 sneak-gap.py                       # full analysis (uses newest export)
python3 sneak-gap.py --csv sneak-gap.csv --markdown sneak-gap.md
python3 sneak-gap.py --no-lookup           # just list the sneak films, no TMDB
python3 sneak-gap.py --export ../letterboxd-....zip
```

## Options

| Option | Meaning |
| --- | --- |
| `--export PATH` | Use a specific data export zip (auto-detected otherwise) |
| `--api-key KEY` | TMDB API key override |
| `--no-lookup` | Only list the sneak films — no TMDB requests |
| `--csv FILE` | Also write the results as CSV |
| `--markdown FILE` | Also write the results as a Markdown table |
| `--sleep SECS` | Delay between TMDB calls (default 0.25 s) |
| `--limit N` | Only process the first N films (testing) |

## Input & output

- **Input:** diary entries tagged `sneak` or `ov sneak` from the export's
  `diary.csv`; German premiere dates from TMDB (country `DE`, theatrical
  release, type 3), looked up once per film.
- **Cache:** German release lookups are cached in `sneak-cache.json`
  (gitignored) so re-runs don't hit TMDB again.
- **`--csv`** writes `sneak-gap.csv` (columns: `title, year, watched,
  kinostart, gap_days, tags, tmdb_id`), sorted by watched date (newest first).
- **`--markdown`** writes `sneak-gap.md`, a human-readable table + summary.

The terminal table is sorted by watched date (newest first) and ends with a
summary, e.g.:

```
Sneaks with a German release: 125/125
On average seen 21.2 days before the release
Biggest lead: OZI: Voice of the Forest (190 days early)
```

## Examples

```sh
# Regenerate both the CSV and the Markdown result files
python3 sneak-gap.py --csv sneak-gap.csv --markdown sneak-gap.md

# Quick preview without any TMDB calls
python3 sneak-gap.py --no-lookup

# Use a specific export
python3 sneak-gap.py --export ../letterboxd-sirconanad-2026-11-19-11-21-utc.zip
```

## Notes / troubleshooting

- A film shows **n/a** if it has no recorded German release (e.g. a 1952
  cartoon short with no German theatrical date on TMDB).
- A negative `gap_days` means the film was already on German screens when the
  "sneak" happened — typically a corrected TMDB match issue.
- `sneak-gap.csv` is consumed by `generate_site.py` for the highlights page,
  and by the deployed site (`sirconanad.github.io`).

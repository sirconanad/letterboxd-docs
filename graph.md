# `graph.py` — contribution graph

Draws a [GitHub-style contribution graph](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/managing-contribution-settings/why-are-my-contributions-not-showing-up-on-my-profile)
from your diary: each day you watched at least one film becomes a shaded
square. Output is a coloured ASCII graph on the terminal and/or a standalone
SVG file.

**Location:** `graphs/graph.py`

## Usage

```sh
cd graphs    # from the project root

python3 graph.py                 # the current year (uses newest export)
python3 graph.py --all           # whole diary, every year
python3 graph.py letterboxd-....zip --all     # explicit export
python3 graph.py --all --svg graph.svg        # also write an SVG
```

Positional argument is optional: if omitted, the newest `letterboxd-*.zip` in
the project is used automatically.

## Options

| Option | Meaning |
| --- | --- |
| `export` | Path to a Letterboxd data export zip (optional; auto-detected) |
| `--year YYYY` | Restrict to one year; repeat for more (`--year 2025 --year 2026`) |
| `--all` | Show the entire diary instead of the current year |
| `--start YYYY-MM-DD` | Graph window start date |
| `--end YYYY-MM-DD` | Graph window end date (default: today) |
| `--days N` | Only the last N days |
| `--months N` | Only the last N months |
| `--svg FILE` | Also write an SVG render (e.g. `graph.svg`) |
| `--no-color` | Plain ASCII output even on a colour terminal |

## Output

After the graph, a short summary is printed:

```
Films watched: 667 across 439 day(s) (78 rewatch)
Busiest day: 2026-01-24 (14)
Avg rating: 1.93 (388 rated)
Shown window: 2024-01-17 .. 2026-11-19
```

The SVG is a white-background graphic with one column per week, month labels
and each watched day's film count available as a tooltip.

## Examples

```sh
# This year, as an SVG for the site or README
python3 graph.py --svg graph.svg

# Years side by side
python3 graph.py --year 2025 --year 2026

# Last 6 months
python3 graph.py --months 6

# A specific export, every year, plain colours
python3 graph.py ~/Downloads/letterboxd-myaccount-....zip --all --no-color
```

## Notes / troubleshooting

- The graph only counts days, not individual films — a day with 10 films and a
  day with 1 film both light up; the intensity levels scale with your busiest
  day.
- If it prints `no diary entries found`, the export doesn't cover the requested
  year — use `--all`.
- The terminal colours require an ANSI-capable terminal; use `--no-color` for
  logs or redirects.

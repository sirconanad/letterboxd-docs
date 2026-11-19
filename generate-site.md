# `generate_site.py` — static highlights page

Builds a single self-contained `site/index.html` — a dark, mobile-friendly
highlights page with stat cards, per-year contribution graphs, a monthly
activity line chart, most-used tags, favourites and the sneak-vs-Kinostart
table. The file is deployed to GitHub Pages (`sirconanad.github.io`).

**Location:** `diary/generate_site.py`

This is the **last step** of the pipeline. It assumes the data is already
there:

- `diary/diary.json` — produced by `build.py`
- `../sneak-preview/sneak-gap.csv` — produced by `sneak-gap.py --csv`

## Usage

```sh
cd diary    # from the project root

python3 generate_site.py                # writes site/index.html
python3 generate_site.py -o out.html    # write somewhere else
```

Run `build.py` (for `diary.json`) and `sneak-gap.py --csv` (for
`sneak-gap.csv`) first, then generate the page. The script imports the
contribution-graph renderer from `../graphs/graph.py` automatically.

## Options

| Option | Meaning |
| --- | --- |
| `-o, --out FILE` | Output path (default: `site/index.html`) |
| `--diary FILE` | Diary JSON to read (default: `diary.json`) |
| `--sneaks FILE` | Sneak CSV to read (default: `../sneak-preview/sneak-gap.csv`) |

## What's on the page

- **Overview** — stat cards: films since first year, average rating, likes,
  rewatches, average sneak lead-in days
- **Cinema days** — per-year contribution graphs
- **Monthly activity** — a line chart of films watched per month
- **Most-used tags** — horizontal bars (top 12)
- **Favorites** — films watched 3+ times, shown with their movie posters
- **Sneak / OV-sneak vs. German cinema release** — the full table, sorted by
  watched date (newest first)

The page is a single file: styles are inline and the graphs are inline SVG,
so it can be opened directly or uploaded as-is.

## Example run

```sh
cd diary    # from the project root
python3 build.py --no-commit           # ensure diary.json is fresh
cd ../sneak-preview
python3 sneak-gap.py --csv sneak-gap.csv
cd ../diary
python3 generate_site.py               # writes site/index.html
```

## Deploying to GitHub Pages

```sh
cd diary    # from the project root
python3 generate_site.py
cp site/index.html ../sirconanad.github.io/index.html
cd ../sirconanad.github.io
git add index.html && git commit -m "Update highlights page"
git push origin main     # GitHub Pages serves it automatically
```

## Accessibility & SEO

The generated page includes a meta description, canonical URL, Open Graph /
Twitter tags, JSON-LD structured data, `role="img"` labelled SVGs, scoped
table headers and dark-theme colour contrast, per Google PageSpeed /
Lighthouse.

# `tags.py` — diary tag analysis

Analyses the tags on your diary entries (e.g. `schauburg`, `cinema`,
`sneak`, `ov sneak`): how often each tag is used, its frequency tier, monthly
trends, and which tags are used together (co-occurrence). Reports to the
terminal, and optionally exports JSON data and an SVG chart.

**Location:** `tags/tags.py`

## Usage

```sh
cd tags     # from the project root

python3 tags.py                        # top 10 tags in detail (newest export)
python3 tags.py letterboxd-....zip     # explicit export
python3 tags.py --top 20 --svg tags.svg --json tags.json
python3 tags.py --tag schauburg --tag cinema
```

The positional export argument is optional — the newest `letterboxd-*.zip`
in the project is used when omitted.

## Options

| Option | Meaning |
| --- | --- |
| `export` | Path to a Letterboxd data export zip (optional; auto-detected) |
| `--top N` | How many top tags to fetch in detail (default: 10) |
| `--tag LABEL` | Analyse specific tag(s) instead of the top-N; repeatable |
| `--year YYYY` | Year for the monthly trends (default: current year) |
| `--min-count N` | Ignore tags used fewer than N times |
| `--group 'REGEX=LABEL'` | Custom grouping rule, e.g. `'cinema|kino=cinemas'`; repeatable. Unmatched tags become "Other" |
| `--matrix M` | Include a co-occurrence matrix of the top M tags in the SVG (default 0 = off) |
| `--all-tags` | Analyse every tag instead of only the top-N (slower) |
| `--json FILE` | Write the analysis data as JSON |
| `--svg FILE` | Write the charts as a single SVG |

## Terminal report

The terminal output has four sections:

1. **Tags by frequency tier** — top / mid / long-tail groups
2. **Custom groups** — only if `--group` rules are given
3. **Most-used diary tags (top 20)** — horizontal bar chart
4. **Monthly usage** — per-tag month-by-month table with a trend sparkline
5. **Co-occurrence** — how many analysed entries share tag pairs

## Examples

```sh
# Analysis of the most common tags, with charts
python3 tags.py --top 20 --svg tags.svg --json tags.json

# Only the cinema-related tags
python3 tags.py --tag schauburg --tag cinema --all-tags

# Group several tags into one bucket in the SVG custom-groups chart
python3 tags.py --group 'cinema|kino=cinemas' --svg out.svg

# Co-occurrence heatmap of the top 8 tags
python3 tags.py --matrix 8 --svg out.svg
```

## Notes / troubleshooting

- Tag matching is case-insensitive but exact: `--tag cinema` does not also
  match `cinema26`.
- The JSON output shape:

  ```json
  { "export": "...", "year": 2026,
    "tags": [{"tag": "schauburg", "count": 361}, ...],
    "monthly": {"schauburg": {"label": "schauburg", "months": [...]}},
    "co_occurrence": {"schauburg + cinema": 12} }
  ```

- If a `--tag` label isn't found in the export it is reported as unknown and
  skipped.

# `build.py` — backdated git history

Turns your diary into a git history: **every film you log becomes one commit,
backdated to the day it was watched** (both author and committer timestamps
set to noon on the watch date, so GitHub's contribution graph shows it on the
right day). Also maintains `diary.json` and one markdown file per film.

**Location:** `diary/build.py` (run from inside the `diary/` git repository)

## What it does

1. Reads the newest `letterboxd-*.zip` export (`diary.csv` for entries+tags,
   `likes/films.csv` for the liked flag)
2. Skips entries that are already committed (incremental)
3. Appends new entries to `diary.json`
4. Writes one `films/YYYY-MM-DD - Title.md` per new film
5. Creates one git commit per new film, oldest first, with backdated dates

## Usage

```sh
cd /home/saltmeyer/opencode/letterboxd/diary

python3 build.py               # read export, write files, backdated commits
python3 build.py --no-commit   # read + write files only (no commits)
python3 build.py --full        # ignore committed state, treat everything as new
python3 build.py --export ../letterboxd-....zip   # explicit export
```

## Options

| Option | Meaning |
| --- | --- |
| `--export PATH` | Use a specific data export zip (auto-detected otherwise) |
| `--no-commit` | Parse the export and write files, but create no git commits |
| `--full` | Ignore what's already committed and treat every entry as new |

## Outputs

- `diary.json` — merged chronological record:
  `[{"when", "title", "year", "rating", "rewatch", "liked", "tags"}, ...]`
- `films/YYYY-MM-DD - Title.md` — one file per film containing the date,
  rating, rewatch flag, liked flag and tags. Duplicate titles on later dates
  get a `(2)`, `(3)`, … suffix.
- One git commit per new film.

## Example run

```
already committed: 667 film(s)
export letterboxd-sirconanad-....zip: 667 diary entry/entries
no new diary entries to add
```

After watching a new film, download a fresh export and run `python3 build.py`
again — only the new entry is committed:

```
already committed: 667 film(s)
export letterboxd-....zip: 668 diary entry/entries
1 new film(s): 2026-11-20 .. 2026-11-20
wrote 1 new film file(s)
  committed 2026-11-20: The Example
```

## Notes

- **Edits to existing entries are NOT picked up** — they're already committed.
  Use `--full` to rebuild all film files from the current export, or edit the
  markdown file and amend/commit manually.
- First run in an empty repo creates a root commit with `README.md`,
  `build.py` and `diary.json`.
- Commits are authored as `sirconanad <sirconanad@cock.li>` (set in this
  repo's git config).
- Download a fresh export before each run so the diary includes new entries.

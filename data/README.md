# data/

- `raw/` — as-pulled API responses, never edited by hand. **Local only: gitignored and never
  committed (D19).**
- `processed/` — cleaned, merged, pseudonymised analysis datasets produced by `scripts/`.

## Rules (see CLAUDE.md)

- The repo is public (D19). Steam IDs are replaced with salted hashes before anything is
  committed; the salt lives in `.env`. No review text in committed data.
- No file over 100 MB enters git history. Bulk raw-corpus storage is an open decision
  (DECISIONS.md O6) — do not commit bulk pulls until settled.
- Every dataset gets a dictionary entry below at creation time: name, provenance
  (endpoint/source + pull date), unit of observation, key columns.

## Data dictionary

(empty — no data yet)

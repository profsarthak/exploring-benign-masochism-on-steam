# Brief 01 — Feasibility probe for the demand-stage review pull

**Date / author:** 2026-10-24 · drafted by Claude Code for Monty; approved by Monty 2026-10-24
**Decision-log anchors:** D3 (outcome source), D7 + D19 (data location and protection),
D9 + D16 (language groups), D20 (runs locally), D21 (feasibility only, no peeking);
informs O6 (storage, pull strategy) and O7 (where the full pull runs). CLAUDE.md data rules
apply throughout.

## Objective
Find out whether the D16 design can be built from Steam's public review endpoint: what the
API actually returns, how many reviews and reviewers each D16 language has in 2021–2022,
and how long and how large a full pull would be. **No outcome is computed.**

## Estimand
No causal estimand. The probe measures feasibility quantities only (M1–M7 below).

**Off limits (D21, read strictly):** the prediction table (D18) includes *total review
volume* as an outcome (the escapism row), so the probe must not produce any pre/post
comparison by language or language group, not just no genre-split counts. Specifically:
- no genre, tag or category data joined to review data, and no `appdetails` or tag calls;
- no per-language counts split at or around 24 Feb 2022 (or any other within-window time
  series by language);
- window totals per language, and the count of reviewers active on *both* sides of
  24 Feb 2022 (an intersection count, which reveals no direction), are allowed.

## Data

**Endpoints**
- App list (sampling frame): `https://api.steampowered.com/ISteamApps/GetAppList/v2/`
  (no key). If it fails or is deprecated, **stop and report**. The fallback is
  `IStoreService/GetAppList`, which needs a Steam Web API key that Monty must obtain; it
  goes in `.env` as `STEAM_API_KEY` and is never committed or logged.
- Reviews: `https://store.steampowered.com/appreviews/<appid>?json=1` with
  `filter=recent`, `language=<code or all>`, `num_per_page=100`, `purchase_type=all`,
  `review_type=all`, `filter_offtopic_activity=0`, `cursor=*` then the returned cursor
  (URL-encoded).

**Languages (Steam API codes), 25 in total**
- Exposed (D16): `polish`, `finnish`, `romanian`, `hungarian`, `norwegian`
- Primary comparison: `spanish`, `portuguese`, `italian`, `french`, `dutch`
- Secondary comparison: `japanese`, `brazilian`, `latam`, `thai`, `vietnamese`, `indonesian`
- Intermediate: `german`, `czech`, `swedish`, `danish`, `bulgarian`, `greek`, `turkish`
- Descriptive only (D16): `russian`, `ukrainian`
- `english` is tabulated for context and for M4 only.

**Count window:** reviews with `timestamp_created` from 2021-01-01 00:00 UTC to
2022-12-31 23:59 UTC. The plan (D21) will fix the analysis window; this is a superset.

**Expected size:** about 30,000 requests in total (hard cap), roughly 12–13 hours at the
fixed pace. Raw JSON is likely a few GB, all local.

## Method

**Step 0 — Setup**
- R only (CLAUDE.md). Packages: `httr2`, `jsonlite`, `data.table`, `openssl`; record the
  environment with `renv` (`renv/library/` is already gitignored).
- Generate the salt once with `openssl::rand_bytes(32)` and store it as hex in `.env` as
  `STEAMID_SALT`. Monty backs it up privately (e.g. a password manager); losing it breaks
  linkage across pulls. Never print it.
- Hash function: `hash_id(steamid) = HMAC-SHA256(key = salt, message = steamid)`, full hex.
- Pace: one request every 1.5 s (about 200 per 5 min, the unofficial figure in CLAUDE.md).
  Do **not** stress-test beyond this. On HTTP 429 or 5xx, back off exponentially (3 s,
  6 s, 12 s … up to 5 min). If 5 consecutive requests fail after backoff, stop and report.
- User-Agent: `exploring-benign-masochism-on-steam feasibility probe
  (https://github.com/profsarthak/exploring-benign-masochism-on-steam)`. No email address.
- Resumable: checkpoint (appid, language, cursor, pages done) to `data/raw/checkpoints/`
  after every page; restarting resumes from the checkpoint.
- Logs go to `data/raw/logs/` (gitignored) and must never contain Steam IDs.

**Step 1 — Sampling frame.** Fetch the full app list; save it to
`data/raw/applist_<YYYYMMDD>.json`; record the number of appids.

**Step 2 — Volume screen.** Draw a simple random sample of **5,000 appids** (seed
`20261024`). For each, request page 1 of reviews with `language=all` and record
`query_summary.total_reviews` and success. This takes 5,000 requests.

**Step 3 — Stratified deep sample.**
- Among screened apps with `total_reviews >= 1`, define strata by total reviews:
  S1 1–99 · S2 100–999 · S3 1,000–9,999 · S4 10,000–99,999 · S5 100,000+.
- Draw up to **8 apps per stratum** (seed `20261025`); a stratum with fewer apps takes all.
- **Strategy A (one pass, all languages):** for every deep-sample app, page with
  `language=all` from newest backwards until a page's oldest review is older than
  2021-01-01 or the cursor is exhausted.
- **Strategy B (one pass per language):** for apps in S3–S5 only, page separately for each
  of the 25 languages over the same span.
- Caps: 2,500 requests per app per strategy, 30,000 in total. Process apps in random order
  within strata so the cap leaves a random subset, and record coverage if a cap binds.
- Store raw pages in `data/raw/reviews_probe/` (local only).

**Step 4 — Pseudonymise.** Build a local review-level table with `steamid` replaced by
`hash_id`, dropping `review` text and any other free text. It stays in `data/raw/` (not
committed); only the aggregates in step 5 are committed.

**Step 5 — Metrics (window totals only; see "Off limits")**
- **M1 — API behaviour:**
  - fields present per review, including `author.*`, `language`, the two timestamps,
    `steam_purchase`, `received_for_free` and `written_during_early_access`;
  - cursor termination;
  - duplicate `recommendationid`s across pages;
  - whether `filter=recent` is strictly newest-first by `timestamp_created`;
  - whether `language=X` returns only language X (compare Strategy B with Strategy A's
    tabulation on the same apps);
  - counts of HTTP errors and 429s, median latency, achieved throughput.
- **M2 — Volume per language:** for each of the 25 languages, window-total reviews and
  distinct reviewers in the deep sample. Extrapolate to the catalog with stratum weights
  (estimated frame size per stratum ÷ apps sampled), giving intervals from 1,000 bootstrap
  resamples of apps within strata. Label the result order-of-magnitude.
- **M3 — Cost model:** requests needed per app to reach 2021-01-01, related to
  `total_reviews`. Project the requests and hours for a full pull of the 25 languages
  under Strategy A and Strategy B, and the storage per million reviews as raw JSON and as
  compressed `.rds`.
- **M4 — Denominator check (feeds D21):** among reviewers with at least one target-language
  review in Strategy A data, the share of their sampled window reviews written in another
  language. Report n; if fewer than 100 such reviewers have 2 or more sampled reviews,
  report "insufficient".
- **M5 — Panel depth:** per language, distinct reviewers with at least one window review on
  **both** sides of 24 Feb 2022 (the intersection count only). Flag languages with fewer
  than 2,000 such reviewers in the extrapolated catalog as "thin". This is a screening
  heuristic; the real power analysis belongs in the plan.
- **M6 — Hashing test:** the same steamid and salt give the same hash across two runs; a
  different salt gives a different hash. Grep every file under `data/processed/`,
  `outputs/` and the logs for 17-digit IDs starting with `7656119`: there must be no
  matches.
- **M7 — Off-topic filter effect:** for 3 S5 apps (or the largest available), compare
  `query_summary.total_reviews` with `filter_offtopic_activity=0` and with the default.
  This takes 6 requests.

**Step 6 — Report.** An R script writes `outputs/brief01_feasibility_report.md` with
M1–M7, the full-pull projections for Strategy A and Strategy B, and plain recommendations
for O6 (pull strategy, storage) and O7 (where the full pull runs). The recommendations are
for Monty to decide.

## Checks / diagnostics
- Before any commit, M6 passes: no raw Steam IDs anywhere in tracked files.
- `git status` shows nothing under `data/raw/`, and `.env` is untracked.
- No genre or tag data was fetched, and no time-split counts by language exist anywhere,
  including scratch outputs (see "Off limits").
- Strategy B per-language counts match Strategy A's tabulation for the same apps and span,
  within 1%. Any larger gap is reported, not fixed silently.
- Checkpoint and resume tested once: interrupt mid-app, restart, and confirm there are no
  duplicate or missing pages.
- Every committed dataset has a `data/README.md` dictionary entry (provenance, pull date,
  unit, columns) created at the same time.

## Done-criteria
- **Scripts** (R, committed):
  - `scripts/lib_steam_api.R` (requests, pacing, backoff, checkpointing, hashing)
  - `scripts/01_pull_applist.R`
  - `scripts/01_pull_review_screen.R`
  - `scripts/01_pull_review_probe.R`
  - `scripts/02_clean_probe_pseudonymise.R`
  - `scripts/05_output_probe_report.R`
  Each states its inputs, outputs, and that it implements Brief 01.
- **Committed aggregates (no IDs, no text):**
  - `data/processed/brief01_app_screen.csv`: appid, stratum, `total_reviews`, success
  - `data/processed/brief01_app_costs.csv`: appid, stratum, strategy, requests, reached
    2021-01-01 (yes/no), cap hit (yes/no)
  - `data/processed/brief01_language_totals.csv`: language, sample reviews, sample
    reviewers, extrapolated estimates and intervals, M5 count, thin flag
- **Report:** `outputs/brief01_feasibility_report.md`.
- **Docs:** `data/README.md` dictionary entries; an AI_USAGE.md session entry (Software
  becomes Extensive once code exists).
- **Local only, never committed:** raw pages, the app list, checkpoints, logs and the
  pseudonymised review table. Delete or move them to private storage after Monty reviews
  the report (D20).
- Results go back to Monty, who uses the report to decide O6, O7 and the D21 denominator
  question. The genre rule (O4) and the analysis plan come next.

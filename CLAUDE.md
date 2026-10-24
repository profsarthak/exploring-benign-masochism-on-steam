# CLAUDE.md — Working rules for this repo

Project: Exploring Benign Masochism on Steam (Monty / sarthakvedantmohanty@gmail.com).
Read `README.md` for the theory and `DECISIONS.md` for the full design history before
doing anything.

## Universal rules

1. **R only.** All pipeline and analysis code is R unless Monty explicitly says otherwise.
2. **Never push or commit without Monty's approval.** Outline the scope of every commit
   first and wait for sign-off.
3. **DECISIONS.md is append-only.** Never delete or rewrite entries; supersede them with
   new dated entries. Every non-trivial design choice gets logged with rationale and
   rejected alternatives — this feeds the paper's methods section.
4. **Never make decisions for Monty.** Surface options with trade-offs; he decides.
   Challenge approaches that look flawed, with reasons, in plain language.
5. **Small verifiable steps.** Add checks/diagnostics when logic changes. Prefer readable
   simplicity over cleverness.
6. **Keep the sIfA log current.** Update AI_USAGE.md (sIfA log) whenever AI's role in a
   CRediT category changes, and append a session entry after each working session.

## Sources of truth

- This repo: code, data, decision log, implementation briefs.
- Drive folder "Steam Project" (not in repo): lit review, write-ups, prose drafts.
- Do not duplicate content across the two; mirror one-way only if agreed.

## Data rules (do not break)

- No file over 100 MB in git history — GitHub hard-rejects it, and undoing it requires
  history rewrites. Raw review corpus strategy (LFS / Releases / external) is an open
  decision (DECISIONS.md O6) — do not commit bulk raw pulls until it is settled.
- Steam API pulls must be incremental and resumable; respect rate limits
  (~200 requests / 5 min unofficial). Never assume a full-catalog pull fits in one run.
- Every dataset in `data/` needs an entry in `data/README.md` (provenance, pull date,
  schema) at the time it is created, not later.
- **The repo is public (D19).** Raw pulls stay in `data/raw/` (gitignored) and never enter
  the repo. Steam IDs must be replaced with salted hashes before any data is committed; the
  salt lives in `.env`, never in the repo or in logs. No review text in committed data.
- **No peeking before pre-registration (D21).** Until the analysis plan is registered, do
  not compute survival, cozy or placebo shares, or any genre-split review counts, around
  Feb 2022. Feasibility work (API behaviour, rate limits, per-language counts, game
  metadata) is fine.

## Workflow loop

Cowork (design, decisions, briefs) → this repo (execution via Claude Code) → results →
Cowork (interpretation) → new briefs. Each analysis task starts from a brief in `briefs/`
(see the template there). If a brief is ambiguous, ask; do not improvise the estimand.

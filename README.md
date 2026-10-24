# Exploring Benign Masochism on Steam

Why do people love survival crafting games — and why are studios releasing so many of them?

## Theory

Motivating hypothesis, a single causal cascade:

> rising global doom → coping via benign masochism → increased consumption of
> survival/apocalyptic games → survival crafting releases unusually successful →
> more studios enter → accelerating release rate

What the paper claims (DECISIONS.md, D17): a salient doom shock shifts gamers' reviewing
toward survival games relative to comparable gamers and to cozy games; separately, survival
hits pull in new releases. The full cascade is argued as an interpretation, not claimed as
identified. The coping mechanism could be benign masochism (pleasure in safe negative
affect) or simulation/practice; the design does not tell them apart (D9).

## Research architecture

Two separately identified stages, chained narratively (see `DECISIONS.md`, D4–D5, for why
a full-chain IV was rejected):

1. **Demand stage** — does doom shift consumption? Individual-level outcome: within-user
   survival review share, built from Steam's public review endpoint. Identification: Feb 2022
   Ukraine-invasion event study (headline) comparing threatened-but-safe language groups
   with distant ones (D16), plus a user-FE / time-FE panel with doom indices (support;
   D10, D15). Survival and cozy shares are estimated separately against a pre-registered
   prediction table, with a placebo genre (D18, D21).
2. **Supply stage** — does success drive entry? Genre-level panel and/or breakout-hit
   event studies (DECISIONS.md, O3).

## Repository structure

```
├── README.md          # this file
├── DECISIONS.md       # append-only decision log (design choices + rationale)
├── CLAUDE.md          # working rules for AI-assisted sessions
├── AI_USAGE.md        # sIfA AI-transparency log (CRediT roles + session log)
├── briefs/            # implementation briefs: one spec per analysis task
├── data/
│   ├── raw/           # as-pulled API data: local only, never committed (D19)
│   └── processed/     # cleaned, pseudonymised analysis datasets
├── scripts/           # R pipeline (R only)
└── outputs/           # figures and tables, regenerable from scripts + data
```

## Workflow

Brainstorming and design happen in Cowork sessions; decisions land in `DECISIONS.md`;
implementation briefs land in `briefs/`; Claude Code executes briefs against this repo;
results return to Cowork for interpretation. Prose deliverables (lit review, drafts) live
in a separate Drive folder, not here.

## Status

Pre-data. Literature first pass complete. Next: Brief 01 (feasibility probe only), then a
pre-registered analysis plan (D21). No pipeline runs yet.

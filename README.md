# Exploring Benign Masochism on Steam

Why do people love survival crafting games — and why are studios releasing so many of them?

## Theory

A single causal cascade:

> rising global doom → coping via benign masochism → increased consumption of
> survival/apocalyptic games → survival crafting releases unusually successful →
> more studios enter → accelerating release rate

## Research architecture

Two separately identified stages, chained narratively (see `DECISIONS.md`, D4–D5, for why
a full-chain IV was rejected):

1. **Demand stage** — does doom shift consumption? Individual-level outcome: within-user
   survival review share, built from Steam's public review endpoint. Identification
   strategy under discussion (DECISIONS.md, O1–O2).
2. **Supply stage** — does success drive entry? Genre-level panel and/or breakout-hit
   event studies (DECISIONS.md, O3).

## Repository structure

```
├── README.md          # this file
├── DECISIONS.md       # append-only decision log (design choices + rationale)
├── CLAUDE.md          # working rules for AI-assisted sessions
├── briefs/            # implementation briefs: one spec per analysis task
├── data/
│   ├── raw/           # as-pulled API data (see data/README.md for size rules)
│   └── processed/     # cleaned, merged analysis datasets
├── scripts/           # R pipeline (R only)
└── outputs/           # figures and tables, regenerable from scripts + data
```

## Workflow

Brainstorming and design happen in Cowork sessions; decisions land in `DECISIONS.md`;
implementation briefs land in `briefs/`; Claude Code executes briefs against this repo;
results return to Cowork for interpretation. Prose deliverables (lit review, drafts) live
in a separate Drive folder, not here.

## Status

Pre-data. Current deliverable: literature review. No pipeline runs yet.

# briefs/

One implementation brief per analysis task — the handoff artifact from design sessions
(Cowork) to execution (Claude Code). A brief must be specific enough that execution
requires no design decisions. If it isn't, execution stops and asks.

## Template

```markdown
# Brief NN — <short title>

**Date / author:** 
**Decision-log anchors:** (e.g., D3, O1a)

## Objective
One sentence: what question this task answers.

## Estimand
Quantity, population, period. Outcome, treatment, comparison, unit of analysis.

## Data
Inputs (with data/README.md dictionary references), sources to pull, expected sizes.

## Method
Specification, fixed effects, clustering, sample restrictions.

## Checks / diagnostics
What must be verified before results are trusted (balance, pre-trends, robustness).

## Done-criteria
Concrete artifacts: datasets, figures, tables, diagnostic notes.
```

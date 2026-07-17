# Decision Log — Exploring Benign Masochism on Steam

Purpose: record design decisions, rationale, and rejected alternatives as they are made,
for use when writing the paper. Referenced from CLAUDE.md.

Format: one entry per decision. Never delete entries — if a decision is reversed, add a
new entry superseding the old one.

---

## Decided

### D1 — Theory stated as a single causal cascade (2026-07-16)
**Decision:** The theory is one chain: rising global doom → coping via benign masochism →
increased consumption of survival/apocalyptic games → survival crafting releases unusually
successful → more studios enter → accelerating release rate.
**Rationale:** Demand mechanism is upstream of supply response; each link is separately testable.
**Alternatives considered:** Treating supply and demand stories as independent claims — rejected
as less coherent; the supply story is a consequence of the demand story.

### D2 — Project standard: empirically defensible claim (2026-07-16)
**Decision:** The project targets a claim that survives reviewer scrutiny, not a
portfolio/pipeline demonstration.
**Implication:** Designs are chosen by identification quality, not convenience.

### D3 — Individual-level outcome: within-user survival review share (2026-07-16)
**Decision:** Outcome = share of a user's reviewing activity devoted to survival games,
over time. Built from Steam's public review endpoint (author steamid, num_games_owned,
num_reviews, playtime, timestamps).
**Rationale:** Only individual-level window that survived Steam's 2018 privacy defaults.
Review timestamps are the only reliable individual-level clock.
**Known limitations (to acknowledge in paper):**
- Measures reviewing, a selected behavior, not consumption directly.
- Numerator requires matching steamids across reviews of a large catalog sample
  (tens of millions of rows), not just survival games.
**Alternatives rejected:**
- "Games bought per year": purchase dates not observable via API; libraries private by
  default since 2018; GetOwnedGames returns nothing for private profiles.
- SteamSpy owner estimates: unreliable post-2018.

### D4 — IV (doom instrumenting consumption → releases) rejected (2026-07-16)
**Decision:** Do not use global doom as an instrument for consumption in a regression of
release rate on consumption.
**Rationale:** Exclusion restriction indefensible — doom affects releases directly, bypassing
observed consumption, through (a) developers' own zeitgeist-driven creative choices (the
theory applied to developers), and (b) studios greenlighting on *anticipated* demand from the
same doom signals. Additional problems: 1–4 year development lags smear timing into an
unidentifiable distributed-lag structure; supply varies only over time, collapsing the design
to one trending global time series that IV does nothing to fix.
**Recorded concession:** No defensible version of "doom doesn't directly move developers"
was found. (Monty, 2026-07-16.)
**Note:** The underlying instinct was correct — the consumption → releases regression IS
endogenous (reverse causality; common shocks like breakout hits). The instrument was the
problem, not the diagnosis.

### D5 — Architecture: two linked designs, chained narratively (2026-07-16)
**Decision:** Replace the full-chain IV with two separately identified stages:
- **Demand stage:** doom → survival consumption (individual-level outcome per D3).
- **Supply stage:** consumption/success signals → studio entry.
Each stage defensible on its own terms; the chain is argued narratively, not econometrically.
**Follows from:** D4.

### D6 — Workflow architecture: Cowork ↔ Drive ↔ GitHub ↔ Claude Code loop (2026-07-16)
**Decision:**
- **Drive folder ("Steam Project")** holds human-facing outputs: Lit Review/, Write-ups/.
- **GitHub repo** `exploring-benign-masochism-on-steam` holds code, pipeline, data, decision
  log, and implementation briefs.
- **Pipeline loop:** brainstorm/design in Cowork → decisions and briefs stored in repo →
  Claude Code executes against the repo → results return to Cowork for interpretation →
  new decisions. Interdependent loop, not a one-way handoff.
**Handoff artifacts:** decision log entries (this file) and implementation briefs
(`briefs/`, one spec per analysis task).

### D7 — Sources of truth and data location (2026-07-16)
**Decision:** Repo is source of truth for code, data, DECISIONS.md, CLAUDE.md, briefs.
Drive is source of truth for prose (lit review, write-ups). Data lives in the Git repo.
Main pre-data deliverable: literature review (Drive folders created 2026-07-16, material
to be pulled in a dedicated session).
**Caveat carried into O6:** GitHub blocks files >100 MB and repos degrade past a few GB;
the raw review corpus will exceed this. Bulk raw data needs LFS / Releases / external
archive — mechanism still open. "Data in repo" is settled for processed/analysis datasets.

### D8 — Execution handoff model (2026-07-16)
**Decision:** Cowork and Claude Code manage the project in tandem; Monty guides on intuition
and decision questions. Initial repo push happens via Claude Code locally, pointed at the
Drive folder (`_repo-staging/`, scope in PUSH_INSTRUCTIONS.md — this entry records Monty's
approval of that commit scope).
**Note:** Monty operating Claude Code locally softens the strict no-local-files constraint
to "nothing lives outside Drive/GitHub"; where the *pipeline* physically runs remains open (O7).

### D9 — Demand-stage identification: both designs (2026-07-16, resolves O1)
**Decision:** Run both — (b) event study around the Feb 2022 Ukraine invasion as the
headline specification, and (a) user FE + time FE panel with GPR country×month as the
supporting/robustness design. Survival share contrasted against cozy games as the
theoretically motivated control genre (escapism literature makes the genre contrast
non-optional: general escapism predicts all-genre rise; benign masochism predicts
reallocation toward survival).
**Location proxy strategy (tiered):**
- Review language is the assignment variable; restrict or tier by mapping quality:
  languages with clean 1:1 country mapping (e.g., Polish, German, Turkish, Korean,
  Japanese, Finnish, Ukrainian, Russian) form the clean analysis set; English pools many
  countries and is either excluded or treated as a population-weighted aggregate exposure.
- loccountrycode (public profiles only) used as validation subsample, not primary
  assignment (selection + self-report staleness).
- Measurement-error direction noted: classical misassignment attenuates toward zero
  (conservative); non-classical channels (strategic language switching post-2022) flagged
  for diagnostics.
**Mechanism honesty (from lit review):** benign masochism (Rozin) and simulation/practice
(Scrivner et al. 2021) are observationally equivalent in this design; the paper tests the
demand response and stays agnostic between micro-mechanisms unless review-text analysis
separates pleasure-language from preparation-language.

### D10 — Doom operationalization: GPR primary, others as separate robustness (2026-07-16, resolves O2)
**Decision:** Caldara–Iacoviello GPR (monthly, country-specific, 1985+, free) as primary
doom measure. OECD Consumer Confidence and Google Trends anxiety terms as robustness —
reported as separate specifications (one index per column), NOT blended into a composite.
**Rationale on composites (recorded for methods section):** averaging standardized indices
reduces noise only when components measure the same latent construct with independent
errors; GPR / CCI / Trends measure different doom flavors (geopolitical, economic, health/
general anxiety), so a composite blurs constructs and invites weight-choice criticism.
Separate columns are more informative (show which doom flavor drives results). If a single
number is ever needed, a pre-specified z-score average goes in an appendix only.
**Google Trends caveat:** term list is a researcher degree of freedom — pre-specify before
estimation.

### D11 — AI-usage transparency via sIfA (2026-07-16)
**Decision:** Adopt the Busara Center's sIfA (Statement of Intellectual Fellowship and
Accountability) as the project's AI-transparency instrument, maintained as AI_USAGE.md in the
repo root alongside DECISIONS.md. Structure: CRediT's 14 contributor roles rated
zero/light/extensive with explanations and audit trails; no numeric scoring (per sIfA design
philosophy); living document updated each session from project start.
**Rationale:** Empirically defensible work (D2) increasingly includes AI-use disclosure;
sIfA is open-source, built on the familiar CRediT taxonomy, and designed for exactly this
kind of ongoing human–AI research collaboration. Starting in week one avoids end-of-project
reconstruction, which sIfA's designers warn against.
**Maintenance rule (also added to CLAUDE.md by Code):** update AI_USAGE.md whenever AI's role
in a CRediT category changes; append a session entry after each working session.
**References:** https://busara.global/our-works/the-sifa-tool-for-a-statement-of-intellectual-fellowship-and-accountability/ ;
https://onthinktanks.org/articles/knowledge-creation-in-the-age-of-ai/ ; https://credit.niso.org/

### D12 — Literature corpus, naming convention, and reading workflow (2026-07-16)
**Decision:**
- Citation-key naming convention for all sources: `AuthorYear_ShortTitle.pdf`
  (disambiguation rule in force: `YeeSng2022` = Andrew Z.H. Yee & Sng; `Yee2006` = Nick Yee).
- Reading tracked in a synthesis-matrix Google Sheet (litreview_matrix, Drive/Lit Review):
  one row per source with findings in own words, limitations, and relevance tagged to
  decision-log entries; plus a paper×theme synthesis tab. Prose drafts in Drive
  (Literature Review.gdoc) per D7 sources-of-truth split.
- Corpus status: 12 PDFs acquired (buckets A–E); 2 requested from authors (escapism/COVID
  papers, bucket B); supply-stage (F) and Steam-methods (G) sweeps deliberately deferred
  until after the first reading pass.
**Gate:** Brief 01 (demand-stage pilot) is written only after Monty's reading pass — the
Scrivner et al. full text already shows outcome-specific pathways (distress vs. resilience
vs. preparedness) that should inform how the demand-stage outcome is specified.

---

## Open

### O1 — Demand-stage identification strategy
Candidates on the table:
- (a) Panel FE: user FE + time FE + country×time doom index; identification from
  differential doom across countries. Threats: country-specific gaming trends, regional
  pricing changes; location proxy (review language / loccountrycode) is coarse.
- (b) Event study around discrete doom shocks (Ukraine invasion Feb 2022 preferred over
  COVID — salience shock without a leisure-time shock), survival share vs. control genre.
- (c) Combination: (b) headline, (a) supporting.
Falsification requirement: cozy games as theoretically motivated control genre (comfort-coping
predicts the opposite allocation — this is what makes the theory falsifiable).
**Status:** Awaiting Monty's call, including comfort with the coarse location proxy.
**Resolved:** by D9 (2026-07-16) — both designs: (b) headline, (a) supporting.

### O2 — Operationalization of "global doom"
Candidates: Geopolitical Risk Index (Caldara & Iacoviello), consumer confidence indices,
country-level Google Trends anxiety terms, survey-based anxiety prevalence.
Choice interacts with O1 (needs country×time variation for design (a)).
**Resolved:** by D10 (2026-07-16) — GPR primary; CCI and Trends as separate robustness columns.

### O3 — Supply-stage design
Candidates:
- (a) Genre-level panel: entry (new releases in genre-quarter) on lagged genre demand
  signals, genre + time FE. Cross-genre variation breaks the single-time-series problem;
  time FE absorb Steam Direct (2017), Early Access (2013), engine democratization.
- (b) Breakout-hit event study: unexpected survival hits (e.g., Valheim, Feb 2021) as
  demand-signal shocks; genre entry response over following 12–24 months vs. control genres.
- (c) Both.

### O4 — Genre definition
How to classify "survival crafting" from noisy user-generated Steam tags; boundary with
survival horror, battle royale, open-world crafting. Needs a documented, reproducible rule.

### O5 — Consumption proxy & time granularity
Review counts vs. Boxleiter-style owner estimates vs. concurrent players; monthly vs.
quarterly. Interacts with O1/O3.

### O6 — Data & pipeline infrastructure
Settled: GitHub-only hosting for pipeline; data in repo for processed datasets (D7).
Still open: bulk raw-corpus storage (LFS vs. Releases vs. external archive such as Zenodo);
GitHub Actions scheduling vs. 6h job limits; incremental scrape strategy under Steam API
rate limits; whether to seed history from an existing public reviews dump; R pipeline
tooling (targets?).

### O7 — Where pipeline execution physically runs
Claude Code runs locally, which puts data transiently on the laptop — in tension with the
no-local-files constraint. Options: (a) GitHub Actions runs the pipeline on schedule,
Claude Code only authors code; (b) accept transient local runs.

### O8 — Feb 2022 event study: exposure groups & the Russia payment confound (new)
The invasion shock hits Russian users through a second channel: sanctions and payment
disruptions (Visa/Mastercard exit, Steam payment friction) mechanically suppressed Russian
purchases — confounding doom with ability to buy. Options to settle before the brief:
(a) define treated groups as high-threat-perception, no-payment-friction languages
(Polish, Finnish, Baltic, German) vs. low-exposure controls (e.g., Japanese, Spanish,
Portuguese-BR); (b) for Russian/Ukrainian users, use engagement with already-owned games
(review timing, playtime) rather than new acquisition; (c) drop RU/UA from the treated set
entirely and treat them as a separate descriptive case. Monty to decide at brief-writing.

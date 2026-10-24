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

### D13 — First-pass literature matrix delegated to AI (2026-10-23; amends D12 workflow)
**Decision:** Monty directed Claude Code to complete the first pass of litreview_matrix
(every empty Papers cell in columns I–N, plus the whole Synthesis tab) from the full texts of
all 15 sources, with no AI markers inside the sheet. Cell-level authorship is recorded in
AI_USAGE.md (Session 4) instead. Status/Priority columns keep tracking Monty's own reading.
**Alternatives offered at the time:** (a) fill everything with in-cell AI markers;
(b) AI fills only extraction columns (purpose, method, limitations, quotes) and leaves
findings, relevance and synthesis to Monty; (c) fill everything without markers (chosen).
**Rationale:** Monty's instruction; no further rationale was recorded in the session.
**Open consequence:** D12's gate says Brief 01 waits for "Monty's reading pass". Whether
Monty's review of the AI-drafted matrix satisfies that gate is not decided here.
**Corpus update (amends D12 status):** 15 PDFs (F1, F2, G1 added Aug 2026).

### D14 — Brief-01 gate defined; corpus housekeeping (2026-10-23; settles the D13 gate question)
**Decision (gate):** D12's Brief-01 gate is met once Monty has personally read Caldara2022
and YeeSng2022 and finished Cahill2025 (Rozin2013 and Scrivner2021 are already read), and has
reviewed the AI-drafted matrix rows for the remaining sources.
**Rationale:** these are the sources that shape Brief 01 directly: Caldara is the doom
measure (see D15), Yee & Sng the cozy control, Cahill the escapism rival, and Scrivner's
outcome pathways were the original reason for the gate. The rest inform background and the
supply stage.
**Alternatives considered:** (a) reviewing the AI-drafted matrix alone satisfies the gate;
(c) Monty reads all sources before Brief 01.
**Decision (housekeeping):**
- Cite keys and PDF names conformed to the D12 convention: Aarseth2013 → Backe2013 (first
  author is Backe); BerryReiss2006 → BerryReiss2007 (cite the published Handbook chapter;
  the PDF on file is the 2006 "do not cite" draft); BresReiss1991.pdf →
  BresnahanReiss1991_...; ONeill2016.pdf given its short title.
- Juul2017 dropped from the corpus: it is about the scholarly framing of game culture, not
  darker content or player demand, so it is not evidence for D1. PDF kept on file, uncited.
  The matrix now holds 14 sources.

### D15 — Country-GPR caveat: state it, lean on local measures, add a proximity check (2026-10-23, resolves O9)
**Decision:** Keep country GPR as specified in D10 and state its limitation explicitly (O9
option i). Give more weight to the locally sourced measures already in D10, country Google
Trends and OECD CCI (option iii). Add a predetermined, proximity-based exposure definition
(option ii, e.g. distance to the conflict, fixed before Feb 2022) as one robustness check,
not as a main measure.
**Rationale:** the headline event study (D9) times the shock by the invasion itself and does
not rely on country GPR, so the caveat mainly affects the supporting panel. Options (i) and
(iii) use data already planned in D10 and avoid hand-picking exposed countries; the proximity
check answers the objection that every continuous measure reacts to the same events.
**Counter-case recorded (for the methods section):** country GPR measures US-press
attention and mis-ranks exposure (aggressor vs. threatened neighbour), so its error is not
classical and D9's "misassignment attenuates toward zero" argument does not cover it.
Google Trends is weak where Google is not the main search engine (Russia: Yandex; South
Korea: Naver), is rescaled per country and resampled per download, and needs translated,
pre-specified terms. OECD CCI measures economic sentiment; in 2022 it moved with inflation
and energy prices, which hit the near-war countries hardest, and its coverage is mainly
OECD members. The proximity check has confounds of its own (energy prices, refugee inflows),
and its exposure rule must be fixed before outcomes are examined.
**Implementation notes for Brief 01:** pre-specify the Trends term list and translations
(D10 caveat); treat Russian- and Korean-language Trends series as low-coverage; pre-register
the proximity rule. O8 (exposure groups and the Russia payment confound) remains open.

### D16 — Feb 2022 exposure groups; Russian/Ukrainian set aside (2026-10-23, resolves O8; corrects D9's language list)
**Decision:**
- **Exposed group (main estimate):** national languages of countries sharing a land border
  with Russia, Belarus or Ukraine that are Steam-supported languages: Polish, Finnish,
  Romanian, Hungarian, Norwegian. These users faced the threat, were physically safe, and
  had no payment disruption. The rule is fixed in advance, as D15 requires.
- **Comparison groups, two tiers:** primary = Western/Southern European languages (Spanish
  (Spain), Portuguese, Italian, French, Dutch), which shared the 2022 energy and inflation
  shock with less direct threat; secondary = distant languages (Japanese, Portuguese-Brazil,
  Spanish-Latin America, Thai, Vietnamese, Indonesian).
- **Intermediate languages** (German, Czech, Swedish, Danish, Bulgarian, Greek, Turkish)
  stay out of the binary contrast and enter the graded-distance robustness check (D15).
- **Russian and Ukrainian** are excluded from the main estimate (O8 option c) and reported
  descriptively, using engagement with already-owned games as the measure (option b).
**Rationale:** (1) the payment shock O8 names; (2) Belarusian and Kazakh are not
Steam-supported languages, so those players review in Russian, as do many Ukrainians, and
the Russian-language group mixes countries with opposite exposure; (3) many Ukrainians
switched from Russian to Ukrainian after the invasion, so group membership moves with the
treatment; (4) benign masochism requires a safety frame (Rozin 2013), which people in a war
zone lack. Using Western/Southern Europe as the primary comparison targets the
energy/inflation confound recorded in D15.
**Alternatives considered:** O8 option (a) as written. Its "Baltic" group cannot be built,
because Estonian, Latvian and Lithuanian are not Steam-supported languages (Steamworks
lists them as game-only). Keeping RU/UA in the treated set was rejected for the reasons
above.
**Correction to D9 (supersedes in part):** Russian is not a clean 1:1 language (see
rationale 2). German pools Germany, Austria and Switzerland; Germany dominates, and German
now sits in the intermediate tier.
**Trade-off and fallback:** the treated group is smaller and loses the most intensely
affected populations. Brief 01 must count active reviewers per language for 2021–2022; if
the treated group is too thin, the fallback is the graded-distance specification with
intermediate languages included.
**Gate status (D14):** met 2026-10-23. Monty has read Caldara2022, YeeSng2022 and
Cahill2025 and agrees with the matrix synthesis.

### D17 — Claim scope (2026-10-24)
**Decision:** The paper claims only what the design identifies:
1. After the Feb 2022 invasion, reviewers in threatened-but-safe language groups (D16)
   shifted their review share toward survival games, relative to the comparison groups and
   relative to cozy games.
2. If the supply stage succeeds: survival breakout hits are followed by more survival
   releases than in comparable genres.
The full cascade (D1) is argued as an interpretation consistent with these results, not
claimed as identified. The paper does **not** claim that doom explains the decade-long rise
in survival releases; that question is the motivation, not a finding.
**Rationale:** D4 already concedes that doom-trend → release-trend collapses to one trending
time series and cannot be identified. Overclaiming is the first point a reviewer attacks
(D2).
**Alternatives considered:** claiming the full cascade with caveats (rejected: not
identified); dropping the release-trend question altogether (rejected: it is the
motivating puzzle).
**Wording consequence:** README and paper describe the mechanism as dark-content /
hardship coping, with benign masochism (Rozin) and simulation/practice (Scrivner) as
candidate explanations, consistent with D9. D1 stands as the motivating hypothesis.

### D18 — Cozy is a rival-prediction genre, not a control; prediction table and placebo genre (2026-10-24; supersedes the "control genre" wording in O1 and D9)
**Decision:** Survival and cozy review shares are estimated separately, each relative to
all other genres. Cozy is a rival-prediction genre (comfort coping predicts it rises), not
a control. The pre-registered prediction table (exposed vs. comparison groups, post vs.
pre):

| Survival share | Cozy share | Total reviews | Reading |
|---|---|---|---|
| up | no change | any | dark-content / hardship coping (benign masochism or simulation) |
| no change | up | any | comfort coping |
| up | up | any | both channels (heterogeneous users) |
| no change | no change | up | general escapism (level effect only) |
| no change | no change | no change | no detectable response |

A **placebo genre** that no hypothesis predicts will move (e.g. sports or racing; exact
choice fixed with the genre rule, O4) is added as a check on denominator shocks such as
Elden Ring's launch on 25 Feb 2022.
**Rationale:** a control must be unaffected by the treatment. Subtracting a genre that also
responds can hide a real effect (both rise, so the difference is near zero). The table
states in advance which results count against the theory.
**Alternatives considered:** survival-minus-cozy as the headline contrast (rejected for
that reason).

### D19 — Data protection for a public repo (2026-10-24; amends D7, partly settles O6)
**Decision:**
1. Raw pulls never enter the repo (`data/raw/` is gitignored).
2. Steam IDs are replaced by salted hashes before any data is saved to the repo; the salt
   is secret and kept outside the repo (e.g. `.env`, already gitignored).
3. No review text is committed, only variables derived from it.
4. D7's "data lives in the repo" now means processed, pseudonymised data only.
Where raw data is stored privately remains part of O6.
**Rationale:** the repo is public; a Steam ID leads to a public profile; the D16 exposed
group is entirely EU/EEA, so GDPR applies (a cautious default, not legal advice). Plain
hashes are reversible because Steam IDs are assigned sequentially from a known base
(O'Neill et al. 2016), hence the salt. Git history is effectively permanent.
**Alternatives considered:** plain (unsalted) hashing (rejected: reversible); making the
repo private (not chosen in this session).
**Cost:** outsiders cannot re-link records to accounts, by design; replication runs through
the published scraping and hashing code.

### D20 — The Brief 01 pilot runs locally (2026-10-24; partly settles O7)
**Decision:** The pilot runs on Monty's laptop as a short-lived local run. Raw files stay in
the gitignored `data/raw/` and are deleted or moved to private storage afterward. Scripts
are written so the same R code can later run on GitHub Actions. Where the full pull runs is
decided after the pilot, using its measured timings and sizes.
**Rationale:** the pilot is small and interactive (watching rate limits and errors);
debugging on Actions is slow; Actions logs on a public repo are public, so a careless log
line could leak identifiers. Consistent with D8's allowance for transient local runs.
**Alternatives considered:** running the pilot on GitHub Actions (rejected for these reasons).

### D21 — Pre-registration; Brief 01 is a feasibility probe only (2026-10-24)
**Decision:** A pre-analysis plan is registered on OSF (embargo allowed) before any outcome
data is pulled or computed, and mirrored in the repo. The plan locks:
- the outcome definition (D3, including whether the within-user denominator is limited to
  the user's reviews in the target language);
- the genre rule (O4), including the placebo genre;
- the language groups (D16) and the event window;
- the specification, with event-study leads and lags;
- the inference method for few clusters (wild-cluster bootstrap or randomization inference);
- the prediction table (D18);
- the robustness list (including the D15 proximity rule and an Elden Ring exclusion);
- the Google Trends terms and translations (D10, D15).

**Allowed before registration:** API behaviour, rate limits, counts of reviews and reviewers
per language, and game metadata and tags. **Not allowed:** survival, cozy or placebo shares,
or any genre-split review counts around Feb 2022. Brief 01 is therefore a feasibility probe
only.
**Sequence:** Brief 01 (feasibility) → genre rule from metadata (O4) → plan written and
registered → Brief 02 (full pull and estimation).
**Rationale:** many researcher choices could be tuned to the result; D2's standard; D10 and
D15 already require pre-specification; OSF registration is the norm in this literature
(Scrivner et al. posted data and code there). Deviations remain allowed if reported.
**Alternatives considered:** plan committed only to the public repo (cheaper and
timestamped, but git history can be rewritten); no pre-registration (rejected given D2).

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
**Partly resolved:** by D19 (2026-10-24): raw data stays out of the public repo and Steam IDs
are salted-hashed. Where raw data is stored privately is still open.

### O7 — Where pipeline execution physically runs
Claude Code runs locally, which puts data transiently on the laptop — in tension with the
no-local-files constraint. Options: (a) GitHub Actions runs the pipeline on schedule,
Claude Code only authors code; (b) accept transient local runs.
**Partly resolved:** by D20 (2026-10-24): the Brief 01 pilot runs locally; where the full
pull runs is decided after the pilot.

### O8 — Feb 2022 event study: exposure groups & the Russia payment confound (new)
The invasion shock hits Russian users through a second channel: sanctions and payment
disruptions (Visa/Mastercard exit, Steam payment friction) mechanically suppressed Russian
purchases — confounding doom with ability to buy. Options to settle before the brief:
(a) define treated groups as high-threat-perception, no-payment-friction languages
(Polish, Finnish, Baltic, German) vs. low-exposure controls (e.g., Japanese, Spanish,
Portuguese-BR); (b) for Russian/Ukrainian users, use engagement with already-owned games
(review timing, playtime) rather than new acquisition; (c) drop RU/UA from the treated set
entirely and treat them as a separate descriptive case. Monty to decide at brief-writing.
**Resolved:** by D16 (2026-10-23): rule-based near-war exposed group, two-tier comparison,
RU/UA set aside (c) and reported descriptively (b).

### O9 — Country GPR reflects US-press attention, not local doom (new, 2026-10-23)
Found in the full text of Caldara & Iacoviello (2022): country-specific GPR indices count
articles in three US newspapers that pair geopolitical terms with a country's name, and
"capture the US perspective on risks posed by, or involving, the country" (p. 1208).
Implications: (a) the index proxies US-press salience, not residents' perceived threat;
(b) an aggressor (Russia, 2022) and a threatened neighbour both spike, so country GPR alone
cannot rank O8 exposure groups; (c) a US-centred English-language source sits awkwardly
with language-based user assignment (D9). Options to weigh before Brief 01: (i) keep
country GPR and state the caveat; (ii) use global GPR or its threats sub-index (GPT)
interacted with a predetermined geographic exposure (e.g., proximity to the conflict);
(iii) lean more on locally sourced robustness measures already in D10 (country Google
Trends, OECD CCI); (iv) a combination. Does not reverse D10. Monty to decide.
**Resolved:** by D15 (2026-10-23): (i) + (iii), with (ii) as a robustness check.

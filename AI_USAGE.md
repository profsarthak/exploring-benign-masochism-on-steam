# Statement of Intellectual Fellowship and Accountability (sIfA)

## Project: Exploring Benign Masochism on Steam

This project maintains an AI-usage transparency log based on **sIfA**, the open-source tool
from the [Busara Center for Behavioral Economics](https://busara.global/our-works/the-sifa-tool-for-a-statement-of-intellectual-fellowship-and-accountability/)
(led by Mareike Schomerus; launched at the On Think Tanks conference — [session summary](https://onthinktanks.org/articles/knowledge-creation-in-the-age-of-ai/)).
sIfA maps AI involvement onto the 14 roles of the [CRediT taxonomy](https://credit.niso.org/),
rating each as **zero / light / extensive**, with explanations of the kind of use, tools,
reason, and audit trail. Per sIfA's philosophy, no numeric scores: the aim is honest
reflection on whether AI shaped *substantive decisions*, not false precision. This is a
living document, updated from project start — not reconstructed at the end.

**Human accountability:** All design decisions are made by Monty (S.V. Mohanty). AI proposes,
challenges, and executes; the human decides. See DECISIONS.md for the decision-level audit
trail; rule 4 in CLAUDE.md ("never make decisions for Monty") is the operating principle.

**AI tools in use:** Claude Fable 5 via Cowork (design sessions, literature work,
documentation); Claude Code (repo execution, R pipeline authoring). Referred to below as
"Cowork" and "Code."

## CRediT role log

Last updated: 2026-10-23 (project pre-data phase)

| CRediT role | AI use | Explanation & audit trail |
|---|---|---|
| Conceptualization | **Light** | Core theory (doom → benign masochism → survival-game consumption → studio entry) is Monty's. Cowork restated it as a testable cascade, surfaced the rival escapism hypothesis and the simulation-vs-masochism mechanism distinction. Audit: DECISIONS.md D1, D9; session transcripts. |
| Methodology | **Extensive** | Cowork proposed candidate identification strategies, diagnosed the IV exclusion failure, proposed the two-stage architecture and genre-contrast falsification design. All choices made by Monty with reasoning recorded. Audit: DECISIONS.md D4, D5, D9, D10. |
| Software | **Not yet — anticipated Extensive** | R pipeline will be authored primarily by Code from human-approved briefs (briefs/). Audit: git history, brief-to-script mapping. |
| Validation | **Not yet — anticipated Light/Extensive** | Planned: AI-run diagnostics specified in briefs; human review of all results before acceptance. To be updated when analysis begins. |
| Formal analysis | **Not yet** | Estimation not begun. Specifications will be fixed in briefs before execution. |
| Investigation | **Extensive** | Literature search and retrieval performed by Cowork (web search, open-access fetching); source verification flags maintained. Feasibility probes of Steam API endpoints by Cowork. Since 2026-10-23 also the first-pass reading: Code extracted and synthesized all 15 sources into litreview_matrix at Monty's direction (cell-level record in the Session 4 entry below). Audit: Lit Review/Reading List & Acquisition Map.md; annotated bibliography; litreview_matrix version history. |
| Resources | **Light** | Cowork identified data sources (Steam review endpoint, GPR index, OECD CCI); Monty selected among them (D10). |
| Data curation | **Not yet — anticipated Extensive** | Scrape and cleaning pipeline to be AI-authored per data rules in CLAUDE.md; provenance entries in data/README.md required at creation. |
| Writing — original draft | **Not yet** | Policy: to be logged when drafting begins; prose deliverables live in Drive (Write-ups/). |
| Writing — review & editing | **Not yet** | — |
| Visualization | **Not yet — anticipated Extensive** | Figures expected to be AI-generated from scripts; human-approved. |
| Supervision | **Zero** | Human throughout. Monty directs all sessions and approves all commits. |
| Project administration | **Extensive** | Cowork maintains the decision log, repo structure, briefs workflow, and this sIfA log. Audit: git history; DECISIONS.md D6–D8. |
| Funding acquisition | **n/a** | Unfunded personal research project. |

## Session log (append per working session)

### 2026-07-16 — Design sessions 1–2 (Cowork) + initial repo push (Code)
Theory formalized; IV rejected; two-stage architecture adopted; O1/O2 resolved (D9, D10);
repo scaffolded and pushed; first-pass lit sweep and acquisition map; sIfA log adopted.
AI use this session: methodology sparring, literature search/retrieval, documentation
authoring, repo scaffolding. All decisions Monty's; full reasoning in DECISIONS.md.

### 2026-07-16 — Session 3 (Cowork): literature acquisition & tracking infrastructure
Lit sweep executed by AI (search, open-access retrieval, full-text extraction of
Pérez-Latorre 2019, Rozin 2013, Scrivner et al. 2021); 12 uploaded PDFs identified and
renamed to citation convention via Drive connector; synthesis-matrix tracker built
(xlsx via openpyxl + verified Google Sheet; one corrupted upload caught by read-back
verification and discarded). sIfA adopted and logged (D11). CRediT deltas: Investigation
remains Extensive; Project administration remains Extensive; no change elsewhere —
reading and synthesis of the literature is Monty's (human) task, by design.

### 2026-10-23 — Session 4 (Code): literature matrix first pass
At Monty's direction ("finish filling in my lit review sheet"), Code read the full text of all
15 PDFs and filled every empty cell in litreview_matrix. Monty chose to have no AI markers
inside the sheet, so this entry is the authorship record:
- **AI-written (this session):** Papers tab columns I–N for Yee2006, Melodia2022,
  PerezLatorre2019a/b, Farca2016, Aarseth2013, Juul2017, YeeSng2022, Caldara2022,
  BresnahanReiss1991, BerryReiss2006, ONeill2016; columns M–N for Rozin2013 and Cahill2025
  (76 cells; quotes carry page numbers); Link/DOI for 4 rows; all Synthesis-tab cells except
  Scrivner2021's two (133 cells; "—" means not addressed).
- **AI-written earlier:** the Scrivner2021 worked example (Session 3).
- **Human-written:** Rozin2013 and Cahill2025 columns I–L.
- **Corrections:** shifted metadata in the BerryReiss2006 and ONeill2016 rows (authors, year,
  title, venue); citations verified for Bresnahan & Reiss 1991, Berry & Reiss 2007, Backe &
  Aarseth 2013 and O'Neill et al. 2016. Status/Priority columns left for Monty.

CRediT deltas: Investigation stays Extensive but now covers first-pass reading, extraction
and synthesis notes. This supersedes the Session 3 note that reading and synthesis would be
human-only (see DECISIONS.md D13). No change elsewhere.

### 2026-10-23 — Session 4, continued (Code): decisions and corpus housekeeping
At Monty's request Code explained the open decisions in plain language, with options and
recommendations, and set out the counter-case to his O9 choice. Monty decided D14 and D15.
Code then renamed four PDFs through the Drive connector (same file IDs), updated cite keys on
both matrix tabs (Backe2013, BerryReiss2007; Backe listed as first author), removed one
sentence made obsolete by the rename (Backe2013 Limitations), and deleted the Juul2017 rows
from both tabs. CRediT: no change. The decisions are Monty's; the AI work here is
documentation and project administration.

### Meta-disclosure
This sIfA log itself was drafted by AI (Cowork) from Busara's published descriptions of the
tool, and reviewed by Monty — consistent with sIfA's own suggestion that AI may assist in
completing the disclosure.

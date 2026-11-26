# Chapter 1 — Critical Review

**Project:** Agentic Framework for Healthcare Clinical Decision Support (GP1, Research Track)
**Documents:** `Chapter1_Final.docx` / `.pdf` (19 pages, 40 references), built from `Chapter1_Evidence_Report.docx`, which is kept unchanged.
**Date:** 26 November 2026

## 1. Files inspected and missing inputs

| Input | Status |
|---|---|
| `Chapter1_Evidence_Report.docx` (and its Markdown source) | Used as the primary research document |
| Official template `#F15-CS-1.docx` | Used: cover kept, chapter inserted after it |
| `Seminar.pdf` (department Research Track structure) | Used: sections 1.1–1.5 follow it |
| Approved proposal (`02-Research Track Form 3.docx`) | Used; superseded on scope by supervisor decisions (see Section 6) |
| `Decision workflow · English.pdf` and `Stage1_Project_Audit.md` | Used for the architecture and the decision log |
| Official citation style | **Not specified anywhere.** IEEE is used as a recommendation; it matches the supervisor's sample report (FYP2-5019). |
| Body-page formatting rules | The template gives only "font size 12, 1.5 line spacing" for its own pages. Body margins, table text size and reference formatting are therefore my choices (see Section 7). |

## 2. What was changed

- The thematic evidence report was rebuilt into the five official sections, with one central problem, four sub-problems (P1–P4), and a clear separation between what the literature shows, what is uncertain and what the project will test (Table 1.1).
- Section 1.3 connects each design mechanism to its rationale and to what is tested, without claiming any effect (Table 1.2).
- Section 1.4 now has a main objective, six specific objectives, four research questions and a traceability table (Table 1.3).
- Section 1.5 gained explicit assumptions (A1–A5), honest statements of the decisions still open, and a scope table (Table 1.4).
- The verification codes (FT/ABS/SEC/UNV/PREPRINT/REG) were taken out of the text. They are kept in the reference audit and the claim ledger.

## 3. What was added

- A precise definition of "agentic framework", so the term does not imply autonomy.
- Definitions of the five statuses, of "red flag" and of the order of precedence (rules → calculators and pack rules → evidence → LLM judgement).
- A baseline that gives the single agent the same verifier, so the verifier's effect can be separated from the multi-agent structure (RQ2).
- Coverage of decision quality and over-abstention (RQ1), so that a system that always abstains cannot score well.
- An ablation question (RQ4) on the critic, the verifier and the safety checks.

## 4. What was removed, and why

| Removed | Reason |
|---|---|
| "49–96% of alerts overridden" (van der Sijs 2006) | Not verified from the primary source |
| Pooled OR 1.35 for medical RAG (JAMIA 2025) | Seen only on a blog |
| Almanac figures; MetaMedQA figures | Secondary sources only |
| Liang et al. 2026 (JMIR AI) | Final title and version not confirmed, although methodologically relevant |
| ChatGPT-3.5 reference-fabrication study, BioRAB, scaling-agents preprint, MedHELM | Not needed for the argument, or version/venue unresolved |
| McNemar citation and statistical-test details | Belong to Chapter 3 |
| The report's research-process notes, "claims to avoid" and "items to verify" tables | Internal material; moved to the audit files |

## 5. Claims corrected, weakened or qualified

- **FDA time-critical question (resolved):** the 29 Jan 2026 guidance (p. 14) still says that functions for a critical, time-sensitive task or decision do not meet Criterion 4. ESCALATE_NOW is scoped as "needs urgent human attention", not as guidance for a time-critical intervention.
- Hager et al.: results attributed explicitly to Llama-2-based models only.
- Med-HALT: 2023-era models named.
- Omiye et al.: reduced to a qualitative 2023 observation about repeated runs.
- Ke et al.: the GPT-4 Turbo model, the three physicians, possible training-data overlap and the small sample are all stated.
- CRAFT-MD: "on four-option questions" added.
- FActScore: cited as a method only. The "claim checking is a requirement" position is presented as the project's own.
- Clinical multi-agent baselines: the claim is limited to "the clinical studies reviewed here". General-domain evidence is kept separate.
- AHRQ: benefits limited to prescribing errors.
- Regulatory documents: used as boundaries only. The design is "informed by" them, never "compliant with" them.

## 6. Conflicts with the proposal and workflow

| Conflict | Handling in the chapter |
|---|---|
| The proposal includes operational management (triage logistics, scheduling, resource allocation) | Excluded, following the supervisor's decision (decision log). Listed in Table 1.4. |
| The proposal says "decentralized"; the workflow is centrally orchestrated | The chapter describes the actual design: a deterministic central orchestrator. |
| Proposal objectives are not measurable ("reduce diagnostic errors in real time", "without human intervention") | Replaced with SO1–SO6 and RQ1–RQ4. Physician authority is kept throughout. |
| Title typo in the approved form ("healthcare care") | The approved title is used: *Agentic Framework for Healthcare Clinical Decision Support*. |

## 7. Unresolved items and limits on submission readiness

**Decisions the team and supervisor must make.** The chapter states openly that each is still open.

1. **Test data (D5):** the plan is team-built, guideline-anchored vignettes. 1.5.4 marks this "subject to supervisor approval".
2. **Clinician reviewer:** not yet secured (stated in 1.5.4).
3. **Target clinical presentation (C10):** not fixed. 1.5.2 gives four selection criteria and limits the decision type to non-medication choices.
4. **GP1/GP2 split:** described as "planned, to be confirmed".
5. **Claim-support labelling by two team members:** a new commitment, stated as a limitation. It needs team approval and an entry in the decision log.
6. **Second pathway pack:** "if time permits". It is not a research question.

**Reference details still incomplete.** These must be obtained from publisher pages and must not be guessed:
- [4] Med-PaLM 2: volume and pages
- [16] MediQ: volume and pages
- [26] SourceCheckup and [35] Hakim: article numbers
- [32] Smit: pages
- [34] MedAgentBoard: NeurIPS track
- [19], [21] WHO: ISBN or URL
- [22] SDAIA: official URL (the copy read was hosted by a university)
- [39] SFDA: exact English title
- [8] Amugongo: article number and DOI taken from the research notes; confirm them.

**Sources read at abstract level only.** Only abstract-level statements are cited from them: [4]–[7], [9], [10], [12], [13], [16]–[18], [26]–[29], [31]–[37].

**Preprints.** HealthBench [29], Gorenshtein et al. [30], Cemri et al. [36] and Kapoor et al. [37] are labelled as preprints in the text where used.

**Local regulation.** A draft SDAIA Responsible AI Policy (published for consultation in November 2026, reported by a law firm) was found but not read. The chapter cites only the 2023 AI Ethics Principles. Re-check before final submission.

**Formatting choices to confirm with the supervisor:**
- Body top margin of 1 inch. The template's 27 pt margin is used on the cover only.
- Table text at about 9.5 pt and references at 10.5 pt, single-spaced.
- Captions in the form "Table 1.x:".
- The cover has no student names or IDs. They should be added when the chapter is merged into the full report.

## 8. Independent review and final readiness

A separate reviewer, who did not write the chapter, checked it against the evidence report, the research notes, the audit, `Seminar.pdf` and the rendered PDF.

- **First review:** no critical issues, 9 major and 22 minor issues; overall 6.5/10.
- **After revision:** all 9 major issues resolved. Seven new minor issues were found, and all were then fixed: the table breaks, the Table 1.3 structure, the Ke citation fit, the repeated sentence in 1.1.1, the labelling limitation and the stale reference notes. The full review is in `qa_review.md`.

| Dimension | Rating (/10) | Note |
|---|---|---|
| Scientific quality | 8.5 | Faithful to verified evidence; model generations named; no superiority or validation claims |
| Academic structure | 8.5 | Follows `Seminar.pdf`; explicit assumptions; status definitions sit in 1.1.4, which is acceptable but could move to 1.4 |
| Research methodology | 8 | RQs cover decision quality, verification, robustness, fair comparison, ablation and cost; the test data is still to be approved |
| Citation integrity | 8 | All 40 references cited, in order; several entries still lack pages or article numbers |
| Project alignment | 8 | Consistent with the workflow and the supervisor's decisions; four project decisions are still open |
| Formatting | 8.5 | Official cover, consistent styles, tables kept on one page with repeating headers; some choices need confirmation |
| **Overall submission readiness** | **8** | **Not yet fully submission-ready.** The open decisions in Section 7 and the incomplete reference details must be resolved first. |

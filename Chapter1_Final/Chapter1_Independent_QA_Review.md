# Independent QA Review: Chapter 1 (Introduction)

**Project:** Agentic Framework for Healthcare Clinical Decision Support (Research Track, GP1)
**Files reviewed:** `chapter1.md`, `Chapter1_Final.pdf` (17 pp.), `refs.py`, `order.json`, the evidence report, raw research notes, `Stage1_Project_Audit.md`, `Seminar.pdf`
**Reviewer stance:** strict supervisor, CS examiner, healthcare-AI researcher, methodology reviewer, citation auditor. I did not write the chapter and did not edit it.
**Date:** 2026-11-26

---

## 0. Verdict in brief

This is a careful, well-hedged chapter. It is clearly better than most GP1 introductions. Citation numbering is mechanically perfect: 40 references, numbered in order of first appearance, all used, and identical to `refs.py`. The chapter avoids every high-risk claim in the evidence report's "Claims Chapter 1 should avoid" table. It never claims multi-agent superiority, clinical validation or regulatory compliance. It names Llama 2 for Hager, marks 2023-era models for Med-HALT and labels Gorenshtein as a preprint. I checked the FDA "critical, time-sensitive" sentence against the live PDF. It is still present in Section IV(4)(a) of the 29 Jan 2026 version, so 1.5.6 is supported.

It is **not yet submission-ready**, for four reasons:

1. **The research questions do not cover what the objectives promise.** No RQ asks whether the recommendations are correct. RQ2 cannot detect a system that abstains on everything. Neither the ablation nor the "condition-agnostic" generality claim traces to any RQ.
2. **Undecided project matters are written as settled.** The test data (audit D5, still open), the clinical presentation and decision type (C10) and the GP1/GP2 split are all described as decided.
3. **Seminar.pdf requires "assumptions" in 1.5, and the chapter has no explicit assumptions.**
4. **A small number of citations carry more than their sources support.** Several reference entries also lack pages or DOIs that are already recorded in the research notes.

I found **no Critical issue**, meaning nothing factually false or fatal. There are **9 Major** and **22 Minor** issues.

---

## (a) Prioritised issue list

Severity: **Critical** = would fail or seriously mislead an examiner; **Major** = must be fixed before submission; **Minor** = should be fixed, low risk.

### MAJOR

**M1. The research questions leave out decision quality and over-abstention.**
- *Text (RQ2):* "How appropriately does the framework return NEED_MORE_INFO, ESCALATE_NOW and ABSTAIN on test cases constructed with missing information, red flags and out-of-scope inputs, compared with the baselines?"
- *Text (Table 1.3, SO5):* "…decision-support performance on the pack's option set…"
- *Problem:* No RQ asks whether the RECOMMEND / RECOMMEND_WITH_CAVEATS outputs and the option ranking are correct. RQ2 is also tested only on cases built to require a non-recommend status. A system that returns ABSTAIN or ESCALATE_NOW for every case would score perfectly on RQ2 and reduce unsupported claims (RQ1) to zero. The Table 1.3 metric "decision-support performance" belongs to no RQ. An examiner will ask what stops the safest system from being the most useless one. That question is the core trade-off in abstention research [17].
- *Fix:* Reword RQ2 to cover all five statuses on a test set that also contains complete, answerable cases. Make it measure both appropriate and inappropriate escalation/abstention, i.e. over-abstention or coverage. Then either add an RQ on decision quality ("How accurately does the framework select or rank the pathway-appropriate option on answerable cases, compared with the baselines?") or fold decision quality explicitly into RQ4.

**M2. The ablation and the generality claim are promised but not traced to any RQ.**
- *Text:* SO5 "…assess the contribution of individual components by removing them one at a time (ablation)"; Table 1.2 "Contribution measured by removing the critic (ablation)"; 1.5.2 "the design's generality is a claim to be tested, not an assumption"; SO2 "at least one versioned pathway pack".
- *Problem:* (i) No RQ asks about component contribution, so the ablation has no research question behind it. (ii) The generality claim is called "to be tested", but with "at least one" pack it cannot be tested, and no objective or RQ tests it. The project audit (Section 4.5 and C7 draft RQ3) treats transfer to a second pack as the test of this claim.
- *Fix:* Add the ablation as a sub-question, e.g. RQ4b: "Which components (critic, verifier, pre-LLM safety checks) account for any differences?" For generality, choose one of two options. (a) Commit to a second, transfer pack and add an RQ/objective for it. (b) Downgrade 1.5.2 to "the design is intended to be condition-agnostic; testing this claim on a second pack is future work."

**M3. RQ1 confounds orchestration, multi-agent structure and verification.**
- *Text:* "Compared with a single agent that has the same model, retrieval and tools, does the framework's combination of deterministic orchestration and claim-to-source verification reduce the proportion of unsupported claims…"
- *Problem:* SO4's strong baseline has "the same retrieval, rules and calculators" but **not** the verifier. A difference in unsupported-claim rate could therefore come from the verifier alone, from the multi-agent structure or from extra calls. RQ1 credits a "combination" it cannot separate. Table 1.1 also says the project measures the rate "with and without claim-to-source verification", which is a different comparison from the one in RQ1.
- *Fix:* Split RQ1. RQ1a compares the framework with and without the verifier (ablation). RQ1b compares the framework with a single agent given the same verifier. RQ1c covers verifier accuracy against human-annotated claim labels. Alternatively, state explicitly that the single-agent baseline also gets the verifier. Say who annotates claim support (see m15).

**M4. The test-data strategy is presented as decided, but the audit records it as open.**
- *Text (1.5.4):* "The primary test set will therefore be constructed by the team: clinical vignettes anchored to the guideline sources…"
- *Problem:* `Stage1_Project_Audit.md` §12: "**D5** Test data and clinician reviewer: open." The decision log (§11) has no entry adopting team-built vignettes. The evidence report *recommends* this option but does not record it as approved. Submitting this text commits the team to a decision the supervisor has not approved. The same applies to the clinician reviewer.
- *Fix:* Either record the decision with the supervisor (decision-log entry #8) before submission, or reword it as a proposal: "The proposed primary test set, subject to supervisor approval, is…" Name the clinician-reviewer arrangement, or state plainly that it is not yet secured.

**M5. The clinical decision and the pathway pack are never named, so the scope is incomplete.**
- *Text (1.5.2):* "…for example the next diagnostic or management step for a clinical presentation."
- *Problem:* Audit C10 (Major): "'Clinical decision support' must be concretised for 1.5: which decision type… which conditions (packs) and which users." Users are now defined. The decision type is given only as an example, and no condition or presentation is named. Readers cannot judge feasibility, the guideline sources or whether "no prescribing" holds, because a management step could be a drug choice. The examples in 1.5.3 are similarly vague.
- *Fix:* Name the presentation and decision type, e.g. "pack 1: <presentation>, decision = next diagnostic step / disposition". If they are still undecided, write a sentence that says so: "The target presentation will be selected with the supervisor by <date> using criteria X, Y, Z." Do not leave the gap unstated.

**M6. No explicit assumptions, which Seminar.pdf requires for 1.5.**
- *Text:* the section headings 1.5.1–1.5.6. No "Assumptions" heading or list.
- *Problem:* Seminar.pdf, Research Track 1.5: "Research scope, **assumptions** and study boundaries." Assumptions are scattered through the text (same base model across systems, validity of guidelines as ground truth, de-identified input, perturbations that preserve clinical meaning) but never stated as assumptions. A checklist-driven examiner will mark this as missing.
- *Fix:* Add "1.5.x Assumptions" with 4–6 numbered items, e.g.:
  - A1: published guidelines in each pack are an acceptable reference standard for test labels.
  - A2: reordered or reworded variants preserve clinical meaning.
  - A3: the input case text is de-identified and in English.
  - A4: the same base model and decoding settings are available for all systems throughout the experiments.
  - A5: model versions are frozen during evaluation, with no online learning.
  - A6: LLM-graded measures are validated against a manual sample.

**M7. Citation [27] is used for a normative claim it does not make.**
- *Text (P3):* "Claim-level checking of model output against its sources is therefore a requirement, not an optional refinement [27]."
- *Problem:* FActScore [27] is ABS-verified as a *method* for atomic-fact scoring, validated on Wikipedia biographies (refs.py: "cited only for the method"). It says nothing about what clinical decision support requires. The "requirement" is the authors' own conclusion.
- *Fix:* "This project therefore treats claim-level checking as a requirement rather than an optional refinement. Atomic-fact checking [27], developed for general-domain text, provides a method for it."

**M8. A claim about the baselines in clinical multi-agent studies is cited to general-domain or benchmark papers, and one rationale overgeneralises abstention.**
- *Text (Table 1.1):* "Gains are reported mainly against weak baselines; strong single agents nearly match multi-agent discussion [30], [31], [33]"
- *Text (P4):* "Reported gains are usually measured against single-pass LLMs or simple prompting." (uncited)
- *Text (Table 1.2):* "Abstention improves reliability [25], [17]"
- *Problems:*
  - (i) The claim about which baselines *clinical* multi-agent studies used concerns [12]–[14] and is supported by [34]. It is not supported by [30]/[31], which are general-domain reasoning studies (refs.py: "General-domain reasoning tasks, not clinical"). The table drops the qualifier "in the general domain" that the P4 prose keeps. The P4 sentence is the central gap claim and has no citation at all.
  - (ii) In MediQ [25] (ABS), "abstention" means the model holding back an answer in order to ask more questions in an interactive setting. That is closer to NEED_MORE_INFO than to ABSTAIN, and it is a single benchmark. Wen et al. [17] is a survey that calls the area open; it does not show that abstention improves reliability.
- *Fix:*
  - (i) Table 1.1: "Clinical gains are reported mainly against single-pass LLMs or simple prompting [13], [14], [34]; in the general domain, strong single agents nearly match multi-agent discussion [30], [31]; a medical benchmark found no consistent multi-agent advantage [33]." Add [12]–[14], [34] to the P4 sentence and soften it to "In the clinical studies reviewed here, …".
  - (ii) Table 1.2: "In an interactive diagnostic benchmark, letting the model hold back an answer until it had gathered more information improved accuracy [25]; abstention is an open research area [17]."

**M9. Citation [24] is used for a general claim from a tangential source, with no model generation named.**
- *Text (P1):* "Inconsistent behaviour across repeated runs of the same question has also been documented [24]."
- *Problem:* Omiye et al. study race-based medical content from 2023 commercial chatbots. The run-to-run point is a qualitative remark ("the concerning outputs do not always appear in all five runs"; the notes rate it "Moderate (qualitative)"). The evidence report says no study with a test-retest agreement statistic was found. The sentence reads as a general, quantified finding and breaks the report's rule to "name the model generation behind every limitation". refs.py also notes the paper's internal inconsistency on question count.
- *Fix:* "In a 2023 study of race-based content, harmful outputs from four commercial chatbots appeared in some but not all of five repeated runs of the same question [24]." Otherwise remove the sentence. RQ3 does not depend on it.

### MINOR

**Evidence fidelity and wording**

- **m1.** "The benefits of CDSS are best established for **medication safety**." AHRQ [2] supports *prescribing errors* and says current systems "are not effective at preventing errors at other stages of the medication use process." *Fix:* "…best established for preventing prescribing errors."
- **m2.** "Flan-PaLM reached 67.6%… and **its successor** Med-PaLM 2…" Med-PaLM 2 succeeds Med-PaLM, not Flan-PaLM. *Fix:* "and the later Med-PaLM 2 reached up to 86.5% [4]."
- **m3.** "…which makes it difficult to update and **impossible** to trace to a source." Lewis [7] (ABS) calls provenance an *open problem*. *Fix:* "difficult to update and to trace to a source."
- **m4.** "LLMs tend to answer even when they should not, and the confidence they state in words consistently overestimates… [16]." The first clause comes from MediQ [25], not Savage [16]. *Fix:* cite [25] after the first clause.
- **m5.** "…a multi-agent conversation that included a devil's-advocate role reached up to 76% diagnostic accuracy, against 48% for the physicians in the comparison [14]." Placed in Background without caveats, a reader takes this as a multi-agent > physician result. The notes say: GPT-4 Turbo; 15 of the 16 cases were published reports, which may be in the training data; cases were chosen for bias-driven misdiagnosis (initial diagnosis 0/80); three physicians. Section 1.2 never returns to Ke specifically, despite the promise "Section 1.2 examines what they do and do not establish." *Fix:* add "(GPT-4 Turbo; published cases that may appear in training data; three physicians)".
- **m6.** "…its accuracy fell from 0.820 to 0.627…" [23]. This is the four-option multiple-choice condition. *Fix:* add "on four-option questions".
- **m7.** "…a citation that does not support the claim it accompanies is worse than no citation…" This is uncited argument stated as fact. *Fix:* "can be worse than no citation".
- **m8.** "Placing a clinician 'in the loop' is necessary, but it is not sufficient on its own because of automation bias." Uncited. *Fix:* cite [19], [20], both of which discuss automation bias.
- **m9.** Table 1.2: "Accuracy shifts of up to 18 percentage points from reordering alone [22]" and "LLMs misuse tools and drift from guidelines [22]". Here the Llama-2 limitation is dropped. *Fix:* "…in Llama-2-based models [22]".
- **m10.** "Human control is a common principle of AI-in-health guidance [18], [20], [21]." [20] (FDA) is device-regulation guidance whose relevant criterion is independent HCP review of the basis, not "human control". [21] is general AI ethics, not health-specific. *Fix:* "Human oversight and reviewability of the basis of recommendations are common requirements in AI and CDS guidance [18], [20], [21]."
- **m11.** In-text preprint labels are missing where the evidence is used substantively: [29] HealthBench (P4 and 1.5.6), [36] Cemri, [37] Kapoor. *Fix:* "an industry benchmark released as a preprint [29]", and similarly for the others.
- **m12.** 1.5.6 names NIST [40] as a design influence, but NIST's content is never stated anywhere in the chapter. *Fix:* add one clause (NIST AI RMF is voluntary; MAP 3.5 asks for documented human-oversight processes) or drop NIST.

**Verification status inconsistencies to resolve, not necessarily errors**

- **m13.** refs.py and the evidence report disagree on several sources. The evidence report labels Li et al. [32] PREPRINT, while refs.py says TMLR. It labels MedAgentBoard [33] PREPRINT ("NeurIPS listing not opened"), while refs.py says the NeurIPS poster page was verified. It labels the Bedi [28] DOI SEC, while refs.py says "None". It labels the Hakim [35] venue SEC (from a DOAJ library record). *Fix:* confirm each on the publisher or venue page and record which document is authoritative.

**Methodology and logic**

- **m14.** "All four questions can be answered with the planned experiments." This is asserted, not shown, and it depends on M1–M5. *Fix:* delete it, or back it with Table 1.3 once M1–M3 are fixed.
- **m15.** RQ1 needs "verifier accuracy against annotated claims", but 1.5.4 never says who labels claim support or how agreement is checked. *Fix:* add one sentence to 1.5.4, e.g. "two team members, with agreement reported and clinician spot checks where available."
- **m16.** "The research questions below refine the four questions proposed during the evidence review." This refers to an internal process document the examiner has never seen. *Fix:* remove the process narration and justify the RQs from Section 1.2 directly.
- **m17.** Traceability Table 1.3 maps SO1 and SO6 to "All", which carries no information. The table also lists "contribution of each component" and "decision-support performance" without an RQ (see M1, M2).
- **m18.** Redundancy. The 18-point reordering figure appears in P1 and Table 1.2. The single-agent-matches-multi-agent argument appears in P4, Table 1.1, the third paragraph of 1.3 and the RQ preamble. "Does not claim regulatory compliance" appears in 1.3, Table 1.4 and 1.5.6. Section 1.3 Table 1.2 re-cites the problem evidence ([22], [25], [16], [26]) and largely describes the solution, which is Chapter 5.1 content. This works against Seminar's "need for further research". *Fix:* cut Table 1.2's rationale column to short back-references ("see P1–P4"). Keep the motivation focused on why the question deserves study: unmeasured properties, the value of a fair comparison, and relevance. Drop or source the thin "engineering education" and "local context" motivations. A research-track motivation should not rest on what the students will learn.

**Project alignment and terminology**

- **m19.** "Graduation Project 1 covers the research foundation and system design (Chapters 1–5) together with an initial implementation of the core workflow." The audit (§6) marks this split "to confirm in Stage 4", and it is not in the decision log. *Fix:* "is planned to cover", or log the decision.
- **m20.** SO4 and 1.5.2 mention "calculators", but SO1's list of framework components omits the score-calculation stage that the audit's workflow includes. "Safety precedence" (Table 1.3) is never defined. RECOMMEND_WITH_CAVEATS is never distinguished from RECOMMEND. The statuses are used in Tables 1.1 and 1.2 before they are defined in 1.4.2. *Fix:* add calculators to SO1. Give a one-line definition of each status and of the safety precedence (rules > calculators/pack rules > retrieved evidence > LLM judgement), at first use or in a short table in 1.1.4 or 1.4.2.
- **m21.** Abbreviations and terms. "CDS" (1.1.5, "FDA guidance on CDS software") is never defined, and is distinct from CDSS. "Red flag" is never defined. The user term drifts: "physician", "clinician", "healthcare professional". 1.5.1 says the users are "licensed healthcare professionals, primarily physicians", yet final authority is always "the physician". *Fix:* define CDS, define "red flag", and either restrict the users to physicians or say "the reviewing clinician" throughout.

**References (completeness and format)**

- **m22.** Several entries lack pages, article numbers or DOIs. For many of them the research notes **already record the values**, so verify and add them:
  - [8] Amugongo: art. e0000877, doi 10.1371/journal.pdig.0000877 (notes `rag_and_evidence_verification.md` l.17; refs.py says "not confirmed", so confirm first).
  - [9] Xiong: pp. 6233–6251, doi 10.18653/v1/2024.findings-acl.372.
  - [11] Guo: pp. 8048–8057, doi 10.24963/ijcai.2024/890.
  - [12] Tang: pp. 599–621, doi 10.18653/v1/2024.findings-acl.33.
  - [13] Kim: vol. 37, pp. 79410–79452, doi 10.52202/079017-2522.
  - [15] Pal: pp. 314–334, doi 10.18653/v1/2023.conll-1.21.
  - [17] Wen: doi 10.1162/tacl_a_00754.
  - [27] Min: pp. 12076–12100, doi 10.18653/v1/2023.emnlp-main.741.
  - [30] Wang: pp. 6106–6131, doi 10.18653/v1/2024.acl-long.331.

  These are still missing and **not** in the notes; do not invent them, obtain them from the publisher:
  - [4] volume and pages.
  - [25] volume and pages.
  - [26] article number.
  - [31] pages.
  - [33] track.
  - [35] article number.
  - [18], [19] URL or ISBN (the WHO publication pages are in the evidence report).
  - [21] official SDAIA URL.
  - [39] exact English title (issued 3 Jan 2023, version date 29 Nov 2022).

  Format consistency:
  - [22] lists 11 authors and [26] lists 10, while others use *et al.*. IEEE uses *et al.* beyond six authors.
  - [2] should use the IEEE form "Accessed: Nov. DD, 2026."
  - Grouped in-text citations are not in ascending order: "[25], [16]", "[25], [17]", "[29], [28]", "[20], [18], [40], [39], [21]". Sort them.
  - refs.py still flags [3] and [7] as "check against publisher page before final submission".

---

## Formatting (PDF) findings

I rasterised and inspected all 17 pages. Overall the layout is clean. There are no blank pages and no orphaned headings. Each table fits on a single page with its caption above it. Heading hierarchy is clear: H1 18 pt, H2 14 pt, H3 12 pt bold. The body is Times New Roman 12 pt at 1.5 line spacing, rendered as Liberation Serif, and left/right margins are 90 pt as the template requires. Remaining points:

- **f1 (Minor).** References are set at 10.5 pt single-spaced, and table text at about 9.5 pt. The template instruction is "font size 12, 1.5 line spacing" (audit D4). Smaller reference and table text is common, but confirm with the supervisor that it is acceptable. If not, set the references to 12 pt, single-spaced, with 6 pt after.
- **f2 (Minor).** The top margin is 72 pt, while audit D4 records the template top margin as 27 pt. That value may apply only to the cover page with its logo header. Check the body pages against `#F15-CS-1.docx`.
- **f3 (Minor).** Table 1.2 is dense: 7 rows × 4 columns at 9.5 pt fills page 8. Shortening the rationale column (m18) will also help readability.
- **f4 (Minor).** Caption style "Table 1.1 Status of…" has no separator. Use "Table 1.1: …" or "Table 1.1. …" consistently, matching the department convention.
- **f5 (Minor).** The cover page carries no team names or student IDs. This is acceptable if the chapter will be merged into the full report with front matter; otherwise add them per the template.
- **f6 (Minor).** The reference paragraphs use mixed styles ("First Paragraph" for [1], "Body Text" for the rest). This is invisible now but will matter if styles are changed later.

---

## (b) Ratings

| Dimension | Score /10 | One-line justification |
|---|---|---|
| Scientific quality | **7.5** | Accurate, hedged and faithful to the evidence report; model generations named; no superiority or validation claims. A few overstatements ([27], [24], abstention, "medication safety") remain. |
| Academic structure | **7** | Follows Seminar 1.1–1.5 with good signposting. Explicit assumptions are missing, and 1.3 drifts into solution design and repeats the problem evidence. |
| Research methodology | **6** | Fair-baseline design (same model, tools, calls) is excellent. The RQs omit decision quality and over-abstention, RQ1 is confounded, and the ablation and generality claims are not traced to any RQ. |
| Citation integrity | **7** | Numbering is perfect and almost every claim is supported at its stated strength. A handful of misattributions, missing in-text preprint labels, and many incomplete entries whose data already exists in the notes. |
| Project alignment | **7** | Matches the audit on the centralised deterministic orchestrator, five statuses, physician authority, no prescribing and CDS-only scope. Open decisions (D5, C10, GP1/GP2 split) are written as settled or left unnamed. |
| Formatting | **8** | Clean, consistent and readable with no layout defects. Only reference/table font size versus the template, top margin and caption punctuation need checking. |
| **Overall submission readiness** | **6.5** | Strong draft. The RQ/objective traceability gaps and the unapproved scope statements must be fixed before an examiner sees it. |

---

## (c) Required before the chapter can be called submission-ready

1. **Fix the RQ set (M1–M3).** Add decision quality and over-abstention or coverage, split RQ1 so that verification is isolated, and add the ablation as an RQ or sub-RQ. Then rebuild Table 1.3 so that every objective and every metric maps to a specific RQ, with no "All" rows.
2. **Resolve or honestly flag the open project decisions (M4, M5, m19).** Agree the test-data strategy, the clinician-reviewer arrangement, the target presentation and decision type, and the GP1/GP2 split with the supervisor, and log them in the audit decision log. If any remain open, the chapter must say so explicitly rather than describe them as decided.
3. **Decide the generality claim (M2).** Either commit to a second, transfer pack with an objective and RQ, or move generality to future work.
4. **Add an explicit Assumptions subsection to 1.5 (M6).**
5. **Correct the citation-strength issues (M7–M9, m1–m12)** using the exact rewordings above.
6. **Complete the reference list (m22).** Add the pages and DOIs already recorded in the research notes after a quick check. Obtain the remaining missing metadata from publisher pages, and resolve the refs.py versus evidence-report status conflicts (m13). Do not fill any gap from memory.
7. **Trim the redundancy in 1.3 (m18)** so that the motivation argues the need for the research rather than re-presenting the problem and the design.
8. **Confirm the template details with the supervisor (f1, f2)**, then regenerate the DOCX/PDF and re-check pagination.

Once items 1–6 are done, I would expect the chapter to rate about 8–8.5/10 for readiness.

---

## Re-check after revision

**Scope of re-check:** M1–M9, m1–m12, m14, m16–m22 and f4, as the coordinator asked. m13, m15, f1–f3, f5 and f6 were not re-checked.
**Revised files checked:**
- `chapter1.md`
- `Chapter1_Final.pdf` (now 19 pp., rendered 21:08)
- `refs.py`
- `order.json`

**Mechanical checks.** Citations are renumbered and still appear in first-use order (1–40). All 40 are used, and every entry matches `refs.py` exactly. All grouped citations are now in ascending order. There are no blank pages and no orphaned headings.

### Status of the original issues

| Issue | Status | Note |
|---|---|---|
| M1 RQs omit decision quality and over-abstention | **Resolved** | New RQ1 covers status accuracy, option selection or ranking, and unnecessary escalation or abstention on a test set that includes answerable cases. SO2 and SO5 were updated to match. |
| M2 Ablation and generality not traced | **Resolved** | RQ4 now asks which components (critic, verifier, safety checks) account for the differences. Generality is explicitly downgraded: the second pack is built only "if time permits" and is not an RQ (1.5.2). |
| M3 RQ1 confounded | **Resolved** | RQ2 isolates the verifier in both directions: removed from the framework, and added to the single agent. SO4 adds the "single agent + verifier" baseline. Verifier accuracy is measured against human-annotated labels. |
| M4 Test data presented as decided | **Resolved** | 1.5.4 now reads "proposed… subject to supervisor approval", and states that no clinician reviewer has been secured yet. The audit decision log still needs an entry once the supervisor decides. |
| M5 Clinical decision and pack not named | **Resolved (flagged as open)** | The decision type is bounded to choices with no medication selection or dosing. 1.5.2 says openly that the presentation is not yet fixed and gives four selection criteria and a time point. This must be settled before Chapter 3.1. |
| M6 No assumptions | **Resolved** | 1.5.6 adds assumptions A1–A5, which cover what was requested. |
| M7 [27] used normatively | **Resolved** | The requirement is now framed as the project's own position, and [27] is cited as a general-domain method only. |
| M8 Baseline claim and abstention overgeneralisation | **Resolved (minor residue)** | P4 is softened to "in the clinical multi-agent studies reviewed here" and cites [13], [14], [30]. Table 1.1 now separates clinical, general-domain and benchmark evidence. "Abstention improves reliability" has been removed. Residue: see N1 on [14]. |
| M9 [24] (now [25]) overgeneralised | **Resolved** | The sentence now names the 2023 study, the four chatbots and the five runs, and draws only a modest inference. |
| m1 "medication safety" | **Resolved** | Introduces a small repetition; see N4. |
| m2 "its successor" | **Resolved** | Now reads "the later Med-PaLM 2". |
| m3 "impossible" | **Resolved** | |
| m4 First clause attributed to Savage | **Resolved** | The clause is now cited to MediQ [16]. |
| m5 Ke result without caveats | **Resolved** | GPT-4 Turbo, three physicians, possible training-data overlap and the small sample are now stated. |
| m6 0.820 → 0.627 without condition | **Resolved** | "on four-option questions" added. |
| m7 "worse than no citation" | **Resolved** | Now "can be worse". |
| m8 Uncited automation-bias sentence | **Resolved** | Cited to [19], [20]; both sources support it. |
| m9 Llama-2 caveat dropped in Table 1.2 | **Resolved** | The rationale cells now point back to P1, which carries the caveat. |
| m10 "Human control" attributed to FDA | **Resolved** | Reworded to "human oversight and a reviewable basis… in AI and CDS guidance". |
| m11 Preprint labels | **Resolved** | [29], [36] and [37] are labelled at first substantive use. |
| m12 NIST content absent | **Resolved** | Now states that the framework is voluntary and that human-oversight processes should be defined, assessed and documented. This matches the evidence report (MAP 3.5). |
| m14 "All four questions can be answered" | **Resolved** | Sentence removed. |
| m16 Process narration | **Resolved** | Replaced by an explicit mapping from RQs to P1–P4. |
| m17 Traceability table | **Partly resolved** | The "All" rows are gone. The SO5 row now puts metrics in the "Research questions" column and reads "As listed in the previous column" (see N2). |
| m18 Redundancy and motivation drift | **Largely resolved** | Table 1.2's rationale cells now point back to P1–P3. The education and local-context motivations were replaced by an operationalisation argument. "Does not claim regulatory compliance" still appears 3 times (1.3, Table 1.4, 1.5.7). |
| m19 GP1/GP2 split | **Resolved** | Now "planned… will be confirmed with the supervisor". |
| m20 Calculators, precedence, status semantics | **Resolved** | All five statuses and the order of precedence are defined in 1.1.4, before first use. Calculators were added to SO1. See N3 on where this text sits. |
| m21 CDS, red flag, user term | **Resolved** | CDS and red flag are defined, and users are now "licensed physicians" throughout. |
| m22 Reference completeness | **Partly resolved** | Pages and DOIs taken from the research notes were added to [8], [9], [11]–[13], [15], [18], [27] and [31]. [2] now uses the IEEE "Accessed" form. [23] and [26] are shortened to *et al.* Still missing (obtain from publishers, do not invent): [4] volume and pages; [16] volume and pages; [26] and [35] article numbers; [32] pages; [34] track; URL or ISBN for [19], [21]; official SDAIA URL for [22]; [39] title. The [8] metadata is marked in `refs.py` as "confirm on publisher page". |
| f4 Caption punctuation | **Resolved** | All four captions now read "Table 1.x: …". |

### New problems introduced by the revision (all Minor)

| # | Location | Problem | Fix |
|---|---|---|---|
| N1 | P4: "gains are reported against single-pass LLMs or simple prompting… [13], [14], [30]" | Ke et al. [14] compared the agents with physicians and with the agents' own initial diagnosis, not with a single-pass or prompting baseline in the usual sense. [13] and [30] support the claim; [14] fits only loosely. | Drop [14] from this sentence, or add "or with clinicians". |
| N2 | Table 1.3, SO5 row | Metrics sit in the "Research questions" column, and the "Evaluation dimensions" cell reads "As listed in the previous column". This breaks the table's structure. | Put "RQ1–RQ4" in column 2 and the metric list in column 3. |
| N3 | 1.1.4, last paragraph | The proposed framework's order of precedence and the five status definitions now sit in *Background*, which Seminar.pdf defines as the research area and technical background. | Acceptable as is. Cleaner: move the paragraph to the start of 1.4.2, or into a short "Proposed approach" paragraph at the end of 1.2, and refer to it from there. |
| N4 | 1.1.1 | "best established for preventing prescribing errors. CPOE with integrated decision support reliably prevents prescribing errors" repeats itself. | Merge into one sentence: "The benefits of CDSS are best established for preventing prescribing errors through CPOE with integrated decision support; evidence for other uses… is weaker [2]." |
| N5 | PDF pp. 7–9 | Table 1.1 now splits across pages 7–8, leaving its last row alone on page 8. Table 1.2 splits across pages 8–9, leaving only its first row under the caption on page 8. Both tables fitted on one page before the revision. The header rows repeat, so this is readable but untidy. | Set "keep with next" / "allow row to break across pages = off", or add a page break before each caption, then regenerate. |
| N6 | 1.5.4: "Claim-support labels… assigned independently by two team members" | This new commitment is not in the audit decision log. The labellers are non-clinicians judging clinical claim support, which is not stated as a limitation. | Add one clause to the limitations, and log the decision. |
| N7 | `refs.py` remaining-issues column | Some notes are stale. For example, `guo` still says "Page numbers not added", although they now are. The m13 status conflicts with the evidence report (`limore`, `zhu`, `bedi`, `hakim`) are unchanged. | Update the notes so the reference ledger matches the chapter. |

### Updated ratings

| Dimension | Before | After | One-line justification |
|---|---|---|---|
| Scientific quality | 7.5 | **8.5** | All flagged overstatements are corrected. Claims now match source strength and model generation. Only the loose fit of [14] remains. |
| Academic structure | 7 | **8.5** | Seminar 1.1–1.5 is fully covered, including assumptions, and the motivation is tighter. Design definitions sitting in Background is a minor placement issue. |
| Research methodology | 6 | **8** | The RQs now cover decision quality, over-abstention, verifier isolation, ablation, the compute-matched baseline and cost. The test set and presentation are still pending. |
| Citation integrity | 7 | **8.5** | Numbering is clean, misattributions are fixed and preprints are labelled. Some metadata is still missing and needs publisher checks. |
| Project alignment | 7 | **8.5** | Open decisions (D5, C10, GP split) are now flagged honestly and match the audit. They still need supervisor sign-off and log entries. |
| Formatting | 8 | **7.5** | Captions are fixed, but Tables 1.1 and 1.2 now split across pages, and the SO5 row of Table 1.3 is malformed. |
| **Overall submission readiness** | 6.5 | **8** | Content is close to submission quality. What remains is small layout and reference work plus supervisor confirmations. |

### Remaining before submission

1. Fix N5 (table page breaks) and N2 (Table 1.3, SO5 row), then regenerate the PDF.
2. Complete the m22 metadata from publisher pages, and update the stale `refs.py` notes and the m13 status conflicts (N7).
3. Optional quick fixes: N1, N4, N6.
4. Supervisor confirmations to record in the audit decision log:
   - D5 test set and clinician reviewer.
   - Target presentation and decision type, before Chapter 3.1.
   - GP1/GP2 split.
   - Template font size and top margin (f1, f2).

---

## Final pre-submission check

**Date:** 2026-11-27
**Files checked:** `chapter1.md` and `Chapter1_Final.pdf` (20 pp.), diffed against `chapter1_src_v2_prefinal.md`; `refs.py` diffed against `refs_v2_prefinal.py`.
**Sources consulted for this check:**
- The FDA guidance PDF, fetched again by me on 2026-11-27.
- The approved workflow PDF, read as text and as a rendered image so that agent and deterministic node colours could be checked.
- Venue listings for Cemri (neurips.cc 2025 virtual pages) and Kapoor (mlanthology.org TMLR 2025).

### 1. FDA accuracy

| Chapter statement | Cited | What the FDA PDF says | Verdict |
|---|---|---|---|
| The guidance concerns HCP-facing software in the US and is not legally binding | [20, pp. 4–5] | p. 4: scope is CDS "intended for health care professionals (HCPs)"; "not binding on FDA or the public". p. 5: "do not establish legally enforceable responsibilities". | Accurate. Patient-facing CDS is handled in Section IV(3) as a device, which does not contradict the chapter. |
| Four criteria for falling outside the device definition; the fourth is independent review of the basis rather than relying primarily on the recommendations | [20, p. 6] | The criteria appear on pp. 5–6, and Criterion 4 wording matches. | Accurate. The criteria are statutory (FD&C Act §520(o)(1)(E)) and the guidance *explains* them, so "sets out" is slightly loose (F7). |
| Critical, time-sensitive functions do not meet Criterion 4, because an HCP is unlikely to have time to review the basis | [20, pp. 14–15] | p. 14 states exactly this, with the same reason. | Accurate. |
| Time-critical nature and level of automation are factors in that assessment | [20, pp. 14–15] | p. 15: "FDA considers the (a) level of software automation and (b) time-critical nature…" | Accurate. |
| Automation bias definition | [20, p. 15] | Verbatim match. | Accurate. |
| Examples of functions that remain devices include software that identifies signs of patient deterioration and alerts an HCP | [20, p. 24] | Device Example 24 analyses **hourly pulse-oximetry and heart-rate measurements**. It fails Criterion 1 because it "analyzes a pattern", and fails Criteria 2, 3 and 4 because it gives a specific diagnostic output and supports time-critical decisions. | Accurate but incomplete (F6). The chapter omits that the example analyses a physiological pattern, which the prototype excludes. The omission errs toward caution, so it is not misleading in a harmful direction. |

**Regulatory claims.** No regulatory classification, compliance, clearance or non-device claim about the prototype remains anywhere in the chapter:
- 1.5.7 states "makes no regulatory classification of the prototype".
- Table 1.4 excludes any such claim.
- 1.3 now reads "aims to operationalize".

### 2. Workflow consistency

The chapter checks out against the approved workflow on every point the coordinator listed:
- The stage order is correct: pack selection, then red flags, then required fields, before any LLM.
- Red flags run before the completeness check.
- The six agent roles match Agents 1, 2, 2c, 3, 4 and 5.
- The calculators run in parallel with the agents.
- The verifier returns issues to Agent 4 at most once, and failing twice gives ABSTAIN.
- The decision core runs hard constraints, then weighted ranking, then the robustness test across missing-value ranges.
- Seven outcomes map to five statuses, and each outcome is triggered as the workflow shows.
- The report composer is deterministic, which matches its dark-grey node.
- The physician accepts, edits or rejects with a reason, and the choice is logged.

**Target pathway.** It is still described as not selected (1.5.2), and no specialty is named. "Abdominal pain" appears only in the Hager literature summary.

There are two inaccuracies (F1, F4) and two minor gaps (F5, F8); see the issue table below.

### 3. Fair comparison

SO4, RQ4, the new paragraph after the RQs, Table 1.3's SO4 row and 1.5.3 now all treat computational budget (calls, tokens, latency, cost) as something to match where practicable and always report, not just the number of calls. They also fix the model version and decoding settings across systems. The baseline configurations are explicitly deferred to Chapter 3, so nothing unapproved is presented as decided.

One internal contradiction (F2) and one feasibility gap (F3) remain.

### 4. Citations

- **Numbering:** citations appear in first-use order 1–40, with all 40 used. Every reference-list entry is identical to `refs.py`, grouped citations are in ascending order, and the pinpoint citations to [20] are well-formed IEEE.
- **Peer review and preprint status:**
  - Cemri [36] is now cited as NeurIPS 2025, confirmed on neurips.cc.
  - Kapoor [37] is now cited as TMLR 2025, confirmed on the ML Anthology.
  - The in-text "preprint" wording has been correctly removed for both.
  - HealthBench [29] and Gorenshtein [30] are still labelled as preprints, both in the text and in the reference list.
- **Numbers:** every figure in the text matches the evidence report and notes. No new figures were introduced except bibliographic metadata.
  - The metadata newly added for [4], [19], [21], [26], [32] and [39] is plausible and recorded as verified in `refs.py`. I did not independently re-verify it.
  - Still incomplete: [16] has no volume or pages, [34] has no track or pages, [35] has no article number, and [22] has no official URL. These are Minor and acceptable for a GP1 submission if noted.

### 5. Layout

Layout is clean:
- No blank pages and no orphaned headings.
- All four tables now sit whole on a single page (pp. 8, 9, 13, 16 in the printed numbering).
- Captions are consistent.
- Page 12 has about one-third of the page left blank because Table 1.3 moved to the next page. This is acceptable.

### Issue table

| # | Severity | Location | Problem | Fix |
|---|---|---|---|---|
| F1 | **Major** | 1.1.4: "The LLM agents contribute content, but the final status is set by the deterministic steps of the workflow." Also Table 1.2: "set by deterministic workflow steps (… claim verification …)" | In the approved workflow, **Agent 5 · Verify is an LLM agent** (blue node: rules V1–V5 plus claim-to-source check V6). The "verification fails twice → ABSTAIN" outcome therefore rests on an LLM judgement. Only the routing gate is deterministic. As written, the chapter misdescribes one of the seven outcomes and overstates how far LLM judgement is excluded. | 1.1.4: "…the routing between outcomes is deterministic, and five of the seven outcomes are decided by deterministic rules; the verification outcome depends on the verifier agent's judgement, which is itself evaluated in RQ2." Table 1.2: "set by deterministic gates over rule results and the verifier's verdict, rather than by the model's self-reported confidence." |
| F2 | **Major** | Paragraph after the RQs: "The comparison will therefore keep the base model version, decoding settings, **tool and retrieval access** and input format the same for **all systems**" | This contradicts SO4, whose first baseline is "a single LLM without tools". It also slightly contradicts the "same single agent **equipped with the framework's verifier**" variant. An examiner will catch it immediately. | "…keep the base model version, decoding settings and input format the same for all systems, and tool and retrieval access the same for all tool-using systems…" |
| F3 | Minor | Same paragraph, 1.5.3 and A4: "same… decoding settings" for all systems | A budget-matched variant is usually built by repeated sampling and voting [33]. That needs a non-zero sampling temperature, which may differ from the framework's settings. As written, the plan may be infeasible. | Add: "except where a baseline's method requires sampling; any such difference will be stated and reported." |
| F4 | Minor | 1.1.4: "LLM agents then extract facts that must quote the case note"; SO1's list of deterministic components | In the workflow, quoting is enforced by a **deterministic Grounding check**: a fact that does not quote the note becomes a gap. The chapter attributes this to the LLM agent and leaves the grounding check out of SO1's deterministic components. | "Agent 1 extracts facts tied to the case note, and a deterministic grounding check turns any fact that does not quote the note into a gap." Add "grounding check" to SO1's deterministic list. |
| F5 | Minor | 1.5.2: "The output is a single report with … the results of the safety checks and a record of every processing step" | The workflow's report contains "status, ranked options, reasons, sources". The step record is the separate append-only **audit log**. Safety-check results in the report are not shown in the workflow. | "…the ranked options where applicable, with reasons and sources; every processing step is recorded in an append-only audit log." Drop the safety-check results unless the design adds them. |
| F6 | Minor | 1.5.7: "Its examples of functions that remain devices include software that identifies signs of patient deterioration and alerts an HCP [20, p. 24]" | This omits that Example 24 analyses hourly pulse-oximetry and heart-rate measurements (a "pattern", failing Criterion 1) and gives a specific output. The analogy to ESCALATE_NOW is therefore looser than "directly relevant" implies. | "…include software that analyses repeated vital-sign measurements to identify patient deterioration and alerts an HCP; FDA treats it as a device partly because it analyses a physiological pattern and supports time-critical decisions [20, p. 24]. The time-critical element is relevant to ESCALATE_NOW…" |
| F7 | Minor | 1.1.5: "sets out four criteria" | The four criteria are statutory (§520(o)(1)(E)); the guidance explains how FDA interprets them. | "explains the four statutory criteria that a software function must meet…" |
| F8 | Minor | 1.5.1: "explains why it cannot: because information is missing, because the case should be escalated or because the case is outside its scope"; Table 1.2 row 1 "out-of-scope cases rejected" | 1.5.1 omits the verification-failure ABSTAIN. In Table 1.2 row 1, out-of-scope rejection is done by pack selection, not by the safety checks named in the mechanism column. | 1.5.1: add "or because its claims could not be verified". Table 1.2 row 1 mechanism: "Pack selection, red-flag rules and required-field checks before any LLM reasoning". |
| F9 | Minor | 1.1.4 (now two long paragraphs, ~330 words) | Earlier note N3 has grown: the full design walk-through sits in *Background*, which Seminar.pdf defines as the research area and technical background. | Acceptable as is. Optionally shorten it to one paragraph and move the outcome-to-status detail to 1.4.2 or Chapter 5. |
| F10 | Minor | References [16], [22], [34], [35] | Bibliographic details are still incomplete, as listed in Section 4 above. | Complete them from publisher pages before the final report, not from memory. |

### Strict readiness verdict

**Not yet submission-ready, but close.** There are no Critical issues. **F1 and F2 are Major.** F1 misdescribes the approved workflow and F2 contradicts SO4. Each is a one-sentence fix.

Once F1 and F2 are corrected, and ideally F3–F5, which take a few minutes, the chapter is ready to submit as a GP1 Chapter 1. The decisions still open with the supervisor (test set, target presentation, GP1/GP2 split, clinician reviewer) are disclosed honestly in the text and do not block submission. They should be recorded in the audit decision log as soon as they are made.

Updated ratings after this revision:

| Dimension | Rating |
|---|---|
| Scientific quality | 8.5 |
| Academic structure | 8.5 |
| Research methodology | 8.5 |
| Citation integrity | 9 |
| Project alignment | 8.5 (9 once F1 is fixed) |
| Formatting | 8.5 |
| Overall readiness | 8, rising to about 9 after F1–F2 |

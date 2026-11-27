# Chapter 1 — Final Revision Log (Acceptance Gate)

**Date:** 27 November 2026
**Baseline:** `Chapter1_Final.docx` before this gate (21 pages), archived as `_archive/Chapter1_Final_pre-gate_2026-11-27.docx`
**Result:** `Chapter1_Final.docx` / `.pdf`, 22 pages
**Page numbers:** printed page numbers. The cover is unnumbered.

## A. Changes from my own audit (Phase 1–2)

| # | Section (page) | Change | Reason | Source used |
|---|---|---|---|---|
| A1 | 1.1.4 (p. 4) | "Five of the seven outcomes are decided by deterministic rules" became: all routing is deterministic; three outcomes are reached by rules before any LLM; the other four depend on deterministic gates applied to LLM output; the verification outcome rests on the verifier agent. | The earlier count overstated how deterministic the outcomes are. "Serious risk open" and the ranking act on LLM-appraised content. | Approved workflow (node types and gates) |
| A2 | RQ1 (p. 12); Table 1.1 (p. 8); Table 1.3 | RQ1 now counts *unsafe recommendations* (recommending when the case should have been escalated, sent back for information or abstained) as well as unnecessary escalation or abstention. | Errors in both directions must be measured; abstaining is not automatically safe. | — |
| A3 | 1.5.5 (p. 16) | Added: decision quality means agreement with guideline-anchored labels, not clinical correctness; calibration is not evaluated because the outputs are categorical; a high abstention rate alone is not evidence of safety. | Separates decision quality, clinical correctness, calibration and abstention. | — |
| A4 | Table 1.1 (p. 8) | The "call budget" in the last column was replaced by: same model, tools and retrieval, with budget matched where practicable and reported. | Consistency with the fairness paragraph in 1.4.3. | — |
| A5 | RQ4 (p. 12); Table 1.3 (SO4) | The sampling exception for decoding settings is now stated wherever decoding settings are mentioned. | Internal consistency. | — |
| A6 | 1.1.2 (p. 2) | "Since 2023" became "In recent years". | Flan-PaLM results first appeared in late 2022. | [3] |
| A7 | 1.1.5 (p. 5) | "Consistently places a human professional in control" became "consistently emphasizes human oversight". | FDA Criterion 4 concerns independent review, not control. | [20] |
| A8 | P1 (p. 6) | "The strongest evidence" became "Some of the most direct evidence". | Removes an unsupported ranking of studies. | — |
| A9 | P2 (p. 6) | "LLMs are trained to answer…" became "As noted in Section 1.1.5, LLMs tend to answer…". The finding is attributed to "one interactive benchmark". | Removes an unsupported causal claim. | [16] |
| A10 | 1.3 (p. 9) | "Optimized and scored on answer accuracy alone" became "use accuracy as their primary measure". | Matches what Bedi et al. report (95.4%). | [28] |
| A11 | 1.5.3 (p. 15) | Added **Table 1.4, Status of project commitments**: current commitments, planned implementation, future work and items awaiting approval. | The gate requires commitments to be clearly separated. | Project audit decision log |
| A12 | 1.5.4 (p. 16) | Labelling by two team members is now worded as a proposal. | It has not been approved. | Project audit |
| A13 | A5 (p. 17) | Now conditional: "If any measure is computed with an LLM…". | LLM grading has not been decided. | — |
| A14 | 1.5.7 (p. 18) | Removed HealthBench [29] from the limitations sentence, which it did not support. It now cites Bedi et al. ("a limitation shared by most published evaluations"). | Citation–claim mismatch. | [28] |
| A15 | Ref. [22] | SDAIA reference corrected to the **Arabic first edition (Sept. 2023)**, after re-reading p. 20. The version label "ver. 1.0" was replaced. The Arabic script was removed from the reference because it rendered incorrectly. | Bibliographic accuracy. | SDAIA document (university-hosted copy) |

## B. Changes from the independent acceptance review

| # | Section (page) | Change | Reason | Source used |
|---|---|---|---|---|
| B1 (M1) | P4 (p. 7); Table 1.1 (p. 8) | MDAgents baselines now described accurately: "single-agent prompting baselines, including sampling-based ones such as self-consistency and Medprompt, but not single agents given the same tools, retrieval and a matched budget". | The old wording ("single-pass LLMs or simple prompting") misdescribed the paper. | [13] (verified by the reviewer against the paper's Table 2) |
| B2 (M2) | RQ1 (p. 12); 1.5.4 (p. 16); 1.5.7 (p. 18) | States the test-set circularity: rule-decided outcomes will score highly by construction. Results will be reported separately for rule-decided and LLM-dependent outcomes. Writing test cases independently of the pack rules is proposed. Added a limitation. | Threat to the validity of RQ1. | — |
| B3 (m1) | 1.1.4 (p. 3) | Ke et al.: 76% is now *top-two* accuracy, against the physicians' 48% *final-diagnosis* accuracy. Removed the claim that the authors call the sample small; the sample size is now stated as our own observation. | Misattribution and an unlike comparison. | [14] |
| B4 (m2) | P1 (p. 6) | Johri et al.: the 71% figure now carries "in an expert-annotated subset of conversations". | Missing qualifier. | [24] |
| B5 (m3) | Table 1.2 (p. 10); 1.3 (p. 10) | FDA Criterion 4 is now described as a criterion for non-device CDS, not a "requirement". Human oversight is attributed as a principle to WHO and SDAIA. | Consistency with 1.1.5 and 1.5.7. | [20], [21], [22] |
| B6 (m4) | 1.5.7 (p. 18) | Removed "the prototype does not analyse physiological signals". | It implied a regulatory distinction that the project has not assessed. | [20, p. 24] |
| B7 (m5) | 1.5.7 (p. 17) | Added a sentence recording that the scope was narrowed from the approved proposal to clinical decision support, following the supervisor's guidance. | Transparency for committee readers. | Form 3 proposal; decision log |
| B8 (m6) | 1.5.3, 1.5.7 | Tables renumbered in order of appearance: the commitments table is now 1.4 and the scope summary is now 1.5. | Numbering order. | — |
| B9 (m7) | Tables | Table rows can no longer break across pages. Long tables continue with a repeated header and never leave a single orphan row. | Layout. | — |
| B10 (m8) | Refs [16], [22] | Volume 37 added to MediQ. SDAIA claim cited with page locator "p. 20". | Bibliographic completeness. | NeurIPS 2024; SDAIA document |
| B11 (o5) | 1.1.1 (p. 1) | "Mainly delivered through EHRs" became "often delivered". | Matches the source wording. | [1] |

| B12 (n1) | 1.1.4 (p. 3) | Ke et al.: "76% of cases" became "76% of its 80 runs (16 cases, five runs each)". | The figure is per run, not per case. | [14] |
| B13 (n2) | RQ1 (p. 12); 1.5.4 (p. 16) | The separate reporting is now split by the category of the *expected* outcome, so the same split applies to every system, including the baselines. | Clarity. | — |

## C. Not changed, with reasons

- **o1 (design detail in 1.1.4):** kept. The text is accurate, and it is needed for the scope-consistency check against the workflow. It can be shortened when Chapter 5 is written.
- **o2, o3, o4, n3:** optional improvements to precision and white space. Not needed for acceptance.
- **m9 (cover names):** a team decision, recorded as D7 in `Chapter1_Open_Decisions.md`.
- **Incomplete reference details** for [34], [35], [36] and the SDAIA URL: not found on any accessible publisher page. They were not guessed and are tracked in the citation audit.

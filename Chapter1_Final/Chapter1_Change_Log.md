# Chapter 1 — Change Log

Source document: `Chapter1_Evidence_Report.docx` (kept unchanged).
Result: `Chapter1_Final.docx` / `Chapter1_Final.pdf`.
Date: 26 November 2026.

## Structural changes

| # | Change | Reason |
|---|---|---|
| S1 | Reorganised the evidence report (organised by research theme) into the five official sections 1.1 Background, 1.2 Problem Statement, 1.3 Motivation, 1.4 Objectives, 1.5 Scope. | Official structure in `Seminar.pdf` (Research Track). |
| S2 | Split 1.1 into five subsections that move from classical CDSS to LLMs, RAG, agentic systems and safety/oversight. | Harness requirement: explain concepts before relying on them, in a logical progression. |
| S3 | Rebuilt 1.2 around one central problem with four connected sub-problems (P1 realistic reliability, P2 missing information and uncertainty, P3 evidence support, P4 evaluation and fair comparison). | The report listed many separate weaknesses; the harness asks for one focused problem. |
| S4 | Added Table 1.1 (demonstrated / uncertain / investigated). | Explicitly separates literature findings from project intentions. |
| S5 | Added Table 1.2 mapping each challenge to a design mechanism, its rationale and what is tested. | Connects motivation to design without claiming effectiveness. |
| S6 | Added a main objective, six specific objectives (SO1–SO6), four refined research questions and Table 1.3 (objective → RQ → evaluation dimension). | Objectives in the approved proposal were not measurable (audit C7). |
| S7 | Wrote 1.5 as six subsections (users and use, functional, technical/experimental, data, evaluation boundaries, exclusions and limitations) plus Table 1.4. | Harness scope checklist. |
| S8 | Removed the verification-label system (FT/ABS/SEC/UNV/PREPRINT/REG) from the chapter body; preserved it in the reference audit and claim ledger. | Labels must not appear as unexplained codes in the academic text. |
| S9 | Converted inline hyperlink citations to IEEE numeric citations with a reference list. | IEEE recommended (matches supervisor's sample report); not an official requirement. |
| S10 | Removed the report's sections "Claims to avoid", "Items still requiring verification" and the research-process explanations. | Internal research material, not chapter content; moved into the audit files and critical review. |

## Scientific changes

| # | Change | Reason |
|---|---|---|
| C1 | Resolved the FDA time-critical question: the 29 Jan 2026 PDF (p. 14) still states that functions for a critical, time-sensitive task or decision do not meet Criterion 4; the p. 27 history entry concerns a different passage on pp. 13–14. ESCALATE_NOW is scoped as a signal for urgent human attention, not as guidance for a time-critical intervention. | Outstanding item 1 in the report. |
| C2 | Confirmed bibliographic details for Hager et al. (Nat Med 30(9):2613–2622), Johri et al. (Nat Med 31(1):77–86), Bedi et al. (JAMA 333(4):319–328, DOI 10.1001/jama.2024.21700), Savage et al. (JAMIA 32(1):139–149), Goh et al. (JAMA Netw Open 7(10):e2440969), McDuff et al. (Nature 642:451–457), Wu et al. (Nat Commun 16), Hakim et al. (Sci Rep 15), Li et al. (TMLR 2024), MedAgentBoard (NeurIPS 2025), Gorenshtein et al. (still a medRxiv preprint). | Outstanding items 5–9, 12, 13. |
| C3 | Dropped every figure that remained secondary or unverified: the 49–96% alert-override range, the pooled OR 1.35 for medical RAG, the Almanac figures and the MetaMedQA figures. | Could not be verified from primary sources. |
| C4 | Dropped Liang et al. (JMIR AI 2026) despite its relevance, because the final title and version could not be confirmed. | Never cite without complete, verified metadata. |
| C5 | Dropped the ChatGPT-3.5 reference-fabrication study, the BioRAB results, the "scaling agent systems" preprint and MedHELM. | Not needed for the argument, or venue/version unresolved. |
| C6 | Named the model generation behind every cited limitation (Llama-2-based models in Hager; 2023-era models in Med-HALT). | Avoids attributing older-model failures to current models. |
| C7 | Kept positive LLM evidence (MedQA scores, AMIE differential diagnosis) in 1.1.2. | Balanced framing: exam strength vs decision readiness. |
| C8 | Stated that the unsupported-claim rate and status-appropriateness measures are the project's own operational definitions. | No source establishes them as standard metrics. |
| C9 | Dropped the McNemar/statistical-test citation. | Belongs to Chapter 3; Dietterich 1998 not verified. |
| C10 | Described regulatory documents as boundaries only; replaced any wording implying compliance with "design informed by". | Guidance is not evidence of effectiveness or a legal determination. |

## Editorial changes

| # | Change |
|---|---|
| E1 | Formal academic register; abbreviations defined at first use (CDSS, EHR, CPOE, LLM, USMLE, RAG, WHO, FDA, SDAIA, SFDA, NIST). |
| E2 | Removed repeated evidence: each key study is presented in one section and only referenced elsewhere. |
| E3 | Defined "agentic framework" precisely (bounded agents coordinated by a deterministic orchestrator) to avoid implying autonomy (audit C6). |
| E4 | Inserted into the official template `#F15-CS-1`: template cover kept, body in Times New Roman 12 pt with 1.5 line spacing, page numbers, captions above tables. |

# Key findings from the extended web research run, Version 2 (27 Nov 2026)

These are condensed notes from the second extended research run, for reconciliation with the structured research notes. Treat them as leads to cross-check, not as verified fact.

## Access honesty
- All searches were general web searches, which return no total hit counts.
- Scopus, WoS, IEEE Xplore, ACM DL, ScienceDirect/SDL, SpringerLink, Google Scholar, Semantic Scholar, OpenAlex and Crossref were NOT queried directly.
- Two PubMed E-utilities attempts failed: "URL too long" and "robots disallowed".
- 18 web queries were run (W1–W18). One delegated sub-search verified FActScore, AlignScore, RAGTruth, ALCE, MedHal and MedScore via ACL Anthology and arXiv.

## Corrections to the first pass
- **SourceCheckup [17]:**
  - "34.7% fixed" is wrong. SourceCleanup *removed* 34.7% (52/150) of unsupported statements entirely. Of the 98 modified statements, experts judged 85.7% (84/98) now supported. The 150 statements came from GPT-4o (RAG), GPT-4o (API) and Claude v2.1.
  - The published Nat Commun version reports 88.7% agreement between the GPT-4o verifier and the 3-doctor consensus. 88.0% vs 85.2% is the preprint (arXiv 2402.02008, GPT-4 verifier).
  - Doctors agreed with the verifier's "unsupported" label in 95.8% of cases (105/110).
  - The "87.0% (Claude Sonnet 3.5)" figure comes from a secondary source only and is unverified.
  - The published abstract says "approximately 30%" of GPT-4o-with-Web-Search statements are unsupported; the exact 30.6% is not confirmed in the published text. "50–90% of responses not fully supported" is confirmed from the abstract.
- **Omar [27]:** r = −0.40, P = .001, is a model-level correlation across 12 LLMs, not per answer. GPT-4o: 74% accuracy, 63% confidence. Qwen2-7B: 46% accuracy, 76% confidence.
- **Savage [28]:**
  - Authors: T. Savage, J. Wang, R. Gallo, A. Boukil, V. Patel, S. A. A. Safavi-Naini, A. Soroush, J. H. Chen. PMC11648734.
  - 723 questions (418 MedQA, 200 NEJM case reports, 105 Stanford); 24,941 physician-graded responses.
  - Sample consistency (SC) by sentence embedding gave ROC AUC 0.68–0.79 with poor calibration. SC by GPT annotation gave 0.66–0.74 with accurate calibration.
  - Best SC AUC, diagnosis/treatment: GPT-3.5 0.77/0.68; GPT-4 0.79/0.71; Llama2 0.77/0.56; Llama3 0.68/0.75.
  - ECE values appear only in Fig. 6: UNVERIFIED.
- **MedCalc-Bench [15]:**
  - The NeurIPS 2024 D&B paper has 1,047 test instances, 55 calculators (19 rule-based, 36 equation-based). The GitHub README says 10,543 train / 1,100 test; the supplement says 10,053 train.
  - The licence is stated as CC BY-SA 4.0 "for any non-commercial purposes", which is internally inconsistent.
  - The audit preprint arXiv:2603.02222 reports label problems ("MedCalc-Bench Verified").
- **MedHELM [14]:** Nat Med 32:943–951 (2026), doi 10.1038/s41591-025-04151-2, published 20 Jan 2026. 121 tasks, 5 categories, 22 subcategories.
- **AgentClinic [9]:** npj Digit Med 9:499 (27 Apr 2026), doi 10.1038/s41746-026-02674-7. The title changed to "…a multimodal benchmark for tool-using clinical AI agents". Preprint and published versions differ (24 vs 23 biases).
- **Lyell & Coiera [5]:** JAMIA 24(2):423–431, 2017, doi 10.1093/jamia/ocw105, PMID 27516495. 40 of 890 papers included, 6 in health care.
- **CRAFT-MD [8]:** 2,000 vignettes, 12 specialties, 4 LLMs.
- **MIMIC-IV-Ext-CDM [7]:** PhysioNet Credentialed Health Data License 1.5.0. Cases: 957 / 648 / 257 / 538 across the four pathologies.
- **HealthBench [13]:** the simple-evals code repository is MIT-licensed; whether that covers the data is UNVERIFIED. The m42-health reimplementation is CC BY-NC 4.0 and is not OpenAI's. 85.0% English (per commentary PMC12547120).

## New sources proposed (verify before citing)
- FActScore (EMNLP 2023, pp. 12076–12100). AlignScore (ACL 2023, pp. 11328–11348, doi 10.18653/v1/2023.acl-long.634). RAGTruth (ACL 2024, pp. 10862–10878).
  - RAGTruth best response-level F1 is 78.7 (fine-tuned Llama-2-13B); a GPT-4-turbo prompt reached F1 63.4; span-level F1 ≤ 52.7.
- ALCE (EMNLP 2023, pp. 6465–6488): about 50% of ELI5 generations lack full citation support.
- MedHal (AACL-IJCNLP 2026); MedScore (arXiv 2505.18452).
- Hybrid prior art:
  - arXiv 2610.15912, "More reasoning is not always better: Executing clinical guidelines with LLMs and rules": deterministic execution is only as good as LLM-extracted inputs.
  - C-Mind, neuro-symbolic KG + LLM for mental health (medRxiv/SSRN 7528492, 2026): 99.1% repeatable decisions; 100% auditable.
  - PainNS-Prove, chronic pain (ResearchGate; venue unverified).
  - Neuro-symbolic LLM in clinical medicine SR, 21 studies (Research Square rs-8902022, preprint).
  - CPGPrompt (arXiv 2601.03475).
  - Guideline2Graph (CVPR 2026, pp. 6942–6951).
  - Prenosil (Communications Medicine 2025): seen only via the secondary SR; reportedly a privacy task, so its relevance is doubtful.
- Determinability benchmarks: ClinDet-Bench (arXiv 2602.22771); "When to stop asking" (arXiv 2610.25819).
- Agent systems vs base LLMs: npj Digit Med 2026, doi 10.1038/s41746-026-02443-6, "only modest accuracy gains".
- Authoritative-cue fragility: Sci Rep 2026, doi 10.1038/s41598-026-38019-3. Near-zero accuracy under authoritative prompts; confidence unchanged (8.89 vs 8.84, P = .14).
- Gao et al., JAMIA Open 8:ooae154, 2025: verbalized-confidence ECE 0.27–0.59 vs Raw+XGB 0.02–0.10.
- J Med Syst 2026, doi 10.1007/s10916-026-02430-0, calibration of self-reported confidence.
- Conformal factuality control (arXiv 2609.38222): at a 95% target, only 10.8% / 9.7% of responses were non-empty.
- UNDCS commentary, npj Digit Med 2026, doi 10.1038/s41746-026-02420-z.

## Guidance
- **SFDA MDS-G010:** v1.0, 29 Nov 2022, listed 3 Jan 2023. PMC11250741 calls its binding nature "unresolved".
- **SDAIA:** AI Ethics Principles 2023. National AI Risk Management Framework per a Saudi Press Agency release of 14 Jul 2026 (official news; framework PDF not opened). The draft Responsible AI Policy is reported only secondarily.
- **FDA CDS guidance:** final 6 Jan 2026, reissued with correction 29 Jan 2026 (docket FDA-2017-D-6569).

## Gap reassessment (V2 proposal)
| Gap | Rating | Proposed change |
|---|---|---|
| G1 | Strongly supported | Keep |
| G2 | Moderately supported | Narrow to framing sensitivity; few studies on reordering effects on decision status |
| G3 | Strongly supported | Keep |
| G4 | Partially supported | Contextual until the triage studies are verified |
| G5 | Insufficiently investigated | Reframe as a methodology requirement |
| G6 | Strongly supported (problem); moderately (clinical verifier accuracy) | Split into G6a and G6b |
| G7 | Strongly supported that the question is open | Keep the nuance |
| G8 | — | Merge into G1/G4 |
| G9 | Strongly supported | Contextual only |
| G10 | Not supported as novelty | Replace with G10′ (positioning only) |
| G11 (new) | Insufficiently investigated | Verifier error propagation and remove-vs-repair effects |
| G12 (new) | Moderately supported | Benchmark label validity (methodological caution) |

## RQ revision proposals
- **RQ1:** use clinician-adjudicated labels with inter-rater κ, and report under- and over-escalation separately.
- **RQ2:** report precision, recall and κ against human claim–source labels. Separate removed from repaired statements, and retrieval hit rate from support.
- **RQ3:** pre-specify perturbation families (field order, synonym rewording, irrelevant detail) and report the status-flip rate.
- **RQ4:** add deterministic-only and LLM-only baselines, and fix the token budget.

## Team decisions
- Clinical presentation and dataset.
- API vs local model under the PhysioNet DUA.
- Labelling protocol and κ target.
- Compute-matching rule: equal tokens or equal calls.
- G10′ wording.
- Running the prepared database searches with PRISMA counts.

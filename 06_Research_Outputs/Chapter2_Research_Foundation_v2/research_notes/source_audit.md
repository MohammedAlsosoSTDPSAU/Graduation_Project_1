# Chapter 2 source audit: bibliographic and numerical verification of sources [1]–[39]

Audit date: 27 November 2026. Input: `ch2/Chapter2_Research_Foundation_Extended_Search.md` (§3–§4). Record per source: `source_audit.jsonl`. Lookups: `source_audit_searchlog.jsonl` (164 entries).

## Method and access limits

- **Scholar_Sidekick.** The tools loaded, but every call returned "You are not subscribed to this API". No resolution or retraction checks were run through them.
- **Crossref, PubMed and the other APIs.** The Crossref API, PubMed E-utilities, OpenAlex, Semantic Scholar, doi.org and arXiv are all blocked from the container and from the user's device (proxy 403). WebFetch accepts only URLs that a search returned, and one Crossref record ([28]) came back that way. PubMed and PMC pages mostly returned reCAPTCHA.
- **What verification rests on.** Publisher pages (Springer, MDPI, JMIR), official proceedings (NeurIPS, ACL Anthology), PhysioNet, arXiv full text, and institutional or repository records (Duke, Erasmus, TUM, UMN, Harvard DBMI, Stanford, UCLouvain). Index-level records are marked as such.
- **Retraction checks.** These were possible only for [28], via Crossref: no update notices. [3] has a published correction that does not affect the cited numbers. For every other source, retraction status is **not checked**. No notice appeared on any page inspected.
- **Usage limit.** A WebFetch usage limit interrupted the audit from 20:24 to 21:30 (Riyadh time). The audit then resumed where it had stopped.

## Summary table

| ID | Source | Status (peer review) | Full text | Audit verdict | Recommendation |
|---|---|---|---|---|---|
| [1] | Kawamoto 2005 BMJ | peer-reviewed journal (systematic review) | abstract | **PARTIALLY VERIFIED** | Retain; quote '68% of trials (70 studies)' and 30/32 (94%); drop CI/48-of-71 until full text read |
| [2] | van der Sijs 2006 JAMIA | peer-reviewed journal (review) | abstract | **PARTIALLY VERIFIED** | Retain; quote 17 papers and 49-96% only |
| [3] | Wong 2021 JAMA IM | peer-reviewed journal (retrospective cohort, external validation) | yes | **PARTIALLY VERIFIED** | Retain; cite correction notice; add page range after check |
| [4] | Goddard 2012 JAMIA | peer-reviewed journal (systematic review) | no | **PARTIALLY VERIFIED** | Retain (context); verify abstract before attributing mediators |
| [5] | Lyell & Coiera 2017 JAMIA | peer-reviewed journal (systematic review) | no | **PARTIALLY VERIFIED** | Qualify: read before citing any claim, or drop |
| [6] | Hager 2024 Nat Med | peer-reviewed journal | yes | **VERIFIED** | Retain (core anchor) |
| [7] | MIMIC-IV-Ext-CDM (PhysioNet) | other (dataset) | yes | **CORRECTED** | Retain with corrected title/DOI |
| [8] | Johri 2025 Nat Med (CRAFT-MD) | peer-reviewed journal | partial | **VERIFIED** | Retain; add 2,000 vignettes |
| [9] | AgentClinic (npj DM 2026) | peer-reviewed journal (journal version); preprint (arXiv) | partial | **PARTIALLY VERIFIED** | Retain; cite npj version, but take bias figures only from the version you cite |
| [10] | MedAgentBench (NEJM AI 2025) | peer-reviewed journal (journal version); preprint (arXiv) | partial | **PARTIALLY VERIFIED** | Retain; confirm vol/issue |
| [11] | Goh 2024 JAMA Netw Open | peer-reviewed journal (RCT) | abstract | **VERIFIED** | Retain |
| [12] | MediQ (NeurIPS 2024) | peer-reviewed conference | yes | **PARTIALLY VERIFIED** | Retain; reword 11.3% as relative, averaged |
| [13] | HealthBench (arXiv) | preprint (industry technical report) | partial | **VERIFIED** | Retain as preprint/technical report |
| [14] | MedHELM (Nat Med 2026) | peer-reviewed journal (journal version); preprint (arXiv) | partial | **CORRECTED** | Retain; cite Nat Med 2026 and its own numbers (37 evaluations) |
| [15] | MedCalc-Bench (NeurIPS 2024 D&B) | peer-reviewed conference | yes | **CORRECTED** | Retain; relabel as NeurIPS 2024 D&B |
| [16] | MIRAGE/MedRAG (Findings ACL 2024) | peer-reviewed conference (Findings) | no | **PARTIALLY VERIFIED** | Retain; re-read abstract for numbers |
| [17] | SourceCheckup (Nat Commun 2025) | peer-reviewed journal (published); preprint (arXiv v1) | partial | **PARTIALLY VERIFIED** | Retain; split preprint vs journal numbers; drop 34.7% and 88.7% |
| [18] | MDAgents (NeurIPS 2024) | peer-reviewed conference (oral per neurips.cc listing in search; not o | partial | **CORRECTED** | Retain; fix 4.2%/11.8% explanation |
| [19] | MedAgentBoard (NeurIPS 2025 D&B) | peer-reviewed conference | partial | **PARTIALLY VERIFIED** | Retain (key P4) |
| [20] | MedChain (NeurIPS 2025 D&B) | peer-reviewed conference | partial | **CORRECTED** | Retain; relabel as NeurIPS 2025 D&B |
| [21] | MAST (NeurIPS 2025 D&B) | peer-reviewed conference | partial | **VERIFIED** | Retain |
| [22] | Tran & Kiela (arXiv 2026) | preprint | abstract | **VERIFIED** | Retain as emerging preprint evidence |
| [23] | PACE (arXiv 2611.17610) | UNVERIFIED (foundation: preprint) | no | **UNVERIFIED** | Drop unless record supplied |
| [24] | Cost of Consensus (arXiv 2026) | preprint | partial | **CORRECTED** | Retain only with corrected numbers (preprint) |
| [25] | Equal inference cost (arXiv 2026) | preprint | partial | **VERIFIED** | Qualify: preprint, small n, unclear venue header |
| [26] | Griot 2025 Nat Commun (MetaMedQA) | peer-reviewed journal | abstract | **PARTIALLY VERIFIED** | Retain; do not cite GPT-4o 73% |
| [27] | Omar 2025 JMIR Med Inform | peer-reviewed journal (cross-sectional evaluation) | yes | **VERIFIED** | Retain |
| [28] | Savage 2025 JAMIA | peer-reviewed journal | partial | **VERIFIED** | Retain; no ECE numbers |
| [29] | Machcha (EACL 2026) | peer-reviewed conference | abstract | **CORRECTED** | Retain; relabel as EACL 2026 |
| [30] | Abstention audit (arXiv 2611.08780) | UNVERIFIED | no | **UNVERIFIED** | Drop unless record supplied |
| [31] | Cocchieri (ACL 2026) | peer-reviewed conference | abstract | **CORRECTED** | Retain |
| [32] | ED triage cohort (PMC13795786) | UNVERIFIED | no | **UNVERIFIED** | Drop unless record supplied |
| [33] | J Clin Med 2026 ESI study | UNVERIFIED | no | **UNVERIFIED** | Drop unless record supplied |
| [34] | EQUITRIAGE (arXiv) | preprint | partial | **PARTIALLY VERIFIED** | Retain as preprint (fairness context) |
| [35] | GI calibration (s44355, 2026) | peer-reviewed journal (per DOI link; journal not confirmed) | abstract | **PARTIALLY VERIFIED** | Qualify: confirm journal before citing |
| [36] | PhysioNet 2025 LLM guidance | institutional report/guidance | yes | **VERIFIED** | Retain (governance) |
| [37] | PhysioNet 2023 GPT guidance | institutional report/guidance | yes | **VERIFIED** | Retain (governance; superseded in part by [36]) |
| [38] | MedQA (Appl Sci 2021) | peer-reviewed journal | abstract | **CORRECTED** | Retain |
| [39] | MedAgents (Findings ACL 2024) | peer-reviewed conference (Findings) | no | **PARTIALLY VERIFIED** | Retain (metadata only; read before citing results) |

Totals: VERIFIED 11; PARTIALLY VERIFIED 15; CORRECTED 9; UNVERIFIED 4.

## Corrections to the foundation

### Wrong or misattributed numbers
- **[24] Cost of Consensus.** Not "single agent 66.7% using 619 tokens". The 66.7% is Qwen2.5-7B with **isolated self-correction at 6,170 tokens per problem**, against debate at 60.7% with 17,401 tokens. The zero-shot single-agent baseline is 57.3% (Tables 1 and 4, arXiv v1).
- **[18] MDAgents.** The "headline differs by version" claim is wrong. The arXiv v3 abstract and the NeurIPS proceedings abstract both report "up to 4.2%" and "11.8%", and the two figures measure different things:
  - 4.2% is the largest gain over the previous best baseline (p < 0.05).
  - 11.8% is the **relative** gain from 71.8% to 80.3% inside MDAgents' own ablation, from adding moderator review plus MedRAG (§4.3).
  - Other figures confirmed: 45.0% vs 39.0% is the sequential setting on high-complexity image+text tasks (App. D.4, Table 8), and 9.3 API calls at N = 3 is in §4.4.
- **[17] SourceCheckup.** The foundation attaches preprint-only numbers to the Nat Commun reference:
  - These figures are confirmed **only in arXiv v1**: 0.694 (CI 0.685–0.703) and 0.543 (CI 0.519–0.569) in App. Table 4, and 88.0% verifier agreement over 284 pairs against 85.2% between doctors, p = 0.21 (§4.1.2).
  - The **34.7% (52/150) SourceCleanup figure is not in v1**, and the published full text could not be read, so it is UNVERIFIED. So is the "88.7% in the published version".
  - The published abstract (16:3615) reports: 7 LLMs, 800 questions, 58,000 statement–source pairs, 50–90% of responses not fully supported, and GPT-4o with Web Search at about 30% of statements unsupported and nearly half of responses not fully supported.
- **[12] MediQ.**
  - The 11.3% is a **relative** drop from the non-interactive Initial setting to BASIC, averaged across LLMs. It does not hold for every model: for GPT-4 on iMedQA, BASIC is 55.4 against Initial 54.5.
  - The 22.3% compares the best system with BASIC; 12.1% compares it with Initial; it closes 51.2% of the gap between Initial and Full (§4.3, NeurIPS camera-ready v3).
- **[1] Kawamoto.** The abstract reports 70 *studies*, with improvement in 68% of *trials*. The "48 of 71 systems" count and the "95% CI 56–78%" are not in the abstract and remain UNVERIFIED.
- **[6] Hager.** The Results text says tools were hallucinated "every two to five patients". "Up to once every two patients" comes only from the Extended Data Fig. 6 caption. Cite the location you quote.
- **[14] MedHELM.**
  - The journal version reports **37 evaluations**; arXiv v2 reports 35 benchmarks (17 existing, 5 reformulated, 13 new).
  - The journal version also reports reasoning models at a 66% win rate, and Claude 3.5 Sonnet comparable at 15% lower cost.
- **[19] MedAgentBoard.** "Same base LLM for both arms" holds only for lay summaries and workflow automation. The single-LLM rows for EHR prediction use other models, and compute was not matched.
- **[21] MAST.** The taxonomy was built from 150 traces across **5** frameworks. The 7-framework dataset has 1,600+ traces. The failure-mode percentages are over 1,642 traces.
- **[36] PhysioNet.** "No individual notes in publications" is not in the 2025 post. Source it from the DUA or drop it.

### Wrong peer-review status or venue
- **[14] MedHELM.** Recorded as a preprint with no journal version. It is ***Nat. Med.* vol. 32, pp. 943–951, 2026**, doi:10.1038/s41591-025-04151-2, PMID 41559415. Volume and pages come from the Stanford GSB record.
- **[15] MedCalc-Bench.** Recorded with acceptance "not verified". It is in the **NeurIPS 2024 Datasets & Benchmarks Track**, vol. 37, pp. 84730–84745, doi:10.52202/079017-2690.
- **[20] MedChain.** Recorded as a preprint with authors and venue unverified. It is in the **NeurIPS 2025 Datasets & Benchmarks Track**, vol. 38, doi:10.52202/085713-2498, and all 10 authors are now verified.
- **[29] MedAbstain (Machcha et al.).** Recorded as a preprint. It is **EACL 2026, Vol. 1, pp. 6153–6182**, doi:10.18653/v1/2026.eacl-long.291.
- **[19] MedAgentBoard and [21] MAST.** The NeurIPS 2025 **Datasets & Benchmarks Track** is now confirmed from the venue line on each arXiv PDF.
- **[9] AgentClinic.** The npj Digital Medicine 2026 version exists, doi:10.1038/s41746-026-02674-7, with 7 authors, per the authors' page. Its volume and article number remain UNVERIFIED.

### Missing metadata now filled
- **Authors:**
  - [20] Liu, Wang, Ma, Huang, Su, Chang, Li, Shen, Lyu, Chen.
  - [24] Bertalanič & Fortuna.
  - [25] Dylan, Brennan, Murphy, O'Sullivan, Kelly, Walsh.
  - [31] Cocchieri, Ragazzi, Tagliavini, Moro.
  - [34] Young & Matthews.
  - [23] and [30] are still not found.
- **DOIs:**
  - [5] 10.1093/jamia/ocw105 (index level: researchr BibTeX), with vol. 24(2):423–431.
  - [38] 10.3390/app11146421 (MDPI).
  - [39] 10.18653/v1/2024.findings-acl.33 (ACL Anthology).
  - [18] 10.52202/079017-2522.
  - [7] 10.13026/ztyg-ah64.
- **Journals:** [35] has a published DOI, 10.1038/s44355-026-00053-3, linked from its arXiv page, but the **journal name is still unconfirmed**. [32] has not been found.
- **CRAFT-MD [8].** 2,000 vignettes: 1,800 MedQA-USMLE, 100 Derm-Public and 100 Derm-Private. Nat Med 31(1):77–86, PMID 39747685.
- **MedCalc-Bench [15].**
  - Counts: 1,047 test and 10,053 training instances; 55 calculators (19 rule-based, 36 equation-based).
  - Licence: CC-BY-SA 4.0. App. B.1 adds "non-commercial purposes", which contradicts CC-BY-SA; flag it if the licence matters.
  - Accuracy: GPT-4 one-shot CoT 50.91%. These figures come from arXiv v4, not the proceedings PDF.
- **HealthBench [13].** MIT licence through openai/simple-evals. The repository has not been updated for new models since July 2025 and is not actively maintained. The theme sizes (482, 594, 1,071) are confirmed.
- **MedHELM access [14].** 14 public, 7 gated (approval required) and 14 private Stanford datasets that are not released (arXiv v2, Table 7).
- **Omar [27].** r = −0.40 (P = .001) is confirmed in the abstract and in Results. It is a correlation **across models** between mean confidence on correct answers and model accuracy.
- **Savage [28].**
  - Journal abstract: sample-consistency AUC 0.68–0.79 with sentence embeddings (poorly calibrated) and 0.66–0.74 with GPT annotation (well calibrated).
  - The per-model AUCs are preprint-only.
  - **No ECE values are reported in the text** (they appear only in Figure 9), so they cannot be quoted.

## Items still UNVERIFIED

- [23] PACE: title, authors and record not found; arXiv 2611.17610 not indexed.
- [30] Abstention audit: title, authors and record not found; arXiv 2611.08780 not indexed.
- [32] ED triage cohort, PMC13795786: not found. One candidate (Research Square rs-9727564) was screened and excluded.
- [33] J Clin Med 15(4):1512: not found.
- [35] Journal name: DOI known, journal not confirmed.
- [17] SourceCleanup 34.7% (52/150) and the 88.7% figure for the published version.
- [1] The 48/71 systems count and the 95% CI 56–78%. [2] The 27% overdose-alert figure and the 2.3/2.5/6% adverse-event figures.
- [3] Print pages 1065–1070. [10] NEJM AI vol. 2, no. 9 and PMID 42932685. [9] npj volume and article number. [12] Proceedings DOI 10.52202/079017-0908 and pages. [19], [21] Proceedings pages and DOIs.
- [9] AgentClinic-MIMIC-IV n = 200. [11] '6 cases'. [26] GPT-4o ≈ 73%. [28] ECE values.
- [16] The MIRAGE numbers (7,663; 41 combinations; up to 18%) were not re-read in this audit. [4], [5], [39] Content claims not read.
- Retraction status for every source except [3] and [28].

## Recommendations

- **Drop** [23], [30], [32] and [33] unless the user supplies the records. Their numbers cannot be cited.
- **Qualify** [5], [25] and [35]:
  - [5]: no content read yet.
  - [25]: preprint, n = 134, and a TPAMI-style header with no stated acceptance.
  - [35]: journal not confirmed.
- **Retain with corrections:** [1], [9], [12], [14], [15], [17], [18], [19], [20], [24] and [29], as listed above.
- **Retain as is:** [2]–[4], [6]–[8], [10], [11], [13], [16], [21], [22], [26]–[28], [31], [34], [36]–[39], subject to the per-source notes in the JSONL.
- **Version discipline.** When citing a journal version, quote only numbers confirmed in that version. The following numbers are preprint-only: [9] (bias figures), [10] (all numbers), [15] (counts and accuracy), [17] (0.694, 0.543, 88.0%), [20] (Table 1) and [28] (per-model AUCs).
- Before submission, run a Crossref or retraction pass from a network that allows api.crossref.org.

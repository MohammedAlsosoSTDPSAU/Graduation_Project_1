# Research Foundation for Chapter 2: "An Agentic Framework for Clinical Decision Support in Healthcare"

> **Provenance.** Report produced on 27 November 2026 by the extended web-research run for the Chapter 2 research protocol, then saved without changes to its findings. It is a **first pass**:
> - Scopus, Web of Science, IEEE Xplore and the Saudi Digital Library were not queried directly.
> - Several 2026 sources have incomplete metadata.
> - The compute-confound evidence is mostly from preprints.
>
> Everything here must be checked against the parallel structured research (`research_notes/`) and Crossref before it is used in Chapter 2. **This is not a Chapter 2 draft.**

## Summary

The literature supports a narrower claim than the project title suggests.

**P1–P3 are well documented in peer-reviewed work:**
- LLMs lose reliability in realistic clinical workflows.
- They handle missing information and uncertainty poorly.
- Their cited sources often do not support their statements.

**P4 is supported, but the evidence cuts against multi-agent designs.** When single agents get matched compute or strong prompting, multi-agent systems usually show no consistent advantage. The defensible contribution is therefore a controlled test of whether a deterministic-plus-agentic design improves safety-relevant outcomes over a matched single agent. It is not a demonstration of multi-agent superiority.

### TL;DR

- **P1–P3 are well supported by peer-reviewed primary studies.**
  - In the MIMIC-IV-Ext-CDM study (2,400 real cases), LLMs did worse than physicians, did not follow guidelines, and were sensitive to the order and amount of information.
  - Diagnostic accuracy dropped in conversational and agentic settings (CRAFT-MD, AgentClinic, MedAgentBench).
  - In the SourceCheckup preprint, 30.6% of statements from GPT-4 with retrieval (RAG) were unsupported (statement-level support 0.694).
- **P4 is supported, but it works against assuming multi-agent superiority.**
  - MedAgentBoard (NeurIPS 2025) found that multi-agent collaboration "does not consistently outperform advanced single LLMs".
  - Compute-matched studies, all non-peer-reviewed 2026 preprints, find single agents match or beat multi-agent systems.
  - Medicine still has few compute-matched comparisons with ablations.
- **What the project can legitimately investigate:** a matched-baseline, ablation-based evaluation of status accuracy, verifier accuracy and robustness.
  - No benchmark labels all five output statuses. HealthBench labels emergency and context-seeking behaviour; MediQ and the abstention benchmarks label "ask" and "abstain".
  - Labels must therefore be constructed, for example on MIMIC-IV-Ext-CDM.
  - MIMIC carries PhysioNet restrictions: zero data retention, and local LLMs are strongly recommended.

## Key Findings

1. **Classical CDSS evidence.** CDSS improve practice often but not reliably: in 68% of the systems tested across 70 RCTs [1]. Two failure modes are well documented: alert fatigue (49–96% of alerts overridden) [2], and performance that collapses outside the development site (Epic Sepsis Model AUC 0.63 against the vendor's reported 0.76–0.83) [3]. Both justify the project's "fewer, higher-precision outputs" and its abstention design.
2. **Exam scores do not carry over to clinical workflows.**
   - In AgentClinic, accuracy on sequential tasks can drop "to below a tenth of the original accuracy" [9].
   - On MedAgentBench, the best model reached a 69.67% task success rate [10].
   - Physicians given GPT-4 did not improve their diagnostic reasoning (76% vs 74%, P = .60), even though the LLM alone outperformed them [11].
3. **Uncertainty handling is poor.**
   - Confidence barely separates right from wrong answers: mean confidence differs by only 0.6–5.4% [27].
   - Models fail to recognise unanswerable items [26].
   - In MediQ, prompting LLMs to ask questions caused an 11.3% accuracy drop compared with answering from the same limited information [12].
   - Sample consistency was the best uncertainty proxy in clinical scenarios [28]. This is directly relevant to the robustness design (RQ3).
4. **Citation is not support.** In the SourceCheckup preprint [17]:
   - 30.6% of GPT-4 (RAG) statements were unsupported (statement-level support 0.694).
   - 45.7% of responses were not fully supported (response-level support 0.543).
   - An automated revision step removed only 34.7% of unsupported statements.

   A verifier with one bounded revision should therefore be expected to reduce unsupported claims, not eliminate them.
5. **Multi-agent evidence is mixed to negative under fair comparison.**
   - MDAgents reports gains on 7 of 10 benchmarks [18].
   - MedAgentBoard [19] and MedChain's own baseline table [20] show multi-agent frameworks failing to beat strong single LLMs.
   - MAST documents 14 multi-agent failure modes, including verification failures [21].
   - Compute-matched studies (preprints) attribute most reported gains to extra computation [22]–[25].

## 1. Search Strategy and Boolean Queries per Database

### 1.1 Concept blocks

Blocks are combined with AND; terms within a block are combined with OR.

| Block | Terms |
|---|---|
| A. LLM/agent | "large language model*" OR LLM* OR GPT* OR "foundation model*" OR "generative AI" OR "LLM agent*" OR "multi-agent" OR "multiagent" OR agentic |
| B. Clinical decision | "clinical decision support" OR CDSS OR "clinical decision-making" OR diagnos* OR triage OR "treatment recommendation*" |
| C1. Robustness (P1) | "information gathering" OR "order effect*" OR perturbation OR robustness OR "prompt sensitivity" OR consistency OR "guideline adherence" |
| C2. Uncertainty (P2) | abstain* OR abstention OR "selective prediction" OR calibration OR overconfiden* OR "uncertainty estimation" OR escalat* OR "follow-up question*" OR "information seeking" |
| C3. Grounding (P3) | "retrieval-augmented" OR RAG OR citation* OR attribution OR faithfulness OR "claim verification" OR "fact-check*" OR hallucinat* |
| C4. Evaluation (P4) | "single-agent" OR baseline* OR "self-consistency" OR "majority voting" OR "test-time compute" OR "compute-matched" OR "token budget" OR ablation |
| D. Classical CDSS | "alert fatigue" OR "override*" OR "automation bias" OR "external validation" |

### 1.2 Database-specific strings

- **PubMed/MEDLINE:**
  ```
  ("Decision Support Systems, Clinical"[MeSH] OR "clinical decision support"[tiab] OR "clinical decision-making"[tiab]) AND ("large language model*"[tiab] OR "LLM*"[tiab] OR "GPT-4*"[tiab] OR "multi-agent"[tiab] OR agentic[tiab]) AND (abstain*[tiab] OR calibration[tiab] OR overconfiden*[tiab] OR "retrieval-augmented"[tiab] OR citation*[tiab] OR robustness[tiab] OR triage[tiab]) AND 2022:2026[dp]
  ```
  Classical arm, no date limit:
  ```
  ("Decision Support Systems, Clinical"[MeSH]) AND ("alert fatigue"[tiab] OR override*[tiab] OR "automation bias"[tiab])
  ```
- **Scopus:**
  ```
  TITLE-ABS-KEY(("large language model*" OR llm* OR "multi-agent" OR agentic) AND ("clinical decision" OR diagnos* OR triage) AND (abstain* OR calibrat* OR "retrieval-augmented" OR citation* OR "single-agent" OR baseline*)) AND PUBYEAR > 2021
  ```
- **Web of Science** (timespan 2022–2026):
  ```
  TS=(("large language model*" OR LLM* OR "multi-agent") AND ("clinical decision*" OR diagnos* OR triage) AND (abstain* OR uncertain* OR "retrieval-augmented" OR verif*))
  ```
- **IEEE Xplore:**
  ```
  ("All Metadata":"large language model" OR "All Metadata":"LLM agent" OR "All Metadata":"multi-agent") AND ("All Metadata":clinical OR "All Metadata":medical) AND ("All Metadata":"decision support" OR "All Metadata":verification OR "All Metadata":uncertainty)
  ```
- **ACM Digital Library** (2022–2026; this also captures NeurIPS proceedings indexed by ACM):
  ```
  [[Abstract: "large language model*"] OR [Abstract: "multi-agent"]] AND [[Abstract: clinical] OR [Abstract: medical]] AND [[Abstract: "decision support"] OR [Abstract: abstention] OR [Abstract: citation]]
  ```
- **ACL Anthology:** site search, or Google Scholar with `source:"ACL" OR source:"EMNLP" OR source:"NAACL"`, combined with:
  ```
  ("medical" OR "clinical") AND ("retrieval-augmented" OR "abstain" OR "multi-agent" OR "citation")
  ```
- **SpringerLink / ScienceDirect** (filter: Article, 2022–2026):
  ```
  "large language model" AND "clinical decision support" AND (agent OR "retrieval-augmented" OR uncertainty)
  ```
- **Saudi Digital Library:** run the Scopus and PubMed strings through SDL federated search, plus a regional arm:
  ```
  ("large language model*" OR "artificial intelligence") AND ("clinical decision support") AND (Saudi OR SFDA OR "Gulf")
  ```
- **Google Scholar** (discovery only; record the first 100 results of each):
  ```
  "multi-agent" "large language model" clinical "single-agent" baseline
  ```
  ```
  "compute-matched" OR "token budget" multi-agent LLM
  ```
- **Semantic Scholar / OpenAlex:**
  - Forward-citation chaining from the anchor papers [6], [8], [11], [17], [18], [19], [21], [22].
  - Backward chaining from their reference lists.
- **Crossref:** verify every DOI at `https://api.crossref.org/works/{DOI}`.
- **arXiv / medRxiv:**
  ```
  (ti:"multi-agent" OR abs:"multi-agent") AND (abs:clinical OR abs:medical) AND (abs:baseline OR abs:"single-agent")
  ```
  Label every result as a preprint until a peer-reviewed publication is verified.

### 1.3 Eligibility criteria

**Include:**
- Primary empirical studies.
- Systematic reviews.
- Benchmark and dataset papers.
- Peer-reviewed conference papers.
- LLM work from 2022 to November 2026.
- Seminal classical CDSS work, with no date limit.
- English language only.

**Exclude:**
- Opinion pieces, except as context.
- Vendor blogs.
- Studies with no quantitative outcome.
- Image-only work, unless it is agentic.
- Patient-facing chatbots without a decision-support component, unless they are benchmark sources.

## 2. Search Log (executed 27 November 2026)

All searches went through a general web search engine and a web-fetch tool, which returned PubMed, publisher, ACL Anthology, NeurIPS, arXiv and PhysioNet records. **The licensed databases (Scopus, Web of Science, IEEE Xplore, SDL) were not queried directly.** The §1.2 strings are a protocol for the team to run and record, so the search is reproducible and meets the Seminar requirement.

| # | Query (as run) | Target | Key records retained | Screening decision |
|---|---|---|---|---|
| 1 | Drive search: project or Seminar files | Google Drive | none found | Seminar.pdf not available to this review |
| 2 | Hager Nature Medicine 2024 … MIMIC-IV-Ext-CDM | PubMed, Nature, PhysioNet | [6], [7] | Included (anchor P1) |
| 3 | MedAgentBoard multi-agent single-LLM medical benchmark | arXiv, OpenReview, NeurIPS | [19], [20] | Included (P4) |
| 4 | MDAgents adaptive collaboration … NeurIPS 2024 | NeurIPS proceedings | [18] | Included (Theme 4) |
| 5 | Why do multi-agent LLM systems fail MAST | NeurIPS 2025 D&B | [21] | Included (Theme 5) |
| 6 | single-agent versus multi-agent equal compute budget | arXiv | [22]–[25] | Included as preprints; news and blog results excluded |
| 7 | Johri CRAFT-MD Nature Medicine 2025 | Nature, medRxiv | [8] | Included |
| 8 | MedAgentBench NEJM AI | NEJM AI, PubMed | [10] | Included |
| 9 | Goh JAMA Netw Open 2024 RCT | JAMA, PubMed | [11] | Included |
| 10 | Wu SourceCheckup Nature Communications 2025 | Nature, arXiv | [17] | Included |
| 11 | MedRAG MIRAGE Xiong ACL Findings 2024 | ACL Anthology | [16] | Included |
| 12 | MediQ … NeurIPS 2024 | NeurIPS, ACM DL | [12] | Included |
| 13 | PhysioNet responsible use MIMIC LLM | PhysioNet | [36], [37] | Included (governance) |
| 14 | HealthBench … 262 physicians | OpenAI PDF, arXiv | [13] | Included (labelled technical report / preprint) |
| 15 | Kawamoto 2005 BMJ | BMJ, PMC | [1] | Included (seminal) |
| 16 | van der Sijs 2006 overriding alerts | JAMIA | [2] | Included (seminal) |
| 17 | Wong 2021 Epic Sepsis Model | JAMA Intern Med | [3] | Included |
| 18 | Automation bias Goddard 2012, Lyell & Coiera | PubMed, Semantic Scholar | [4], [5] | Included |
| 19 | AgentClinic Schmidgall | arXiv, npj Digit Med | [9] | Included |
| 20 | MedHELM / MedCalc-Bench | (search budget exhausted) | covered by row 21 | Delegated to row 21 |
| 21 | Delegated sub-search: abstention, calibration, triage, MedHELM, MedCalc-Bench | arXiv, Nature, JAMIA, JMIR, ACL, MDPI, PMC | [14], [15], [26]–[35] | Included with flags |
| 22 | Full-text fetches: arXiv 2604.02460 and 2406.12036 | arXiv | [22], [15] | Abstracts confirmed; Nature full text for [6] returned a client error |

**Screening totals for this pass:**
- About 150 unique records surfaced; 39 were retained in the inventory.
- About 10 more are listed in §7 as "identified, not verified".
- Exclusions were aggregator, blog or news pages (for example VentureBeat and metavert summaries of [22]), duplicates, and records off-topic for CDS.

**Stopping rule:** this pass stopped at the tool budget, not at saturation. Run §1.2 before claiming that any gap is "unprecedented".

## 3. Source Inventory (IEEE style, with verified metadata and peer-review status)

**Verification key:**
- **V-pub**: checked against the publisher or PubMed record in this pass.
- **V-idx**: checked against an index or proceedings record (ACL Anthology, NeurIPS, ACM DL, arXiv).
- **V-sec**: metadata from a secondary source only; must be re-verified with Crossref.

| ID | IEEE reference | DOI / ID | Status | Verified | Full text read? |
|---|---|---|---|---|---|
| [1] | K. Kawamoto, C. A. Houlihan, E. A. Balas, D. F. Lobach, "Improving clinical practice using clinical decision support systems: a systematic review of trials to identify features critical to success," *BMJ*, vol. 330, no. 7494, p. 765, 2005. | 10.1136/bmj.38398.500764.8F | Peer-reviewed SR | V-pub | Partial (abstract and results via repository) |
| [2] | H. van der Sijs, J. Aarts, A. Vulto, M. Berg, "Overriding of drug safety alerts in computerized physician order entry," *J. Am. Med. Inform. Assoc.*, vol. 13, no. 2, pp. 138–147, 2006. | 10.1197/jamia.M1809 | Peer-reviewed review | V-pub | Partial |
| [3] | A. Wong *et al.*, "External validation of a widely implemented proprietary sepsis prediction model in hospitalized patients," *JAMA Intern. Med.*, vol. 181, no. 8, pp. 1065–1070, 2021. | 10.1001/jamainternmed.2021.2626 | Peer-reviewed cohort | V-pub | Partial (abstract and results excerpts) |
| [4] | K. Goddard, A. Roudsari, J. C. Wyatt, "Automation bias: a systematic review of frequency, effect mediators, and mitigators," *J. Am. Med. Inform. Assoc.*, vol. 19, no. 1, pp. 121–127, 2012. | 10.1136/amiajnl-2011-000089 | Peer-reviewed SR | V-pub (PMID 21685142) | Abstract only |
| [5] | D. Lyell, E. Coiera, "Automation bias and verification complexity: a systematic review," *J. Am. Med. Inform. Assoc.*, 2017. | DOI not verified | Peer-reviewed SR | V-sec | No |
| [6] | P. Hager *et al.*, "Evaluation and mitigation of the limitations of large language models in clinical decision-making," *Nat. Med.*, vol. 30, no. 9, pp. 2613–2622, 2024. | 10.1038/s41591-024-03097-1 | Peer-reviewed | V-pub (PubMed 38965432) | Partial (abstract and sections; full-text fetch failed) |
| [7] | P. Hager, F. Jungmann, D. Rueckert, "MIMIC-IV-Ext Clinical Decision Making (v1.1)," PhysioNet. | PhysioNet record | Dataset | V-idx | Landing page |
| [8] | S. Johri *et al.*, "An evaluation framework for clinical use of large language models in patient interaction tasks," *Nat. Med.*, vol. 31, pp. 77–86, 2025. | 10.1038/s41591-024-03328-5 | Peer-reviewed | V-pub | Partial |
| [9] | S. Schmidgall *et al.*, "AgentClinic: a multimodal agent benchmark to evaluate AI in simulated clinical environments," arXiv:2405.07960; published as "AgentClinic: a multimodal benchmark for tool-using clinical AI agents," *npj Digit. Med.*, 2026. | arXiv 2405.07960; npj s41746-026-02674-7 | Preprint; journal version peer-reviewed | V-idx; journal DOI not Crossref-checked | Partial |
| [10] | Y. Jiang, K. C. Black, G. Geng, D. Park, J. Zou, A. Y. Ng, J. H. Chen, "MedAgentBench: a virtual EHR environment to benchmark medical LLM agents," *NEJM AI*, vol. 2, no. 9, AIdbp2500144, 2025. | 10.1056/AIdbp2500144 | Peer-reviewed | V-pub (PubMed 42932685) | Partial |
| [11] | E. Goh *et al.*, "Large language model influence on diagnostic reasoning: a randomized clinical trial," *JAMA Netw. Open*, vol. 7, no. 10, e2440969, 2024. | 10.1001/jamanetworkopen.2024.40969 | Peer-reviewed RCT (NCT06157944) | V-pub (PMID 39466245) | Partial (open access) |
| [12] | S. S. Li *et al.*, "MediQ: question-asking LLMs and a benchmark for reliable interactive clinical reasoning," in *Proc. NeurIPS*, 2024. | 10.52202/079017-0908 | Peer-reviewed conference | V-idx (ACM DL, NeurIPS) | Partial |
| [13] | R. K. Arora *et al.*, "HealthBench: evaluating large language models towards improved human health," arXiv:2505.08775, 2025. | arXiv 2505.08775 | Technical report / preprint | V-idx | Partial (PDF) |
| [14] | S. Bedi *et al.*, "MedHELM: holistic evaluation of large language models for medical tasks," arXiv:2505.23802, 2025. | arXiv 2505.23802 | Preprint (no peer-reviewed version verified) | V-idx | Abstract only |
| [15] | N. Khandekar *et al.*, "MedCalc-Bench: evaluating large language models for medical calculations," arXiv:2406.12036, 2024. | arXiv 2406.12036 | Preprint (NeurIPS 2024 D&B acceptance not verified) | V-idx | Abstract |
| [16] | G. Xiong, Q. Jin, Z. Lu, A. Zhang, "Benchmarking retrieval-augmented generation for medicine," in *Findings ACL 2024*, pp. 6233–6251. | 10.18653/v1/2024.findings-acl.372 | Peer-reviewed conference | V-idx | Partial |
| [17] | K. Wu *et al.*, "An automated framework for assessing how well LLMs cite relevant medical references," *Nat. Commun.*, vol. 16, 3615, 2025. | 10.1038/s41467-025-58551-6 | Peer-reviewed | V-pub | Partial |
| [18] | Y. Kim *et al.*, "MDAgents: an adaptive collaboration of LLMs for medical decision-making," in *Proc. NeurIPS*, vol. 37, pp. 79410–79452, 2024. | arXiv 2404.15155 | Peer-reviewed conference (oral) | V-idx | Partial |
| [19] | Y. Zhu *et al.*, "MedAgentBoard: benchmarking multi-agent collaboration with conventional methods for diverse medical tasks," in *Proc. NeurIPS (D&B)*, 2025. | arXiv 2505.12371 | Peer-reviewed conference | V-idx | Partial |
| [20] | "MedChain: bridging the gap between LLM agents and clinical practice with interactive sequence," arXiv:2412.01605. | arXiv 2412.01605 | Preprint (authors and venue not verified) | V-sec | Tables only |
| [21] | M. Cemri, M. Z. Pan, S. Yang *et al.*, "Why do multi-agent LLM systems fail?" in *Proc. NeurIPS (D&B)*, 2025. | arXiv 2503.13657 | Peer-reviewed conference | V-idx | Partial |
| [22] | D. Tran, D. Kiela, "Single-agent LLMs outperform multi-agent systems on multi-hop reasoning under equal thinking token budgets," arXiv:2604.02460, 2026. | 10.48550/arXiv.2604.02460 | Preprint | V-idx | Abstract |
| [23] | "PACE: More agents, more samples, or more thinking? Untangling coordination, ensembling, and test-time compute in multi-agent LLMs," arXiv:2611.17610, 2026. | arXiv 2611.17610 | Preprint | V-idx (authors not recorded) | Partial (HTML excerpt) |
| [24] | "The cost of consensus: isolated self-correction prevails over unguided homogeneous multi-agent debate," arXiv:2605.00914, 2026. | arXiv 2605.00914 | Preprint | V-idx | Excerpt |
| [25] | "At equal inference cost, multi-agent structure does not beat a single frozen agent," arXiv:2609.04217, 2026. | arXiv 2609.04217 | Preprint | V-idx | Excerpt |
| [26] | M. Griot, C. Hemptinne, J. Vanderdonckt, D. Yuksel, "Large language models lack essential metacognition for reliable medical reasoning," *Nat. Commun.*, vol. 16, 642, 2025. | 10.1038/s41467-024-55628-6 | Peer-reviewed | V-pub (sub-search) | No (page error) |
| [27] | M. Omar, R. Agbareia, B. S. Glicksberg, G. N. Nadkarni, E. Klang, "Benchmarking the confidence of large language models in answering clinical questions," *JMIR Med. Inform.*, vol. 13, e66917, 2025. | JMIR e66917 | Peer-reviewed | V-pub (sub-search) | Abstract |
| [28] | T. Savage *et al.*, "Large language model uncertainty proxies: discrimination and calibration for medical diagnosis and treatment," *J. Am. Med. Inform. Assoc.*, vol. 32, no. 1, pp. 139–149, 2025. | 10.1093/jamia/ocae254 | Peer-reviewed | V-pub (PMID 39396184) | Abstract |
| [29] | S. Machcha *et al.*, "Knowing when to abstain: medical LLMs under clinical uncertainty," arXiv:2601.12471, 2026. | arXiv 2601.12471 | Preprint | V-idx | Abstract |
| [30] | Audit of biomedical fine-tuning and abstention, arXiv:2611.08780, 2026. | arXiv 2611.08780 | Preprint | V-idx (title and authors not recorded) | Excerpt |
| [31] | "LLMs (almost) never abstain under medical uncertainty," *Proc. ACL 2026* (long paper 1365). | ACL Anthology 2026.acl-long.1365 | Peer-reviewed conference | V-idx (authors not verified) | Partial |
| [32] | Real-world ED triage cohort comparing physicians and LLMs, PMC13795786. | PMC13795786 | Peer-reviewed (journal not confirmed) | V-sec | Excerpt |
| [33] | LLM ESI triage agreement study, *J. Clin. Med.*, vol. 15, no. 4, 1512, 2026. | MDPI jcm15041512 | Peer-reviewed | V-idx | Excerpt |
| [34] | "EQUITRIAGE" gender-bias audit of LLM triage, arXiv:2605.03998, 2026. | arXiv 2605.03998 | Preprint | V-idx | Excerpt |
| [35] | Calibration of 48 LLMs on ACG 2022 gastroenterology self-assessment exam questions, Nature-portfolio journal, 2026. | s44355-026-00053-3 | Peer-reviewed (journal title not confirmed) | V-sec | Snippet only |
| [36] | PhysioNet, "Use of MIMIC data with large language models and online services," 2025. | PhysioNet news post | Institutional guidance | V-idx | Yes |
| [37] | PhysioNet, "Responsible use of MIMIC data with online services like GPT," 2023. | PhysioNet news post | Institutional guidance | V-idx | Yes |
| [38] | D. Jin *et al.*, "What disease does this patient have? A large-scale open domain question answering dataset from medical exams (MedQA)," *Appl. Sci.*, vol. 11, no. 14, 6421, 2021. | DOI not verified | Peer-reviewed | V-sec | No |
| [39] | X. Tang *et al.*, "MedAgents: large language models as collaborators for zero-shot medical reasoning," in *Findings ACL 2024*, pp. 599–621. | DOI not verified | Peer-reviewed conference | V-sec | No |

## 4. Thematic Evidence Matrix

### Theme 1: Classical knowledge-based and ML-based CDSS

| Source | Method / data | Metrics | Findings (exact) | Limitations | Supports |
|---|---|---|---|---|---|
| [1] Kawamoto, BMJ 2005 | Systematic review of 70 RCTs; 15 CDSS features coded; univariate and multivariate analysis | Proportion of trials with significant practice improvement | 48 of 71 systems (68%; 95% CI 56–78%) improved practice. Four independent predictors: automatic provision in workflow; recommendations rather than assessments only; delivery at the time and place of decision; computer-based. 30 of 32 systems (94%) with all four features succeeded. | Process outcomes, not patient outcomes; pre-2005 systems | Design rationale: actionable statuses delivered at the decision point (RQ1) |
| [2] van der Sijs, JAMIA 2006 | Review of 17 papers on drug-alert overrides | Override rate | Alerts overridden in 49–96% of cases (high-level overdose alerts: 27%). Adverse events in 2.3%, 2.5% and 6% of overridden alerts in three studies. | Heterogeneous alert definitions | Alert fatigue: argues for high-precision outputs and limiting needless ESCALATE (RQ1) |
| [3] Wong, JAMA Intern Med 2021 | External validation of the Epic Sepsis Model; 38,455 hospitalisations; Univ. of Michigan, Dec 2018–Oct 2019 | AUC, sensitivity, PPV | AUC 0.63 (0.62–0.64) vs vendor-reported 0.76–0.83. Sensitivity 33%, specificity 83%, PPV 12%. Alerted on 18% of patients; missed 67% of sepsis cases. | Single site | External-validation gap (P4); alert burden (P2) |
| [4] Goddard, JAMIA 2012 | Systematic review of automation bias | Frequency, mediators | Automation bias occurs consistently. Mediators: cognitive style, task experience, trust, confidence. | Mostly pre-LLM | Human-in-the-loop risk: the physician decides but may over-rely (RQ1) |
| [5] Lyell & Coiera, 2017 | Systematic review: automation bias and verification complexity | — | Not extracted in this pass | Metadata to verify | Same as [4] |

### Theme 2: Single LLMs in realistic clinical reasoning

| Source | Method / data | Models | Metrics / baselines | Findings (exact) | Limitations | Supports |
|---|---|---|---|---|---|---|
| [6] Hager, Nat Med 2024 | Simulated autonomous workup on MIMIC-CDM: 2,400 real MIMIC-IV cases, 4 abdominal pathologies; full-information variant (MIMIC-CDM-FI) | Open-weight Llama-2-class models (Llama 2 Chat, OASST, WizardLM, and others) | Diagnostic accuracy vs clinicians; guideline adherence; lab interpretation; instruction following; order sensitivity | Significantly worse than physicians. Followed neither diagnostic nor treatment guidelines. Could not reliably interpret labs. Failed to order necessary tests. Instruction-following errors every 2–4 patients; hallucinated tools up to once every 2 patients. Sensitive to the quantity and order of information. | Older open models only (no frontier models, for data-use reasons); only the first lab event per test included | **P1, RQ1, RQ3** (core anchor) |
| [8] Johri, Nat Med 2025 (CRAFT-MD) | Vignettes converted to multi-turn dialogue with patient-AI and grader-AI agents; 12 specialties | GPT-4, GPT-3.5, Mistral, LLaMA-2-7b | Diagnostic accuracy, vignette vs conversation | Accuracy fell significantly in the conversational format for all models; history-taking was weak | Simulated patients; vignette count not re-verified here | **P1** (information gathering) |
| [9] Schmidgall, AgentClinic | Sequential dialogue, tests and tools; 9 specialties, 7 languages; MedQA, NEJM and MIMIC-IV variants | GPT-4/4o, Claude 3.5, Llama-3, Mixtral, others | Diagnostic accuracy; bias perturbations | Accuracy can drop "to below a tenth of the original accuracy". MedQA accuracy only weakly predicts AgentClinic accuracy. Llama-3 improves by up to 92% (relative) with a notebook tool. GPT-4 under bias perturbations: maximum absolute drop 4%, average 1.5%. | The choice of patient-agent LLM affects scores | **P1, RQ3**; simulation caveat |
| [10] Jiang, NEJM AI 2025 (MedAgentBench) | FHIR virtual EHR; 300 physician-written tasks in 10 categories; 100 patients; >700,000 data elements | 12 LLMs | Task success rate | Best model (Claude 3.5 Sonnet v2) 69.67%; large variation across task categories | Narrow task scope; maximum 8 rounds | **P1, P4** (realistic environment) |
| [11] Goh, JAMA Netw Open 2024 | Single-blind RCT; 50 physicians; 6 cases; GPT-4 vs conventional resources | GPT-4 (ChatGPT Plus) | Structured diagnostic reasoning score | 76% vs 74%; adjusted difference 2 pp (95% CI −4 to 8; P = .60). The LLM alone outperformed both physician groups. | Vignettes; no training in LLM use | **Human-in-the-loop (RQ1)**: adding an LLM to the physician does not automatically improve outcomes |

**Run-to-run inconsistency and wording sensitivity:** [6] shows effects of order and quantity of information. [28] relies on variance across samples, which presupposes inconsistency between runs. No dedicated clinical rewording study was fully verified in this pass (see §7).

### Theme 3: Medical RAG, citation faithfulness and claim verification

| Source | Method / data | Metrics | Findings (exact) | Limitations | Supports |
|---|---|---|---|---|---|
| [16] Xiong, Findings ACL 2024 (MIRAGE / MedRAG) | 7,663 questions from 5 medical QA datasets; 41 corpus/retriever/LLM combinations; >1.8 trillion prompt tokens | QA accuracy | MedRAG improved the accuracy of six LLMs by up to 18% over prompting without retrieval | Multiple-choice QA only; measures accuracy, not faithfulness | P3 background; retrieval design choices |
| [17] Wu, Nat Commun 2025 (SourceCheckup) | 7 LLMs; 800 questions; ~58,000 statement–source pairs; an LLM "Source Verification" model validated against 3 physicians | Statement-level and response-level support | GPT-4 (RAG), from the preprint: 30.6% of statements unsupported (support 0.694, 95% CI 0.685–0.703); 45.7% of responses not fully supported (0.543). The verifier agreed with the 3-doctor consensus 88.0% of the time across 284 statement–source pairs, against 85.2% average agreement between doctors (p = 0.21); 88.7% in the published version, per a secondary source. SourceCleanup revision removed 34.7% (52/150) of unsupported statements. | Consumer-style questions; URL sources rather than guidelines | **P3, RQ2**: unsupported-claim rate, verifier accuracy, limits of revision |

### Theme 4: Clinical multi-agent LLM systems

| Source | Architecture | Data / baselines | Findings (exact) | Limitations | Supports |
|---|---|---|---|---|---|
| [18] MDAgents, NeurIPS 2024 | A complexity classifier routes each case to a solo agent, a multidisciplinary team or an integrated-care team; moderator review; MedRAG | 10 medical benchmarks (MedQA, PubMedQA, DDXPlus and others). Baselines: solo (zero-/few-shot, CoT, self-consistency) and group | Best on 7 of 10 benchmarks. The headline differs by version: "up to 4.2% (p < 0.05)" over the prior best (arXiv) vs an "11.8%" average gain from moderator review plus MedRAG (NeurIPS page). With vs without discussion: 45.0% vs 39.0% (sequential). About 9.3 API calls at N = 3. | Baselines not matched on compute, tools or retrieval; mostly exam-style QA | Theme 4; **P4** (unmatched baseline) |
| [39] MedAgents, Findings ACL 2024 | Role-play expert discussion | Medical QA | Not extracted | Metadata to verify | Theme 4 |
| [20] MedChain (preprint) | MedChain-Agent | Five-stage clinical-workflow benchmark. Compared CoT, self-consistency, RAG, AutoGen, MDAgents and others, all on InternVL2-8B | MedChain-Agent average 0.5200. AutoGen (multi-agent) scored 0.4084, below single-agent CoT (0.4189). | Small backbone; the authors' own benchmark | **Contradictory evidence (P4)** |

### Theme 5: Single-agent versus multi-agent evidence

| Source | Status | Design | Findings (exact) | Supports |
|---|---|---|---|---|
| [19] MedAgentBoard | Peer-reviewed (NeurIPS 2025 D&B) | 4 task categories: medical QA/VQA, lay summaries, EHR prediction, workflow automation. Same base LLM (DeepSeek-V3-0324) for the single-LLM and multi-agent arms; conventional baselines. | Multi-agent helps in some scenarios (task completeness in workflow automation). It "does not consistently outperform advanced single LLMs" (e.g., textual QA) or conventional methods (VQA, EHR prediction). | **P4, RQ4**: in medicine, single agents match or beat multi-agent systems |
| [21] MAST | Peer-reviewed (NeurIPS 2025 D&B) | Grounded theory on 150 traces (κ = 0.88); 7 frameworks; LLM-as-judge annotation | 14 failure modes in 3 categories: system design, inter-agent misalignment, task verification. Premature termination 6.20%; no or incomplete verification 8.20%; incorrect verification 9.10%. Design fixes gave a +9.4% success gain for ChatDev. | Failure taxonomy; justifies deterministic orchestration and verifier testing (RQ2) |
| [22] Tran & Kiela | Preprint | Information-theoretic argument plus experiments; Qwen3, DeepSeek-R1-Distill-Llama, Gemini 2.5; matched thinking-token budgets | Single agents "consistently match or outperform" multi-agent systems on multi-hop reasoning. API budget-control artifacts (Gemini 2.5) inflate apparent multi-agent gains. | **Compute confound (RQ4)**; non-medical, no tools |
| [23] PACE | Preprint | Non-systematic synthesis of 9 studies and 516 multi-agent configurations | Against sample- or budget-matched single-agent baselines, multi-agent accuracy averages 0.042 lower and is at or below baseline in 74% of contrasts | Baseline-ladder methodology (RQ4) |
| [24] Cost of Consensus | Preprint | Debate vs self-correction vs extended single agent | Example: Qwen on MMLU-Hard, single agent 66.7% using 619 tokens vs debate 60.7% using 17,401 tokens | Cost and latency (RQ4) |
| [25] Equal inference cost | Preprint | Planner→executor→critic team vs single agent at an equal number of LLM calls | Team minus single = +0.015 (p = 0.80, not significant) | Template for a matched-calls design (RQ4) |

### Theme 6: Safety mechanisms (uncertainty, abstention, escalation, human-in-the-loop)

| Source | Design | Findings (exact) | Supports |
|---|---|---|---|
| [12] MediQ (NeurIPS 2024) | Patient system plus expert system; single-turn benchmarks converted to interactive form; an abstention module decides whether to ask | Prompting LLMs to ask questions caused an 11.3% accuracy drop compared with answering from the same limited information without asking. The best abstention setup (Scale + Rationale Generation + Self-Consistency = 3) improved on the BASIC interactive setup by 22.3% and on the non-interactive Initial setup by 12.1%. It still closed only 51.2% of the gap to full information. | **P2, NEED_MORE_INFO** |
| [26] MetaMedQA (Nat Commun 2025) | 1,373 MedQA-derived items with fictional, malformed and answer-removed questions; "none of the above" and "I don't know" options; confidence scores | Models fail to recognise unanswerable or missing-answer items; gap between perceived and actual capability | **P2, ABSTAIN** |
| [27] Omar (JMIR Med Inform 2025) | 12 LLMs; 1,965 MCQs; 5 specialties | Mean confidence differed by only 0.6–5.4% between correct and incorrect answers; worse models were more confident | **P2 (overconfidence)** |
| [28] Savage (JAMIA 2025) | GPT-3.5/4 and Llama 2/3 on 3 datasets; confidence elicitation vs token probability vs sample consistency | Sample consistency discriminated best; elicited confidence was overconfident; better for diagnosis than for treatment | **P2, RQ3** (consistency as an uncertainty signal) |
| [29] MedAbstain (preprint) | MedQA and AMBOSS; conformal prediction; perturbations; explicit abstain option | High-accuracy models often fail to abstain; an explicit abstain option helps far more than model scale | **P2** |
| [30] Abstention audit (preprint) | Base vs biomedically fine-tuned models on MedAbstain | Median appropriate abstention 8.3% (base) vs 4.2% (fine-tuned); abstention fell in 21 of 25 pairs | **P2** |
| [31] MedQAbstain (ACL 2026) | Gold answer removed; abstain option added | Models "(almost) never abstain", even when the question is hidden | **P2** |
| [32] ED triage cohort | Physicians vs LLMs against a reference standard | Undertriage ranged from 6.23% (a physician) to 36.39% (GPT-4.1); overtriage 6–56%; specialty assignment 0.88–0.93 (physicians) vs 0.62–0.80 (LLMs) | **ESCALATE_NOW safety** |
| [33] J Clin Med 2026 | 39,375 ED encounters; LLM vs physician-assigned ESI | Best weighted κ ≈ 0.467, accuracy 61.7%; no model reached κ > 0.80 | Escalation |
| [34] EQUITRIAGE (preprint) | 5 LLMs; 374,275 evaluations on 18,714 MIMIC-IV-ED vignettes | Undertriage flips that differ by gender (e.g., 2.03:1 female:male at ESI-1 for one model) | Escalation fairness |
| [35] Gastroenterology calibration study | 48 LLMs; 300 exam questions | Best-calibrated models: Brier 0.15–0.2, AUROC around 0.6 | P2 |

**Not adequately searched in this pass:** hybrid neuro-symbolic (rules plus LLM) CDS. No primary study of a deterministic decision core combined with LLM agents was verified. This is the main hole to close (see §7).

### Theme 7: Datasets, benchmarks and metrics

| Benchmark | Size | Data source | Licence / access | Abstain / need-more-info / escalate labelled? |
|---|---|---|---|---|
| MedQA [38] | 12,723 English questions, plus 34,251 simplified-Chinese and 14,123 traditional-Chinese questions (Jin *et al.*, arXiv 2009.13081) | Licensing-exam questions | Public (verify licence) | No. Derivatives add abstention: MetaMedQA [26], MedAbstain [29], MedQAbstain [31]. |
| MIMIC-IV-Ext-CDM [6], [7] | 2,400 cases; 4 abdominal pathologies | MIMIC-IV (real EHR) | PhysioNet credentialed access (CITI training, individual DUA). No sharing via third-party APIs unless zero retention, no training use and no human review are verified. Locally deployed LLMs strongly recommended [36], [37]. No individual notes in publications. | No status labels. It has diagnoses, treatments and the tests clinicians ordered, so NEED_MORE_INFO can be derived from missing required tests. |
| CRAFT-MD [8] | 12 specialties (vignette count to verify) | Vignettes converted to conversations | Code public (GitHub) | No; measures history-taking |
| AgentClinic [9] | MedQA, NEJM (multimodal) and MIMIC-IV variants (e.g., AgentClinic-MedQA n = 215; MIMIC-IV n = 200, per a later paper) | MedQA, NEJM cases, MIMIC-IV | Code and data public; MIMIC variant subject to PhysioNet terms | No explicit abstain label; information gathering is implicit |
| MedAgentBench [10] | 300 tasks; 100 patients; >700,000 data elements | Synthetic or de-identified FHIR profiles | Public (GitHub) | No; tasks are queries or actions |
| HealthBench [13] | 5,000 conversations; 48,562 rubric criteria; 262 physicians (60 countries, 26 specialties); Consensus subset 3,671; Hard subset 1,000 | Mostly model-generated conversations, with physician-written rubrics | Open-source (licence to verify) | **Partly.** The "emergency referrals" theme (482 conversations) is labelled emergent, conditionally emergent or non-emergent. The "context-seeking" theme (594) includes "more context needed". There is also a "responding under uncertainty" theme. |
| MedHELM [14] | 5 categories, 22 subcategories, 121 tasks; 35 benchmarks (17 existing, 18 new); 9 LLMs | Mixed, including Stanford EHR data | Framework open; several benchmarks gated (licences vary; verify) | No dedicated abstention metric found |
| MedCalc-Bench [15] | >1,000 manually reviewed instances; 55 calculators (1,047 test instances, unverified) | Patient notes (reportedly mainly PMC-Patients) | Licence unverified (reportedly CC-BY-SA 4.0) | No; a single ground-truth value per instance |
| MediQ [12] | Interactive versions of MedQA (10,178 / 1,272 / 1,273 train / dev / test) and CRAFT-MD (140 dermatology records: 100 from an online question bank, 40 written by expert clinicians) | Existing QA benchmarks | Public code; both source datasets CC-BY 4.0 | **Yes, implicitly**: ask vs answer decisions |
| MIRAGE [16] | 7,663 questions from 5 datasets | MMLU-Med, MedQA, MedMCQA, PubMedQA, BioASQ | Public | No |
| SourceCheckup [17] | 800 questions; ~58,000 statement–source pairs | Generated from medical reference texts | Code on GitHub | Supported / unsupported labels per statement (useful for RQ2) |
| MetaMedQA / MedAbstain / MedQAbstain [26], [29], [31] | 1,373 items (MetaMedQA) | MedQA-derived (MedAbstain also uses proprietary AMBOSS) | MetaMedQA on Hugging Face; AMBOSS proprietary | **Yes**: abstain or unanswerable labels |

**Key conclusion for Theme 7:** no benchmark identified here labels all five project statuses. The nearest analogues are:
- HealthBench, for escalation and context-seeking;
- MediQ, for asking questions;
- the abstention benchmarks, for ABSTAIN.

The project must therefore construct and validate a status reference standard, for example clinician-adjudicated labels on MIMIC-IV-Ext-CDM cases.

## 5. Comparative Analysis of the Most Relevant Studies

**5.1 Methods.**
- *Realistic-workflow evaluations* [6], [8], [9], [10] share one design: information is withheld and must be gathered. This consistently lowers performance compared with full-information or exam formats. The pattern holds across independent groups, datasets (real EHR, vignettes, FHIR) and model generations, which makes it the strongest evidence for P1.
- *Multi-agent clinical systems* [18], [20], [39] mostly evaluate on exam-style QA. Their baselines are prompting variants without matched calls or retrieval; MDAgents, for example, adds MedRAG and moderator review only to its group arm.
- *Benchmark studies with matched backbones* [19] reverse the conclusion.

**5.2 Datasets.**
- Only [6] and [10] use real or realistically structured EHR data.
- Only [11] involves practising physicians in a randomised design.
- HealthBench [13] is large, but mostly synthetic and model-graded.
- The benchmarks with abstention labels [26], [29], [31] are all multiple-choice derivatives of MedQA. They test whether a model recognises an unanswerable question, not clinical NEED_MORE_INFO.

**5.3 Metrics.** The literature reports:
- accuracy and task success rate [6], [9], [10];
- rubric scores [13];
- statement support rate [17];
- confidence discrimination and calibration (AUROC, Brier, ECE) [28], [35];
- undertriage and overtriage rates [32];
- frequencies of multi-agent failure modes [21].

No study found reports all of the following in one system: status accuracy, a safety-weighted confusion matrix (unsafe RECOMMEND vs unnecessary ESCALATE or ABSTAIN), verifier precision and recall, and robustness to reordering. That combination is the project's methodological niche. It is a synthesis gap, not a "first ever" claim.

**5.4 Reported results in context.** The numbers cannot be compared across studies because the tasks differ, but the directions are consistent:
- Accuracy drops when the task becomes interactive: below one-tenth of the original in [9]; 69.67% best success in [10].
- In the SourceCheckup preprint, 30.6% of GPT-4 (RAG) statements were unsupported even with retrieval [17].
- Multi-agent gains shrink or reverse under matched baselines: [19], and the preprints [22]–[25].

**5.5 Critical appraisal.**
- **Simulation dependence.** [8], [9], [12] and [13] rely on LLM-simulated patients or graders, and [9] shows that the choice of patient-agent LLM changes scores.
- **Model obsolescence.** [6] tested Llama-2-era models. Replicating it with frontier models on MIMIC-CDM is constrained by PhysioNet rules [36], and the same constraint applies to this project.
- **Preprint weight.** The compute-confound argument rests mainly on 2026 preprints [22]–[25], and [23] explicitly uses a non-systematic selection of studies. Cite these as emerging evidence, alongside the peer-reviewed MedAgentBoard [19] and MAST [21].
- **Version discrepancies.**
  - MDAgents' headline gain is 4.2% in one version and 11.8% in another, because the two figures compare different things.
  - SourceCheckup's verifier agreement is 88.0% in the preprint (284 pairs) and 88.7% in the published version (per a secondary source).
  - In both cases, quote the figure from the version you cite.

## 6. Preliminary Research-Gap Map

| Gap | Problem / RQ | Evidence | Rating | What the project can legitimately claim |
|---|---|---|---|---|
| G1. LLM reliability falls in realistic, information-gathering clinical tasks | P1 / RQ1 | [6], [8], [9], [10], [11] | **Well supported** | The motivation is established; there is no need to re-prove it |
| G2. Sensitivity to order and wording; inconsistency between runs | P1 / RQ3 | [6] (order and quantity); [28] (sample consistency); [9] (bias perturbations, small for GPT-4) | **Partly supported** | Test the robustness of status and ranking explicitly. Note that GPT-4 showed small perturbation effects in [9]. |
| G3. Poor handling of missing information, overconfidence, little abstention | P2 / RQ1 | [12], [26], [27], [28], [29]–[31], [13] | **Well supported** | Explicit status outputs are justified. [29] finds that an explicit abstain option helps most, which supports the five-status design. |
| G4. Escalation and triage safety (undertriage) | P2 / RQ1 | [32], [33], [34]; HealthBench emergency theme [13] | **Partly supported** (several sources lack verified venue or metadata) | Report unsafe RECOMMEND and missed ESCALATE_NOW as primary safety metrics |
| G5. No benchmark labels RECOMMEND, CAVEATS, NEED_MORE_INFO, ESCALATE and ABSTAIN together | P2, P4 / RQ1 | Absence across [7]–[16], [26], [31] | **Partly supported** (search not exhaustive) | Building a clinician-adjudicated status reference set is a legitimate contribution; avoid "first" claims |
| G6. A citation is not support; verifiers are imperfect; revision only partly fixes errors | P3 / RQ2 | [17]: 30.6% of GPT-4 (RAG) statements unsupported, 88.0% verifier agreement, 34.7% fixed by revision. [21]: verification failure modes. | **Well supported** for the problem; **partly supported** for the accuracy of verifiers on clinical guidelines | Measure verifier precision and recall against clinician labels, and the unsupported-claim rate with and without verification |
| G7. A multi-agent advantage is not established under matched baselines | P4 / RQ4 | [19] (medical, peer-reviewed) and [20] (medical, preprint); [22]–[25] (general, preprints); [18] (positive but unmatched) | **Well supported** that the question is open; **partly supported** that medicine lacks compute-matched studies | A matched single-agent baseline plus ablation is the core legitimate contribution. A null result is publishable and informative. |
| G8. Few safety-focused evaluations on real data | P4 / RQ1, RQ4 | [6], [10], [11] exist but are few | **Partly supported** | Use MIMIC-IV-Ext-CDM under PhysioNet rules, with local models |
| G9. Physician plus AI does not automatically improve decisions; automation bias | Human-in-the-loop / RQ1 | [11], [4], [2] | **Well supported** | Present outputs as support for physician judgement, not a replacement for it; this justifies ABSTAIN and caveats |
| G10. A deterministic decision core combined with LLM agents (neuro-symbolic CDS) is under-evaluated | P4 / RQ4 | Not adequately searched | **Not supported (insufficient search)** | Do not claim novelty until §1.2 has been run with neuro-symbolic and knowledge-graph terms |

**Integrity note.** The evidence does *not* support the hypothesis that multi-agent orchestration itself improves accuracy. It fits better with the hypothesis that deterministic guards, explicit statuses and verification improve *safety* metrics, whether or not multiple agents are used. That makes RQ4's comparison against a matched single agent with the same deterministic core the critical test.

## 7. Unresolved Evidence Gaps and Inaccessible Full Texts

**Full texts not read, or read only in part:**
- [6]: the Nature full text returned a client error, so exact per-model accuracies, the list of evaluated models and pathology-level results were not extracted. Read the PMC or author version.
- [17]: the published Nature Communications page could not be loaded, so the GPT-4o (web search) figures were not extracted. The statement- and response-level figures used here come from the preprint (GPT-4 with RAG).
- [26]: the Nature page errored, so per-model numbers were not verified. A recalled figure of about 73% for GPT-4o is unconfirmed.
- [27]: a confidence–accuracy correlation (r = −0.40) seen in a secondary source has not been confirmed in the original.
- [28]: exact AUC and ECE values not extracted.
- [5], [38], [39]: DOIs not verified.
- [20], [23]–[25], [30], [31]: authors not recorded.
- [32]: journal not confirmed.

**Unverified dataset details:**
- MedCalc-Bench: exact counts (1,047 test, about 10,053 training), licence, and the headline accuracy (a reported 50.9% for GPT-4 one-shot CoT).
- MedHELM: access terms for each benchmark.
- HealthBench: licence.
- CRAFT-MD: vignette count.

**Identified but not verified (screen before use):**
- Bedi *et al.*, "Fidelity of medical reasoning in large language models," *JAMA Netw. Open* 8(8):e2526021, 2025. Likely relevant to wording sensitivity (P1/RQ3).
- Chen *et al.*, multi-agent conversational diagnosis, *npj Digit. Med.* 8:159, 2025.
- Gaube *et al.*, "Do as AI say," *npj Digit. Med.* 4:31, 2021 (automation bias).
- Everett *et al.*, an RCT of a clinician–AI collaborative workflow, *npj Digit. Med.* 9:409, 2026.
- Gu *et al.*, stress-testing the readiness of health AI, *Nat. Med.* 2026.
- DECIDE-AI (Vasey *et al.*, *Nat. Med.* 28:924–933, 2022), as a reporting guideline.
- Almanac (Zakka *et al.*, *NEJM AI* 2024), a clinical RAG system.
- arXiv 2507.10911, a multi-agent system for safer therapy recommendation.
- arXiv 2607.18086, prompting based on evidence sufficiency.

**Topics not searched:**
- Neuro-symbolic or rule-plus-LLM CDS, and guardrail frameworks.
- General attribution and claim-verification methods (e.g., FActScore, AlignScore, RAGTruth), including the accuracy of NLI-based verifiers.
- Self-consistency in medical LLMs (e.g., Med-PaLM-style ensemble refinement).
- Regulatory guidance (WHO guidance on large multimodal models, FDA CDS software guidance, SFDA AI/ML medical-device guidance, SDAIA AI ethics principles, NIST AI RMF), to be cited only as institutional context.
- Saudi and Gulf regional studies (via SDL).

## 8. Recommended Detailed Chapter 2 Outline (under the mandatory 2.1–2.4)

**2.1 Review of Existing Work**
- 2.1.1 Classical CDSS: effectiveness and failure modes. Sources [1], [2], [3]; automation bias [4], [5].
- 2.1.2 LLMs in clinical reasoning: from exam benchmarks to workflow evaluation. MedQA [38] vs [6], [8], [9], [10]; the physician–LLM trial [11].
- 2.1.3 Uncertainty, abstention, information seeking and escalation. Sources [12], [26]–[35]; HealthBench themes [13].
- 2.1.4 Retrieval-augmented generation and claim-to-source verification. Sources [16], [17], plus the verifier literature still to be added (§7).
- 2.1.5 Clinical multi-agent LLM systems. Sources [18], [39], [20].
- 2.1.6 Single-agent vs multi-agent evidence and multi-agent failure modes. Sources [19], [21], [22]–[25].
- 2.1.7 Hybrid deterministic–LLM designs, guardrails and human-in-the-loop. To be written after the §7 search.
- 2.1.8 Datasets, benchmarks, metrics and data governance. Theme 7; PhysioNet rules [36], [37].

**2.2 Comparative Analysis**
- 2.2.1 Method comparison table: architecture, orchestration, type of baseline, whether compute was matched.
- 2.2.2 Dataset comparison table: size, source, licence, status labels (adapted from the Theme 7 table in §4).
- 2.2.3 Metric comparison: accuracy, rubric scores, support rate, calibration, triage error, failure-mode rate.
- 2.2.4 Reported results and the conditions they apply to, with a critical appraisal covering simulation dependence, preprint status and version discrepancies.

**2.3 Identified Gaps**
- 2.3.1–2.3.5: map G1–G10 to P1–P4, giving the evidence rating from §6 for each. Include G7's counter-evidence and G10's "insufficiently searched" status.

**2.4 Summary and Justification**
- 2.4.1 What is established: G1, G3, G6, G9.
- 2.4.2 What remains open: G2, G5, G7, G8.
- 2.4.3 How RQ1–RQ4 follow from this. The project is framed as a matched-baseline, ablation-based safety evaluation in which a null multi-agent result is a valid outcome.
- 2.4.4 Constraints. PhysioNet rules require local LLMs, which limits comparisons with frontier models. The target presentation has not yet been chosen.

## Caveats

- **First pass only.** This used web search and fetches of publisher and index pages, not direct queries of Scopus, Web of Science, IEEE Xplore or SDL. Run and log the §1.2 searches before submitting Chapter 2.
- **Incomplete metadata.** Several 2026 items found through search and the delegated sub-search have incomplete metadata. All are labelled in §3 and should be checked with Crossref.
- **Compute-confound evidence.** It is mostly non-peer-reviewed and mostly outside medicine. Cite it as emerging, and anchor P4 in MedAgentBoard [19] and MAST [21].

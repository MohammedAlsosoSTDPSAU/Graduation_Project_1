# Benchmarks, datasets, testbeds and evaluation metrics for LLM and agent-based clinical decision support (theme: benchmarks_metrics)

Scope: evidence for Chapter 2.2 (datasets, evaluation metrics, reported results) and for the open test-set decision. All sources were opened on 2026-11-27 unless marked "reused", which means verified in `/home/claude/research_notes/Agentic clinical decision support evidence/evaluation_datasets_safety.md` on 2026-11-23 and not re-fetched today.

Verification labels follow `_protocol.md`: FULL, PARTIAL, ABSTRACT, SECONDARY. Per-study records are in `benchmarks_metrics_studies.jsonl`, and every search is in `benchmarks_metrics_searchlog.jsonl`.

Access note: Semantic Scholar, Crossref and OpenAlex APIs could not be reached from this session. The proxy refused shell access, and WebFetch refused unlisted or long URLs. Metadata was therefore checked on publisher, proceedings, ACL Anthology, PMLR, NeurIPS proceedings and arXiv pages, recorded per study. PMC full texts (Bedi et al.; the published SourceCheckup paper) returned a captcha.

---

## Which benchmarks and testbeds are most relevant, and what are their size, source, access/licence and limitations?

### Takeaway
The candidate test sets fall into five families:
1. Exam MCQ: MedQA, MedMCQA, PubMedQA.
2. Interactive or sequential simulation: CRAFT-MD, AgentClinic, MediQ.
3. Real-EHR agent or decision environments: MIMIC-IV-Ext-CDM, MedAgentBench.
4. Rubric- or harm-annotated open-ended evaluation: HealthBench, NOHARM, and MedHELM as an aggregator.
5. Narrow skill benchmarks: MedCalc-Bench (calculation), MIRAGE (RAG), MedAbstain (abstention), real-world triage data.

No benchmark verified here has gold labels for all five project statuses (RECOMMEND, RECOMMEND_WITH_CAVEATS, NEED_MORE_INFO, ESCALATE_NOW, ABSTAIN). Some cover one or two:
- MediQ and MedAbstain operationalise abstain or ask.
- HealthBench has rubric themes for emergency referral, context seeking and uncertainty.
- Triage datasets (ESI levels) are an escalation analogue.
- NOHARM annotates harm per management option.

The test set therefore has to be constructed or relabelled. That conclusion is unchanged from the earlier Chapter 1 notes, and the new sources strengthen it.

### Cited Findings

**A. Exam-style MCQ (open, always answerable, contamination-prone)**
- **MedQA** (Jin et al., *Applied Sciences* 11(14):6421, 2021, DOI 10.3390/app11146421; reused, ABSTRACT).
  - Size: 12,723 English USMLE-style MCQs, plus 34,251 simplified-Chinese and 14,123 traditional-Chinese questions. The best baseline at the time scored 36.7% on English. — [DOAJ](https://doaj.org/article/11ebc944223d456caec3066e6152ebd1)
  - Licence: the GitHub README states none. Its only usage term is "If you would like to use the data or code, please cite the paper", and the data sits in a Google Drive folder. — [MedQA README](https://raw.githubusercontent.com/jind11/MedQA/master/README.md)
  - The BigBio Hugging Face card lists `license: unknown`. — [BigBio card](https://huggingface.co/datasets/bigbio/med_qa/raw/main/README.md)
- **MedMCQA** (Pal, Umapathi, Sankarasubbu, *Proceedings of CHIL*, PMLR 174:248–260, 2022; ABSTRACT).
  - Size: more than 194k MCQs from "AIIMS & NEET PG entrance exam" questions, covering 2.4k topics and 21 subjects, each with an explanation. — [PMLR](https://proceedings.mlr.press/v174/pal22a.html)
  - The Hugging Face card lists **Apache-2.0**, with splits of 183k train, 4.18k validation and 6.15k test. The visible card did not say whether test answers are public. — [HF card](https://huggingface.co/datasets/openlifescienceai/medmcqa)
- **PubMedQA** (Jin, Dhingra, Liu, Cohen, Lu, EMNLP-IJCNLP 2019, pp. 2567–2577, DOI 10.18653/v1/D19-1259; ABSTRACT).
  - Content: yes/no/maybe answers to research questions over PubMed abstracts. Size: 1k expert-annotated, 61.2k unlabeled and 211.3k artificially generated instances.
  - Results: the best model scored 68.1%, single-human performance was 78.0%, and the majority baseline was 55.2%. — [ACL Anthology](https://aclanthology.org/D19-1259/)
  - The BigBio card lists **MIT**. — [BigBio card](https://huggingface.co/datasets/bigbio/pubmed_qa/blob/main/README.md)
  - Note: "maybe" is a label for an inconclusive answer, the nearest thing in a classic dataset to an "uncertain" outcome. It reflects uncertainty in the evidence, not the model abstaining.

**B. Interactive / sequential simulation (information gathering, P1/P2)**
- **CRAFT-MD** (Johri et al., *Nature Medicine* 2025, DOI 10.1038/s41591-024-03328-5; reused, PARTIAL).
  - Data: 2,000 vignettes (1,800 MedQA-USMLE across 12 specialties, 100 Derm-Public, 100 Derm-Private), run 5 times each to give 10,000 conversations.
  - Results: GPT-4 4-choice MCQ accuracy fell from 0.820 (vignette) to 0.627 (multi-turn), and free-response from 0.486 to 0.264. The grader-AI agreed with experts 93.35% of the time.
  - Release and licence of the vignettes: not verified. — [PDF mirror](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- **AgentClinic** (Schmidgall et al., arXiv 2405.07960, v5 2025-05-25; no journal reference on the arXiv page; ABSTRACT).
  - Content: 9 specialties, 7 languages, multimodal data, tool use and bias perturbations. Sequential MedQA accuracy falls to "less than a tenth" of the original in some cases.
  - The abstract gives no case counts for AgentClinic-MedQA, MIMIC-IV or NEJM, and the distribution and licence of the MIMIC-derived cases were not verified. — [arXiv](https://arxiv.org/abs/2405.07960)
- **MediQ** (Li, Balachandran, Feng, Ilgen, Pierson, Koh, Tsvetkov, arXiv 2406.00922 v3, 2024-11-07; ABSTRACT). A neurips.cc 2024 poster page appeared in search results but was not opened, so the venue is unconfirmed.
  - Setup: a Patient System that may start with incomplete information, and an Expert System that "withholds diagnoses when unsure" and asks follow-up questions.
  - Results: directly prompting LLMs to ask questions *reduced* performance, while abstention strategies improved diagnostic accuracy by **22.3%**. Performance stayed below the full-information upper bound.
  - It provides a pipeline for converting single-turn benchmarks into interactive ones. — [arXiv](https://arxiv.org/abs/2406.00922)
  - This is the closest verified benchmark to the NEED_MORE_INFO behaviour.

**C. Real-EHR decision or agent environments**
- **MIMIC-IV-Ext-CDM** (Hager et al., *Nature Medicine* 30:2613–2622, 2024, DOI 10.1038/s41591-024-03097-1; reused, FULL via Springer page).
  - Data: 2,400 real MIMIC-IV ED patients with acute abdominal pain (appendicitis 957, cholecystitis 648, pancreatitis 538, diverticulitis 257).
  - Results: models were 16–25 points below physicians. Changing the order of information shifted accuracy by up to 18.0 points, and tools were hallucinated every 2–5 patients. — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)
  - Access: PhysioNet **credentialed** access, PhysioNet Credentialed Health Data License 1.5.0, DUA 1.5.0, and CITI "Data or Specimens Only Research" training (reused). — [PhysioNet](https://www.physionet.org/content/mimic-iv-ext-cdm/)
- **MedAgentBench** (Jiang, Black, Geng, Park, Zou, Ng, Chen, arXiv 2501.14654 v2, 2025-02-12; PARTIAL, arXiv HTML).
  - Environment: 300 clinician-written tasks in 10 categories, run against 100 real patient profiles from Stanford's STARR warehouse (785,207 records). The records are de-identified and "jittered", with names and IDs generated by Faker. The environment is a HAPI FHIR server in Docker exposing 9 FHIR functions.
  - Metric: pass@1 task success. Query tasks are checked by response comparison; action tasks by rule-based checks on POST payloads. An invalid action or more than 8 rounds counts as a failure.
  - Results: Claude 3.5 Sonnet v2 was best at **69.67%** (query 85.33%, action 54.00%), GPT-4o scored 64.00%, and Mistral v0.3 7B 4.00%.
  - Inconsistency: the text says 11 models were evaluated, but Table 3 lists 12. — [arXiv HTML](https://arxiv.org/html/2501.14654v2)
  - Publication: Stanford HAI states it was "published in the New England Journal of Medicine AI" (SECONDARY; DOI not verified). — [Stanford HAI](https://hai.stanford.edu/news/stanford-develops-real-world-benchmarks-for-healthcare-ai-agents)
  - Licences for the code and data were not stated on the page read. The authors list single-institution data, no reliability test on repeated actions, and no security or production logging as limitations.

**D. Rubric- or harm-annotated open-ended evaluation**
- **HealthBench** (Arora et al., OpenAI, arXiv 2505.08775; reused, PARTIAL).
  - Content: 5,000 multi-turn conversations, mostly synthetically generated, scored with 48,562 unique physician-written rubric criteria.
  - Themes, with the share of examples: "Responding under uncertainty" 21.4%, "Context seeking" 11.9%, "Emergency referrals" 9.6%. — [arXiv HTML](https://arxiv.org/html/2505.08775v1)
  - Results: o3 scored 60%, and the top HealthBench Hard score was 32%. — [arXiv](https://arxiv.org/abs/2505.08775)
  - Licence (new): the reference implementation lives in `openai/simple-evals` under the **MIT** licence. Since July 2025 the repo "will no longer be updated for new models or benchmark results" but keeps hosting the HealthBench reference implementation. — [GitHub](https://github.com/openai/simple-evals)
- **NOHARM** (Wu et al., 57 authors, arXiv 2512.01241, v4 2026-07-13; preprint; ABSTRACT).
  - Content: 1,100 primary-care-to-specialist consultation cases across 10 specialties, covering 4,249 clinical management options with 12,747 expert annotations.
  - Models: 20 LLMs and 4 RAG clinical AI tools. Following their recommendations directly carried a potential for **severe harm in up to 24.6% of cases**, and **errors of omission were more than 80% of severe errors**.
  - Clinical AI tools beat generalist LLMs, and "multi-agent AI teaming further improved generalist models".
  - In an RCT with 101 physicians, AI assistance helped, but physicians omitted valuable AI recommendations.
  - The benchmark and leaderboard are public; the licence was not verified. — [arXiv](https://arxiv.org/abs/2512.01241)
- **MedHELM** (Bedi, Cui, Fuentes et al., *Nature Medicine* 2026, DOI 10.1038/s41591-025-04151-2, PMID 41559415; ABSTRACT via Stanford Health Care record).
  - Coverage: 5 categories, 22 subcategories, 121 tasks, **37** evaluations and 9 frontier LLMs. The reasoning models reached a 66% win rate.
  - Cost: Claude 3.5 Sonnet was "comparable at **15%** lower computational cost". — [Stanford Health Care publication record](https://stanfordhealthcare.org/publications/969/969084.html)
  - **Conflict:** the arXiv v2 reports 35 benchmarks and "~40% lower estimated cost" (reused). Cite the journal version's figures if citing the journal. — [arXiv](https://arxiv.org/abs/2505.23802)
  - Clinical decision support scored 0.56–0.72 normalized accuracy in arXiv v2 (reused). HAI reports 0.63–0.77 for the journal version (SECONDARY).

**E. Narrow skill benchmarks relevant to components**
- **MedCalc-Bench** (Khandekar, Jin, Xiong, … Lu, NeurIPS 2024 Datasets & Benchmarks Track; PARTIAL).
  - Data: 1,047 manually reviewed evaluation instances across 55 calculators (19 rule-based, 36 equation-based). Patient notes come from Open-Patients (PMC-Patients case reports, MedQA vignettes and TREC synthetic cases), plus clinician- or template-synthesized notes.
  - Scoring: exact match for rule-based and date-based calculators, and a ±5% tolerance for lab, physical and dosage values.
  - Results: GPT-4 one-shot CoT was best at **50.91%**, versus 20.82% zero-shot direct and 37.92% zero-shot CoT.
  - Errors: wrong entity extraction, wrong equation, arithmetic. — [NeurIPS proceedings](https://proceedings.neurips.cc/paper_files/paper/2024/hash/99e81750f3fdfcaf9613db2dbf4bd623-Abstract.html); [arXiv HTML v4](https://arxiv.org/html/2406.12036v4)
  - Licence: the arXiv paper is CC BY-SA 4.0. The dataset's own licence (Appendix B.1) was not read.
- **MIRAGE / MedRAG** (Xiong, Jin, Lu, Zhang, Findings of ACL 2024, pp. 6233–6251, DOI 10.18653/v1/2024.findings-acl.372; ABSTRACT).
  - Content: 7,663 questions from five medical QA datasets, with 41 combinations of corpus, retriever and LLM, using more than 1.8 trillion prompt tokens.
  - Results: MedRAG improved accuracy by up to 18% over CoT. Log-linear scaling and "lost-in-the-middle" effects were observed. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.372)
- **MedAbstain** (Machcha, Yerra, Gupta, Sahoo, Sultana, Yu, Yao, arXiv 2601.12471 v2, 2026-01-22; preprint; ABSTRACT).
  - Content: an abstention benchmark for medical MCQ that combines **conformal prediction, adversarial question perturbations and explicit abstention options**. The authors frame discrete choice as a proxy for "agentic action selection".
  - Results: high-accuracy models often fail to abstain. Explicit abstention options increase safe abstention much more than perturbations do. Scale and prompting help little.
  - The abstract gives no numbers or dataset names. An ACL Anthology entry "2026.eacl-long.291" appeared in the same search but was not opened, so a link to this paper is unconfirmed. — [arXiv](https://arxiv.org/abs/2601.12471v2)
- **Real-world ED triage, 16 LLMs** (Benning et al., preprint, DOI 10.64898/2026.05.28.26353935; ABSTRACT via BIFOLD record).
  - Data: 16,107 consecutive adult walk-in encounters at a German tertiary ED (October 2023 to February 2024), with nurse-assigned ESI as the reference.
  - Metrics: primary outcome quadratic-weighted κ; secondary outcomes over- and under-triage patterns, ECE and reproducibility.
  - Results: κ_qw ranged from 0.18 to 0.75, and only a structured stepwise prompt reached 0.747. ECE ranged from 0.099 to 0.355, indicating overconfidence. ED-vs-urgent-care agreement was κ ≤ 0.30, and outputs varied on rerun in 23% of cases. The data is not stated to be public. — [BIFOLD record](https://www.bifold.berlin/impact-transfer/publications/view/publication-detail/performance-evaluation-and-benchmarking-across-16-large-language-models-on-a-comprehensive-real-world-emergency-department-triage-data-set)

### Inferences
- **Fit to the five statuses.**
  - ESCALATE_NOW ≈ triage under-triage, HealthBench "emergency referrals", and NOHARM's severe harm.
  - NEED_MORE_INFO ≈ MediQ question-asking and HealthBench "context seeking".
  - ABSTAIN ≈ MedAbstain's abstention option and selective prediction.
  - RECOMMEND / RECOMMEND_WITH_CAVEATS ≈ NOHARM option appropriateness and MIMIC-CDM diagnosis and treatment.
  - No single set covers all of these, so a constructed or relabelled set remains necessary.
- **Data-source trade-off.**
  - Real EHR (MIMIC-IV-Ext-CDM, MedAgentBench/STARR, ED triage) gives realism but brings access burdens or no public release.
  - Exam-derived sets (MedQA, CRAFT-MD, AgentClinic-MedQA, MediQ) are open but derived from USMLE-style items. That makes them vulnerable to training-data contamination; this is a general concern, and no verified source here measured it for these sets.
  - Synthetic data (HealthBench) has no PHI but is acknowledged by its authors as a limitation.
- **Licences.**
  - MedMCQA (Apache-2.0), PubMedQA (MIT) and the HealthBench code (MIT) are the clearest.
  - MedQA has no stated licence. Deriving and redistributing relabelled variants of MedQA items should be cleared with the supervisor; this is an inference, not a legal finding.
- **Component-level benchmarks** (MedCalc-Bench, MIRAGE) are not CDS outcome benchmarks. They can still test specific components: required-field extraction, and RAG retrieval and grounding.

### Gaps
- Not verified: the dataset licences of MedCalc-Bench, NOHARM, MedAgentBench (code and data), CRAFT-MD vignettes and AgentClinic MIMIC cases; the MedQA US split sizes; the MedAgentBench NEJM AI DOI and volume.
- No contamination study for MedQA or MedMCQA was opened in this pass.
- No guideline-QA benchmark (for example, guideline-adherence QA sets) was verified. The searches went to RAG (MIRAGE) instead.
- MIETIC and other PhysioNet triage corpora were not checked.
- MediQ's venue (NeurIPS 2024?) and MedAbstain's possible EACL 2026 version are unconfirmed.

---

## How do systematic reviews characterise evaluation practice, and which metric families are recommended?

### Takeaway
The reviews and holistic frameworks agree that evaluation is dominated by exam-QA accuracy. The 519-study JAMA review found 95.4% used accuracy as the primary dimension, only 5% used real patient data, and only 1.2% measured calibration or uncertainty. The recommended families add:
- safety and harm, plus trust and confidence (QUEST);
- context awareness, completeness and safety-relevant behaviour via rubrics (HealthBench);
- cost alongside performance (MedHELM, Kapoor et al.);
- interactive history-taking and open-ended responses (CRAFT-MD's 10 recommendations).

### Cited Findings
- **Bedi et al., JAMA 2025** (DOI 10.1001/jama.2024.21700; reused, ABSTRACT via NSF PAR). Across 519 studies from January 2022 to February 2024:
  - 5% used real patient-care data.
  - Medical-knowledge tasks were 44.5% and diagnosis 19.5%.
  - QA was 84.2% of NLP tasks, while conversational dialogue was 3.3%.
  - Accuracy was the primary dimension in 95.4%.
  - Fairness, bias and toxicity were measured in 15.8%, deployment considerations in 4.6%, and calibration and uncertainty in 1.2%.
  — [NSF PAR](https://par.nsf.gov/biblio/10616638)
  - The PMC full text (PMC11480901) and a library record were blocked by bot protection today.
- **QUEST** (Tam et al., *npj Digital Medicine* 7, 2024, DOI 10.1038/s41746-024-01258-7; reused, ABSTRACT). From 142 studies (January 2018 to February 2024), five human-evaluation principles: Quality of information; Understanding and reasoning; Expression style and persona; **Safety and harm**; **Trust and confidence**. Plus three phases: planning, implementation and adjudication, scoring and review. — [DOAJ](https://doaj.org/article/35d95c2463094fad97d777e1d8376e44)
- **MedHELM** (journal 2026).
  - Taxonomy validated by clinicians; cost reported next to performance (15% lower computational cost at comparable performance for Claude 3.5 Sonnet). — [Stanford Health Care record](https://stanfordhealthcare.org/publications/969/969084.html)
  - arXiv v2 (reused): LLM-jury agreement with clinicians was ICC 0.47, against clinician–clinician ICC 0.43, ROUGE-L 0.36 and BERTScore-F1 0.44. — [arXiv](https://arxiv.org/abs/2505.23802)
- **HealthBench** (reused). Five rubric axes, with their share of criteria: completeness 39%, accuracy 33%, context awareness 16%, communication 8%, instruction following 4%. Grading is by model, and negative criteria subtract points. — [arXiv HTML](https://arxiv.org/html/2505.08775v1)
- **CRAFT-MD** (reused). Ten recommendations, including open-ended rather than MCQ questions, assessing history-taking, and combining automated grading with expert review. — [PDF mirror](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- **NOHARM**. The authors conclude that strong medical-knowledge benchmark scores "do not guarantee safety" and that clinical safety must be measured explicitly. — [arXiv](https://arxiv.org/abs/2512.01241)
- **Kapoor et al., "AI Agents That Matter"** (arXiv 2407.01502; reused, ABSTRACT). Agent evaluation focuses narrowly on accuracy; the authors argue for jointly optimizing accuracy and cost, and say holdout sets are often inadequate. — [arXiv](https://arxiv.org/abs/2407.01502v1)

### Inferences
- A defensible metric suite for Chapter 2.2 has five layers:
  1. Decision correctness: status accuracy and option correctness.
  2. Safety: severe or unsafe recommendation rate, and omission vs commission errors (following NOHARM).
  3. Uncertainty: abstention and coverage–risk, and calibration (following Bedi's 1.2% gap, Savage, and Benning's ECE).
  4. Grounding: citation support (following ALCE and SourceCheckup).
  5. Robustness and efficiency: perturbation consistency, cost and latency (following Hager, MedFuzz, MedHELM and Kapoor).
  Each layer maps to a recognised family rather than an invented one.
- LLM-as-judge agreement in these sources is moderate:
  - MedHELM ICC 0.47.
  - CRAFT-MD 93.35% on diagnosis matching only.
  - SourceCheckup 88.0% vs doctor consensus, with inter-doctor agreement 85.2%.
  This supports deterministic scoring where labels are closed (status, option) and expert spot checks where they are not.

### Gaps
- The full texts of Bedi et al. and QUEST were not read. Bedi's specific recommendations (for example, standardized task definitions) are therefore not quoted.
- The MedHELM journal full text was not read, so its category scores and metric definitions are unverified at journal level.

---

## Which papers define metrics close to ours (unsafe/harm rate, under-/over-triage, abstention coverage–risk, citation support, robustness/consistency, cost/latency)?

### Takeaway
Close published precedents now exist for most of the project's metrics, so they can be presented as adaptations rather than inventions. Each source below is summarised under its metric family in the Cited Findings.

The exception is a joint, status-level safety metric over five statuses, which has no precedent in the verified literature. That combination remains the project's own operational definition.

### Cited Findings

**Unsafe recommendation / harm**
- NOHARM:
  - severe-harm rate per case: up to 24.6%;
  - share of severe errors that are omissions: more than 80%;
  - harm annotated per management option, with 12,747 expert annotations over 4,249 options. — [arXiv](https://arxiv.org/abs/2512.01241)
  - The abstract does not define the severity scale or the commission metrics.
- HealthBench uses negative rubric criteria that subtract points for harmful or incorrect content (reused). — [arXiv HTML](https://arxiv.org/html/2505.08775v1)
- MedAgentBench counts an invalid action as a task failure, using rule-based payload checks. — [arXiv HTML](https://arxiv.org/html/2501.14654v2)

**Under- / over-triage**
- Benning et al. use:
  - quadratic-weighted κ against nurse ESI (0.18–0.75);
  - over- and under-triage patterns and ECE (0.099–0.355) as secondary outcomes;
  - sectoral (ED vs urgent care) κ ≤ 0.30. — [BIFOLD record](https://www.bifold.berlin/impact-transfer/publications/view/publication-detail/performance-evaluation-and-benchmarking-across-16-large-language-models-on-a-comprehensive-real-world-emergency-department-triage-data-set)
  - The abstract does not give exact under-triage rates.

**Abstention / selective prediction**
- Geifman & El-Yaniv (NIPS 2017; PARTIAL, PDF) give the formal definitions:
  - coverage φ(f,g) = E_P[g(x)];
  - selective risk R(f,g) = E_P[ℓ(f(x),y)·g(x)] / φ(f,g);
  - the risk–coverage curve, which plots risk as a function of coverage.
  - Example: 2% top-5 ImageNet error guaranteed with 99.9% probability at almost 60% coverage. — [NeurIPS PDF](https://papers.NeurIPS.cc/paper_files/paper/2017/file/4a8423d5e91fda00bb7e46540e2b0cf1-Paper.pdf); [arXiv](https://arxiv.org/abs/1705.08500)
- MedAbstain uses conformal prediction and explicit abstention options. — [arXiv](https://arxiv.org/abs/2601.12471v2)
- MediQ reports a 22.3% diagnostic-accuracy gain from abstention-based question-asking. — [arXiv](https://arxiv.org/abs/2406.00922)
- Savage et al. (JAMIA 32(1):139–149, DOI 10.1093/jamia/ocae254; reused, ABSTRACT):
  - uncertainty proxies were scored by discrimination (ROC AUC) and calibration;
  - sample consistency with embeddings reached AUC 0.68–0.79 but was poorly calibrated;
  - sample consistency with GPT annotation reached AUC 0.66–0.74 and was well calibrated;
  - verbalized confidence consistently overestimated. — [Crossref](https://api.crossref.org/works/10.1093%2FJAMIA%2FOCAE254)
- The Wen et al. abstention survey (TACL 13:529–556, 2025; reused, ABSTRACT) reviews abstention metrics. Its metric section was not read. — [ACL Anthology](https://aclanthology.org/2025.tacl-1.26)

**Citation support**
- ALCE (Gao, Yen, Yu, Chen, EMNLP 2023, pp. 6465–6488, DOI 10.18653/v1/2023.emnlp-main.398; PARTIAL via ar5iv):
  - **Citation recall:** a statement scores 1 if it has at least one citation and the NLI model (TRUE, T5-11B) judges the concatenated cited passages to entail it.
  - **Citation precision:** a citation is "irrelevant" if it alone does not entail the statement and the remaining citations still do. A citation scores 1 if its statement has recall 1 and the citation is not irrelevant.
  - Human–automatic agreement: Cohen's κ 0.698 for recall and 0.525 for precision.
  - On ELI5 the best models lacked complete citation support 50% of the time. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2305.14627); [ACL Anthology](https://aclanthology.org/2023.emnlp-main.398)
- SourceCheckup (Wu, Wu, … Ho, Zou, arXiv 2402.02008 v1; PARTIAL):
  - **Metrics:** URL validity (HTTP 200 and non-empty text); statement-level support (share of statements supported by at least one source in the same response); response-level support (share of responses in which every statement is supported).
  - **Data:** 1,200 questions (400 each from MayoClinic, UpToDate and Reddit r/AskDocs).
  - **Results:** GPT-4 with RAG (Bing) had 99.2% URL validity, 69.4% statement-level and **54.3% response-level support**. GPT-4 API scored 22.7% response-level support and Gemini Pro 7.6%.
  - **Judge agreement:** the automated judge agreed with doctor consensus 88.0% of the time (N=284), against 85.2% inter-doctor agreement. — [arXiv HTML](https://arxiv.org/html/2402.02008v1)
  - The published version exists as PMC12003634 under a new title ("An Automated Framework for Assessing How Well LLMs Cite Relevant Medical References"), but its journal could not be confirmed because PMC returned a captcha. — [Stanford Law listing](https://law.stanford.edu/publications/an-automated-framework-for-assessing-how-well-llms-cite-relevant-medical-references/)

**Robustness / consistency**
- Hager et al.: changing information order shifted accuracy by up to 18.0 points (reused). — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)
- MedFuzz (Ness et al., arXiv 2406.06573 v2; preprint; ABSTRACT): adversarial edits to MedQA items targeting assumptions about patient characteristics, with a **permutation test** for whether an attack is significant. No numbers in the abstract. — [arXiv](https://arxiv.org/abs/2406.06573v2)
- AgentClinic uses bias perturbations. — [arXiv](https://arxiv.org/abs/2405.07960)
- Benning et al.: outputs were not reproducible in 23% of reruns. — [BIFOLD](https://www.bifold.berlin/impact-transfer/publications/view/publication-detail/performance-evaluation-and-benchmarking-across-16-large-language-models-on-a-comprehensive-real-world-emergency-department-triage-data-set)

**Cost / latency**
- MedHELM journal: 15% lower computational cost at comparable performance; arXiv v2 says ~40%. — [record](https://stanfordhealthcare.org/publications/969/969084.html)
- HealthBench: GPT-4.1 nano beat GPT-4o at about 25× lower cost (reused). — [arXiv](https://arxiv.org/abs/2505.08775)
- MIRAGE reports a compute budget of more than 1.8 trillion prompt tokens. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.372)
- Kapoor et al. argue for joint accuracy–cost reporting. — [arXiv](https://arxiv.org/abs/2407.01502v1)
- No verified CDS benchmark reports latency as a standard metric.

### Inferences
- **Correction to the earlier Chapter 1 notes.** Those notes said no verified source defines an unsafe-recommendation metric. NOHARM now provides a severe-harm-rate precedent (preprint), and ALCE and SourceCheckup provide citation recall, precision and support metrics. The project's "unsafe recommendation rate" can cite NOHARM as the closest analogue. Its "citation support rate" can follow SourceCheckup's statement- and response-level definitions, and ALCE's precision definition gives a way to penalise padded citations.
- **Abstention quality.** Report the risk–coverage curve (selective risk over answered cases, against coverage) following Geifman & El-Yaniv. Add unnecessary-abstention and unnecessary-escalation rates, which are the RQ1 counterparts of over-triage.
- **Robustness.** Measure status and ranking agreement between the original and the reordered or reworded case. MedFuzz's permutation test is a ready significance procedure for perturbation effects.

### Gaps
- No verified source gives a standard formula for "under-triage rate" in LLM triage. Benning's abstract names the concept without definitions.
- No medical paper verified here reports AURC or abstention-F1 for CDS.
- The NOHARM severity scale and the definition of commission errors need the full text.

---

## What are the access conditions for MIMIC/PhysioNet data with LLMs, and for other candidate datasets?

### Takeaway
MIMIC-derived data (MIMIC-IV-Ext-CDM, possibly AgentClinic-MIMIC-IV) requires, for each person:
- PhysioNet credentialing, with a supervisor as reference for students;
- CITI "Data or Specimens Only Research" training;
- a signed DUA for each dataset.

Under PhysioNet's 24 September 2025 guidance, local LLMs are "Strongly Recommended". Cloud APIs are acceptable only with verified zero data retention, no training use and no human review, and ChatGPT-style use is excluded.

The other candidates are far easier to access:
- open sets: MedMCQA (Apache-2.0), PubMedQA (MIT), the HealthBench code (MIT);
- open with no stated licence: MedQA;
- licence unverified: MedCalc-Bench, NOHARM, MedAgentBench;
- not stated to be public: the real-world triage cohorts.

### Cited Findings
- **MIMIC-IV-Ext-CDM page** (reused): credentialed access, PhysioNet Credentialed Health Data License 1.5.0, DUA 1.5.0, CITI "Data or Specimens Only Research". — [PhysioNet](https://www.physionet.org/content/mimic-iv-ext-cdm/)
- **PhysioNet, 24 September 2025** (reused):
  - sharing access with third parties is prohibited under the DUA;
  - locally deployed LLMs are "Strongly Recommended";
  - cloud services are allowed only with zero data retention, no training use and no human review, and these must be verified;
  - "If a service's data handling practices are unclear or cannot be fully verified, do not use the service." — [PhysioNet 2025](https://physionet.org/news/post/llm-responsible-use/)
- **PhysioNet, 18 April 2023** (reused): it prohibits sending data through APIs "provided by companies like OpenAI" and using ChatGPT, and lists conditional options (Azure OpenAI with human-review opt-out, Bedrock, Vertex, Claude). — [PhysioNet 2023](https://physionet.org/news/post/gpt-responsible-use)
- **MIMIC access steps** (reused): CITI completion report, a supervisor as reference for students, processing "within 1-2 weeks", and a DUA for each dataset. — [MIMIC FAQ](https://mimic.mit.edu/docs/faq/how-to-get-access.html)
- **Hager et al.**: closed models such as GPT-4 could not be tested under the MIMIC data agreements at the time (reused, paraphrase). — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)
- **MedAgentBench**: STARR-derived, de-identified, jittered profiles shipped in a Docker image. The access terms for the patient data were not stated on the page read. — [arXiv HTML](https://arxiv.org/html/2501.14654v2)
- **Licences of other candidates**: MedMCQA Apache-2.0 ([HF](https://huggingface.co/datasets/openlifescienceai/medmcqa)); PubMedQA MIT ([BigBio](https://huggingface.co/datasets/bigbio/pubmed_qa/blob/main/README.md)); MedQA none stated ([README](https://raw.githubusercontent.com/jind11/MedQA/master/README.md)); simple-evals/HealthBench MIT ([GitHub](https://github.com/openai/simple-evals)).

### Inferences
- **Feasibility ranking for the test-set decision** (researcher's inference):
  1. Primary: a constructed, guideline-grounded vignette set. Seed it from open, clearly licensed sources: MedMCQA and PubMedQA items, or MedQA items with supervisor clearance. Convert it to interactive form with MediQ's pipeline idea, and plant cases for missing information, red flags and out-of-scope inputs.
  2. A secondary external check on HealthBench subsets (emergency-referral, context-seeking and uncertainty themes) and/or NOHARM, if its licence allows.
  3. An optional real-data arm on MIMIC-IV-Ext-CDM with local open-weight models only.
  4. MedAgentBench is relevant only if the framework adds FHIR tool actions, which is outside the current scope (no ordering or prescribing).
- **MIMIC with commercial APIs.** Using the API of the project's own LLM vendor with MIMIC requires written verification of zero retention. Under the "if unclear, do not use" rule, local models are the low-risk default for a student team.

### Gaps
- The full text of the PhysioNet DUA 1.5.0 was not read.
- It was not verified whether AgentClinic-MIMIC-IV cases need credentialing.
- Licences for NOHARM, MedCalc-Bench and MedAgentBench data are not verified.
- The PhysioNet pages were not re-fetched today; they were verified on 2026-11-23.

---

## Synthesis

### (a) What the evidence establishes
- Exam-QA accuracy dominates evaluation: 95.4% accuracy-primary, 5% real data and 1.2% uncertainty across 519 studies.
- Performance drops under interactive, real-EHR or safety-focused evaluation:
  - CRAFT-MD: GPT-4 0.820 → 0.627 (MCQ), 0.486 → 0.264 (free response).
  - Hager: 16–25 points below physicians.
  - MedAgentBench: best model 69.67% overall, 54.00% on action tasks.
  - NOHARM: severe-harm potential in up to 24.6% of cases.
  - Real-ED triage: κ_qw from 0.18 to 0.75, and ECE up to 0.355.
- Recognised metric families exist for:
  - harm (NOHARM);
  - selective prediction (Geifman & El-Yaniv, MedAbstain);
  - citation support (ALCE, SourceCheckup);
  - robustness (Hager, MedFuzz);
  - cost (MedHELM, HealthBench, Kapoor).
- Abstention and question-asking help when designed deliberately (MediQ +22.3%), whereas naive prompting to ask questions hurts.

### (b) Contradictions and cautions
- MedHELM preprint vs journal: 35 vs 37 benchmarks; ~40% vs 15% lower cost; CDS scores 0.56–0.72 vs 0.63–0.77 (the latter SECONDARY).
- NOHARM reports that "multi-agent AI teaming further improved generalist models". This is a pro-multi-agent finding, but it is in a preprint abstract, and whether compute was matched is unknown. It should be weighed against Wang et al. (ACL 2024; theme reused), where a strong single agent matched multi-agent discussion.
- Clinical RAG tools beat generalist LLMs on harm (NOHARM). Yet even GPT-4 with RAG fully supported only 54.3% of its responses (SourceCheckup). Retrieval improves grounding but does not secure it, which supports P3.
- MedAgentBench text vs table: 11 vs 12 models.
- LLM judges are near clinician-level on narrow judgments (88.0% vs 85.2%; κ 0.698 for citation recall) but only moderate on holistic ones (ICC 0.47).

### (c) Gaps relevant to P1–P4 and RQ1–RQ4
- RQ1 / P2: no benchmark labels all five statuses. Under-triage, unnecessary-abstention and unnecessary-escalation metrics lack standard published formulas, so the project must define them, citing NOHARM, Benning and Geifman as analogues.
- RQ2 / P3: citation-support metrics exist (ALCE, SourceCheckup), but no CDS benchmark verified here pairs guideline-grounded claims with gold support labels. Verifier accuracy will need a hand-labelled claim–source subset.
- RQ3 / P1: no benchmark verified here reports status-level consistency under rewording. Perturbation work measures accuracy flips (MedFuzz, Hager).
- RQ4 / P4: no benchmark verified here reports compute-matched single-vs-multi-agent comparisons, and none reports latency. NOHARM's multi-agent gain is not compute-controlled, as far as the abstract shows.

### (d) Inaccessible or unverified items
- PMC full texts of Bedi et al. (PMC11480901) and the published SourceCheckup paper (PMC12003634) were blocked by captcha. The journal of the published SourceCheckup paper is unconfirmed.
- Not confirmed: MedAgentBench's NEJM AI DOI (SECONDARY via Stanford HAI); MediQ's NeurIPS 2024 acceptance (search listing only); MedAbstain's possible EACL 2026 version.
- Licences not verified: MedCalc-Bench data, NOHARM, MedAgentBench data, CRAFT-MD, AgentClinic MIMIC cases.
- The Semantic Scholar, Crossref and OpenAlex APIs were unreachable, so metadata was checked on publisher and proceedings pages instead.
- Benning et al. is a preprint; its journal and full text were not seen.

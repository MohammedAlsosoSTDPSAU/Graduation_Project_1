# Safety mechanisms and hybrid rule-plus-LLM designs for LLM-based clinical decision support (theme: safety_hybrid)

Scope: evidence for Chapter 2 (2.1 review, 2.2 comparative analysis, 2.3 gaps) on how existing work handles **P2** (missing information, uncertainty, abstention, escalation) and on **hybrid deterministic-plus-LLM CDS** designs comparable to the proposed red-flag rules, required-field checks, closed option set and deterministic decision core. All sources were opened on 2026-11-27 unless marked "reused" (verified in the earlier pass on 2026-11-23, files `evaluation_datasets_safety.md` and `cdss_foundations_and_guidance.md`). Verification levels follow the protocol: FULL, PARTIAL, ABSTRACT, SECONDARY. The fetch tool returns a model-made extraction of each page, so numbers should get a final check against the PDF before going into the thesis as exact figures. Study keys in brackets match `safety_hybrid_studies.jsonl`.

Access notes: the session could not reach the Crossref/OpenAlex/Semantic Scholar APIs from the shell (no network), and several nature.com, ACL Anthology and arXiv abstract pages were blocked by the fetch allowlist. Where that happened, metadata come from PubMed, PMC, IDEAS/RePEc, Crossref (via WebFetch where allowed), institutional repositories or author PDFs, as recorded per study.

---

## KQ1. Which abstention, selective-prediction and uncertainty methods have been evaluated for medical LLMs, with what metrics and results, and how is over-abstention measured?

### Takeaway
Medical-LLM abstention work falls into four families: (i) **uncertainty proxies** such as sample consistency, verbalized confidence and token probability; (ii) **benchmark-level abstention options and missing-information perturbations**, as in MetaMedQA and MedAbstain; (iii) **interactive "ask instead of answer"** designs such as MediQ; (iv) **learned deferral to clinicians**, as in CoDoC (imaging, not LLM). The results agree on one point. Models rarely abstain when they should: MedAbstain reports 13.16% recall of human-warranted abstentions, and in MetaMedQA models gave confident answers when the correct option was missing. Verbalized confidence is consistently overconfident. Sample consistency is the best tested proxy, but only moderately discriminative (ROC AUC 0.66–0.79). **Over-abstention is rarely measured directly.** The closest measures are abstention precision against human judgment (MedAbstain 71.43%), question counts and stagnation in MediQ, and specificity or over-triage in escalation studies (KQ3). No verified medical study reports risk–coverage curves or AURC for a CDS decision task.

### Cited Findings

**[savage2025] Savage et al., JAMIA 32(1):139–149 (online 2024-10-12; issue 2025-01-01), DOI 10.1093/jamia/ocae254.** Peer-reviewed journal; ABSTRACT; metadata re-checked this pass via Crossref.
- Compared three uncertainty proxies on GPT-3.5, GPT-4, Llama2 and Llama3, using 3 datasets of open-ended diagnosis and treatment scenarios: confidence elicitation (CE, i.e. verbalized), token-level probability (TLP) and sample consistency (SC). — [Crossref record](https://api.crossref.org/works/10.1093%2FJAMIA%2FOCAE254)
- SC with sentence embeddings had the highest discrimination (ROC AUC 0.68–0.79) but poor calibration. SC with GPT annotation reached ROC AUC 0.66–0.74 with accurate calibration. CE "consistently overestimated" confidence. SC was the most effective proxy tested. — [Crossref record](https://api.crossref.org/works/10.1093%2FJAMIA%2FOCAE254)
- Metrics: discrimination (ROC AUC) and calibration. The abstract does not report abstention or coverage metrics.

**[griot2025] Griot, Hemptinne, Vanderdonckt, Yuksel, "Large Language Models lack essential metacognition for reliable medical reasoning", Nature Communications 16 (2025), DOI 10.1038/s41467-024-55628-6.** Peer-reviewed journal; ABSTRACT (IDEAS/RePEc record; nature.com was not reachable).
- MetaMedQA adds confidence scores and metacognitive tasks to medical multiple-choice questions. Twelve models were evaluated on confidence-based accuracy, **missing-answer recall** and **unknown recall**. — [IDEAS/RePEc](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-024-55628-6.html)
- All models showed significant metacognitive deficits despite high MCQ accuracy. They often failed to recognise their knowledge limits and gave confident answers when the correct option was missing. — [IDEAS/RePEc](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-024-55628-6.html)
- Per-model numbers were not available in the record read (see Gaps).

**[machcha2026medabstain] Machcha, Yerra, Gupta, Sahoo, Sultana, Yu, Yao, "Knowing When to Abstain: Medical LLMs Under Clinical Uncertainty" (MedAbstain), arXiv 2601.12471v2 (Jan 2026).** Preprint; PARTIAL (first ~47% of the HTML read).
- The protocol combines **conformal prediction** (LAC and APS scores, α = 0.1, i.e. 90% target coverage, 30/70 calibration/test split), **question perturbations that remove key details**, and **explicit abstention options**. Four settings: no abstention option (NA), abstention option (A), perturbed without option (NAP), perturbed with option (AP). Data: MedQA (public) and AMBOSS (private). — [arXiv HTML](https://arxiv.org/html/2601.12471v2)
- Metrics: accuracy, conformal set size (as the uncertainty measure) and abstention rate. Human raters judged 77.55% of perturbed cases as warranting abstention. **Model abstention against human judgment, AP subset: precision 71.43%, recall 13.16%.** The authors read this as "rarely abstaining unnecessarily but often failing to abstain when warranted." — [arXiv HTML](https://arxiv.org/html/2601.12471v2)
- Few-shot and chain-of-thought gave negligible gains in abstention rate. Qwen "thinking" mode slightly improved accuracy and tightened LAC sets, but abstention rate **decreased** consistently. — [arXiv HTML](https://arxiv.org/html/2601.12471v2)
- Limitations stated by the authors: MCQ only, English only, and closed-model uncertainty depends on API logprobs. — [arXiv HTML](https://arxiv.org/html/2601.12471v2)

**[li2024mediq] Li, Balachandran, Feng, Ilgen, Pierson, Koh, Tsvetkov, "MediQ: Question-Asking LLMs and a Benchmark for Reliable Interactive Clinical Reasoning", NeurIPS 2024 (poster; arXiv 2406.00922v3).** Peer-reviewed conference; FULL (arXiv PDF); acceptance confirmed on the NeurIPS 2024 virtual poster page. That page does not state the track explicitly.
- A Patient system and an Expert system. The Expert "refrains from making diagnostic decisions when unconfident" and asks follow-up questions. Datasets: iMEDQA (test n = 1,273) and iCRAFT-MD (140 dermatology cases). — [arXiv PDF](https://arxiv.org/pdf/2406.00922v3); [NeurIPS 2024 poster](https://neurips.cc/virtual/2024/poster/94856)
- Accuracy collapses when information is withheld. GPT-4 on iMEDQA: Full 79.7% → Initial (age, sex, chief complaint only) 54.5%. BASIC interactive scored 55.4% and the best abstention configuration 66.1%. GPT-4 on iCRAFT-MD: Full 91.4%, Initial 67.9%, BASIC 73.6%, best 84.3%. Llama-3-70B iMEDQA: Full 84.7%, Initial 58.5%, best 60.9%. — [arXiv PDF](https://arxiv.org/pdf/2406.00922v3)
- Abstention modules: Numerical (0–1 threshold), Binary, Scale (5 levels), Rationale Generation, Self-Consistency. The best was Scale + RG + SC (SC = 3). Thresholds were set by grid search on the dev set. **Self-consistency alone degraded performance unless paired with rationale generation.** — [arXiv PDF](https://arxiv.org/pdf/2406.00922v3)
- The headline "22.3% improvement" is over BASIC (and 12.1% over the non-interactive Initial setting). The paper does not make clear whether these are relative or absolute. — [arXiv abstract page](https://arxiv.org/abs/2406.00922v3)
- Over-asking: question counts differed widely (GPT-3.5 averaged 0.47 questions, Llama-3-70B 6.38). Accuracy stagnated after a point because of irrelevant or repetitive questions. Removing repeats and reformatting the dialogue improved BASIC by 5.7 points. — [arXiv PDF](https://arxiv.org/pdf/2406.00922v3)

**[wen2025knowyourlimits] Wen et al., "Know Your Limits: A Survey of Abstention in Large Language Models", TACL 13:529–556 (2025), DOI 10.1162/tacl_a_00754.** Peer-reviewed journal (survey); reused, ABSTRACT. The ACL Anthology page was blocked this pass, so the metadata rest on the 2026-11-23 verification.
- Frames abstention from three perspectives: query, model and human values. Reviews methods, benchmarks and metrics. — [ACL Anthology](https://aclanthology.org/2025.tacl-1.26)

**[dvijotham2023codoc] Dvijotham, Winkens, Barsbey, et al. (DeepMind; Karthikesalingam among the senior contributors), "Enhancing the reliability and accuracy of AI-enabled diagnosis via complementarity-driven deferral to clinicians" (CoDoC), Nature Medicine 29(7):1814–1820 (2023), DOI 10.1038/s41591-023-02437-x.** Peer-reviewed journal; ABSTRACT (PubMed and the DOI landing page). Not an LLM study, but the main medical **learning-to-defer** precedent.
- CoDoC learns whether to rely on the AI model or defer to the clinical workflow. UK breast screening, against double reading with arbitration: **25% fewer false positives at the same false-negative rate and a 66% reduction in clinician workload.** TB triage: 5–15% fewer false positives at the same false-negative rate for 3 of 5 commercial AI systems. — [PubMed](https://www.pubmed.gov/37460754/); [DOI page](https://doi.org/10.1038/s41591-023-02437-x)
- Evaluation design worth copying: deferral quality is judged at a **fixed false-negative rate**, which makes the safety/efficiency trade-off explicit.

**Reused context.** Only 1.2% of 519 health-LLM evaluations measured calibration or uncertainty (Bedi et al., JAMA 2025). — [NSF PAR record](https://par.nsf.gov/biblio/10616638). HealthBench gives 21.4% of its examples to "responding under uncertainty" and 11.9% to "context seeking" (industry preprint). — [arXiv HTML](https://arxiv.org/html/2505.08775v1)

### Inferences
- The data favour **external, model-independent abstention triggers** over model self-report: verbalized confidence overestimates (savage2025), confident answers persist when the answer is missing (griot2025), and abstention recall is low even with an explicit option (machcha2026medabstain). The proposed NEED_MORE_INFO and ABSTAIN logic (required-field checks; a robustness test across missing-value ranges) fits that evidence. Whether it outperforms SC-based abstention is an empirical question for RQ1; nothing here shows it does.
- MediQ shows that a *naive* "ask more" policy can hurt, and that SC alone did not help. A NEED_MORE_INFO status should therefore be evaluated for *which* information it requests, not only for how often it fires.
- Over-abstention should be reported explicitly. Suitable measures are: unnecessary abstention/escalation rate on answerable cases, abstention precision and recall against reference labels (as in MedAbstain), and accuracy at matched coverage. A CoDoC-style "fixed unsafe-error rate" comparison is a defensible way to compare statuses. These are design suggestions drawn from the sources, not established standards.

### Gaps
- MetaMedQA per-model numbers (for example, unknown recall) were not read because the nature.com full text was blocked. Do not quote numbers until the full text is checked.
- MedAbstain's venue is not confirmed. A search surfaced an ACL Anthology entry "2026.eacl-long.291" alongside the arXiv record, but it was not opened and the arXiv page names no venue. Treat it as a preprint until confirmed.
- No verified medical-LLM study reports risk–coverage curves or AURC for a closed-option CDS task. The TACL survey's metrics section was not read this pass.
- CoDoC's dataset sizes and deferral algorithm were not read (abstract only).
- Kadavath et al. 2022 and general LLM conformal-prediction papers were not verified.

---

## KQ2. Which systems combine rules, guidelines, calculators or knowledge graphs with LLMs for clinical decisions, and how did they compare with pure-LLM baselines?

### Takeaway
A clear pattern recurs across independent groups: **let the LLM extract, and let deterministic code compute, traverse or check.** This pattern gives large gains over pure-LLM baselines on tasks with a defined rule or calculator:
- clinical calculators: AgentMD 87.7% vs GPT-4 40.9%; Goodell et al. cut GPT-based errors from 64% to 4.8%;
- decision rules: CDR-Agent exact-match selection 0.983 vs 0.420 on synthetic notes;
- guideline execution: MedDEx 79.9% vs 59.5% for the best prompting configuration on scenario routing;
- neuro-symbolic extraction checking: F1 1.00 vs 0.63 for GPT-4 alone on study inclusion.

Gains are **smaller or mixed on realistic or real-note benchmarks**. On CDR-Bench, sensitivity fell from 0.936 to 0.683 and F1 was 0.592 vs 0.608. On MedGUIDE MCQ the gap was under 0.5 points. Most evaluations use synthetic cases, a single guideline or condition, and author-checked rules. No study compared a hybrid against a tool-, retrieval- and compute-matched single-LLM baseline in the sense of RQ4.

### Cited Findings

**Clinical calculators as tools**
- **[jin2025agentmd] Jin et al., "AgentMD: Empowering language agents for risk prediction with large-scale clinical tool learning", Nature Communications 16 (2025), DOI 10.1038/s41467-025-64430-x.** Peer-reviewed; ABSTRACT (IDEAS/RePEc; PMC was blocked). AgentMD curated **2,164 executable clinical calculators** from PubMed (>85% accuracy on quality checks, >90% unit-test pass rate). It then selects and applies them. **Risk-prediction accuracy 87.7% vs 40.9% for GPT-4.** Also applied to 698 real ED notes. — [IDEAS/RePEc](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-64430-x.html)
- **[goodell2025] Goodell, Chu, Rouholiman, Chu, "Large language model agents can use tools to perform clinical calculations", npj Digital Medicine 8:163 (2025), DOI 10.1038/s41746-025-01475-8.** Peer-reviewed; ABSTRACT (eScholarship). 48 calculation tasks. Unaided ChatGPT was wrong in one-third of trials (n = 212). Over 10,000 trials, task-specific tools (OpenMedCalc) cut incorrect responses **from 88% to 16% (LLaMa-based) and from 64% to 4.8% (GPT-based)**. Results for the RAG and code-interpreter arms were not given in the abstract. — [eScholarship](https://escholarship.org/uc/item/8xv9r6tn)

**Clinical decision rules and guideline-as-code**
- **[xiang2025cdragent] Xiang, Hsu, Zane, Kornblith, Lin-Martore, Kaur, Dokiparthi, Li, Yu, "CDR-Agent: Intelligent Selection and Execution of Clinical Decision Rules Using LLM Agents", arXiv 2505.23055v1 (2025).** Preprint; FULL (HTML). Rules are selected by embedding similarity plus an anomaly test, with no LLM in that step. GPT-4o extracts variables, and missing variables default to "negative". **Deterministic Python** then executes 15 trauma CDRs, such as PECARN. — [arXiv HTML](https://arxiv.org/html/2505.23055v1)
  - Synthetic (400 PECARN notes): exact-match 0.983 vs 0.420 for a GPT-4o baseline given all CDR text; F1 0.994 vs 0.735; total time 1.22 s vs 4.12 s.
  - CDR-Bench (144 real notes from MIMIC-IV, MedQA and ACN): exact-match 0.513 vs 0.426; **F1 0.592 vs 0.608 (baseline higher); sensitivity 0.683 vs 0.936 (baseline higher); specificity 0.983 vs 0.652.** — [arXiv HTML](https://arxiv.org/html/2505.23055v1)
  - Caveats: imaging outcome labels inferred by GPT-4o; negative imputation of missing variables; text/table timing inconsistencies. — [arXiv HTML](https://arxiv.org/html/2505.23055v1)
- **[lu2026meddex] Lu, Guo, Cheng, Liu, "More Reasoning Is Not Always Better: Executing Clinical Guidelines with LLMs and Rules" (MedDEx), arXiv 2610.15912 (2026).** Preprint; PARTIAL–FULL (PDF extraction). Three stages: LLM extraction of condition values and evidence, then a **deterministic rule executor** that computes derived values (e.g. AJCC 8 TNM) and traverses a guideline decision graph, then an LLM answer **restricted to the options the executor returns**. Small local models (Qwen2.5-7B, Qwen3-8B, Llama-3.1-8B). — [arXiv PDF](https://arxiv.org/pdf/2610.15912)
  - MEGA-CDP conclusion agreement, Qwen3-8B: 96.29 vs 83.17 for single LLM (best LLM-executed: MA-RAG 83.71). Llama-3.1-8B: 92.24 vs 74.31.
  - MedGUIDE accuracy: 69.70 vs 68.77 (gap < 0.5 points).
  - Scenario routing: deterministic executor **79.9%** vs best prompting 59.5% (CoT 58.3%); metastatic cases 86.8% vs 52.1%.
  - Ablations: removing the stage calculator drops TNM accuracy from 26.90 to 8.08; giving all nine guidelines instead of the selected one cuts treatment agreement by 15.7 points; a structured state contributed more than rule staging (end-to-end F1 55.9 vs 48.9). — [arXiv PDF](https://arxiv.org/pdf/2610.15912)
  - Cost: 2.0 LLM calls and 1.1 s per case on MedGUIDE vs 20.7 calls and 152.3 s for MedAgents. — [arXiv PDF](https://arxiv.org/pdf/2610.15912)
  - **Contradicts the multi-agent premise:** MedAgents (56.09) and MDAgents (50.29) scored *below* a single LLM (66.65) on Llama MedGUIDE, and Guideline-RAG underperformed direct prompting on Qwen3 MedGUIDE (51.68 vs 68.77). — [arXiv PDF](https://arxiv.org/pdf/2610.15912)
  - Limitations: rule packs drafted with coding assistants and checked by the authors, not independently validated clinically; best TNM accuracy only 30.30%. — [arXiv PDF](https://arxiv.org/pdf/2610.15912)
- **[oniani2024] Oniani et al., "Enhancing Large Language Models for Clinical Decision Support by Incorporating Clinical Practice Guidelines", arXiv 2401.11120v2.** Cited elsewhere as IEEE ICHI 2024; the IEEE record was not opened. Preprint per verified record; FULL (HTML).
  - Method: three ways to inject the CDC/IDSA COVID-19 outpatient treatment guideline (8 leaf nodes): Binary Decision Tree (the LLM traverses yes/no nodes), Program-Aided Graph Construction, and CoT few-shot. Tested on 39 synthetic patients. — [arXiv HTML](https://arxiv.org/html/2401.11120v2)
  - F-score: GPT-4 BDT 1.001 (sic, unexplained >1), CoT-FSP 0.972, PAGC 0.834, zero-shot 0.47. PaLM 2 BDT 0.715 vs zero-shot 0.01. LLaMA-13b gained little (0.37 vs 0.31). — [arXiv HTML](https://arxiv.org/html/2401.11120v2)
  - The best automatic method (GPT-4 BDT) ranked lowest among top methods in a two-physician human evaluation. The direction of the 0–2 scale was not confirmed in the extraction. — [arXiv HTML](https://arxiv.org/html/2401.11120v2); ICHI 2024 venue as cited in [Xu et al. CEUR 2025](https://ceur-ws.org/Vol-4001/paper3.pdf)
- **[xu2025ceur] Xu, Dumontier, Sun, "Using Clinical Guidelines, Domain Ontology, and LLMs for Personalized Leukemia Treatment Recommendations", SeWebMeDa-2025, CEUR-WS Vol-4001.** Peer-reviewed workshop; PARTIAL. On 20 synthetic ALL cases (NCCN guideline), performance rose from baseline → RAG → markdown + CoT → **guideline graph linked to HPO/Orphanet ontologies**. GPT-4 reached 0.662–0.698 across four metrics with the graph approach. The authors call the results preliminary. — [CEUR-WS](https://ceur-ws.org/Vol-4001/paper3.pdf)
- **[sivasothy2026] Sivasothy et al., "Large language models and conditional rules in clinical decision support systems", Health Information Science and Systems 14:32 (2026), DOI 10.1007/s13755-026-00428-z.** Peer-reviewed; ABSTRACT (Crossref). LLMs and reasoning models asked to **generate** a COVID-19 triage rule set, compared against the PiMS clinical rule set. Without PiMS variables, LLMs produced screening rather than triage rules. With them, LLM accuracy was 31.62%–70.71% and reasoning models up to 81.70%. The authors conclude LLMs are "limited in reproducing clinical rule sets". — [Crossref](https://api.crossref.org/works/10.1007%2FS13755-026-00428-Z)

**Knowledge graphs and neuro-symbolic checking**
- **[gao2025drknows] Gao et al., "Leveraging Medical Knowledge Graphs Into LLMs for Diagnosis Prediction" (DR.KNOWS), JMIR AI 4:e58670 (2025), DOI 10.2196/58670.** Peer-reviewed; FULL (PMC). UMLS/SNOMED knowledge-graph paths are retrieved and added to prompts.
  - MIMIC-III ProbSum (1,005 notes), ChatGPT 5-shot: CUI-F 20.96 → 26.02 with paths, ROUGE-L 21.23 → 25.43. Zero-shot ChatGPT: CUI-F rose (16.04 → 18.21) but **ROUGE-L fell (19.77 → 15.49)**. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11894347/)
  - Human evaluation (92 notes): diagnosis omission 16% vs 10% (not significant, P = .16). The model sometimes ignored correct paths, and one retrieved path contradicted the note. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11894347/)
- **[prenosil2025] Prenosil, Weitzel, et al., "Neuro-symbolic AI for auditable cognitive information extraction from medical reports", Communications Medicine 5:491 (2025), DOI 10.1038/s43856-025-01194-x.** Peer-reviewed; FULL (PMC). GPT-4 extracts facts; a **Prolog rule-based expert system** checks them against medical rules and study criteria and produces traceable labels. 206 PSMA PET/CT reports, 5,356 data points; McNemar tests. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12638795)
  - F1 for study inclusion: GPT-4 alone 0.63 vs neuro-symbolic 1.00 vs physicians 1.00. Recurrence F1: 0.95 vs 1.00 vs 0.99. GPT-4 alone had sensitivity 0.45 for inclusion. The system also intercepted reports with residual identifiers. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12638795)
  - Caveats: single centre; a reference partly built from AI outputs (circularity risk); co-authors with commercial ties; table/abstract inconsistencies for PSA accuracy. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12638795)

**Guardrails (reused)**
- **[hakim2025] Hakim et al., "The need for guardrails with large language models in pharmacovigilance and other medical safety critical settings", Scientific Reports (2025), DOI 10.1038/s41598-025-09138-0.** Peer-reviewed; ABSTRACT (reused). Three guardrails around a fine-tuned LLM: document anomaly detection, checks of drug and adverse-event terms against controlled vocabularies, and uncertainty communication. The abstract gives no numbers. — [arXiv v2](https://arxiv.org/abs/2407.18322v2)

### Inferences
- The strongest hybrid results come where **the decision logic already exists as a computable artefact**: a calculator, a CDR or a guideline tree. That is the situation the proposed "pack-defined option set + deterministic decision core" creates. The literature therefore supports the *design pattern*. It does not yet support claims about *safety outcomes* (unsafe recommendation rate, escalation accuracy), because none of these studies reports them.
- The **extraction step is the weak link** in every hybrid. CDR-Agent lost sensitivity on real notes because missing variables defaulted to "negative". DR.KNOWS models ignored correct paths. Prenosil's gain came from rules catching extraction errors. This backs the proposed deterministic grounding check and required-field checks. Defaulting missing values to "normal/negative" is a documented failure mode, and the proposed NEED_MORE_INFO and robustness-across-missing-ranges approach is the opposite design choice.
- MedDEx is the closest published analogue to the proposed architecture: LLM extraction, a deterministic executor, and an LLM answer constrained to executor options. It also gives direct evidence that multi-agent baselines can underperform a single LLM at about 10× the calls. That supports RQ4's compute-matched baselines. It is a preprint with author-validated rules.
- Rule *generation* by LLMs is unreliable (sivasothy2026). This supports clinician- or guideline-authored packs over LLM-authored rules.

### Gaps
- **Decision-analytic ranking** (multi-criteria or weighted ranking of options combined with LLMs) in clinical CDS: one search found no verified clinical study. The hits were general MCDA-with-LLM papers and non-clinical operations work, and none was opened. Treat this as an evidence gap.
- **Constrained decoding to a closed option set**: MedDEx's ablation table includes constrained decoding, but those numbers were not extracted. No dedicated medical study was verified.
- AgentMD's and Goodell's full texts (error types, dataset details) were not read.
- Oniani et al.'s IEEE ICHI 2024 record (pages, DOI) was not verified.
- CDR-Agent's publication venue was not checked.
- No hybrid study reports abstention or escalation statuses or an unsafe-recommendation rate.

---

## KQ2b (depth pass). Prior systems that already pair a deterministic decision core, rules, solvers or guardrails with LLM agents, and what this means for novelty claims

### Takeaway
**Claiming novelty for "a deterministic decision core with LLM agents" is NOT supported.** Several prior systems already do this:
- **EndoGov** (2026 preprint): a knowledge-governed multi-agent system with deterministic hard-path rules the LLM "does not have authority over" and a post-hoc deterministic validator.
- **MedDEx** (KQ2): an LLM extractor, a deterministic guideline executor, and an LLM answer restricted to the executor's options.
- **CDR-Agent** (KQ2): deterministic CDR execution.
- **Polaris** (Hippocratic AI, 2024): deterministic orchestration with specialist safety agents and human-intervention escalation.
- **MedDM** (2023): LLM-executable guideline trees with an explicit "Unable to determine" → follow-up question branch, a direct precursor of NEED_MORE_INFO.
- **SMT-solver (Z3) verification** of LLM-formalised guideline rules (Xie & Du 2026).
- **Post-hoc symbolic verifiers** that override LLM labels (MedRule-KG; Prenosil et al.).

Most of these are preprints with self-built or rule-derived reference labels. What does **not** appear in any verified prior system is the specific *combination*: (i) multi-status output including NEED_MORE_INFO / ESCALATE_NOW / ABSTAIN, (ii) a robustness test of the ranking across missing-value ranges, (iii) claim-to-source verification, and (iv) evaluation against tool-, retrieval- and compute-matched single-LLM baselines on safety metrics. Novelty should be framed around (i)–(iv) and the evaluation, not the architecture pattern.

### Cited Findings

**[dai2026endogov] Dai, Shi, He, Ma, Lin, Sun, Nie, "EndoGov: A knowledge-governed multi-agent expert system for endometrial cancer risk stratification", arXiv 2604.23802 (2026).** Preprint; PARTIAL (about 85% of the PDF extraction). Date and venue not stated in the text read.
- **Tier 1 specialist agents** mix deterministic rules and LLMs: a deterministic molecular priority rule (POLEmut > MMRd > p53abn > NSMP), rule-based FIGO staging with LLM parsing, and pathology prototype matching with an LLM report. — [arXiv PDF](https://arxiv.org/pdf/2604.23802)
- **Tier 2 governance**: deterministic rule execution on the hard path (the LLM "does not have authority over hard-path outcomes"). On the soft path, deterministic ESMO mapping comes first and an LLM chair reasons only below a confidence threshold τ. A **post-hoc deterministic validator** accepts, rejects or corrects outputs. The rules are 12 executable rules in a Guideline-KG; the hard/soft split is reported inconsistently (5/7 vs 7/5). — [arXiv PDF](https://arxiv.org/pdf/2604.23802)
- TCGA-UCEC (N = 541): EndoGov accuracy 0.943, macro AUC 0.973, guideline-violation rate (C-LVR) 0.93%. Baselines:
  - GPT-4o direct: accuracy 0.460, C-LVR 17.3%;
  - **GPT-4o + post-hoc rules: accuracy 0.529, C-LVR 0.0%**;
  - **Structured Rules Only: accuracy 0.800**;
  - Rules + soft-path logistic regression: accuracy 0.884, macro AUC 0.968. — [arXiv PDF](https://arxiv.org/pdf/2604.23802)
- External CPTAC (N = 95): accuracy 0.842. Validator stress test: 26/26 adversarial cases intercepted. Three oncologists rated 20 cases (audit clarity 4.0/5). — [arXiv PDF](https://arxiv.org/pdf/2604.23802)
- Limitations stated by the authors: compliance depends on upstream trigger detection; CPTAC labels are guideline-derived (partial circularity); rules were checked by two researchers; there is no formal abstain status (referral appears only as simulated policy). — [arXiv PDF](https://arxiv.org/pdf/2604.23802)
- Prior systems it cites as having **no** deterministic rule layer: Mapis, OMGs, EvoMDT, Kg4Diagnosis, MedAgent-Pro, CLARITY. — [arXiv PDF](https://arxiv.org/pdf/2604.23802)

**[li2023meddm] Li, Shi, Meng, Zhai, Ruan, "MedDM: LLM-Executable Clinical Guidance Tree for Clinical Decision-Making", arXiv 2312.02441 (Dec 2023).** Preprint; FULL (HTML).
- Built **1,202 clinical guidance trees** (443 differential-diagnosis and 759 treatment trees; 12 departments, >500 diseases) from guideline flowcharts using computer vision and OCR. — [arXiv HTML](https://arxiv.org/html/2312.02441v1)
- Execution: tree traversal is deterministic, and the LLM answers each condition node with Yes, No or "**Unable to determine**". The last triggers a follow-up question to the patient. — [arXiv HTML](https://arxiv.org/html/2312.02441v1)
- **No clinical or diagnostic evaluation and no baselines were reported.** The only numbers are for flowchart shape detection (97% mAP, or 98% "accuracy", reported inconsistently). — [arXiv HTML](https://arxiv.org/html/2312.02441v1)

**[shen2026medguidex] Shen, Cao, Du, Wang, Zhou, Peng, Guo, "MedGuideX: Internalizing Decision Logic from Executable Guidelines into LLMs for Clinical Reasoning", arXiv 2605.26567 (May 2026).** Preprint; PARTIAL–FULL.
- Approach: 841 US guidelines → 3,796 recommendations → **2,793 executable Python functions**. These label synthetic QA and act as **verifiable RL rewards** (GRPO). The decision logic is *trained into* the model rather than executed at inference. — [arXiv PDF](https://arxiv.org/pdf/2605.26567)
- Average accuracy over MedCaseReasoning, MedQA, MIMIC-CDM-FI and ER-Reason: MedGuideX-9B 58.77 vs 53.29 for the Qwen3.5-9B base. Guideline baselines: RAG 54.12, 3-shot 54.08, CPGPrompt 40.20, fine-tuning on CPG text 48.45, RL with LLM-judged process rewards 56.15. GPT-5.0 scored 65.30. — [arXiv PDF](https://arxiv.org/pdf/2605.26567)
- Prior guideline-as-code work it cites: MedDM, CPGPrompt (Deng et al. 2026), MedCheckLLM (Schubert et al. 2025). — [arXiv PDF](https://arxiv.org/pdf/2605.26567)

**[xie2026z3] Xie, Du, "Neuro-Symbolic Resolution of Recommendation Conflicts in Multimorbidity Clinical Guidelines", arXiv 2604.17340 (Apr 2026).** Preprint; FULL.
- Method: LLM agents formalise guideline recommendations into typed predicates and rules (SNOMED CT, LOINC, RxNorm), and **Z3 SAT/SMT** checks pairs for redundancy and conflict. Data: 12 Chinese SGLT2-inhibitor guidelines, 226 labelled pairs. — [arXiv PDF](https://arxiv.org/pdf/2604.17340)
- **Conflict-detection F1: 0.861 for the neuro-symbolic pipeline vs 0.298 for GPT-5 and 0.148 for DeepSeek-V3.1** (recall 0.898 vs 0.186 vs 0.082). — [arXiv PDF](https://arxiv.org/pdf/2604.17340)
- Formalisation was only 80.1% exact-match accurate, with contextual loss the main error source (56.7% of errors). The gold set was built partly from pipeline output. — [arXiv PDF](https://arxiv.org/pdf/2604.17340)
- Prior LLM + solver work cited (general domain): Logic-LM, LINC, NL2FOL, LELMA. The authors claim only that neuro-symbolic methods are unexplored "for clinical guideline coordination". — [arXiv PDF](https://arxiv.org/pdf/2604.17340)

**[su2025medrulekg] Su, "MedRule-KG: A Knowledge-Graph–Steered Scaffold for Mathematical Reasoning with a Lightweight Verifier", arXiv 2510.16309 (2025).** Preprint; FULL.
- A drug–drug interaction safety check with a tiny KG (CYP inhibition, QT prolongation) and a **post-hoc symbolic verifier that overrides the LLM's label**. — [arXiv HTML](https://arxiv.org/html/2510.16309v1)
- Exact match: CoT 0.767 → CoT + KG 0.900 → KG + verifier 1.000 (90 examples). **Circular by construction**: gold labels come from the same rules. Model not named. — [arXiv HTML](https://arxiv.org/html/2510.16309v1)

**[mukherjee2024polaris] Mukherjee et al. (Hippocratic AI), "Polaris: A Safety-focused LLM Constellation Architecture for Healthcare", arXiv 2403.13313 (Mar 2024).** Industry preprint; PARTIAL (about 40% read).
- A patient-facing primary LLM plus specialist agents (Medication, Labs & Vitals, Checklist, Privacy & Compliance, Hospital & Payor Policy, **Human Intervention** escalation). An orchestration layer uses **deterministic, imperative control flow**, and structured agent outputs are parsed by a code harness. — [arXiv HTML](https://arxiv.org/html/2403.13313v1)
- Evaluated with more than 1,100 nurses and 130 physicians posing as patients. The developer reports parity with nurses on aggregate and that specialist agents "significantly outperform" GPT-4 and LLaMA-2 70B on component tasks. Exact numbers were not in the portion read. Non-diagnostic scope. — [arXiv HTML](https://arxiv.org/html/2403.13313v1)

**[chandrasekaran2026neuron] Chandrasekaran et al., "NEURON: A Neuro-symbolic System for Grounded Clinical Explainability", arXiv 2605.01189v3.** The authors say it was accepted at IEEE CHASE 2026; the proceedings were not checked. FULL.
- SNOMED CT knowledge-graph embeddings plus ontology counts feed classical predictors of AHF ICU mortality (MIMIC-IV; validation n = 3,951). A RAG-grounded local LLM writes explanations from SHAP drivers and **deterministically generated threshold statuses**. — [arXiv PDF](https://arxiv.org/pdf/2605.01189)
- XGBoost AUC 0.73 → 0.77. The LLM still hallucinated drug use (e.g. dobutamine when the feature was 0). Explanation quality was judged only by an LLM, not clinicians. — [arXiv PDF](https://arxiv.org/pdf/2605.01189)

**[jiang2025kare] Jiang, Xiao, Jiang, Bhatia, Kass-Hout, Sun, Han, "Reasoning-Enhanced Healthcare Predictions with Knowledge Graph Community Retrieval" (KARE), ICLR 2025.** Peer-reviewed conference; ABSTRACT (ICLR proceedings).
- Multi-source KG with hierarchical community retrieval feeding LLM reasoning. Outperformed leading models by up to 10.8–15.0% (MIMIC-III) and 12.6–12.7% (MIMIC-IV) on mortality and readmission prediction. — [ICLR proceedings](https://proceedings.iclr.cc/paper_files/paper/2025/hash/cb5a3b4589c1a16f14740396625cbfc8-Abstract-Conference.html)

**[quadri2026medigemma] Quadri, Xue, Ady, Roshan, "Medi-Gemma: A Hybrid CDS System Integrating Deterministic EMR Analytics and RAG", arXiv 2607.04907.** Preprint; FULL.
- Deterministic intent router, ground-truth state injection, keyword-based protocol risk tiers and a **banned-phrase SafetyVerifier** around a local Gemma model (wound care). — [arXiv PDF](https://arxiv.org/pdf/2607.04907)
- **No quantitative evaluation and no baselines.** It is an example of hybrid-safety claims made without evidence. — [arXiv PDF](https://arxiv.org/pdf/2607.04907)

**Not verified (seen in search or citations only):**
- MedCheckLLM (Schubert et al., JMIR Formative Research 2025, e53335): the page returned empty. — [JMIR listing](https://formative.jmir.org/2025/1/e53335)
- CPGPrompt (Deng et al. 2026): cited by MedGuideX only.
- Medical-safety benchmarks surfaced by search (RxSafeBench, arXiv 2511.04328; a drug-contraindication RAG system, arXiv 2508.06145): not opened.

### Inferences
- Pattern across EndoGov, MedDEx, CDR-Agent and Prenosil: **deterministic layers sharply cut rule or guideline violations** (EndoGov C-LVR 17.3% → 0.0% with post-hoc rules on GPT-4o). They do **not** by themselves fix accuracy: GPT-4o + rules reached only 0.529, below *rules alone* at 0.800. RQ1 should therefore separate "violation/unsafe rate" from "decision quality". A **rules-only baseline** should be added alongside the single-LLM baselines, because two independent studies (EndoGov; MedDEx scenario routing at 79.9%) show rules alone can beat LLM-centred pipelines on rule-defined tasks. This is a threat to the multi-agent premise that the proposal should test, not hide.
- Many hybrid results are **circular by construction**: gold labels derived from the same rules (MedRule-KG; EndoGov CPTAC; MedGuideX synthetic QA). The project's reference labels should be independent of its packs, for example clinician-adjudicated or from a different guideline source, or the evaluation will overstate the decision core.
- Prior art for each proposed component:
  - deterministic orchestration ← Polaris, EndoGov;
  - guideline-as-code ← MedDM, MedDEx, MedGuideX, Oniani;
  - post-hoc rule verifier ← EndoGov, MedRule-KG, Prenosil;
  - "unable to determine → ask" ← MedDM, MediQ;
  - solver-based consistency checking ← Xie & Du.
  The defensible contribution is the **combination with multi-status safety outputs, missing-value robustness and matched-baseline safety evaluation**, which none of these reports.

### Gaps
- Several relevant systems are 2026 preprints without independent validation. EndoGov's venue was not found, and Polaris's quantitative results were not read.
- No verified hybrid system reports NEED_MORE_INFO / ABSTAIN / ESCALATE statuses as evaluated outputs. MedDM has the branch but no evaluation; EndoGov's referral is simulated.
- MedCheckLLM, CPGPrompt, MedGraphRAG, Kg4Diagnosis, MedAgent-Pro and CLARITY were not opened.

---

## KQ3. What evidence exists on escalation and triage safety of LLMs (under-triage vs over-triage, red flags, emergency referral)?

### Takeaway
The evidence shows **two failure directions, depending on setting and evaluation format.** In clinician-facing studies with full notes or vignettes, LLMs tend to **over-escalate**. GPT-3.5 recommended admission with sensitivity 1.00 and specificity 0.07 (Williams et al.), and LLMs over-assigned the highest MTS category (Masanneck et al.). In consumer-facing tests, a product (ChatGPT Health) **under-triaged more than half of emergencies** in a structured test (Ramaswamy et al., Nature Medicine 2026, secondary sources only). A critique argues that the forced-choice format drove part of that failure. When real people used LLMs, correct disposition was below 44.2%, even though the LLM alone reached 56.3% (Bean et al.). Escalation accuracy is therefore not a fixed model property. It depends on the information supplied, the output format and the human in the loop. That argues for **deterministic red-flag rules evaluated separately from LLM judgment**.

### Cited Findings

- **[williams2024] Williams, Miao, Kornblith, Butte, "Evaluating the use of large language models to provide clinical recommendations in the Emergency Department", Nature Communications 15:8236 (2024), DOI 10.1038/s41467-024-52415-1.** Peer-reviewed; FULL (PMC).
  - Data: UCSF ED notes (history and examination only). Tasks: admission, radiology and antibiotics. Models: GPT-3.5-turbo and GPT-4-turbo, zero-shot, four prompts. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11461960)
  - Unbalanced n = 1,000 accuracy, admission: physician 0.83 vs GPT-4-turbo 0.43–0.58 vs GPT-3.5 0.29–0.53. Averaged over tasks, GPT-4-turbo was 8% less accurate than the physician and GPT-3.5 24% less. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11461960)
  - **"Overly cautious":** GPT-3.5 (Prompt A, balanced admission) sensitivity 1.00, specificity 0.07; 714 of 1,000 false positives on the unbalanced set. Prompting for "only when absolutely required" moved the trade-off toward specificity at the cost of sensitivity. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11461960)
- **[masanneck2024] Masanneck et al., "Triage Performance Across Large Language Models, ChatGPT, and Untrained Doctors in Emergency Medicine", J Med Internet Res 26:e53297 (2024), DOI 10.2196/53297.** Peer-reviewed; FULL (PMC).
  - 124 ED vignettes from one day in Düsseldorf, Manchester Triage System. Quadratic-weighted κ against consensus: professional raters 0.91; untrained doctors 0.68; GPT-4-based ChatGPT 0.67; Llama 3 70B 0.52; Mixtral 0.42. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11214027)
  - **LLMs tended to over-triage and untrained doctors to under-triage.** Red (Level 1) assigned in 4% of consensus cases vs 12.5% by GPT-4, 22.5% by Llama 3 70B and 37.1% by Mixtral. All raters caught the consensus-Red cases, except 2 doctor ratings. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11214027)
  - Adding a GPT-4 second opinion to untrained doctors gave κ 0.70 vs 0.68 (P = .97). — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11214027)
- **[ramaswamy2026] Ramaswamy et al., "ChatGPT Health performance in a structured test of triage recommendations", Nature Medicine (2026), DOI 10.1038/s41591-026-04297-7.** Peer-reviewed; **SECONDARY** (institutional press release plus a critique; the paper itself was not opened).
  - Design: 60 scenarios across 21 specialties × 16 contextual variations = 960 interactions, with urgency set by 3 physicians using guidelines from 56 societies. — [Mount Sinai press release](https://www.mountsinai.org/about/newsroom/2026/research-identifies-blind-spots-in-ai-medical-triage)
  - Results: "more than half" of physician-judged emergencies were under-triaged. Suicide-crisis alerts were "inverted relative to clinical risk". In one asthma case the tool identified warning signs but still advised waiting. — [Mount Sinai press release](https://www.mountsinai.org/about/newsroom/2026/research-identifies-blind-spots-in-ai-medical-triage)
  - A critique cites the headline figure as 51.6% of emergencies. — [Fraile Navarro et al. arXiv](https://arxiv.org/html/2603.11413v1)
- **[fraile2026] Fraile Navarro, Magrabi, Coiera, "Evaluation format, not model capability, drives triage failure in the assessment of consumer health AI", arXiv 2603.11413 (2026).** Preprint; PARTIAL.
  - Five frontier models on a 17-scenario partial replication. Naturalistic accuracy 70.1% vs 63.6% under constrained A/B/C/D output (+6.4 pp, p = 0.015). GPT-5.2 scored 16% on asthma under forced choice vs 100% with free text. — [arXiv HTML](https://arxiv.org/html/2603.11413v1)
  - Anchoring cut Claude Opus 4.6's emergency recommendations for DKA from 8/8 to 1/8. Adjudication used LLM judges, not clinicians. — [arXiv HTML](https://arxiv.org/html/2603.11413v1)
- **[bean2026] Bean et al., "Reliability of LLMs as medical assistants for the general public: a randomized preregistered study", Nature Medicine 32(2):609–615 (2026), DOI 10.1038/s41591-025-04074-y** (correction noted, Nat Med 32(5):1937). Peer-reviewed; FULL (PMC).
  - 1,298 UK adults; GPT-4o, Llama 3, Command R+ vs control; 10 scenarios with gold-standard dispositions. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12920132/)
  - LLMs alone: relevant condition in 94.9% of cases, correct disposition 56.3%. With users: conditions below 34.5%, disposition below 44.2%, with no disposition advantage over control. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12920132/)
  - Failure modes: in 16 of 30 sampled transcripts the first message lacked key information; similar subarachnoid-haemorrhage descriptions got opposite advice; **MedQA scores were largely uncorrelated with user performance; simulated users overestimated human performance.** — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12920132/)
- **[lee2025triage] Lee et al., "From Promising Capability to Pervasive Bias: Assessing LLMs for ED Triage", arXiv 2504.16273v2.** The page states it is a World Scientific open-access chapter; the specific proceedings were not identified. PARTIAL.
  - ESI/KTAS/MIMIC-IV-ED acuity prediction. kNN-retrieved few-shot (KATE) was strongest. — [arXiv HTML](https://arxiv.org/html/2504.16273v2)
  - LLMs were "hesitant to assign Level 1" acuity. Counterfactual sex-by-race edits shifted acuity, and CoT reduced the bias. **No under- or over-triage rates were reported.** — [arXiv HTML](https://arxiv.org/html/2504.16273v2)
- **Reused:** HealthBench gives 9.6% of examples to "emergency referrals" (industry preprint). — [arXiv HTML](https://arxiv.org/html/2505.08775v1)

### Inferences
- An ESCALATE_NOW status must be scored in **both directions**. Over-escalation (Williams, Masanneck) is the common failure in clinician-facing, full-information settings. Under-escalation (Ramaswamy; Bean's real-user interactions) appears in consumer and forced-format settings. RQ1's "unnecessary escalation" metric is therefore as necessary as missed-escalation recall.
- The Fraile Navarro critique and MediQ (KQ1) both show that **output format and information completeness change escalation behaviour**. A closed status set is itself a format constraint, so the proposal should test (RQ3) whether constraining output to five statuses changes escalation accuracy against free-text explanation. That is a risk the proposal should name, not assume away.
- Deterministic red-flag rules that run *before* the LLM are not evaluated in any study found here. Masanneck's finding that all raters caught the obvious Red cases suggests the hard failures sit in borderline cases, where simple rules may also struggle. This is an inference.

### Gaps
- Ramaswamy et al.: exact under-triage and over-triage percentages, demographic effects and the definitions used were not read in the primary paper (SECONDARY only).
- Masanneck et al.: numeric over- and under-triage rates are in a supplementary figure that was not read.
- No verified study evaluates a hybrid red-flag-rule + LLM escalation design against an LLM-only design.

---

## KQ4. What human-factors evidence exists on automation bias and physician–AI interaction with LLM decision support?

### Takeaway
Randomized trials give mixed and sobering results:
- LLM access did **not** improve physicians' diagnostic reasoning (76% vs 74%, P = .60), while the LLM alone scored 16 points higher than physicians with conventional resources (Goh et al., JAMA Netw Open 2024).
- It **did** improve management reasoning by 6.5 points, but physicians + LLM were no better than the LLM alone (Goh et al., Nature Medicine 2025).
- Among 44 AI-trained physicians, deliberately erroneous LLM suggestions lowered diagnostic reasoning scores by 14.0 points and top-diagnosis accuracy by 18.3 points (Qazi et al., preprint; reportedly in NEJM AI).
- Lay users did worse than the LLM alone (Bean et al.).

So "human-in-the-loop" is not a safety mechanism by itself. Errors in the AI output pass through to the clinician's decision.

### Cited Findings
- **[goh2024jno] Goh, Gallo, Hom, … Rodman, Chen, "Large Language Model Influence on Diagnostic Reasoning: A Randomized Clinical Trial", JAMA Netw Open 7(10):e2440969 (2024), DOI 10.1001/jamanetworkopen.2024.40969.** Peer-reviewed; ABSTRACT (University of Minnesota experts record and AHRQ PSNet).
  - 50 physicians, up to 6 vignettes in 60 minutes. Diagnostic reasoning median 76% (LLM arm) vs 74% (conventional); adjusted difference 2 pp (95% CI −4 to 8; P = .60). — [UMN record](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/); [PSNet](https://psnet.ahrq.gov/issue/large-language-model-influence-diagnostic-reasoning-randomized-clinical-trial)
  - **LLM alone 16 pp higher than the conventional arm (95% CI 2–30; P = .03).** Registered as NCT06157944. — [UMN record](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/)
- **[goh2025natmed] Goh et al. (18 authors; last author Rodman), "GPT-4 assistance for improvement of physician performance on patient care tasks: a randomized controlled trial", Nature Medicine (2025), DOI 10.1038/s41591-024-03456-y.** Peer-reviewed; FULL (author-hosted PDF); the medRxiv preprint (PMC11326321) was also opened.
  - 92 physicians, 5 management vignettes. Total score 43.0% vs 35.7%, difference 6.5% (95% CI 2.7–10.2; P < .001). — [Author PDF](https://www.erichorvitz.com/GPT-4-management_reasoning_Nature_Med.pdf)
  - More time per case: 801.5 s vs 690.2 s (P = .022). — [Author PDF](https://www.erichorvitz.com/GPT-4-management_reasoning_Nature_Med.pdf)
  - **LLM alone 43.7% vs physicians + LLM 43.0%** (not significant). The sign of the difference conflicts between the abstract and the results. — [Author PDF](https://www.erichorvitz.com/GPT-4-management_reasoning_Nature_Med.pdf); [medRxiv/PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11326321/)
  - Exploratory harm ratings were similar between arms; high likelihood of harm 4.2% vs 2.9%. Wrong answers were not penalized in the primary score. — [Author PDF](https://www.erichorvitz.com/GPT-4-management_reasoning_Nature_Med.pdf)
- **[qazi2025] Qazi, Ali, Khawaja, Akhtar, Sheikh, Alizai, "Automation Bias in Large Language Model Assisted Diagnostic Reasoning Among AI-Trained Physicians", medRxiv 10.1101/2025.08.23.25334280 (v1 2025-08-26; v2 2025-09-08).** Preprint; FULL (medRxiv PDF).
  - Single-blind RCT, 44 physicians in Pakistan, all with 20 hours of AI-literacy training. ChatGPT-4o suggestions contained deliberate, clinically significant errors in 3 of 6 vignettes; consultation was voluntary (68.9% vs 66.7%). — [medRxiv PDF](https://www.medrxiv.org/content/10.1101/2025.08.23.25334280.full.pdf)
  - Reasoning score 73.3% vs 84.9%, adjusted −14.0 pp (95% CI −18.9 to −9.1; P < .0001). Top-choice accuracy −18.3 pp (95% CI −26.6 to −10.0). Registered NCT06963957. — [medRxiv PDF](https://www.medrxiv.org/content/10.1101/2025.08.23.25334280.full.pdf)
  - Sciety links no journal version. — [Sciety](https://sciety.org/articles/activity/10.1101/2025.08.23.25334280)
  - A NEJM Journal Watch review (June 2026) describes a matching trial (44 physicians, 20-hour programme, GPT-4o, 6 vignettes) as published in *NEJM AI*. That citation was not verified. — [NEJM Journal Watch](https://clinician.nejm.org/when-ai-steers-clinicians-wrong-CLINjwNA60155)
- **[bean2026]** (see KQ3): users + LLM identified relevant conditions in <34.5% of cases vs 94.9% for the LLM alone, and carried over correct suggestions poorly. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12920132/)
- **[masanneck2024]** (see KQ3): a GPT-4 second opinion did not improve untrained doctors' triage (κ 0.70 vs 0.68, P = .97). — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11214027)
- **Guidance (reused, not new):** the FDA CDS guidance (Jan 2026) defines automation bias as over-reliance on automated suggestions, more likely under time pressure. — [FDA](https://www.fda.gov/media/109618/download)

### Inferences
- Taken together, the Goh trials and Qazi suggest that LLM output quality largely passes through to clinicians. Correct output adds little over the LLM alone; erroneous output pulls clinicians down by 14–18 points. Safety mechanisms must therefore act **on the output before it reaches the physician**: verification, closed options and abstention. Relying on physician review to catch errors is not supported.
- None of the RCTs tested *abstention or escalation statuses* or *evidence-linked claims* as an intervention. Whether showing ABSTAIN or NEED_MORE_INFO and claim-level citations reduces automation bias is untested in the verified literature. It is future work (live or user study), not something this project can claim.

### Gaps
- The NEJM AI version of the Qazi et al. trial (DOI, final numbers) was not verified.
- No verified RCT tests explanations or citations as mitigation for LLM automation bias. Pre-LLM evidence (for example, Jabbour et al., JAMA 2023, on systematically biased AI and explanations) was not opened this pass.
- Goh JAMA Netw Open: only the abstract was read.

---

## Cross-cutting synthesis for Chapter 2 (what is established, contradictions, gaps, unverified items)

### Takeaway
The evidence base supports the proposal's *direction*: external abstention triggers, deterministic execution of computable logic, and checks before physician review. It does **not** yet contain a study that evaluates the proposal's *combination* (five statuses + deterministic decision core + verifier) against matched single-LLM baselines on safety metrics. That combination is the defensible gap for 2.3.

### Cited Findings
**(a) What the evidence establishes**
- LLMs abstain or ask too rarely, and self-reported confidence is unreliable. — [Savage JAMIA](https://api.crossref.org/works/10.1093%2FJAMIA%2FOCAE254); [Griot Nat Commun](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-024-55628-6.html); [MedAbstain](https://arxiv.org/html/2601.12471v2)
- Withholding information sharply cuts accuracy, and well-designed abstention or asking recovers part of the loss. — [MediQ](https://arxiv.org/pdf/2406.00922v3)
- Deterministic tools and rule executors beat pure LLMs where the logic is computable. — [AgentMD](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-64430-x.html); [Goodell](https://escholarship.org/uc/item/8xv9r6tn); [MedDEx](https://arxiv.org/pdf/2610.15912); [CDR-Agent](https://arxiv.org/html/2505.23055v1); [Prenosil](https://pmc.ncbi.nlm.nih.gov/articles/PMC12638795)
- LLM escalation is miscalibrated in both directions, depending on setting. — [Williams](https://pmc.ncbi.nlm.nih.gov/articles/PMC11461960); [Masanneck](https://pmc.ncbi.nlm.nih.gov/articles/PMC11214027); [Bean](https://pmc.ncbi.nlm.nih.gov/articles/PMC12920132/)
- Human oversight does not neutralise LLM errors. — [Qazi](https://www.medrxiv.org/content/10.1101/2025.08.23.25334280.full.pdf); [Goh Nat Med](https://www.erichorvitz.com/GPT-4-management_reasoning_Nature_Med.pdf)
- **"Deterministic decision core + LLM agents" is established prior art, not a novel pattern.** Examples: EndoGov (hard-path rules plus a deterministic validator over multi-agent LLMs); MedDEx (deterministic guideline executor); Polaris (deterministic orchestration plus safety specialist agents); MedDM (LLM-executable guideline trees with an "unable to determine → ask" branch); Z3-verified guideline rules. — [EndoGov](https://arxiv.org/pdf/2604.23802); [MedDEx](https://arxiv.org/pdf/2610.15912); [Polaris](https://arxiv.org/html/2403.13313v1); [MedDM](https://arxiv.org/html/2312.02441v1); [Xie & Du](https://arxiv.org/pdf/2604.17340)
- Deterministic layers remove guideline violations but do not by themselves fix accuracy: GPT-4o C-LVR 17.3% → 0.0% with rules, yet accuracy only 0.460 → 0.529, against rules-only 0.800. — [EndoGov](https://arxiv.org/pdf/2604.23802)

**(b) Contradictions and null findings**
- Hybrid gains shrink or reverse on real notes. CDR-Agent on CDR-Bench: sensitivity 0.683 vs 0.936 and F1 0.592 vs 0.608 for the pure-LLM baseline. MedDEx on MedGUIDE: under 0.5 points. — [CDR-Agent](https://arxiv.org/html/2505.23055v1); [MedDEx](https://arxiv.org/pdf/2610.15912)
- Knowledge-graph injection improved concept F but lowered ROUGE-L for zero-shot ChatGPT, and did not significantly change diagnosis omission. — [DR.KNOWS](https://pmc.ncbi.nlm.nih.gov/articles/PMC11894347/)
- **Multi-agent and RAG baselines underperformed a single LLM** in MedDEx (MedAgents 56.09 and MDAgents 50.29 vs 66.65). MediQ found self-consistency alone degraded interactive performance. — [MedDEx](https://arxiv.org/pdf/2610.15912); [MediQ](https://arxiv.org/pdf/2406.00922v3)
- Oniani: the best automatic score (GPT-4 BDT) ranked lowest in human evaluation. — [arXiv](https://arxiv.org/html/2401.11120v2)
- Under-triage (Ramaswamy, per secondary sources) vs over-triage (Williams, Masanneck). Format effects (Fraile Navarro) partly explain the conflict. — [Mount Sinai](https://www.mountsinai.org/about/newsroom/2026/research-identifies-blind-spots-in-ai-medical-triage); [Fraile Navarro](https://arxiv.org/html/2603.11413v1)
- **Rules-only baselines beat LLM-centred pipelines on rule-defined tasks.** EndoGov: rules-only 0.800 vs GPT-4o 0.460. MedDEx: executor 79.9% vs best prompting 59.5%. This challenges both the LLM premise and the multi-agent premise for packs whose logic is fully encodable. — [EndoGov](https://arxiv.org/pdf/2604.23802); [MedDEx](https://arxiv.org/pdf/2610.15912)
- Many hybrid "perfect" results are circular, with labels derived from the same rules (MedRule-KG EM 1.000; EndoGov CPTAC). — [MedRule-KG](https://arxiv.org/html/2510.16309v1); [EndoGov](https://arxiv.org/pdf/2604.23802)
- MedAbstain reports high abstention precision (few unnecessary abstentions). That argues against over-abstention being the main risk in MCQ, but it may not transfer to a CDS setting with explicit statuses. — [MedAbstain](https://arxiv.org/html/2601.12471v2)

### Inferences
**(c) Gaps relevant to P1–P4 and RQ1–RQ4**
- P2/RQ1: no study scores a multi-status output (recommend / caveats / need-more-info / escalate / abstain) with both missed-escalation and unnecessary-abstention or escalation rates. MedAbstain's precision/recall and CoDoC's fixed-error-rate comparison are the closest templates.
- P4/RQ4: hybrid studies compare against weak pure-LLM baselines, such as a GPT-4o baseline given all CDR text or zero-shot prompting. None matches tools, retrieval and compute in the RQ4 sense. MedDEx reports calls and latency, which is a useful precedent.
- RQ3: format and order sensitivity of escalation is shown (Fraile Navarro; MediQ), but not for closed status sets.
- Real-data evaluations are rare. Most hybrid studies use synthetic cases and author-checked rules: Oniani (39 synthetic), Xu (20 synthetic), CDR-Agent (synthetic plus 144 notes), MedDEx (benchmarks).
- Decision-analytic or weighted ranking with LLMs, and constrained decoding to closed option sets, have no verified clinical evidence in this pass.
- **Novelty framing:** no verified prior system combines a deterministic decision core with (i) the five safety statuses evaluated as outputs, (ii) a robustness test across missing-value ranges, (iii) claim-to-source verification, and (iv) tool-, retrieval- and compute-matched single-LLM *and* rules-only baselines on safety metrics. 2.3 should claim the gap in this combination and its evaluation, not in the architecture pattern.
- RQ4 implication: add a **rules-only baseline** and use reference labels independent of the packs, to avoid circular validation.
- No new or updated Saudi or international guidance was searched or found in this pass. Regulatory coverage is in the earlier ledger.

### Gaps
**(d) Inaccessible or unverified items**
- nature.com full texts (Griot, Dvijotham, AgentMD, Ramaswamy) were not accessible; their numbers come from abstracts or secondary sources. Ramaswamy is SECONDARY only.
- The ACL Anthology page for Wen et al. (TACL) was blocked this pass; metadata rest on the 2026-11-23 verification.
- MedAbstain's possible EACL 2026 publication, the Qazi et al. NEJM AI publication, the Oniani IEEE ICHI 2024 record, the CDR-Agent venue and the Lee et al. World Scientific proceedings are all unverified.
- Shell access to the Crossref/OpenAlex/Semantic Scholar APIs failed (no network), so DOI checks used WebFetch only where allowed.
- Not opened: EndoGov's venue and date, Polaris's quantitative results (only about 40% of the HTML was read), NEURON's IEEE CHASE 2026 proceedings, MedCheckLLM (JMIR page returned empty), CPGPrompt, MedGraphRAG, RxSafeBench and arXiv 2508.06145.

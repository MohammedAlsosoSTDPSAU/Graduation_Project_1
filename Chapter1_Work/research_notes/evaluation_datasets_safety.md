# Evaluation, Datasets/Testbeds, and Safety Mechanisms for LLM-based Clinical Decision Support (evidence ledger for Chapter 1: 1.2 Problem Statement, 1.4 Objectives, 1.5 Scope)

Verification legend used throughout:
- **FT** = full text read (pages/sections named); **ABS** = abstract or record page only ("abstract-only"); **SEC** = detail confirmed only via a secondary page (news, institutional page, or library record), so check against the publisher before final citation.
- **Preprint** = not peer-reviewed at time of checking; **Industry** = produced by a company; **Regulatory/Policy** = official agency or data-custodian page.
- All sources were opened on 2026-11-23. Nothing below comes from a search snippet alone.

---

## KQ1. Evaluation frameworks for clinical LLMs: what they measure, and the limits of benchmark-only (exam QA) evaluation

### Takeaway
Recent frameworks (MedHELM, HealthBench, CRAFT-MD, QUEST) all start from the same problem: exam-style multiple-choice accuracy does not reflect real clinical decision-making. A 519-study systematic review found that only 5% of evaluations used real patient data, 95.4% used accuracy as the main measure, and only 1.2% measured calibration or uncertainty. When the same cases are moved from static vignettes into interactive or sequential formats, accuracy falls sharply (GPT-4: 0.820 → 0.627 in CRAFT-MD; "below a tenth" of the original in AgentClinic). This is the core justification for the 1.2 Problem Statement and for using safety-centred metrics instead of accuracy alone in 1.4.

### Cited Findings

**R01 — MedHELM**
- Title: "MedHELM: Holistic Evaluation of Large Language Models for Medical Tasks". Authors: Suhana Bedi (first) … Nigam H. Shah (last); the author list is long. Year: arXiv 2025 (v1 2025-05-26, v2 2025-06-02). Peer-reviewed version: *Nature Medicine* 32, 943–951 (2026), "Bedi, S., Cui, H., Fuentes, M. et al.", URL https://www.nature.com/articles/s41591-025-04151-2 (DOI implied: 10.1038/s41591-025-04151-2). Status: preprint verified (ABS); journal version verified only through Stanford HAI's citation (SEC). — [arXiv](https://arxiv.org/abs/2505.23802); [Stanford HAI](https://hai.stanford.edu/news/measuring-what-medical-ai-can-actually-do)
- Problem and method: holistic evaluation of LLMs across real medical tasks, using a clinician-built taxonomy of 5 categories, 22 subcategories and 121 tasks, developed with 29 clinicians. The arXiv version has 35 benchmarks (17 existing, 18 new) and evaluates 9 frontier LLMs. — [arXiv](https://arxiv.org/abs/2505.23802)
- Findings (arXiv v2): DeepSeek R1 had a 66% win-rate and o3-mini 64%. Claude 3.5 Sonnet was comparable at about 40% lower estimated cost. Normalized accuracy by category: Clinical Decision Support 0.56–0.72, the second-lowest category after Administration & Workflow at 0.53–0.63, compared with Clinical Note Generation at 0.73–0.85. LLM-jury agreement with clinicians: ICC = 0.47, against clinician–clinician ICC 0.43, ROUGE-L 0.36 and BERTScore-F1 0.44. — [arXiv](https://arxiv.org/abs/2505.23802)
- Conflict to note: the Nature Medicine version, as reported by HAI, gives **37** benchmark datasets and reports win rates, with clinical decision support at **0.63–0.77**. Cite the version you actually use. — [Stanford HAI](https://hai.stanford.edu/news/measuring-what-medical-ai-can-actually-do)
- Limitations: jury agreement with clinicians is only moderate (ICC 0.47), and cost figures are estimates. It is not clear from the pages read which datasets are gated or private.
- Relevance: shows clinical decision support is among the weakest categories even for frontier models, and it gives a precedent for reporting cost next to performance. Supports 1.2 and 1.4 (the cost metric).

**R02 — HealthBench** (Industry, Preprint)
- Title: "HealthBench: Evaluating Large Language Models Towards Improved Human Health". Authors: Rahul K. Arora (first) … Karan Singhal (last), OpenAI. arXiv 2505.08775, submitted 2025-05-13. Status: preprint, industry. Verification: ABS + HTML full text read up to about 46% (the Discussion section was not reached). — [arXiv abs](https://arxiv.org/abs/2505.08775); [arXiv HTML](https://arxiv.org/html/2505.08775v1)
- Dataset: 5,000 multi-turn conversations; rubrics written by 262 physicians; 48,562 unique criteria (Table 3 counts 57,237 rubric criteria in total). — [arXiv abs](https://arxiv.org/abs/2505.08775); [HTML](https://arxiv.org/html/2505.08775v1)
- What it measures: 7 themes, with the share of examples in each: Global health 21.9%, **Responding under uncertainty 21.4%**, Expertise-tailored communication 18.4%, **Context seeking 11.9%**, **Emergency referrals 9.6%**, Health data tasks 9.5%, Response depth 7.2%. Five axes: Completeness 39%, Accuracy 33%, Context awareness 16%, Communication quality 8%, Instruction following 4%. Grading is rubric-based and model-graded, and negative criteria subtract points. — [HTML](https://arxiv.org/html/2505.08775v1)
- Findings: GPT-3.5 Turbo scored 16%, GPT-4o 32%, o3 60%. The top score on HealthBench Hard was 32%. GPT-4.1 nano beat GPT-4o at about 25× lower cost. — [arXiv abs](https://arxiv.org/abs/2505.08775)
- Limitations stated by the authors: "Most conversations in HealthBench were synthetically generated." HealthBench Consensus "has greater precision but lower recall." Physician baselines depend on how the task is framed, and scores correlate weakly with response length. The grader model was not confirmed in the text read. — [HTML](https://arxiv.org/html/2505.08775v1)
- Relevance: its themes "emergency referrals", "context seeking" and "responding under uncertainty" correspond directly to ESCALATE_NOW, NEED_MORE_INFO and RECOMMEND_WITH_CAVEATS/ABSTAIN. Supports 1.4 (status-level metrics) and 1.5 (a possible secondary testbed).

**R03 — CRAFT-MD**
- Title: "An evaluation framework for clinical use of large language models in patient interaction tasks". Authors: Shreya Johri (first) … Pranav Rajpurkar (last; Roxana Daneshjou co-senior). *Nature Medicine*, 2025 (accepted 2024-10-01). DOI 10.1038/s41591-024-03328-5. Volume and pages were not visible in the copy read. Status: peer-reviewed. Verification: FT, first half of the published PDF read through a third-party mirror. — [PDF mirror](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf); [Nature Index record](https://www.nature.com/nature-index/article/10.1038/s41591-024-03328-5)
- Method: the clinical LLM under test talks to a patient-AI agent and is scored by a grader-AI (GPT-4), with dermatologists reviewing 180 conversations. Formats tested: vignette, multi-turn, single-turn and summarized conversation, each as MCQ and free-response. Each case was run 5 times, giving 10,000 conversations. — [PDF](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Dataset: 2,000 vignettes: 1,800 from MedQA-USMLE across 12 specialties, 100 Derm-Public and 100 Derm-Private. A further 74 NEJM Image Challenge cases were used for multimodal testing. — [PDF](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Findings (4-choice MCQ, vignette → multi-turn): GPT-4 0.820 → 0.627; GPT-3.5 0.657 → 0.467; Mistral-v2-7b 0.637 → 0.426; LLaMA-2-7b 0.395 → 0.319. In free-response, GPT-4 fell from 0.486 to 0.264. According to the experts, GPT-4 gathered a complete history in only 71.11% of conversations. Grader-AI agreed with the experts 93.35% of the time. — [PDF](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Recommendations (10), including: use open-ended rather than MCQ questions; assess history-taking; combine automated grading with expert review; use simulated patients. — [PDF](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Limitations: the patient-AI can fail on questions beyond the vignette; many vignettes are not specific enough without answer options; expert review covered only dermatology; open models tested were 7B only.
- Relevance: strong evidence that MCQ accuracy overstates real performance. Free-response accuracy roughly halved, which also supports a closed, guideline-defined option set as a deliberate scoping choice. Supports 1.2 and 1.5.

**R04 — QUEST (Tam et al.)**
- Title: "A framework for human evaluation of large language models in healthcare derived from literature review". Authors: Thomas Yu Chow Tam (first) … Yanshan Wang (last), 15 authors. *npj Digital Medicine* 7 (2024), pp. 1–20 per DOAJ. DOI 10.1038/s41746-024-01258-7. Status: peer-reviewed. Verification: ABS (DOAJ record and arXiv abstract); the PMC full text was blocked by a captcha. — [DOAJ](https://doaj.org/article/35d95c2463094fad97d777e1d8376e44); [arXiv](https://arxiv.org/abs/2405.02559v2)
- Method: PRISMA review of literature from January 2018 to February 2024, covering **142 studies**. — [DOAJ](https://doaj.org/article/35d95c2463094fad97d777e1d8376e44)
- Output: the QUEST framework, with five principles (**Q**uality of Information, **U**nderstanding and Reasoning, **E**xpression Style and Persona, **S**afety and Harm, **T**rust and Confidence) and three phases (Planning; Implementation and Adjudication; Scoring and Review). The authors report gaps in the reliability, generalizability and applicability of current human-evaluation practice. — [DOAJ](https://doaj.org/article/35d95c2463094fad97d777e1d8376e44)
- Limitations: numbers on evaluator counts, blinding and sample sizes were not visible because the full text was not accessed.
- Relevance: methodological basis for any human or expert review of a subset of outputs (Safety and Harm; Trust and Confidence). Supports 1.4 and the human-review part of 1.5.

**R05 — Systematic review of LLM evaluation in health care (Bedi et al.)**
- Title: "Testing and Evaluation of Health Care Applications of Large Language Models: A Systematic Review". Authors: S. Bedi (first) … N. H. Shah (last). *JAMA* 333(4):319 (2025 issue; covered by news on 2024-10-18). DOI 10.1001/jama.2024.21700. Verification: the abstract numbers come from the NSF PAR record (ABS); the DOI and authors come from a news page (SEC). — [NSF PAR](https://par.nsf.gov/biblio/10616638); [News-Medical](https://www.news-medical.net/news/20241018/Study-finds-health-care-evaluations-of-large-language-models-lacking-in-real-patient-data-and-bias-assessment.aspx)
- Findings: 519 studies from January 2022 to February 2024. **Only 5% used real patient care data.** Medical-knowledge tasks made up 44.5% (mostly licensing exams) and diagnosis 19.5%. Question answering was 84.2% of language tasks, while conversational dialogue was 3.3%. **Accuracy was the primary dimension in 95.4%**; fairness, bias and toxicity were measured in 15.8%, deployment considerations in 4.6%, and **calibration and uncertainty in 1.2%**. — [NSF PAR](https://par.nsf.gov/biblio/10616638)
- Relevance: the strongest single quantitative source for the claim that "evaluation is dominated by exam-QA accuracy and neglects uncertainty and safety". Supports 1.2 and the choice of abstention and calibration metrics in 1.4.

**R07 — Hager et al. (MIMIC-CDM)**: the decision-making results are summarized under KQ2. Relevance to KQ1: LLM diagnostic accuracy was 16–25 points below physicians, and adherence to guidelines was poor. — [Nature Medicine via Springer](https://link.springer.com/10.1038/s41591-024-03097-1)

**R09 — AgentClinic** (Preprint): in the sequential, interactive format, MedQA diagnostic accuracy can "drop to below a tenth of the original accuracy". — [arXiv](https://arxiv.org/abs/2405.07960)

### Inferences
- Five independent sources point the same way: exam-style accuracy overstates readiness for decision support. These are R05 (5% real data, 95.4% accuracy-only), R03 (a 0.193 drop for GPT-4), R09, R07 (16–25 points below physicians) and R01 (CDS scores sit near the bottom). Together they let 1.2 argue that the gap lies in evaluation (safety, abstention, escalation) as well as in capability.
- Very few existing evaluations measure calibration or uncertainty (1.2%, R05). That gives a literature-backed novelty argument for making abstention quality and NEED_MORE_INFO/ESCALATE_NOW behaviour first-class metrics.
- HealthBench's theme structure is the closest published counterpart to the five-status output, since it scores emergency referral, context seeking and responding under uncertainty. Citing it lets the team frame the status labels as operationalising behaviours others already treat as important.

### Gaps
- MedHELM: the full Nature Medicine text was not opened. Cite the arXiv numbers unless the team reads the journal version, where the benchmark count (35 vs 37) and the CDS score type differ.
- HealthBench: the meta-evaluation results (macro-F1 between grader and physicians) and the Discussion were not read, and the grader model was not confirmed.
- QUEST: details on evaluator numbers, blinding and statistics were not read (abstract-only).
- CRAFT-MD: volume and pages were not seen.

---

## KQ2. Datasets/testbeds, access constraints (including third-party LLM API use), and feasibility for 4 students in about 2 semesters

### Takeaway
MIMIC-based testbeds (MIMIC-IV, MIMIC-IV-Ext-CDM) are the most realistic decision-making data, but they need individual PhysioNet credentialing, a supervisor as reference, CITI "Data or Specimens Only Research" training and a signed DUA. Since September 2025 they can be used with cloud LLM APIs **only if** zero data retention, no training use and no human review can be verified; ChatGPT-style consumer use is not allowed. MedQA-derived cases (via AgentClinic or CRAFT-MD) are open and the most feasible. The most defensible plan for 2 semesters is a **primary curated, guideline-grounded vignette set** (derived from MedQA, published cases, or synthetic cases with clinician-checked labels), plus an **optional MIMIC-IV-Ext-CDM arm** run with local open-weight models or a verified zero-retention API.

### Cited Findings

**R06 — MedQA (Jin et al. 2021)**
- Title: "What Disease Does This Patient Have? A Large-Scale Open Domain Question Answering Dataset from Medical Exams". Authors: Di Jin, Eileen Pan, Nassim Oufattole, Wei-Hung Weng, Hanyi Fang, Peter Szolovits. *Applied Sciences* 11(14):6421, July 2021. DOI 10.3390/app11146421. Peer-reviewed. Verification: ABS. — [DOAJ](https://doaj.org/article/11ebc944223d456caec3066e6152ebd1)
- Content: multiple-choice board-exam questions: English 12,723, simplified Chinese 34,251, traditional Chinese 14,123. The best baseline at the time scored 36.7% on English. — [DOAJ](https://doaj.org/article/11ebc944223d456caec3066e6152ebd1)

**R07 — Hager et al. 2024 (MIMIC-CDM)**
- Title: "Evaluation and mitigation of the limitations of large language models in clinical decision-making". Authors: Paul Hager, Friederike Jungmann, Robbie Holland, … Daniel Rueckert, Georgios Kaissis, Rickmer Braren (the middle authors appeared only as initials). *Nature Medicine* 30, 2613–2622 (2024). DOI 10.1038/s41591-024-03097-1. Peer-reviewed. Verification: FT (Springer page). — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)
- Dataset: 2,400 MIMIC-IV emergency-department patients with acute abdominal pain: appendicitis 957, cholecystitis 648, pancreatitis 538, diverticulitis 257. The MIMIC-CDM-FI variant gives all information upfront. — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)
- Method: the LLM requests physical exam, labs and imaging through action fields, then gives a diagnosis and treatment plan. Models were Llama 2 Chat, OASST and WizardLM (70B). A reader study compared them with 4 hospitalists on 80 patients. — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)
- Findings:
  - Models were significantly worse than physicians (P < 0.001), by 16–25 points.
  - Autonomous mean accuracy: 45.5%, 54.9% and 53.9%.
  - Only Llama 2 Chat consistently requested a physical exam first (97.1%, against 79.8% and 53.1%).
  - Accuracy on low lab results ranged from 26.5% to 70.2%.
  - Formatting errors appeared every 2–4 patients, and **non-existent tools were hallucinated every 2–5 patients**.
  - Changing the order of information shifted accuracy by up to 18.0 points.
  - Less information sometimes improved accuracy (for example, +18.5% on cholecystitis with radiologist reports only).
  - Filtering out normal labs helped.
  — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)
- Limitations: 4 pathologies and 1 complaint only; US and English only; small reader study; **"Closed models such as GPT-4 could not be tested due to MIMIC data agreements"** (paraphrased from the fetched summary); lenient matching of diagnosis names; only the first lab value per test. — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)
- Relevance: the closest existing testbed to "select among a closed set of guideline-defined options". The four diagnoses and their guideline-based treatments form a closed option set. It also shows that tools get hallucinated, which motivates deterministic validation of tool calls and output schemas. Supports 1.2, 1.4 and 1.5.

**R08 — MIMIC-IV-Ext-CDM on PhysioNet**
- "MIMIC-IV-Ext Clinical Decision Making: A MIMIC-IV Derived Dataset for Evaluation of Large Language Models on the Task of Clinical Decision Making for Abdominal Pathologies", by Hager, Jungmann and Rueckert. v1.0 released 2024-05-17, v1.1 on 2024-07-08. Parent dataset: MIMIC-IV v2.2. **Credentialed access**; licence: PhysioNet Credentialed Health Data License 1.5.0; DUA 1.5.0; required training: **CITI Data or Specimens Only Research**. — [PhysioNet](https://www.physionet.org/content/mimic-iv-ext-cdm/)
- Files: HPI, physical exam, discharge diagnosis and procedures; 138,788 lab results covering 480 tests; 4,403 microbiology results; 5,959 radiology reports; ICD codes; pathology IDs. The page says nothing specific about online LLMs. — [PhysioNet](https://www.physionet.org/content/mimic-iv-ext-cdm/)

**R09 — AgentClinic** (Preprint)
- Title: "AgentClinic: a multimodal agent benchmark to evaluate AI in simulated clinical environments". Authors: Samuel Schmidgall, Rojin Ziaei, Carl Harris, Eduardo Reis, Jeffrey Jopling, Michael Moor. arXiv 2405.07960, v1 2024-05-13, v5 2025-05-25. No journal reference shown. Verification: ABS. — [arXiv](https://arxiv.org/abs/2405.07960)
- Content: simulated patient interaction, multimodal data collection under incomplete information, and tool use, across 9 specialties and 7 languages, with bias perturbations. Some cases draw on "real-world electronic health records". Accuracy in the sequential format falls to as little as a tenth of static MedQA. Claude-3.5-based agents did best in most settings. Llama-3 improved by up to 92% (relative) with a notebook tool. — [arXiv](https://arxiv.org/abs/2405.07960)

**R10 — PhysioNet policy, 18 April 2023: "Responsible use of MIMIC data with online services like GPT"** (Policy)
- The credentialed DUA "explicitly prohibits sharing access to the data with third parties". This page **prohibits** "sending it through APIs provided by companies like OpenAI" and "using it in online platforms like ChatGPT". It suggests options with conditions: Azure OpenAI (opt out of human review), Amazon Bedrock, Google Vertex (with opt-outs), and Anthropic Claude (no training on prompts by default, no routine human review). — [PhysioNet 2023](https://physionet.org/news/post/gpt-responsible-use)

**R11 — PhysioNet policy, 24 September 2025: "Use of MIMIC Data with Large Language Models and Online Services"** (Policy; the newer guidance)
- Sharing access with third parties is prohibited under the DUA. **Locally deployed LLMs are "Strongly Recommended."** Cloud services and APIs are allowed only with **zero data retention, no use for training and no human review**, and those settings must be verified. "If a service's data handling practices are unclear or cannot be fully verified, do not use the service." The page warns that "zero data retention" claims may still involve logging or caching. Researchers are responsible for compliance. PhysioNet does not endorse any platform. — [PhysioNet 2025](https://physionet.org/news/post/llm-responsible-use/)

**R12 — MIMIC access procedure** (official MIMIC documentation)
- Steps: PhysioNet account → credentialing (human-subjects training, affiliation, reference) → CITI **"Data or Specimens Only Research"**, submitting the *Completion Report* → **students must list a supervisor as reference**, not a peer → processing "within 1-2 weeks" → sign a separate DUA for each dataset ("legally binding") → cloud access (recommended) or download. — [MIMIC FAQ](https://mimic.mit.edu/docs/faq/how-to-get-access.html)

**R02 / R03 as testbeds**
- HealthBench: 5,000 synthetic, physician-rubric conversations with emergency-referral, context-seeking and uncertainty themes. Its release licence was not verified here. — [arXiv](https://arxiv.org/abs/2505.08775)
- CRAFT-MD: 1,800 MedQA-USMLE vignettes plus dermatology sets; the Derm-Private set was written by dermatologists. Whether it is released was not verified. — [PDF](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)

#### Dataset feasibility table
Effort and fit ratings are the researcher's inferences from the cited facts. "Fit" means fit with a five-status, closed-option decision task that includes abstention.

| Dataset / testbed | Content | Access requirements | Licence / use restrictions (incl. third-party LLM API) | Fits decision-with-abstention task? | Effort (4 students, ~2 semesters) |
|---|---|---|---|---|---|
| MedQA (R06) | 12,723 English USMLE-style MCQs (plus Chinese sets) | Open download | Licence not verified here (check the repo or Hugging Face card); no patient data, so API use raises no PHI issue | **Partly**: one correct answer, always answerable, no abstention or escalation labels; would need relabelling or "information-withheld" variants to test NEED_MORE_INFO/ABSTAIN | Low |
| AgentClinic-MedQA (R09) | Interactive simulated-patient versions of MedQA cases across 9 specialties | Open code and cases (arXiv; distribution not verified) | Derived from MedQA; MIMIC-derived cases would carry MIMIC restrictions (not verified how they are distributed) | **Partly**: sequential information gathering suits NEED_MORE_INFO, but there are no gold labels for escalation or abstention | Low–Medium |
| CRAFT-MD vignettes (R03) | 1,800 MedQA vignettes + 100 Derm-Public + 100 Derm-Private | Release status not verified | Not verified | **Partly**: conversational; free-response is much harder | Medium |
| HealthBench (R02) | 5,000 synthetic multi-turn conversations, 48,562 physician criteria | Release not verified here | Industry (OpenAI) benchmark; licence not verified; synthetic, so no PHI | **Partly**: the themes map to escalate, need-more-info and uncertainty, but the tasks are open-ended, not closed-option, and grading is model-based | Medium |
| MIMIC-IV-Ext-CDM (R07, R08) | 2,400 real ED abdominal-pain cases (4 pathologies) with HPI, exam, labs, radiology, diagnoses and procedures | PhysioNet credentialing, CITI "Data or Specimens Only Research", supervisor reference, DUA for each person; ~1–2 weeks processing (R12) | PhysioNet Credentialed Health Data License 1.5.0. Third-party LLM APIs allowed only with verified zero retention, no training and no human review; local LLMs strongly recommended; no ChatGPT web use (R10, R11) | **Yes / partly**: real data and a closed diagnosis set with guideline treatments; abstention and escalation labels would need to be added | Medium–High (credentialing, compliant compute, data engineering) |
| MIMIC-IV (full) | Full EHR (parent of CDM) | Same as above | Same as above | Partly: the team would have to build its own task and labels | High |
| Published clinical vignettes (journals, guideline case examples) | Small numbers of curated cases | Usually open to read | Copyright varies; reuse needs checking (not researched here) | Yes if the team writes labels against guidelines | Medium (labelling effort) |
| Synthetic vignettes generated by the team | Any size; can be designed to include missing-information, red-flag and out-of-scope cases | None | No PHI; HealthBench (R02) shows synthetic conversations are accepted practice but lists synthetic origin as a limitation | **Yes**: the only option where all five statuses can be planted on purpose with known ground truth | Medium (needs clinician or guideline validation of labels) |

### Inferences
- The MIMIC API barrier has eased: Hager et al. could not test GPT-4 in 2024 (R07), but PhysioNet's 2025 guidance (R11) allows cloud APIs under verified zero-retention, no-training, no-human-review conditions. The burden of proof sits with the researcher, and "if unclear, do not use". For a student team, the safest reading is to use open-weight local models on MIMIC, or to get written confirmation of zero-retention settings with the supervisor's sign-off.
- Every team member who touches MIMIC data probably needs their own credentialing and DUA, since the DUA is signed per user and per dataset (R12). Budget 2–4 weeks at the start of semester 1 for this. This is an inference from the 1–2 week processing time plus the CITI course.
- No surveyed dataset has gold labels for all five statuses. 1.5 (Scope) should therefore state that the test set will be **constructed or relabelled**: a guideline-defined option set, plus deliberately planted missing-information, red-flag (escalate) and out-of-scope (abstain) variants. Labels should be checked by a clinician advisor, or traced to a guideline sentence, if no clinician is available.
- MIMIC-CDM's closed four-diagnosis set fits the user's constraint that the scenario be general rather than single-condition only partly, because it covers one presenting complaint. Present it as one **case-study domain** inside a general framework, not as the framework itself.

### Gaps
- Licences for MedQA, HealthBench and the AgentClinic case files were not verified.
- How AgentClinic-MIMIC-IV cases are distributed, and whether they need credentialing, was not verified.
- The full text of the PhysioNet Credentialed Health Data Use Agreement 1.5.0 was not read; only the policy pages were.
- Whether the 2025 PhysioNet post replaces or supplements the 2023 post is not stated explicitly. Treat the 2025 post as current.

---

## KQ3. Selective prediction / abstention and uncertainty in LLMs (peer-reviewed)

### Takeaway
Abstention is a recognized and still-open research area. The TACL 2025 survey frames it from three angles: the query, the model and human values. In medicine specifically, sample consistency is the best uncertainty proxy tested (ROC AUC 0.66–0.79), and verbalized confidence consistently overestimates confidence. The system's ABSTAIN and NEED_MORE_INFO statuses should therefore not rely on the model's own stated confidence.

### Cited Findings

**R13 — "Know Your Limits: A Survey of Abstention in Large Language Models"**
- Authors: Bingbing Wen, Jihan Yao, Shangbin Feng, Chenjun Xu, Yulia Tsvetkov, Bill Howe, Lucy Lu Wang. **Transactions of the Association for Computational Linguistics, Vol. 13, pp. 529–556 (2025)**. DOI 10.1162/tacl_a_00754. Peer-reviewed; the venue is verified. Verification: ABS (ACL Anthology). — [ACL Anthology](https://aclanthology.org/2025.tacl-1.26); [arXiv](https://arxiv.org/abs/2407.18418)
- Content: presents abstention as a way to reduce hallucination and improve safety. Introduces "a framework to examine abstention from three perspectives: the query, the model, and human values". Reviews methods, benchmarks and metrics, and names abstention as a possible meta-capability across tasks as an open direction. — [ACL Anthology](https://aclanthology.org/2025.tacl-1.26)
- Relevance: the three angles map onto the statuses. Query-side problems (underspecified input) → NEED_MORE_INFO. Model-side limits (low confidence or out of scope) → ABSTAIN. Human-values concerns (safety) → ESCALATE_NOW. Supports 1.2 and 1.4.

**R14 — Savage et al., JAMIA**
- Title: "Large language model uncertainty proxies: discrimination and calibration for medical diagnosis and treatment". Authors: Thomas Savage, John Wang, Robert Gallo, Abdessalem Boukil, Vishwesh Patel, Seyed Amir Ahmad Safavi-Naini, Ali Soroush, Jonathan H. Chen. *JAMIA* 32(1):139–149 (online 2024-10-12; issue 2025-01-01). DOI 10.1093/jamia/ocae254. Peer-reviewed. Verification: ABS (Crossref). — [Crossref](https://api.crossref.org/works/10.1093%2FJAMIA%2FOCAE254)
- Method: compared verbalized confidence, token-level probability and sample consistency (SC) on GPT-3.5, GPT-4, Llama2 and Llama3, using 3 datasets of open-ended patient scenarios for diagnosis and treatment.
- Findings: SC with sentence embeddings reached ROC AUC 0.68–0.79 but was poorly calibrated; SC with GPT annotation reached ROC AUC 0.66–0.74 and was well calibrated. **Verbalized confidence consistently overestimated confidence.** The authors conclude "SC is the most effective method" among those tested. — [Crossref](https://api.crossref.org/works/10.1093%2FJAMIA%2FOCAE254)
- Relevance: grounds the abstention trigger (for example, disagreement across samples or agents) in peer-reviewed medical evidence. It also shows that "ask the model how confident it is" is a weak baseline. Supports 1.4 (abstention quality) and the design rationale.

**R05**: only 1.2% of 519 health-LLM evaluations measured calibration or uncertainty. — [NSF PAR](https://par.nsf.gov/biblio/10616638)

### Inferences
- A multi-agent design gives a natural sample-consistency signal through agreement between agents. That makes R14 a direct justification for using disagreement to trigger ABSTAIN or NEED_MORE_INFO. A fair comparison then needs a single-LLM **self-consistency** baseline with the same number of samples (see KQ5).
- Abstention quality should be measured with selective-prediction style metrics, such as risk at a given coverage or the accuracy of answered cases against the abstention rate. No source verified here specifies such a metric for medical LLMs; see Gaps.

### Gaps
- A peer-reviewed source defining standard abstention metrics (risk–coverage curves, AURC, or "abstention F1" as used in the TACL survey's metric review) was not opened in detail. Read section 5 (Evaluation) of R13 before naming metrics in 1.4.
- Kadavath et al. 2022, "Language Models (Mostly) Know What They Know", is an arXiv preprint and was **not** verified in this pass.
- No medical-domain paper on selective prediction for closed-option decision tasks was verified.

---

## KQ4. Deterministic guardrails, human-in-the-loop oversight, and auditability/traceability

### Takeaway
There is peer-reviewed precedent (Hakim et al., Scientific Reports 2025) for wrapping a medical LLM in rule-based and uncertainty-based guardrails, such as document anomaly detection, checks on drug or adverse-event terms, and uncertainty communication, to eliminate specific "never-event" style errors. Regulatory framing (FDA CDS guidance) requires that clinicians can **independently review the basis** of recommendations, and treats time-critical decisions and automation bias as disqualifying for non-device CDS. Together these support citation-backed outputs, ESCALATE_NOW instead of autonomous action in urgent cases, and audit logs. Auditing frameworks (Mökander et al.) separate governance, model and application audits; this project sits at the application level.

### Cited Findings

**R15 — Hakim et al. (guardrails)**
- Title: "The need for guardrails with large language models in pharmacovigilance and other medical safety critical settings". Authors: Joe B. Hakim, Jeffery L. Painter, Darmendra Ramcharran, Vijay Kara, Greg Powell, Paulina Sobczak, Chiho Sato, Andrew Bate, Andrew Beam. *Scientific Reports*, published 2025-07-01. DOI 10.1038/s41598-025-09138-0. Peer-reviewed. Verification: ABS (arXiv v2 abstract); venue and DOI from a library DOAJ record (SEC). — [arXiv](https://arxiv.org/abs/2407.18322v2); [library record](https://library.kab.ac.ug/Record/doaj-art-1dc80227d455455e99e75d0d73be755a)
- Method: a proof-of-concept guardrail suite around an LLM fine-tuned to turn adverse-event report data (ICSRs) into text. The guardrails (1) detect anomalous documents so inappropriate data is not ingested, (2) identify incorrect drug names or adverse-event terms, and (3) communicate uncertainty. — [arXiv](https://arxiv.org/abs/2407.18322v2)
- Findings: the abstract reports effective application and says the framework helps eliminate key errors such as incorrect pharmacovigilance terms. No numbers appeared in the abstract read.
- Relevance: the closest verified precedent for "deterministic guardrails + LLM" in a medical safety-critical setting. Its pattern of input checks, output-term validation against a controlled vocabulary, and uncertainty reporting matches validating the selected option against the closed guideline set. Supports 1.2 and 1.4.

**R16 — FDA, "Clinical Decision Support Software: Guidance for Industry and Food and Drug Administration Staff"** (Regulatory; US)
- The version fetched was **issued January 29, 2026**, superseding a January 6, 2026 version. The 2022 final guidance it replaces was not separately verified. — [FDA](https://www.fda.gov/media/109618/download)
- Criterion 4 for non-device CDS: software must be "intended for the purpose of enabling such health care professional to independently review the basis for such recommendations … so that it is not the intent that such health care professional rely primarily on any of such recommendations". The guidance recommends plain-language explanation of inputs, the algorithm and validation, and showing what is known and unknown about the patient. — [FDA](https://www.fda.gov/media/109618/download)
- "FDA does not consider software functions intended for a critical, time-sensitive task or decision … to meet Criterion 4." "Automation bias is the propensity of humans to over-rely on a suggestion from an automated system", and it increases in urgent situations. — [FDA](https://www.fda.gov/media/109618/download)
- Relevance: a regulatory basis for (a) citations and a traceable evidence trail (independent review of the basis), (b) routing urgent cases to ESCALATE_NOW instead of giving recommendations, and (c) the "decision support, not decision making" scope statement in 1.5. Saudi regulation was not researched, so present this as international or US reference guidance.

**R17 — Mökander et al., auditing LLMs**
- Title: "Auditing large language models: a three-layered approach". Authors: Jakob Mökander, Jonas Schuett, Hannah Rose Kirk, Luciano Floridi. DOI 10.1007/s43681-023-00289-2. The DOI prefix s43681 belongs to Springer's *AI and Ethics*, but the journal name was not shown on the page read. arXiv 2302.08500, v2 2023-06-27. Verification: ABS. — [arXiv](https://arxiv.org/abs/2302.08500v2)
- Content: three coordinated layers: governance audits (of providers), model audits (before release) and application audits (of LLM-based applications). The authors stress that audits manage some risks but cannot remove them. — [arXiv](https://arxiv.org/abs/2302.08500v2)
- Relevance: locates the project's auditability features (per-decision logs of retrieved evidence, agent messages, rule firings and the final status) at the **application-audit** layer. Supports 1.4 (an auditability objective) and 1.5.

**R07 (supporting)**: LLMs hallucinated non-existent tools every 2–5 patients and made formatting errors every 2–4 patients, which argues for deterministic schema and tool validation. — [Springer](https://link.springer.com/10.1038/s41591-024-03097-1)

**R04 (supporting)**: QUEST lists "Safety and Harm" and "Trust and Confidence" as principles for human evaluation. — [DOAJ](https://doaj.org/article/35d95c2463094fad97d777e1d8376e44)

### Inferences
- Design-to-literature mapping for Chapter 1:
  - Deterministic guardrails (schema and option-set validation, red-flag rules that force ESCALATE_NOW, refusing out-of-scope inputs) ← R15, R07.
  - Abstention/selective prediction ← R13, R14.
  - Human oversight (the clinician stays the decision-maker; no autonomous action on time-critical cases) ← R16.
  - Auditability (traceable basis and citations; logs) ← R16 (independent review of the basis), R17.
- The FDA's automation-bias statement implies that human-in-the-loop alone is **not** a sufficient safety mechanism. This supports layering deterministic checks plus abstention on top of human review, rather than relying on review alone.

### Gaps
- EU AI Act Article 14 (human oversight) and WHO guidance on large multi-modal models (2024) were not opened or verified in this pass. Do not cite them without checking.
- Saudi-specific guidance (SFDA or SDAIA) on AI clinical decision support was not researched.
- Quantitative results from Hakim et al. (full text) were not read.
- No peer-reviewed study was found, in this pass, that quantifies how much rule-based layers reduce unsafe LLM clinical recommendations in a decision-support (not pharmacovigilance) task.

---

## KQ5. Fair comparison methodology: controlling compute and information access in multi-agent vs single-agent comparisons; paired statistical tests

### Takeaway
There is direct peer-reviewed evidence that multi-agent gains can disappear against a strong single agent. Wang et al. (ACL 2024) found a single LLM with strong prompts nearly matches the best multi-agent discussion, and multi-agent discussion only won when there were no demonstrations. Separately, simply sampling more and voting improves performance as the number of "agents" grows (Li et al., preprint), so a multi-agent system that uses more calls must be compared with a single-LLM baseline given the same call or token budget. Kapoor et al. (preprint) argue agent evaluations must report cost together with accuracy. This supports the planned three baselines (bare; same retrieval and tools; compute-matched). For paired significance testing, Dror et al. (ACL 2018) give a protocol for choosing tests. McNemar's test itself was not verified in a methodological source in this pass.

### Cited Findings

**R19 — Wang et al., ACL 2024**
- Title: "Rethinking the Bounds of LLM Reasoning: Are Multi-Agent Discussions the Key?". Authors: Qineng Wang, Zihao Wang, Ying Su, Hanghang Tong, Yangqiu Song. *Proceedings of ACL 2024 (Vol. 1: Long Papers)*, pp. 6106–6131. DOI 10.18653/v1/2024.acl-long.331. Peer-reviewed. Verification: ABS. — [ACL Anthology](https://aclanthology.org/2024.acl-long.331)
- Findings: a single-agent LLM with strong prompts reaches nearly the same best performance as the best existing discussion approach, across many reasoning tasks and backbones. Multi-agent discussion beats a single agent only when the prompt has no demonstrations. — [ACL Anthology](https://aclanthology.org/2024.acl-long.331)
- Relevance: the main justification for requiring strong single-LLM baselines that get the same prompts and information. Supports 1.2 (the open question of whether multi-agent helps) and 1.4 (fair comparison).

**R20 — Li et al., "More Agents Is All You Need"** (Preprint; Tencent authors)
- Authors: Junyou Li, Qin Zhang, Yangbin Yu, Qiang Fu, Deheng Ye. arXiv 2402.05120, 2024-02-03. No journal reference on the page read. Verification: ABS. — [arXiv](https://arxiv.org/abs/2402.05120v1)
- Findings: sampling-and-voting ("Agent Forest") improves performance as the number of agents grows. It can be combined with other methods, and the gain depends on task difficulty. No numbers were given in the abstract. — [arXiv](https://arxiv.org/abs/2402.05120v1)
- Relevance: shows that the number of LLM calls is itself a performance lever. Without a compute-matched baseline (for example, single-LLM self-consistency with N samples), part of a multi-agent gain could be explained by extra compute alone. Supports 1.4 (compute-matched baseline).

**R18 — Kapoor et al., "AI Agents That Matter"** (Preprint, Princeton)
- Authors: Sayash Kapoor, Benedikt Stroebl, Zachary S. Siegel, Nitya Nadgir, Arvind Narayanan. arXiv 2407.01502, v1 2024-07-01. Journal publication was not verified. Verification: ABS. — [arXiv](https://arxiv.org/abs/2407.01502v1)
- Key claims: "there is a narrow focus on accuracy without attention to other metrics"; "SOTA agents are needlessly complex and costly"; they motivate "jointly optimizing the two metrics" (accuracy and cost); holdout sets are often inadequate; and the field lacks standardized, reproducible evaluation. — [arXiv](https://arxiv.org/abs/2407.01502v1)
- Relevance: justifies reporting latency and cost as secondary metrics, and using a held-out test split. Supports 1.4.

**R21 — MDAgents, NeurIPS 2024** (medical multi-agent precedent)
- Title: "MDAgents: An Adaptive Collaboration of LLMs for Medical Decision-Making". Authors: Yubin Kim, Chanwoo Park, Hyewon Jeong, Yik Siu Chan, Xuhai Xu, Daniel McDuff, Hyeonhoon Lee, Marzyeh Ghassemi, Cynthia Breazeal, Hae Won Park. *Advances in NeurIPS 37* (2024). Peer-reviewed. Verification: ABS. — [NeurIPS proceedings](https://proceedings.neurips.cc/paper_files/paper/2024/hash/90d1fc07f46e31387978b88e7e057a31-Abstract.html)
- Findings: assigns a solo or group structure to each case based on its complexity. Best on 7 of 10 medical benchmarks, with gains of up to 4.2% over the previous best (p < 0.05). Group collaboration with moderator review and external knowledge added 11.8% on average. — [NeurIPS](https://proceedings.neurips.cc/paper_files/paper/2024/hash/90d1fc07f46e31387978b88e7e057a31-Abstract.html)
- Relevance: precedent for medical multi-agent decision-making, evaluated on accuracy. The abstract does not report safety, abstention or compute-matched comparisons, which is the gap this project can fill. Supports 1.2.

**R22 — Dror et al., ACL 2018 (statistical significance)**
- Title: "The Hitchhiker's Guide to Testing Statistical Significance in Natural Language Processing". Authors: Rotem Dror, Gili Baumer, Segev Shlomov, Roi Reichart. *Proceedings of ACL 2018 (Vol. 1)*, pp. 1383–1392. DOI 10.18653/v1/P18-1128. Peer-reviewed; outside the 2021–2026 window, but it is the standard methodological reference. Verification: ABS. — [ACL Anthology](https://aclanthology.org/P18-1128/)
- Content: a survey of ACL and TACL 2017 papers found significance testing "often ignored or misused". The paper gives a practical protocol for choosing tests, with a survey of the relevant tests. — [ACL Anthology](https://aclanthology.org/P18-1128/)
- Relevance: supports a statistical-testing plan in 1.4. Whether it names McNemar's test for paired binary outcomes was **not** confirmed from the abstract.

**R01 / R02 (supporting cost reporting)**: MedHELM reports Claude 3.5 Sonnet comparable at about 40% lower estimated cost; HealthBench reports GPT-4.1 nano beating GPT-4o at about 25× lower cost. — [MedHELM](https://arxiv.org/abs/2505.23802); [HealthBench](https://arxiv.org/abs/2505.08775)

### Inferences
- The three planned baselines answer three distinct confounds:
  1. Bare LLM: does the architecture help at all?
  2. Same retrieval and tools: does the agentic structure help beyond information access? (Hager R07 shows the amount and order of information alone can move accuracy by up to 18 points, so information access must be held equal.)
  3. Compute-matched (for example, self-consistency or majority vote with the same number of calls or tokens): R20 and R19 together.
  This mapping can be stated explicitly in 1.4.
- Unsafe-recommendation rate and must-not-miss recall are paired binary outcomes on the same cases, so McNemar's test (or an exact binomial or bootstrap test) is appropriate. The specific citation for McNemar still needs verification (see Gaps).

### Gaps
- Dietterich (1998), "Approximate statistical tests for comparing supervised classification learning algorithms" (*Neural Computation*), is the usual McNemar reference, but it was **not opened or verified**. Verify it, or confirm that Dror et al.'s protocol covers McNemar, before citing.
- Peer-reviewed status of "AI Agents That Matter" and "More Agents Is All You Need" (both possibly published in TMLR) was not verified. Cite them as arXiv preprints.
- "Why Do Multi-Agent LLM Systems Fail?" (Cemri et al., 2025) was not verified in this pass.
- No source was found that prescribes compute-matching specifically for medical multi-agent systems; the argument is assembled from general-domain sources.

---

## KQ6. Claim-to-source list for Chapter 1, and commonly made claims that are NOT supported

### Takeaway
Most of the planned Chapter 1 claims can be tied to verified, mostly peer-reviewed sources. Several claims that often appear in student proposals are either unsupported or contradicted by the sources checked, most notably "multi-agent beats single-agent", "LLMs passing medical exams are ready for clinical use", and "the model's stated confidence can drive abstention". No verified source defines "unsafe recommendation rate", "must-not-miss recall" or "citation precision" as standard clinical-LLM metrics, so the team should present them as **its own operational definitions**, motivated by the sources below.

### Cited Findings

#### Claim-to-source list (supported)
| # | Claim (usable in Chapter 1) | Subsection | Source(s) | Strength |
|---|---|---|---|---|
| C1 | Most LLM health-care evaluations use exam-style QA, rarely use real patient data (5%), and mostly measure accuracy (95.4%) | 1.2 | [R05 Bedi, JAMA](https://par.nsf.gov/biblio/10616638) | Strong (systematic review, 519 studies) |
| C2 | Calibration and uncertainty are rarely evaluated (1.2% of studies) | 1.2, 1.4 | [R05](https://par.nsf.gov/biblio/10616638) | Strong |
| C3 | LLM accuracy drops substantially when moving from static vignettes to interactive conversation (GPT-4 MCQ 0.820 → 0.627; FRQ 0.486 → 0.264) | 1.2 | [R03 CRAFT-MD](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf) | Strong (Nature Medicine) |
| C4 | In sequential clinical environments, MedQA accuracy can fall to below a tenth of the static result | 1.2 | [R09 AgentClinic](https://arxiv.org/abs/2405.07960) | Moderate (preprint) |
| C5 | On real MIMIC cases, LLMs were 16–25 points less accurate than physicians, followed guidelines poorly, and hallucinated tools | 1.2 | [R07 Hager](https://link.springer.com/10.1038/s41591-024-03097-1) | Strong (Nature Medicine; open models from 2023–24 only) |
| C6 | LLM performance is sensitive to the order and amount of information (up to 18 points) | 1.2, 1.4 (hold information access equal) | [R07](https://link.springer.com/10.1038/s41591-024-03097-1) | Strong |
| C7 | Clinical decision support is among the weakest task categories even for frontier LLMs | 1.2 | [R01 MedHELM](https://arxiv.org/abs/2505.23802) | Moderate–strong (numbers differ between preprint and journal) |
| C8 | Recent benchmarks treat emergency referral, context seeking and responding under uncertainty as core behaviours | 1.4 (status labels) | [R02 HealthBench](https://arxiv.org/html/2505.08775v1) | Moderate (industry preprint) |
| C9 | Human evaluation of health LLMs is non-standardized; QUEST offers 5 principles, including Safety and Harm | 1.4 (expert review) | [R04 QUEST](https://doaj.org/article/35d95c2463094fad97d777e1d8376e44) | Strong (review of 142 studies) |
| C10 | Abstention is an established and still-open research problem in LLMs (query, model and human-values perspectives) | 1.2, 1.4 | [R13 Wen et al., TACL 2025](https://aclanthology.org/2025.tacl-1.26) | Strong |
| C11 | In medical diagnosis and treatment, sample consistency is a better uncertainty signal than verbalized confidence, which overestimates | 1.4 (abstention design) | [R14 Savage, JAMIA](https://api.crossref.org/works/10.1093%2FJAMIA%2FOCAE254) | Strong |
| C12 | Rule-based and uncertainty guardrails around LLMs have been proposed to prevent specific critical errors in medical safety-critical settings | 1.2, 1.4 | [R15 Hakim, Sci Rep 2025](https://arxiv.org/abs/2407.18322v2) | Moderate (proof of concept; pharmacovigilance) |
| C13 | CDS should let clinicians independently review the basis for recommendations; time-critical uses and automation bias are concerns | 1.4 (auditability, citations), 1.5 (decision support, not autonomous) | [R16 FDA CDS guidance (Jan 2026)](https://www.fda.gov/media/109618/download) | Strong (regulatory; US) |
| C14 | LLM applications can be audited at the application layer, separate from model and governance audits | 1.4, 1.5 | [R17 Mökander et al.](https://arxiv.org/abs/2302.08500v2) | Moderate |
| C15 | A strong single-agent LLM can nearly match multi-agent discussion, so baselines must be strong | 1.2, 1.4 | [R19 Wang et al., ACL 2024](https://aclanthology.org/2024.acl-long.331) | Strong (general reasoning, not medical) |
| C16 | Performance improves with more sampled calls (sampling-and-voting), so compute must be matched | 1.4 | [R20 Li et al.](https://arxiv.org/abs/2402.05120v1) | Moderate (preprint) |
| C17 | Agent evaluations should report cost with accuracy and use proper holdout sets | 1.4 | [R18 Kapoor et al.](https://arxiv.org/abs/2407.01502v1) | Moderate (preprint) |
| C18 | Medical multi-agent frameworks exist and report accuracy gains (up to 4.2% over the previous best) | 1.2 | [R21 MDAgents, NeurIPS 2024](https://proceedings.neurips.cc/paper_files/paper/2024/hash/90d1fc07f46e31387978b88e7e057a31-Abstract.html) | Strong for the claim made |
| C19 | MIMIC data cannot be used with ChatGPT-style online platforms; cloud APIs only with verified zero retention, no training and no human review; local LLMs strongly recommended | 1.5 | [R10](https://physionet.org/news/post/gpt-responsible-use); [R11](https://physionet.org/news/post/llm-responsible-use/) | Authoritative (data custodian) |
| C20 | MIMIC access needs CITI "Data or Specimens Only Research", a supervisor reference for students, and a DUA for each dataset; about 1–2 weeks processing | 1.5 | [R12](https://mimic.mit.edu/docs/faq/how-to-get-access.html); [R08](https://www.physionet.org/content/mimic-iv-ext-cdm/) | Authoritative |
| C21 | Significance testing in NLP is often ignored or misused; a protocol for choosing tests exists | 1.4 | [R22 Dror et al., ACL 2018](https://aclanthology.org/P18-1128/) | Strong (pre-2021) |

#### Commonly made claims NOT supported (or contradicted) by the verified sources
- **"Multi-agent LLM systems outperform single LLMs."** Not generally supported. R19 found that a strong single agent nearly matches the best multi-agent discussion, and R21's medical gains are modest (up to 4.2%) and not compute-matched in the abstract. Frame it as an open hypothesis the project tests. — [R19](https://aclanthology.org/2024.acl-long.331); [R21](https://proceedings.neurips.cc/paper_files/paper/2024/hash/90d1fc07f46e31387978b88e7e057a31-Abstract.html)
- **"LLMs pass medical licensing exams, so they are ready for clinical decision support."** Contradicted by R03, R07, R09 and R05. — [R03](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf); [R07](https://link.springer.com/10.1038/s41591-024-03097-1)
- **"An LLM's self-reported confidence can be used to decide when to abstain."** Contradicted: verbalized confidence consistently overestimates. — [R14](https://api.crossref.org/works/10.1093%2FJAMIA%2FOCAE254)
- **"Guardrails eliminate hallucinations."** Not supported. R15 claims to eliminate *specific* key errors in a pharmacovigilance proof of concept, not hallucination in general. — [R15](https://arxiv.org/abs/2407.18322v2)
- **"Keeping a human in the loop makes the system safe."** Not supported as a standalone claim. The FDA notes that automation bias causes over-reliance, especially under time pressure. — [R16](https://www.fda.gov/media/109618/download)
- **"LLM-as-judge is as reliable as clinicians."** Only partly supported. MedHELM's jury ICC of 0.47 is moderate, though above clinician–clinician agreement of 0.43; CRAFT-MD's grader agreed with experts 93.35% of the time on diagnosis matching only. Present model grading as an approximation backed by expert spot checks. — [R01](https://arxiv.org/abs/2505.23802); [R03](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- **"We can use MIMIC with GPT/ChatGPT."** Not supported as stated: prohibited on ChatGPT-style platforms, and conditional for APIs. — [R11](https://physionet.org/news/post/llm-responsible-use/)
- **"Unsafe recommendation rate / must-not-miss recall / citation precision are standard clinical-LLM metrics."** No verified source in this pass defines them as standard. They are defensible operational definitions motivated by C1–C3, C8 and C13, and should be presented as the project's own definitions.
- **"Synthetic vignettes are equivalent to real patient data."** Not supported. HealthBench lists synthetic generation as a limitation, and R05 criticizes the lack of real-data evaluation. — [R02](https://arxiv.org/html/2505.08775v1); [R05](https://par.nsf.gov/biblio/10616638)

### Inferences
- The most defensible problem statement (1.2) combines three points: (i) evaluation gap (C1–C4, C7); (ii) safety and uncertainty gap (C2, C10, C11); (iii) the unresolved question of whether multi-agent helps once compute and information are controlled (C15–C18).
- Objectives (1.4) can be written as measurable comparisons against three baselines, with metrics tied to C8, C11, C13 and C17 and significance testing tied to C21 (pending the McNemar citation).
- Scope (1.5) should state: decision **support** with the clinician as final decision-maker (C13); a closed, guideline-defined option set (motivated by the free-response collapse in C3); a constructed or relabelled test set (KQ2 inference); MIMIC only under the C19/C20 conditions; and no claims about real-world clinical outcomes.

### Gaps
- McNemar's test citation (Dietterich 1998, or another) is unverified.
- EU AI Act Article 14, WHO LMM guidance (2024), and Saudi SFDA/SDAIA guidance are unverified or unresearched.
- Full texts of the MedHELM journal version, QUEST, Bedi et al., and Hakim et al. were not read; their numbers come from abstracts or record pages.
- Licences and release status of MedQA, HealthBench, CRAFT-MD and AgentClinic data are unverified.

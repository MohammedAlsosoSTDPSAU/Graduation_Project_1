# Classical CDSS and single-LLM approaches to clinical decision support: evidence base for Chapter 2.1 / 2.2 (theme: single_llm_cds)

Scope and method note for the report writer:
- **Session.** Searched and fetched on 2026-11-27 under `_protocol.md`. Every study is logged in `single_llm_cds_studies.jsonl`, and every search or fetch in `single_llm_cds_searchlog.jsonl`.
- **Reused notes.** The verified notes from 2026-11-23 (`cdss_foundations_and_guidance.md`, `llm_clinical_reasoning_limits.md`) were reused only where marked "(prior session)". Where this session re-checked a record, the new detail is given.
- **Verification labels follow the protocol:**
  - FULL: full text read.
  - PARTIAL: substantial parts read.
  - ABSTRACT: abstract or record only.
  - SECONDARY: via another source only.
- **Access limits.** PubMed and PMC returned a reCAPTCHA wall for most pages. The bash sandbox had no network, so Crossref, OpenAlex and Semantic Scholar API checks could not be run. A batched OpenAlex URL was also refused because it was over 250 characters. Metadata was therefore checked on publisher pages, institutional repositories (TUM, EUR, eScholarship), RePEc/IDEAS, AHRQ PSNet, NSF PAR, arXiv, and the ACM FAccT proceedings page. The JSONL records which one was used for each study.
- **Extraction caveat.** The fetch tool returns a model-made extraction of each page. Re-check numbers that will appear in tables or in the abstract against the PDF.

---

## Q1. What do seminal and recent systematic reviews conclude about CDSS effectiveness and failure modes, and which original studies underpin those claims?

### Takeaway
Across 20 years, the controlled-trial evidence is consistent:
- **Process of care:** computerised CDSS usually improve it, by a small, highly variable amount (a pooled +5.8 percentage points of patients receiving recommended care in Kwan 2020).
- **Patient outcomes:** effects are rarely shown (7 of 52 trials in Garg 2005; a median +0.3% on clinical targets in Kwan 2020).
- **Success factors:** effectiveness depends on workflow integration, automatic prompting and actionable recommendations.
- **Failure modes:** documented failure modes are alert overload and override (49–96% of drug alerts overridden), automation bias, and poor real-world validity of ML models (Epic Sepsis Model AUC 0.63 on external validation, vs 0.76–0.83 claimed by the developer).

### Cited Findings

**Seminal effectiveness reviews (knowledge-based CDSS)**
- **Garg et al. 2005** (JAMA 293(10):1223–1238; DOI 10.1001/jama.293.10.1223). Systematic review of 100 controlled trials; abstract and results read from a PDF copy. — [Garg 2005 PDF (Univ. Alberta copy)](https://sites.ualberta.ca/~dcl3/ABCDreview/SRs_immunizations/Journal%20of%20the%20American%20Medical%20Association_Garg_Effects%20of%20computerized%20clinical%20decision_2005.pdf); [PSNet record](https://psnet.ahrq.gov/issue/effects-computerized-clinical-decision-support-systems-practitioner-performance-and-patient)
  - Practitioner performance improved in 62 of 97 trials (64%).
  - By system type: diagnostic systems 4/10 (40%), reminder systems 16/21 (76%), disease management 23/37 (62%), drug dosing and prescribing 19/29 (66%).
  - Patient outcomes improved in 7 of 52 trials that assessed them (13%).
  - Automatic prompting succeeded in 73% (44/60) of trials vs 47% (17/36) when the user had to start the system (P = .02).
  - Trials run by the system's own developers succeeded in 74% (51/69) vs 28% (5/18) otherwise (P = .001). This is a developer-evaluation bias signal.
- **Kawamoto et al. 2005** (BMJ 330(7494):765). Systematic review of 70 RCTs. — [PSNet record](https://psnet.ahrq.gov/issue/improving-clinical-practice-using-clinical-decision-support-systems-systematic-review-trials)
  - CDSS improved clinical practice in "nearly 70%" of trials.
  - Features independently associated with success: automatic provision as part of the clinical workflow; recommendations rather than only assessments; support at the time and place of the decision; computer-based delivery.
  - Only the PSNet summary was read; PMC was captcha-blocked. The often-quoted "94% success when all features are present" figure was NOT verified.
- **Kwan et al. 2020** (BMJ 370:m3216; DOI 10.1136/bmj.m3216). Meta-analysis of controlled trials of CDSS embedded in clinical systems. Medline searched to August 2019. — [BMJ](https://www.bmj.com/content/370/bmj.m3216)
  - **Size:** the abstract reports 108 studies, 122 analysable trials, 1,203,053 patients and 10,790 providers. The Results section says 115 studies and 129 trials. This inconsistency is in the source.
  - **Pooled effect:** absolute improvement in patients receiving desired care of 5.8% (95% CI 4.0–7.6%), with I² = 76%. The control-group median adherence was 39.4%. The top quartile of trials improved by 10–62%.
  - **By behaviour:** prescribing +4.4%, test ordering +6.8%, documentation +7.1%, vaccination +5.9%.
  - **Clinical endpoints** (30 trials): median improvement in patients reaching guideline targets of 0.3% (IQR −0.7% to 1.9%).
  - **Design features (univariate):** requiring a documented reason for non-adherence +4.8% (95% CI 0.1–9.6). The ability to execute the recommended action through the CDSS +4.4% (0.9–7.9).
  - **Setting:** paediatric settings +13.6% in the multivariable model.
  - **Alert fatigue:** considering it in the design gave +1.7%, not significant (−3.5 to 6.8). Only about 17–19 of 122–129 trials considered alert fatigue at all.
  - **Authors' conclusion:** achieving worthwhile improvements "remains largely a case of trial and error". Small process gains with a presumed link to outcomes "may not justify" the risk of alert fatigue.
- **Sutton et al. 2020** (npj Digit Med 3:17; DOI 10.1038/s41746-020-0221-y). Narrative review (prior session, FULL). — [Springer PDF](https://link.springer.com/content/pdf/10.1038/s41746-020-0221-y.pdf)
  - Knowledge-based (IF-THEN) vs non-knowledge-based (ML) taxonomy.
  - Lists the main failure modes: fragmented workflows, alert fatigue (up to 95% of alerts inconsequential), automation bias and impact on skill, maintenance burden, poor data quality, poor interoperability, cost and user distrust.

**Failure mode 1: alert overload and override**
- **van der Sijs et al. 2006** (JAMIA 13(2):138–147; DOI 10.1197/jamia.M1809). Literature review of 17 papers. — [Erasmus University repository record](https://repub.eur.nl/pub/60465)
  - Clinicians overrode drug safety alerts in 49% to 96% of cases. This closes the prior session's gap; the figure is now verified at abstract level.
  - Overriding is often justified, and "appropriate" alerts should be distinguished from "useful" ones.
  - Error-producing conditions include low sensitivity and specificity, unclear information content and unnecessary workflow disruption.

**Failure mode 2: automation bias**
- **Goddard, Roudsari & Wyatt** (JAMIA; online 16 June 2011, PMID 21685142; the OUP URL indicates vol. 19, issue 1, p. 121, i.e. the 2012 print issue). Systematic review. — [WUSTL journal-club record](https://journalclub.wustl.edu/?p=11051)
  - 13,821 papers retrieved, 74 included.
  - Mediators: cognitive style, task-specific experience, trust and confidence, workload, task complexity and time pressure.
  - Mitigators: training, stressing user accountability, on-screen position of advice, updated confidence levels on output, and giving information rather than a recommendation.
  - The abstract gives no pooled effect sizes, so do not quote a numeric automation-bias rate from this source.

**Failure mode 3: ML-based CDSS that do not hold up on real data**
- **Wong et al. 2021** (JAMA Intern Med 181(8); DOI 10.1001/jamainternmed.2021.2626). External validation of the proprietary Epic Sepsis Model (ESM). — [PMC8218233](https://pmc.ncbi.nlm.nih.gov/articles/PMC8218233)
  - Setting: Michigan Medicine, 27,697 patients and 38,455 hospitalizations; sepsis in 7%.
  - Hospitalization-level AUC was 0.63 (95% CI 0.62–0.64). Epic's internal documentation reported 0.76–0.83.
  - At score ≥6: sensitivity 33%, specificity 83%, PPV 12%.
  - The ESM missed 67% (1,709/2,552) of sepsis cases. It alerted on 18% of all hospitalizations.

### Inferences
- The classical CDSS literature fixes three design lessons that any LLM CDS should be judged against:
  1. Integration and automatic, actionable support at the point of decision (Kawamoto, Garg, Kwan).
  2. Process gains do not equal outcome gains (Garg 13%; Kwan +0.3% on clinical targets).
  3. Developer-run evaluations overstate benefit (Garg 74% vs 28%), and so do internally reported ML metrics (ESM 0.76–0.83 vs 0.63).

  Lesson 3 maps directly to P4 and RQ4: independent, matched-baseline evaluation.
- Override data (49–96%) and Kwan's "requiring a reason for non-adherence" finding together argue for a small number of high-specificity outputs. They also argue for showing reasons rather than issuing frequent alerts. This is relevant to how many cases end up as ESCALATE_NOW or RECOMMEND_WITH_CAVEATS (RQ1: unnecessary escalation is a cost, not a free safety margin).
- Goddard's mitigator "information rather than recommendation" and "updated confidence levels" fit a non-directive status output. This is an inference, not a tested claim for LLM CDS.

### Gaps
- Kawamoto 2005 full text (odds ratios, and whether the "94%" figure exists) was not readable because PMC was captcha-blocked.
- Bright et al. 2012 (Ann Intern Med, AHRQ CDSS review) and Lyell & Coiera 2017 (automation bias and verification complexity) were not opened, for lack of budget. Add them only after reading.
- Goddard's numeric automation-bias findings (for example, the rate of switching from correct to incorrect answers after wrong advice) are in the full text, which was not accessible.

---

## Q2. How do single LLMs perform on realistic clinical decision tasks, and under which conditions does performance drop?

### Takeaway
Single LLMs score at or above physician level on curated exam questions and case-report diagnosis: Medprompt-GPT-4 90.2% on MedQA, AMIE top-10 differential diagnosis 59.1% vs 33.6% for clinicians. Performance drops sharply when:
- the model must gather information itself (CRAFT-MD GPT-4 0.820 → 0.627; MIMIC-CDM about −11 to −13 points; AgentClinic to below a tenth of the original at worst);
- it works from real EHR notes (GPT-4-turbo 8% below a resident on average across 3 ED decisions; admission accuracy 0.43–0.58 vs 0.83);
- it must act in an EHR (MedAgentBench best model 69.67% success; action tasks 54%);
- it must follow treatment guidelines (MIMIC-CDM).

Physician-plus-LLM trials are mixed. There was no gain in diagnostic reasoning (Goh 2024, +2 points, P = .60) but a gain in management reasoning (Goh 2025, +6.5 points, P < .001). In both trials the LLM alone scored about the same as or higher than the physician arms.

### Cited Findings

**Exam-style and curated-case performance (the "ceiling")**
- **Singhal et al. 2023** (Nature 620:172–180; prior session FULL). — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
  - Flan-PaLM reached 67.6% on MedQA.
  - Human evaluation (140 questions): Med-PaLM still gave incorrect or inappropriate content in 18.7% of long-form answers vs 1.4% for clinicians.
- **Singhal et al. 2025, Med-PaLM 2** (Nat Med; DOI 10.1038/s41591-024-03423-7; prior session ABSTRACT). — [arXiv 2305.09617](https://arxiv.org/abs/2305.09617)
  - Up to 86.5% on MedQA.
  - Answers preferred over physicians' on 8 of 9 axes (1,066 questions).
- **Nori et al. 2023, Medprompt** (arXiv 2311.16452; preprint). — [arXiv HTML](https://arxiv.org/html/2311.16452v1)
  - Generalist GPT-4 with prompting only scored 90.2% on MedQA (90.6% in the maximal setting) vs 86.5% for the fine-tuned Med-PaLM 2. The GPT-4 5-shot baseline was 81.4%.
  - The authors themselves caution that benchmark contamination cannot be ruled out, that MCQs do not capture real clinical tasks, and that results do not show clinical efficacy.
- **McDuff et al. 2025, AMIE differential diagnosis** (Nature 642(8067):451–457; prior session ABSTRACT). — [RePEc](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08869-4.html)
  - 302 NEJM CPC cases, top-10 accuracy: AMIE alone 59.1% vs unassisted clinicians 33.6%.
  - Clinicians assisted by AMIE reached 51.7%, below AMIE alone.
- **Tu et al. 2025, conversational AMIE** (Nature 642(8067):442–450; DOI 10.1038/s41586-025-08866-7). Randomized, double-blind crossover remote-OSCE study with patient actors. — [RePEc record](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08866-7.html); [arXiv v1](https://arxiv.org/html/2401.05654v1)
  - **Published version:** 159 scenarios from Canada, the UK and India; 20 PCPs. AMIE had greater diagnostic accuracy and was rated superior on 30 of 32 axes by specialists and 25 of 26 by patient actors.
  - **Version mismatch:** arXiv v1 reported 149 scenario packs, 28/32 and 24/26. Cite the Nature numbers.
  - **Method:** AMIE is a PaLM-2-based fine-tuned model trained with self-play (simulated patient, doctor and critic agents) and a 3-step chain-of-reasoning at inference.
  - **Limits:** the synchronous text chat was unfamiliar to PCPs; psychiatry, paediatrics and inpatient cases were excluded; patient actors may have given unusually structured histories.

**Realistic decision tasks: where performance drops**
- **Hager et al. 2024, MIMIC-CDM** (Nat Med 30(9):2613–2622; DOI 10.1038/s41591-024-03097-1; volume and pages verified this session on the TUM portal; full-text findings from the prior session). — [TUM portal](https://portal.fis.tum.de/en/publications/evaluation-and-mitigation-of-the-limitations-of-large-language-mo/); [accepted manuscript](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
  - **Dataset:** 2,400 real ED abdominal-pain cases (MIMIC-IV).
  - **Information gathering:** accuracy fell when Llama-2-70B-based models had to gather information autonomously: Llama 2 Chat 58.8% → 45.5%, OASST 67.8% → 54.9%, WizardLM 65.1% → 53.9%.
  - **Vs physicians:** models were significantly worse than physicians (P < 0.001).
  - **Guidelines and tools:** models did not follow treatment guidelines, made action errors every 2–4 patients, and hallucinated tools every 2–5 patients.
  - **Medical fine-tuning:** the medically fine-tuned Clinical Camel and Meditron could not complete the autonomous task.
  - **Scope:** only open Llama-2-era models were tested (MIMIC data-use rules).
- **Johri et al. 2025, CRAFT-MD** (Nat Med; DOI 10.1038/s41591-024-03328-5; prior session FULL). — [full-text copy](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
  - **Design:** 2,000 vignettes, 5 repeats each.
  - **Vignette vs multi-turn conversation:** GPT-4 4-option accuracy 0.820 → 0.627; free-response 0.486 → 0.264.
  - **Summarization:** summarizing the conversation partly recovered accuracy (0.669).
  - **History-taking:** GPT-4 gathered complete relevant history in 71.11% of expert-rated conversations.
- **Li et al. 2024, MediQ** (NeurIPS 2024 poster; prior session ABSTRACT). — [arXiv](https://arxiv.org/abs/2406.00922)
  - Naively prompting LLMs to ask questions degraded performance.
  - Abstention strategies improved diagnostic accuracy by 22.3% but stayed below the full-information upper bound.
- **Schmidgall et al., AgentClinic** (arXiv 2405.07960 v5, 25 May 2025; preprint; no venue listed on arXiv). — [arXiv](https://arxiv.org/abs/2405.07960)
  - Converting MedQA to sequential decision-making with patient interaction and incomplete information cut diagnostic accuracy "to below a tenth of the original" at worst.
  - Claude-3.5 agents were best in most settings.
  - Tool use differed sharply: Llama-3 gained up to 92% relative improvement with a notebook tool.
  - 9 specialties, 7 languages. Only the abstract was read.
- **Williams et al. 2024** (Nat Commun 15:8236; DOI 10.1038/s41467-024-52415-1). Real EHR data, FULL. — [eScholarship PDF](https://escholarship.org/content/qt67p669r1/qt67p669r1.pdf)
  - **Data:** UCSF ED notes (251,401 adult visits, 2012–2023). Models saw only the presenting history and physical examination.
  - **Tasks:** admission, radiology request and antibiotic prescription, judged against actual decisions.
  - **Unbalanced 1,000-visit sample, accuracy:**

    | Task | Resident | GPT-4-turbo | GPT-3.5-turbo |
    |---|---|---|---|
    | Admission | 0.83 | 0.43–0.58 | 0.29–0.53 |
    | Radiology | 0.79 | 0.74 | 0.68–0.71 |
    | Antibiotics | 0.78 | 0.83 | 0.35–0.43 |

  - **Overall:** GPT-4-turbo averaged 8% below the resident and GPT-3.5-turbo 24% below. The models were "overly cautious", recommending interventions and producing many false positives.
  - **Prompting:** the four zero-shot prompts, including chain-of-thought, improved results only "marginally".
  - **Limits:** a single resident as comparator; actual decisions used as ground truth.
- **Jiang et al. 2025, MedAgentBench** (NEJM AI; DOI 10.1056/AIdbp2500144 per Stanford HAI; arXiv 2501.14654 v2 read). — [arXiv HTML v2](https://arxiv.org/html/2501.14654v2); [Stanford HAI](https://hai.stanford.edu/news/stanford-develops-real-world-benchmarks-for-healthcare-ai-agents)
  - **Environment:** a FHIR-compliant virtual EHR with 100 Stanford-derived patient profiles (>700,000 data elements) and 300 physician-written tasks (150 query, 150 action).
  - **Agent setup:** a single function-calling agent with 9 FHIR functions, at most 8 rounds, temperature 0, pass@1.
  - **Success rates (overall / query / action):**
    - Claude 3.5 Sonnet v2: 69.67% / 85.33% / 54.00%
    - GPT-4o: 64.00%
    - DeepSeek-V3: 62.67%
    - Gemini 1.5 Pro: 62.00%
    - o3-mini: 51.67%
    - Llama 3.3 70B: 46.33%
    - Mistral 7B: 4.00%
  - **Errors:** mostly output-format and malformed-request errors. Gemini 2.0 Flash output invalid actions in 54% of cases.
  - **Unmeasured:** reliability over repeated runs was not tested.
- **Korom et al. 2025, Penda Health "AI Consult"** (arXiv 2507.16947 v1; preprint). An LLM-based CDS deployed in real primary care. — [arXiv](https://arxiv.org/abs/2507.16947)
  - **Design:** a quality-improvement study across 15 Nairobi clinics and 39,849 visits, comparing clinicians with vs without AI Consult.
  - **Results:** 16% relative reduction in diagnostic errors and 13% in treatment errors, rated by independent physicians.
  - **Gaps in the record:** randomization method, absolute rates, patient outcomes and the model used are not stated in the record read.
  - **Conflict of interest:** the author list includes Karan Singhal and Johannes Heidecke. Their OpenAI affiliation was NOT confirmed from the page and should be checked.

**Physician + LLM randomized trials**
- **Goh et al. 2024** (JAMA Netw Open 7(10):e2440969). Diagnostic reasoning, ABSTRACT. — [UMN record](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/)
  - 50 physicians: median score 76% with GPT-4 vs 74% with conventional resources (+2 points, 95% CI −4 to 8, P = .60).
  - Time −82 s/case, not significant.
  - GPT-4 alone was +16 points vs the conventional arm (95% CI 2–30, P = .03; exploratory).
- **Goh et al. 2025** (Nat Med; DOI 10.1038/s41591-024-03456-y). Management reasoning, FULL author copy. — [PDF](https://www.erichorvitz.com/GPT-4-management_reasoning_Nature_Med.pdf)
  - **Design:** 92 physicians, 5 vignettes based on real cases.
  - **Physicians + GPT-4 vs conventional resources:** 43.0% vs 35.7% (difference 6.5, 95% CI 2.7–10.2, P < 0.001).
  - **GPT-4 alone:** 43.7%, no different from physicians + GPT-4 (P = 0.80; the sign of the CI is inconsistent between the abstract and the results).
  - **Time:** +119.3 s/case (P = 0.022).
  - **Harm (exploratory, single reviewer):** similar between arms. High-likelihood harm was 4.2% vs 2.9%.
  - **Limits stated by the authors:** wrong answers were not penalised; prompting was not optimised.

**Meta-evidence on how LLMs are evaluated (P4)**
- **Bedi et al. 2025** (JAMA 333(4):319 per the NSF PAR record; preprint full text read). — [NSF PAR record](https://par.nsf.gov/biblio/10616638); [medRxiv v2](https://medrxiv.org/content/10.1101/2024.04.15.24305869v2.full.pdf)
  - **Scope:** 519 studies published 1 Jan 2022 to 19 Feb 2024 (PubMed and Web of Science).
  - **Data used:** only 5% used real patient care data.
  - **Tasks:** medical knowledge, such as exam questions (44.5%), diagnosis (19.5%), treatment recommendations (9.2%).
  - **Dimensions:** accuracy (95.4%), fairness/bias/toxicity (15.8%), deployment considerations (4.6%), calibration/uncertainty (1.2%).

### Comparative summary for 2.2 (methods, data, metrics, results)

| Study | Setting / data | Single-LLM method | Metric | Key result |
|---|---|---|---|---|
| Nori 2023 (preprint) | MedQA MCQ | GPT-4 + dynamic few-shot + self-CoT + choice-shuffle ensemble | Accuracy | 90.2% (vs Med-PaLM 2 86.5%) |
| Savage 2024 | MedQA free-response (518), NEJM cases | GPT-3.5/GPT-4, 5 CoT prompt styles | Accuracy | GPT-4 72–78%; no diagnostic prompt beat CoT significantly |
| Hager 2024 | MIMIC-CDM, 2,400 real ED cases | Llama-2-70B-based agent with tools | Diagnostic accuracy, guideline adherence | −11 to −13 points when gathering info; guideline non-adherence |
| Johri 2025 | 2,000 vignettes → simulated dialogue | GPT-4 etc. as doctor agent | Accuracy | 0.820 → 0.627 (MCQ) |
| Williams 2024 | 251k UCSF ED visits (real notes) | GPT-3.5/4-turbo zero-shot, CoT | Accuracy vs resident | GPT-4 −8% average; over-recommends |
| Jiang 2025 | FHIR virtual EHR, 300 tasks | Function-calling agent | Success rate | best 69.67%; action 54% |
| Tu 2025 | 159 OSCE scenarios, patient actors | AMIE (fine-tuned, self-play, 3-step inference) | Specialist / actor ratings | 30/32 and 25/26 axes superior |
| Goh 2025 | 92 physicians, 5 cases | GPT-4 chat assistant | Rubric score | +6.5 points with GPT-4; GPT-4 alone ≈ assisted |
| Korom 2025 (preprint) | 39,849 real visits, Kenya | LLM error-flagging copilot | Physician-rated errors | −16% diagnostic, −13% treatment errors (relative) |

### Inferences
- The pattern across studies is consistent. Single LLMs are strong when the case is complete, curated and diagnosis-only. They degrade when they must:
  1. decide what information to collect (Hager, CRAFT-MD, AgentClinic, MediQ);
  2. map a real, noisy note to an action threshold (Williams);
  3. execute structured actions (MedAgentBench action tasks 54%; Hager tool errors).

  This is the P1 claim, and it is supported by both peer-reviewed real-data studies (Hager, Williams) and benchmarks.
- **Physicians and LLMs.** Physician-plus-LLM results do not beat LLM-alone in any of the three trials read (Goh 2024, Goh 2025, McDuff 2025). This is a caution for the project's "physician decides" design. Human oversight is a safety and regulatory requirement, but the evidence does not show it improves accuracy over the model alone. The project should not claim that it does.
- **Error direction.** Williams' "overly cautious" pattern (false-positive admission, imaging and antibiotics) is the LLM analogue of alert over-firing. RQ1 should measure unnecessary escalation as a harm, as the protocol already plans.
- **Real-data deployments.** The only real-world deployment evidence for an LLM CDS (Korom 2025) uses a single-model, error-flagging, non-directive design. It is an unrandomised preprint with industry authorship.

### Gaps
- No study read tests GPT-4o-class or reasoning models on MIMIC-CDM itself. MedAgentBench tested o3-mini, which scored below GPT-4o (51.67% vs 64.00%), so "reasoning models fix this" is not supported by what was read.
- The exact Hager Fig. 2 physician vs LLM per-pathology values were not captured (prior-session gap remains).
- AgentClinic full results (bias perturbations, the real EHR subset, human ratings) were not read; there is no venue for it beyond arXiv.
- Korom 2025: absolute error rates, allocation method, model identity and conflicts of interest need the full PDF.

---

## Q3. What evidence exists on order/wording perturbation sensitivity and run-to-run inconsistency in clinical LLM outputs (RQ3)?

### Takeaway
There is direct, quantified evidence that clinically irrelevant changes alter LLM clinical outputs:
- **Order and wording:** reordering information changed MIMIC-CDM accuracy by up to 18 points, and rewording by up to about 16 points.
- **Non-clinical edits:** typos, whitespace, tone and gender changes shifted treatment recommendations by about 7% (self-management) and lowered conversational diagnostic accuracy by about 7 points. At temperature 0, about 24% of recommendations flipped on average across perturbations.
- **Demographic labels:** these changed triage and testing across 1.7 million outputs. Only secondary detail was verified for that study.
- **Adversarial edits:** these flip MCQ answers, and the model's explanation often does not mention the inserted cue.

Evidence on run-to-run inconsistency of clinical decisions is thinner and mostly qualitative or methodological (Omiye 2023; Shyr et al. framework preprint). No study read reports a clean "decision flip rate across repeated identical runs" for a clinical CDS task.

### Cited Findings
- **Hager 2024, order and wording** (prior session FULL). — [Hager accepted manuscript](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
  - Reordering the same diagnostic information changed accuracy by up to 18.0% (Fig. 6).
  - Small wording changes moved accuracy by up to about 16 points (Extended Data Fig. 7).
  - Adding more information sometimes lowered accuracy (Fig. 5).
- **Gourabathina, Gerych, Pan & Ghassemi 2025**, "The Medium is the Message" (ACM FAccT '25; DOI 10.1145/3715275.3732121). FULL text from the proceedings HTML. — [FAccT proceedings HTML](https://camps.aptaracorp.com/ACM_PMS/PMS/ACM/FACCT25/120/bb50c1a8-2ab6-11f0-ada9-16bb50361d1f/OUT/facct25-120.html)
  - **Setup:** GPT-4, Llama-3-70B, Llama-3-8B and Palmyra-Med were tested on OncQA, r/AskaDocs and CRAFT-MD USMLE/Derm.
  - **Perturbations:** gender swap or removal; uncertain or "colorful" tone; and syntactic edits (exclamation, typo, whitespace, case changes).
  - **MANAGE question (self-management):** treatment shift rate 7.31 ± 2.38%, reduced-care rate 4.55 ± 1.98%, reduced-care error rate 4.12 ± 1.27%. The "colorful" tone had the largest effect.
  - **Temperature 0:** about 24% of treatment recommendations flipped on average across perturbations.
  - **Gender:** after whitespace insertion, female patients had about 7% more errors than male patients (p < 0.005).
  - **Conversational setting:** diagnostic accuracy fell about 6.9–7.6 points under perturbation across vignette, single-turn, multi-turn and summarized formats.
  - **Limits:** perturbations were LLM-generated, and outputs were annotated by Llama-3-8B, not humans.
- **Omar et al. 2025**, sociodemographic biases (Nat Med; DOI 10.1038/s41591-025-03626-6). SECONDARY: institutional press release only. — [Mount Sinai press release](https://www.mountsinai.org/about/newsroom/2025/is-ai-in-medicine-playing-fair)
  - **Design:** 9 LLMs, 1,000 ED cases × 32 sociodemographic profiles, more than 1.7 million outputs.
  - **Findings:** decisions on triage, testing, treatment and mental-health evaluation changed with the label alone. High-income labels drew more advanced imaging.
  - No numbers beyond the design are verified; read the paper before quoting effect sizes.
- **Ness et al. 2024, MedFuzz** (arXiv 2406.06573 v2; preprint; Microsoft Research, MIT, JHU). PARTIAL. — [arXiv HTML](https://arxiv.org/html/2406.06573v2)
  - **Method:** an attacker LLM rewrites MedQA items (1,181 US items) by adding patient characteristics that appeal to stereotypes, without changing the correct answer.
  - **Robustness:** GPT-4 and Claude 3.5 stay near human-comparable accuracy after 5 attacks. Exact numbers are only in Fig. 2 and were not extracted.
  - **Faithfulness:** for GPT-3.5 and GPT-4, about 10–20% of successful attacks had chain-of-thought explanations that omitted all inserted information. In most cases explanations failed to mention at least one inserted cue.
- **Omiye et al. 2023** (npj Digit Med 6:195; prior session FULL). — [Nature PDF](https://www.nature.com/articles/s41746-023-00939-z.pdf)
  - 9 questions × 5 runs × 4 commercial LLMs.
  - "The concerning outputs do not always appear in all five runs of the same question": qualitative evidence of run-to-run inconsistency.
- **Shyr et al.**, "A statistical framework for evaluating the repeatability and reproducibility of large language models" (medRxiv 10.1101/2025.08.06.25333170; listed as 2026; a PMC record PMC12637745 appears in search, but the journal version was NOT confirmed). PARTIAL. — [medRxiv full text](https://www.medrxiv.org/content/10.1101/2025.08.06.25333170.full.pdf)
  - **Setup:** ChatGPT-4, GPT-4o-mini and LLaMA 3.2-1B; 518 free-response MedQA vignettes and 90 rare-disease (UDN) cases; 5 CoT prompts; 100 runs per combination (912,000 generations, temperature 0.5).
  - **Findings:** repeatability and reproducibility depend jointly on model, prompt and dataset. Consistency and accuracy are distinct properties. For GPT-4 on USMLE, repeatability generally did not differ between correct and incorrect diagnoses.
  - **What it lacks:** an answer-flip proportion. It uses embedding similarity and entropy metrics.
- **Johri 2025, CRAFT-MD** (prior session FULL). — [full text](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
  - Each conversation was repeated 5 times. The paper also shows performance changing across model versions (Mistral v1 vs v2) and recommends continuous monitoring.
- **Nori 2023, Medprompt.** — [arXiv HTML](https://arxiv.org/html/2311.16452v1)
  - Choice-shuffle ensembling (shuffling answer-option order over 5 calls with a majority vote) added +2.2% on MedQA in the ablation. This implies single-call answers are sensitive to option order. The order sensitivity itself is not reported separately in what was read.

### Inferences
- RQ3 is well-motivated. At least three independent peer-reviewed studies (Hager, Gourabathina, Omar), plus two preprints (MedFuzz, Shyr), show that clinically irrelevant input changes or repeated sampling alter outputs. The project's design choice to extract structured facts before reasoning and rank deterministically targets exactly this failure. Whether it helps is the empirical question RQ3 must answer against a matched single-agent baseline.
- **Faithfulness.** MedFuzz's finding that explanations often omit the cue that changed the answer means that self-explanations are not reliable evidence of why a status changed. This supports claim-to-source verification (P3) over rationale inspection. Savage 2024 also found 65% of incorrect GPT-4 rationales contained false logic, vs 18% of correct ones.
- **Metrics.** A suitable RQ3 metric set, inferred from the methods read:
  1. status flip rate under reorder and reword;
  2. treatment-shift and reduced-care rates (Gourabathina);
  3. repeated-run agreement at fixed temperature (CRAFT-MD 5 repeats; Shyr 100 runs).

### Gaps
- No peer-reviewed study read quantifies the proportion of clinical decisions that change across repeated identical runs at temperature 0 or above for a decision/triage task. This is a genuine gap, or at least not found in this session.
- Order effects in multiple-choice option position for medical LLMs (Pezeshkpour & Hruschka-style analyses) were not searched for clinical datasets specifically.
- Omar 2025 effect sizes were not verified (secondary only).

---

## Q4. Which single-model approaches (prompting, self-consistency, fine-tuning, tool use) are the strongest single-agent baselines in clinical tasks?

### Takeaway
No single method dominates across settings.
- **MCQ benchmarks:** a generalist frontier model with retrieval-selected few-shot examples, self-generated CoT and answer-shuffled self-consistency ensembling (Medprompt) beat domain fine-tuning (90.2% vs Med-PaLM 2's 86.5%).
- **Clinical-style CoT variants:** these add nothing over plain CoT for GPT-4 (Savage).
- **Incomplete information:** abstention or confidence gating (MediQ +22.3%; Med-PaLM self-consistency deferral to 82.5% at 45% deferral) and context summarization or filtering (CRAFT-MD +4.2 points; Hager) are the most effective single-model add-ons.
- **Agentic EHR tasks:** a single function-calling agent on a strong model is the baseline to beat (69.67%).
- **Multi-agent designs:** the one head-to-head benchmark read (MedAgentBoard) found they do not consistently beat strong single LLMs.

### Cited Findings
- **Prompt-only generalist vs fine-tuned specialist** (Nori 2023). — [arXiv HTML](https://arxiv.org/html/2311.16452v1)
  - GPT-4 + Medprompt scored 90.2% on MedQA vs 86.5% for Med-PaLM 2. It also beat or tied Med-PaLM 2 on all 9 MultiMedQA sets: it tied on MMLU professional medicine (95.2 vs 95.2), so "outperforms on every benchmark" is slightly overstated.
  - Ablation on MedQA: CoT +3.4%, few-shot +2.2%, choice-shuffle ensemble +2.2%. GPT-4-generated CoT exemplars beat expert-written ones (86.9% vs 83.8%).
- **Clinical-reasoning prompt styles** (Savage et al. 2024, npj Digit Med 7:20; DOI 10.1038/s41746-024-01010-1; FULL). — [Nature PDF](https://www.nature.com/articles/s41746-024-01010-1.pdf)
  - On 518 free-response MedQA items, GPT-4 scored 76% with traditional CoT.
  - Diagnostic prompts gave intuitive 77%, analytic 78%, differential diagnosis 78% and Bayesian 72%. All differences were non-significant.
  - For GPT-3.5, analytic (−6.0%, p = 0.001) and differential-diagnosis (−8.9%, p < 0.001) prompts were significantly worse than CoT.
  - NEJM cases: GPT-4 scored 38% with CoT and 34% with differential diagnosis.
- **Self-consistency as an uncertainty signal and deferral** (Singhal 2023, prior session). — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
  - Agreement over 41 decodes was used to defer low-confidence answers, which raised MedQA accuracy to 82.5% at a deferral fraction of 0.45. The authors call this preliminary.
- **Abstention and question-asking** (MediQ, prior session). — [arXiv](https://arxiv.org/abs/2406.00922)
  - Abstention improved diagnostic accuracy by 22.3%. Naive "ask questions" prompting hurt performance.
- **Verbalised confidence is unreliable** (Griot et al. 2025, Nat Commun 16; DOI 10.1038/s41467-024-55628-6; prior session ABSTRACT). — [RePEc](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-024-55628-6.html)
  - 12 models "consistently failed to recognize their knowledge limitations". Numbers are only secondary.
- **Context management**
  - CRAFT-MD: conversation summarization raised GPT-4 from 0.627 to 0.669. — [Johri 2025](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
  - Hager: filtering to abnormal labs and summarization helped modestly. — [Hager](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- **Prompt engineering on real EHR notes** (Williams 2024). — [eScholarship PDF](https://escholarship.org/content/qt67p669r1/qt67p669r1.pdf)
  - Zero-shot CoT and "only if absolutely required" prompts improved results only marginally. Specificity was traded against sensitivity.
  - Few-shot prompting and fine-tuned clinical NLP baselines were not tested.
- **Domain fine-tuning**
  - Hager: the medically fine-tuned Llama-2 derivatives (Clinical Camel, Meditron) could not complete the autonomous MIMIC-CDM task. — [Hager](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
  - AMIE: the strongest conversational results come from a fine-tuned model trained with self-play and a 3-step inference chain. — [Tu 2025](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08866-7.html); [arXiv](https://arxiv.org/html/2401.05654v1)
- **Tool use**
  - MedAgentBench: a single agent with 9 FHIR tools reached 69.67% (Claude 3.5 Sonnet v2). Errors were dominated by format and API-call mistakes, not clinical reasoning. — [arXiv HTML](https://arxiv.org/html/2501.14654v2)
  - AgentClinic: a persistent notebook tool gave Llama-3 up to 92% relative improvement. — [arXiv](https://arxiv.org/abs/2405.07960)
- **Single vs multi-agent head-to-head** (Zhu et al., MedAgentBoard; arXiv 2505.12371 v2, 30 Oct 2025; a NeurIPS 2025 poster page exists, but the track was not confirmed). ABSTRACT. — [arXiv](https://arxiv.org/abs/2505.12371v2); [NeurIPS 2025 poster page](https://neurips.cc/virtual/2025/poster/121792)
  - Multi-agent collaboration "do[es] not consistently" beat advanced single LLMs (for example, in textual medical QA).
  - Specialised conventional methods generally beat multi-agent approaches on medical VQA and EHR prediction.
  - Multi-agent collaboration helped task completeness in clinical workflow automation. The abstract gives no numbers.

### Inferences
- For RQ4, a defensible strongest single-agent baseline would combine:
  1. the same backbone model as the multi-agent system;
  2. the same retrieval corpus and tools;
  3. retrieval-selected few-shot examples plus CoT;
  4. self-consistency over N samples, with N matched to the multi-agent system's total LLM calls or tokens (the compute match);
  5. an abstention or deferral rule from sample agreement;
  6. the same deterministic pre-checks and decision core where they are not part of the claimed contribution.

  This is a synthesis of Medprompt, Singhal and MediQ, not a design any single paper prescribes. Matching N to compute matters: Medprompt's gains come partly from 5–11 ensemble calls, so an unmatched single-call baseline would flatter any multi-call architecture.
- **Fine-tuning.** There is no evidence read that domain fine-tuning alone outperforms a prompted frontier generalist on realistic decision tasks. A student project can therefore reasonably use prompted generalist models as baselines.
- **Verbalised confidence.** Given Griot 2025, a baseline that gates on the model's verbalised confidence would be a weak strawman. Agreement-based (sampling) gating is the fairer comparator.
- **Agentic baselines.** MedAgentBench errors are mostly interface errors. A single-agent baseline with schema-validated tool calls may close much of the gap that multi-agent "tool specialist" designs claim to address. This is an inference to test, not a finding.

### Gaps
- No study read reports a compute-matched comparison (same token or call budget) of single-agent self-consistency vs a multi-agent pipeline on a clinical decision task. This is directly relevant to P4 and RQ4, and is likely a genuine literature gap. The parallel multi-agent researcher should confirm.
- Med-Gemini, o1/o3-class "reasoning" models on realistic clinical tasks, and "From Medprompt to o1" (Nori 2024) were not read. Their claims should not be cited from this file.
- MedAgentBoard numeric results were not read; there is abstract only.

---

## Cross-cutting synthesis for Chapter 2 (what is established, contradictions, gaps, unverified items)

### Takeaway
The evidence supports framing Chapter 2 as follows. Classical CDSS showed that small, workflow-integrated, actionable support improves processes but rarely outcomes, and fails through alert overload, automation bias and poor external validity. Single LLMs then moved the "knowledge" ceiling far up, but reproduce and amplify the same realism gap. Performance falls under information gathering, real EHR inputs, action execution and irrelevant perturbations, and is evaluated mostly on exam data (5% real-data studies).

### Cited Findings

**(a) What the evidence establishes**
1. CDSS improve process adherence modestly (+5.8 points pooled; 64% of trials) and patient outcomes rarely (13% of trials; +0.3% median on clinical targets). — [Kwan 2020](https://www.bmj.com/content/370/bmj.m3216); [Garg 2005](https://sites.ualberta.ca/~dcl3/ABCDreview/SRs_immunizations/Journal%20of%20the%20American%20Medical%20Association_Garg_Effects%20of%20computerized%20clinical%20decision_2005.pdf)
2. Drug-alert override rates of 49–96% are documented. — [van der Sijs 2006](https://repub.eur.nl/pub/60465)
3. A widely deployed ML CDSS performed far below its developer-reported accuracy on external validation (AUC 0.63 vs 0.76–0.83). — [Wong 2021](https://pmc.ncbi.nlm.nih.gov/articles/PMC8218233)
4. LLM accuracy drops substantially from complete vignettes to interactive or autonomous information gathering. — [Johri 2025](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf); [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883); [AgentClinic](https://arxiv.org/abs/2405.07960)
5. On real ED notes, GPT-4-turbo underperforms a resident on admission and imaging decisions and over-recommends interventions. — [Williams 2024](https://escholarship.org/content/qt67p669r1/qt67p669r1.pdf)
6. Clinically irrelevant edits change LLM recommendations. — [Gourabathina 2025](https://camps.aptaracorp.com/ACM_PMS/PMS/ACM/FACCT25/120/bb50c1a8-2ab6-11f0-ada9-16bb50361d1f/OUT/facct25-120.html); [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
7. LLM evaluation practice is dominated by exam-style accuracy: 5% of studies used real patient data, and 1.2% assessed calibration. — [Bedi 2025](https://par.nsf.gov/biblio/10616638)

**(b) Contradictions and challenges to the project's premises**
- **LLM vs physicians.** LLM-alone scored at or above physicians in several trials. Goh 2024 (+16 points), Goh 2025 (GPT-4 alone ≈ physicians + GPT-4) and McDuff 2025 (59.1% vs 51.7% assisted) contradict any blanket claim that LLMs are worse than clinicians, or that human-in-the-loop improves accuracy. — [Goh 2024](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/); [Goh 2025](https://www.erichorvitz.com/GPT-4-management_reasoning_Nature_Med.pdf); [McDuff 2025](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08869-4.html)
- **AMIE.** A single fine-tuned conversational LLM outperformed PCPs on 30/32 specialist-rated axes in a randomized OSCE. This is strong evidence that a well-built single-model system can be competitive in interactive settings, though in a text-chat simulation. — [Tu 2025](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08866-7.html)
- **Multi-agent vs single LLM.** MedAgentBoard reports that multi-agent designs do not consistently beat strong single LLMs. This directly challenges the multi-agent premise and must be presented in 2.3. — [Zhu 2025](https://arxiv.org/abs/2505.12371v2)
- **Prompting vs fine-tuning.** Prompting a generalist beat domain fine-tuning on MCQ (Medprompt), while clinical-reasoning-style prompts gave no gain over plain CoT (Savage). Prompt engineering effects are task-dependent and small on real notes (Williams). — [Nori 2023](https://arxiv.org/html/2311.16452v1); [Savage 2024](https://www.nature.com/articles/s41746-024-01010-1.pdf)
- **Real-world deployment.** The only real-world LLM CDS deployment read (Korom 2025) reports benefit, but it is an unrandomised preprint with industry authors. It contrasts with the negative realistic-benchmark results, and it uses a single-model design. — [Korom 2025](https://arxiv.org/abs/2507.16947)
- **Reasoning models.** In MedAgentBench, o3-mini scored lower than GPT-4o (51.67% vs 64.00%), so newer "reasoning" models are not uniformly better on agentic EHR tasks. — [Jiang 2025](https://arxiv.org/html/2501.14654v2)

### Inferences

**(c) Gaps relevant to P1–P4 and RQ1–RQ4**
- **P1 / RQ3.** No peer-reviewed study read reports a status or decision flip rate under reorder, reword and repeated runs for a closed-option CDS task with matched baselines. Existing studies measure diagnostic accuracy or treatment-shift rates. This is a defensible contribution gap.
- **P2 / RQ1.** Abstention is evaluated as accuracy-at-coverage (Singhal, MediQ). None of the studies read scores unnecessary abstention or escalation against a reference standard. Williams' over-recommendation shows the cost side exists but is rarely measured.
- **P3 / RQ2.** MedFuzz and Savage show rationales are unfaithful or contain false logic. None of the single-LLM CDS studies read measured claim-to-source support. This theme provides motivation only, and the RAG theme must supply the evidence.
- **P4 / RQ4.** Only 5% of LLM evaluations use real patient data (Bedi). No compute-matched single- vs multi-agent clinical comparison was found. MedAgentBoard is the closest, and it is not compute-matched as far as the abstract shows. Latency and cost appear only incidentally: Goh 2025 reports +119 s per case for physicians using GPT-4.
- **Classical CDSS lesson for the evaluation.** Developer-run evaluations inflate effects (Garg 74% vs 28%). The project's evaluation should pre-register metrics and use baselines it did not tune more weakly than its own system.

### Gaps

**(d) Inaccessible or unverified items**
- **Kawamoto 2005:** full text not read (PMC captcha). The "94%" figure and odds ratios are unverified.
- **Goddard 2012:** full text blocked (City repository returned an Anubis block; OUP gave a server error). Only abstract-level mediators and mitigators are verified. The print volume and pages (19(1):121) come from the OUP URL path only.
- **Bedi 2025:** the JAMA version is confirmed as 333(4):319 via NSF PAR. The DOI was not displayed on any page read, so it is left blank in the JSONL. The full text read was the medRxiv v2 preprint.
- **Omar 2025:** SECONDARY only (press release). Do not quote effect sizes.
- **Shyr et al.:** journal version not confirmed. PMC12637745 appeared in search but was not opened.
- **MedAgentBench:** the NEJM AI publication (DOI 10.1056/AIdbp2500144) is confirmed only via the Stanford HAI page. The NEJM AI page was not opened, so volume and issue are unknown.
- **CRAFT-MD (Johri 2025), Med-PaLM 2 (Singhal 2025), Goh 2025:** volume and pages not verified.
- **Wong 2021:** pages not verified. The PMC page showed "181(8):1–6", which looks like early-release pagination.
- **MedAgentBoard:** NeurIPS 2025 poster page found in search but not opened. The track (main vs Datasets & Benchmarks) is unconfirmed.
- **Korom 2025:** author affiliations and conflicts of interest not verified from the page read.
- **Not read at all** (do not cite from this file): Bright 2012; Lyell & Coiera 2017; Kanjee 2023 (JAMA, GPT-4 on NEJM CPCs); Med-Gemini; Nori 2024 "From Medprompt to o1"; Brodeur et al. on o1-preview; Pezeshkpour & Hruschka on MCQ option-order bias.
- **Infrastructure:** the Crossref, OpenAlex and Semantic Scholar APIs were not reachable from the bash sandbox (no network). A batched OpenAlex URL was refused by the fetch proxy (over 250 characters).

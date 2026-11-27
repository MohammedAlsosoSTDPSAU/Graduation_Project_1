# Clinical and medical LLM multi-agent systems (theme: clinical_mas) — evidence for Chapter 2.1 and the 2.2 comparative table

Scope note. This file covers clinical/medical multi-agent LLM systems, the baselines each one used, and the reviews that assess them. General-domain single-vs-multi-agent studies (Wang et al. ACL 2024, Smit et al. ICML 2024, MAST, Kim et al. scaling preprint) belong to another researcher. They are mentioned only where a clinical study's interpretation depends on them.

Verification labels (per _protocol.md):
- FULL = full text read.
- PARTIAL = substantial parts read.
- ABSTRACT = abstract or record only.
- SECONDARY = seen only via another source.

Peer-review status is given in brackets after each source. All 22 included studies are in clinical_mas_studies.jsonl, and the 44 searches and fetches are in clinical_mas_searchlog.jsonl.

Access limits this session:
- nature.com and the arXiv HTML for 2311.10537 (unversioned URL) were blocked by the allowlist.
- PubMed record 40082662 and PMC record PMC12407621 returned reCAPTCHA walls.
- Several arXiv HTML pages were read only up to 100k characters.

## Which clinical/medical multi-agent systems have been published (2023–2026), and what exactly did they compare against?

### Takeaway
At least 15 clinical multi-agent systems and benchmarks can be traced to primary sources. The peer-reviewed core is MedAgents, MDAgents, Ke et al., Chen et al. MAC, DeepRare, AI Hospital, KG4Diagnosis, MedAgentBoard, the MLHC tumour-board study and probably ColaCare. Almost all of these compare against single-pass LLMs or prompting baselines (CoT, CoT-SC, few-shot, Self-Refine). Only MDAgents and TAO used broad baseline sets with other multi-agent frameworks, and TAO's set includes a single-LLM "multi-role" control. No clinical study found uses a single-agent baseline matched for tools, retrieval and compute.

### Cited Findings

**Role-play, discussion and consensus systems**

MedAgents (Tang et al.) [peer-reviewed conference: Findings of ACL 2024, pp. 599–621, DOI 10.18653/v1/2024.findings-acl.33; numbers from arXiv v4, PARTIAL]
- Design: five steps (gather experts, individual analyses, report summary, iterative consultation, decision), with no tools or retrieval.
- Baselines: zero-shot, CoT and CoT+SC, plus few-shot variants, Flan-PaLM and small open medical models.
- GPT-4 MedQA: 83.7, against 61.8 (zero-shot CoT) and 74.5 (zero-shot CoT+SC).
- GPT-4 average over 9 datasets: 86.7, against 85.4 for few-shot CoT+SC. That is close to a tie with a single-agent prompting method.
- GPT-3.5 PubMedQA: MedAgents scored 72.9, below zero-shot CoT+SC at 75.7.
- Sources: [arXiv v4](https://arxiv.org/html/2311.10537v4); [ACL Anthology](https://aclanthology.org/2024.findings-acl.33)

MDAgents (Kim et al.) [peer-reviewed conference: NeurIPS 2024, DOI 10.52202/079017-2522; PARTIAL]
- Baselines were the broadest of any clinical system:
  - 6 single-agent: zero-shot, few-shot, CoT, CoT-SC, Ensemble Refinement, Medprompt.
  - 8 group: majority, weighted and Borda voting, MedAgents, Meta-Prompting, ReConcile, AutoGen, DyLAN.
- MedQA: 88.7±4.0, against CoT-SC 83.9±2.7 and ReConcile 81.3±3.0.
- MDAgents was not best everywhere: PubMedQA 75.0 vs ReConcile 79.7, and MedVidQA 56.2 vs weighted voting 57.8.
- Sources: [arXiv v3](https://arxiv.org/html/2404.15155v3); [NeurIPS](https://proceedings.neurips.cc/paper_files/paper/2024/hash/90d1fc07f46e31387978b88e7e057a31-Abstract.html)

Ke et al. [peer-reviewed journal: JMIR 2024;26:e59439; FULL, from an earlier session]
- Design: AutoGen conversations with a devil's-advocate resident and a bias-aware senior doctor.
- Baselines: the model's own initial single-pass diagnosis (0/80 correct) and 3 human doctors (48% final).
- Best framework (4-C): 61/80 (76%) final accuracy, on 16 cases.
- No single-agent self-critique baseline was used.
- Source: [JMIR](https://jmir.org/2024/1/e59439)

Chen et al. MAC [peer-reviewed journal: npj Digital Medicine 8:159 (2025), DOI 10.1038/s41746-025-01550-0; ABSTRACT]
- Design: doctor agents plus a supervisor, tested on 302 rare-disease cases.
- Baselines: single GPT-3.5 and GPT-4, CoT, Self-Refine and Self-Consistency.
- MAC beat them "with higher performance and more output tokens."
- Best configuration: 4 doctor agents plus 1 supervisor.
- Source: [OpenAlex](https://api.openalex.org/works/doi:10.1038%2FS41746-025-01550-0)

Li et al., the MAC preprint [preprint: Research Square 2023, DOI 10.21203/rs.3.rs-3757148/v1; PARTIAL]
- This earlier version differs from the npj paper:
  - 150 rare diseases, not 302.
  - 3 doctor agents plus a supervisor, not 4.
  - Baselines were zero-shot CoT, tree-of-thoughts and "reflections of thoughts". Self-Refine and Self-Consistency are not mentioned.
- Most-likely-diagnosis accuracy:

  | Presentation | GPT-3.5 | GPT-4 | MAC |
  |---|---|---|---|
  | Initial | 16.5% | 19.5% | 28% |
  | Complete | 27.3% | 32.2% | 48.0% |

- Grading was done by 3 blinded physicians.
- Do not cite these numbers as the npj results.
- Source: [Research Square](https://researchsquare.com/article/rs-3757148/v1)

**Tumour-board and multidisciplinary-team (MDT) style**

Kim et al. [peer-reviewed conference: MLHC 2025, PMLR 298; ABSTRACT]
- Compared single-agent with multi-agent conversation on real cancer MDT meeting data.
- Multi-agent systems with specialised roles "better approximate" consensus decisions.
- No numbers in the abstract.
- Source: [PMLR](https://proceedings.mlr.press/v298/kim25a.html)

Ellis-Caleo et al. [preprint; ABSTRACT]
- A tumour-board summarisation tool that was deployed and monitored at the Stanford Thoracic Tumor Board.
- Compared several automated summarisation methods against physician gold-standard summaries using fact-based rubrics and a validated LLM judge.
- Source: [Microsoft Research](https://www.microsoft.com/en-us/research/?p=1169017)

**Rare disease, and systems integrating knowledge graphs, retrieval or tools**

DeepRare [peer-reviewed journal: Nature 2026, DOI 10.1038/s41586-025-10097-9; ABSTRACT]
- Design: an LLM multi-agent system with more than 40 tools.
- Data: 9 datasets from Asia, North America and Europe, covering 2,919 diseases.
- Phenotype-based (HPO) tasks: average Recall@1 57.18%, 23.79% above the next-best method.
- Multimodal tests: 69.1% vs Exomiser at 55.9% (168 cases).
- Experts agreed with 95.4% of the reasoning chains.
- Source: [PubMed](https://pubmed.ncbi.nlm.nih.gov/41708847/)

ColaCare [DOI 10.1145/3696410.3714877, probably TheWebConf 2025 but unconfirmed; ABSTRACT]
- Design: a MetaAgent orchestrates DoctorAgents, alongside EHR expert models and retrieval over the Merck (MSD) Manual.
- Tasks: mortality and readmission prediction on 3 EHR datasets.
- The baselines are not stated in the abstract.
- Source: [arXiv](https://arxiv.org/abs/2410.02551v2)

KG4Diagnosis [peer-reviewed workshop-level venue: AAAI Bridge Program, PMLR 281; ABSTRACT]
- Design: a general-practitioner agent coordinates specialist agents, and a knowledge graph covers 362 diseases.
- No evaluation is reported on the abstract page.
- Source: [PMLR](https://proceedings.mlr.press/v281/zuo25a.html)

ClinicalAgent [preprint; ABSTRACT]
- Design: GPT-4 multi-agent with Least-to-Most decomposition and ReAct, for clinical trial outcome prediction.
- Result: PR-AUC 0.7908, against standard prompting only (+0.3326).
- Source: [arXiv](https://arxiv.org/abs/2404.14777v2)

**Sequential, interactive and simulator-based systems**

MAI-DxO (Nori et al.) [preprint, arXiv 2506.22405; ABSTRACT]
- Design: an orchestrator simulating a physician panel. A "gatekeeper" model reveals case findings only when asked.
- Data: 304 sequential NEJM clinicopathological conference cases.
- Baselines: generalist physicians (20% average) and the same model without orchestration (off-the-shelf o3).
- Results: 80% accuracy, or 85.5% in the maximum-accuracy configuration.
- Cost: 20% lower than physicians and 70% lower than off-the-shelf o3.
- Source: [arXiv](https://arxiv.org/abs/2506.22405v2)

AgentClinic [preprint, v5 May 2025, no journal reference]
- A simulator with patient and measurement agents.
- Sequential testing can reduce MedQA accuracy to less than one-tenth of static accuracy.
- Source: [arXiv](https://arxiv.org/abs/2405.07960)

AI Hospital [peer-reviewed conference: COLING 2025, pp. 10183–10213]
- A Doctor agent interacts with Patient and Examiner agents.
- GPT-4 shows "significant performance gaps" in multi-turn compared with non-interactive settings.
- A dispute-resolution mechanism among doctor agents improves results, but no effect size is given in the abstract.
- Source: [ACL Anthology](https://aclanthology.org/2025.coling-main.680)

**Benchmarks comparing multi-agent and single-agent systems**

MedAgentBoard [peer-reviewed conference: NeurIPS 2025 poster; the Datasets & Benchmarks track is stated only on the authors' GitHub]
- Multi-agent collaboration "does not consistently outperform advanced single LLMs" (for example, on textual medical QA).
- Conventional specialised methods "generally maintain better performance" on medical visual QA and EHR prediction.
- Multi-agent collaboration helps task completeness in workflow automation.
- Source: [NeurIPS](https://neurips.cc/virtual/2025/poster/121792)

MedAgentsBench [preprint]
- 862 hard questions drawn from 8 datasets, with contamination screening.
- Compared 9 agent frameworks, 3 reasoning models and 7 base models.
- Best result: o3-mini with MDAgents, at 35.1%, on the cost–performance Pareto frontier.
- Source: [arXiv](https://arxiv.org/abs/2503.07459v2)

Optimization Paradox (Bedi et al.) [preprint]
- Data: 2,400 real MIMIC-CDM cases covering 4 abdominal conditions.
- Compared a single agent with a multi-agent pipeline split into information gathering, interpretation and differential diagnosis.
- Multi-agent systems generally did better.
- A "Best of Breed" system built from the top individual components reached 85.5% on information gathering but only 67.7% diagnostic accuracy, against 77.4% for the best multi-agent system.
- Source: [arXiv](https://arxiv.org/abs/2506.06574v2)

### Inferences
- For the 2.2 table, the baselines fall into four tiers:
  - (a) single-pass or the model's own initial answer: Ke, ClinicalAgent.
  - (b) prompting baselines such as CoT, CoT-SC, few-shot and Self-Refine: MedAgents, MAC.
  - (c) prompting plus other multi-agent frameworks: MDAgents, TAO, ConfAgents.
  - (d) humans or task-specific conventional systems: Ke, MAI-DxO, TAO, DeepRare (Exomiser), MedAgentBoard.
- No system uses a single agent given the same tools, retrieval and token budget.
- Strong single-agent prompting often comes close to multi-agent systems on exam-style QA:
  - MedAgents GPT-4 average: 86.7 vs 85.4 for few-shot CoT+SC.
  - MDAgents on MedQA: CoT-SC (83.9) beat every group baseline.
  - MedAgentBoard found no consistent multi-agent advantage.
- Gains look larger where the task is sequential or case-based (Ke, MAC, MAI-DxO, Bedi). Those are also the studies with the weakest or least controlled baselines.

### Gaps
- Exact per-benchmark numbers were not read for the npj MAC paper (full text blocked), MedAgentBoard, MedAgentsBench, DeepRare, ColaCare, the MLHC tumour-board study, or AI Hospital's dispute-resolution mechanism.
- Several 2025–2026 oncology and MDT preprints found in searches were not opened (budget), so they are unverified:
  - CoMMa (arXiv 2602.09159)
  - GI-oncology multi-agent system (arXiv 2512.08674)
  - Gynecologic oncology agent teams (medRxiv 10.1101/2025.10.30.25339199)
  - Breast-cancer agentic systems (arXiv 2607.12051)
  - arXiv 2511.20490 and 2602.13793
  - MeDxAgent (arXiv 2606.03416)
  - ClinicalAgents with dual memory (arXiv 2603.26182)

## How are orchestration and control designed, and does any study isolate the effect of orchestration from tools or extra computation?

### Takeaway
Clinical multi-agent systems use three control patterns:
- Decentralised discussion: MedAgents, Ke, the AI Hospital dispute mechanism.
- LLM-centralised moderation or hierarchy: MDAgents moderator, MAC supervisor, ColaCare MetaAgent, KG4Diagnosis GP agent, TAO router and final-decision agent, MAI-DxO orchestrator.
- Fixed workflow pipelines: Bedi's staged pipeline, ConfAgents' gated escalation.

None of them uses a deterministic, non-LLM controller over a shared state with a deterministic decision step. No clinical study cleanly isolates orchestration from tools and compute. Four studies give partial evidence: a single-LLM multi-role control (TAO), a same-model comparison with and without orchestration (MAI-DxO), cost or latency reporting (ConfAgents, MedAgentsBench, MedAgents), and component-composition effects (Bedi).

### Cited Findings
- Decentralised discussion: MedAgents iterates consultation until consensus. Its own ablation on MedQA:

  | Configuration | MedQA accuracy |
  |---|---|
  | Direct prompting | 49.0 |
  | CoT | 55.0 |
  | + analysis | 62.0 |
  | + report summary | 65.0 |
  | + consultation | 67.0 |

  The multi-round discussion adds only 2 points, and the authors say the initial analysis step gives the largest gain. — [MedAgents arXiv v4](https://arxiv.org/html/2311.10537v4)
- LLM-routed, centralised (MDAgents):
  - An LLM classifies case complexity and routes it to a solo agent, a moderated team, or an integrated care team.
  - "Moderator review and external medical knowledge" together gave +11.8% average accuracy. The two components are not separated in the abstract.
  - Latency rises with complexity: 14.7 s (low), 95.5 s (moderate), 226 s (high) per case.
  - [MDAgents arXiv v3](https://arxiv.org/html/2404.15155v3)
- Hierarchical with tiers (TAO):
  - A Recruiter selects roles, a Router assigns them to tiers 1–3, disagreement triggers escalation, and a Final Decision Agent weighs higher tiers.
  - Baselines include "Multi-role": one LLM simulating all tiers. This is the closest clinical control for role-play versus genuinely separate agents. The paper argues its gains come from the multi-agent design rather than sequential role-play.
  - Cost is reported per method (TAO 55.2 USD in the Gemini-2.5 Pro setting).
  - The authors name the centralised router as a possible upstream point of failure.
  - [TAO arXiv v2](https://arxiv.org/html/2506.12482v2)
- Same-model orchestration (MAI-DxO): the comparison with off-the-shelf o3 holds the base model fixed and changes only the orchestration. Accuracy was 80%, and cost was 70% lower than unorchestrated o3. The comparison is not compute-matched, and the role mechanics were not read. [preprint] — [arXiv](https://arxiv.org/abs/2506.22405v2)
- Uncertainty-gated escalation (ConfAgents):
  - A conformal-prediction judge sends only cases with non-singleton prediction sets to a multi-agent retrieval step.
  - On MedQA with GPT-4o: 80.21% at 9.11 s, against MDAgents (75.10% at 25.15 s) and MedAgents (78.80% at 70.21 s).
  - About 2.01× fewer completion tokens than MDAgents.
  - Internal inconsistency: 80.21% in Table 2 but 77.00% in Table 6.
  - [preprint] — [arXiv PDF](https://arxiv.org/pdf/2508.04915)
- Composition effects (Bedi et al.): the best component models combined into "Best of Breed" (67.7%) did worse than the top integrated multi-agent system (77.4%). Information flow between agents matters, and component metrics do not predict system accuracy. [preprint] — [arXiv](https://arxiv.org/abs/2506.06574v2)
- Cost and compute reporting:
  - MAC used more output tokens than its prompting baselines. — [OpenAlex](https://api.openalex.org/works/doi:10.1038%2FS41746-025-01550-0)
  - MedAgents costs about $1.41 per 100 questions, at about 40 s per question. — [arXiv v4](https://arxiv.org/html/2311.10537v4)
  - MedAgentsBench reports a cost–performance Pareto analysis. [preprint] — [arXiv](https://arxiv.org/abs/2503.07459v2)
- Structural failures of discussion (MedAgentAudit) [preprint]:
  - 6 multi-agent architectures, 14,400 audited cases.
  - Agents repeated their initial views in 98.42% of cases.
  - Authority bias rose from 35.30% to 68.75% across rounds.
  - Unsupported observations appeared in 16.63% of cases and propagated downstream.
  - Minority suppression: 5.11%.
  - Consensus often rests on authority or majority rather than evidence.
  - [arXiv](https://arxiv.org/abs/2510.10185v1)

### Inferences
- No clinical study holds tools, retrieval and token budget equal between single-agent and multi-agent arms. TAO's multi-role baseline and MAI-DxO's same-model comparison are the nearest designs. Neither matches compute. RQ4's matched-baseline design would therefore fill a gap documented in this literature.
- MedAgentAudit (extreme anchoring and growing authority bias over discussion rounds) and the MedAgents ablation (discussion adds little) both suggest that free discussion is a weak mechanism for correcting errors. This supports replacing consensus with explicit rule and claim checks and a deterministic decision core. Both sources are on QA tasks, and MedAgentAudit is a preprint.
- Bedi's composition effect argues for end-to-end evaluation plus component ablation, not component metrics alone. This maps directly to RQ4's ablation design.

### Gaps
- TAO Appendix I (cost estimates), MDAgents Appendix D.2 (cost) and the detailed MDAgents RAG-versus-moderator ablation (Section 4.3) were not read.
- No clinical study comparing an LLM orchestrator with a deterministic, code-based controller was found.
- No study reports variance in status or decision outcomes under input reordering or rewording for clinical multi-agent systems (RQ3).

## Which systems include safety mechanisms (critic, verifier, abstention, escalation, human-in-the-loop), and how were these evaluated?

### Takeaway
The safety mechanisms in clinical multi-agent systems, and how they were evaluated:
- Critic or devil's-advocate roles (Ke): evaluated on diagnostic accuracy only.
- Escalation tiers with human handoff (TAO): evaluated on safety benchmarks plus a 5-physician, 20-case study.
- Uncertainty-triggered escalation (ConfAgents): evaluated on accuracy and efficiency, not on calibration of the decision to escalate.
- Evidence-linked traceability (DeepRare): evaluated by expert agreement.
- External auditing (MedAgentAudit): measures process failures.

No clinical multi-agent system found evaluates abstention or "need more information" outputs as a scored status. No claim-to-source verifier with measured unsupported-claim rates was found within multi-agent clinical systems.

### Cited Findings
- Critic role (Ke et al.): a devil's-advocate resident and a bias-aware senior doctor. Final accuracy reached 76% (Framework 4-C, OR 3.49, p=.002) against 0% initially. Bias reduction was measured through diagnostic accuracy, not as a separate bias metric. — [JMIR](https://jmir.org/2024/1/e59439)
- Escalation and human-in-the-loop (TAO) [preprint]:
  - Each agent outputs a risk level (low, medium, high or critical) and an escalation flag. Human clinician review is the top escalation tier.
  - Evaluated on SafetyBench, MedSafetyBench, LLM red-teaming, Medical Triage and MM-SafetyBench.
  - TAO absorbs 16.9–24.3% of individual agent errors, with error amplification below 8.4%.
  - Clinician study (5 physicians, 20 real triage scenarios): TAO alone scored 40% (8/20), rising to 60% (12/20) with doctor feedback. Unaided doctors averaged 51.8%.
  - Inter-rater agreement: ICC 0.610 for oversight necessity, 0.259 for output appropriateness, and −0.101 for safety confidence.
  - The paper's "best on 4 of 5 benchmarks" claim conflicts with its Gemini-2.5 Pro table, which shows a positive gain on all 5.
  - [arXiv v2](https://arxiv.org/html/2506.12482v2)
- Uncertainty-based escalation (ConfAgents): conformal prediction with α=0.05 decides when to escalate to collaboration. It is evaluated on accuracy and efficiency. The fetched text does not report coverage or calibration of the escalation decision itself. [preprint] — [arXiv PDF](https://arxiv.org/pdf/2508.04915)
- Moderator review (MDAgents): moderator review is part of the +11.8% average accuracy gain together with external knowledge. It was not evaluated as a safety mechanism. — [arXiv v3](https://arxiv.org/html/2404.15155v3)
- Traceability (DeepRare): reasoning is linked to "verifiable medical evidence", and experts agreed with 95.4% of reasoning chains. This is expert agreement, not an automatic claim-support rate. — [PubMed](https://pubmed.ncbi.nlm.nih.gov/41708847/)
- Cost-aware action control (MAI-DxO): the orchestrator selects "high-value, cost-effective tests". Safety outcomes such as harmful tests or missed diagnoses were not reported in the abstract. [preprint] — [arXiv](https://arxiv.org/abs/2506.22405v2)
- Safety in human ratings (MAC preprint): physicians rated disease-knowledge outputs for potential harm (likelihood and extent) on Likert scales. All three models averaged above 4.5, with no per-model breakdown. [preprint] — [Research Square](https://researchsquare.com/article/rs-3757148/v1)
- Process auditing (MedAgentAudit): 16.63% unsupported observations, 18.53% self-contradiction and 5.48% contradiction neglect. These process failures are hidden by final-answer accuracy. [preprint] — [arXiv](https://arxiv.org/abs/2510.10185v1)

### Inferences
- The five-status output (RECOMMEND … ABSTAIN) and the measurement of unnecessary abstention or escalation (RQ1) have no direct counterpart in the clinical multi-agent literature found. TAO's risk levels and escalation flags are the nearest. TAO scores escalation through benchmark safety metrics, not escalation accuracy against a reference standard.
- TAO's clinician study is a caution for the chapter. With human feedback the system reached 60%, but on its own (40%) it scored below unaided doctors (51.8%), and raters disagreed on safety confidence (ICC −0.101). Human-in-the-loop evaluation of safety is feasible but noisy at small sample sizes.
- Claim-to-source verification (RQ2) appears in this literature only indirectly: DeepRare's expert-checked evidence links, and MedAgentAudit showing that unsupported observations propagate. That supports a dedicated verifier as a design gap. The RAG-verification theme may hold the direct evidence.

### Gaps
- No clinical multi-agent study found reports an unsupported-claim rate with and without a verifier.
- No study found measures how well escalation or abstention decisions are calibrated against clinician-labelled "should escalate" or "should abstain" ground truth.
- MAI-DxO's handling of harmful or unnecessary tests was not checked in the full text.

## What do systematic reviews and surveys of medical AI agents report about evidence quality, real-data use and baselines?

### Takeaway
One systematic review (Gorenshtein et al., medRxiv preprint) and one peer-reviewed scoping review (Njei et al., PLoS One 2026) were verified. They agree:
- Evaluations are mostly simulated, synthetic or benchmark-based.
- Real patient data and safety endpoints are rare.
- Studies are single-centre.
- Head-to-head multi-agent versus single-agent comparisons are absent (Gorenshtein) or thin (Njei).

Both reviews have internal count inconsistencies, which should be noted when citing them.

### Cited Findings
Gorenshtein et al. [preprint: medRxiv 2025, DOI 10.1101/2025.08.22.25334232; FULL, from an earlier session; still no journal version found]
- Screening: 6,023 records screened, 20 studies included.
- Gains over the baseline LLM:

  | Group | Median gain | Spread |
  |---|---|---|
  | All 20 studies | +36% | range +3.5% to +76% |
  | Single-agent tool calling | +53.0% | IQR 36.0–56.9 |
  | Multi-agent without tools | +14.05% | IQR 8.95–45.15 |
  | Multi-agent with tools | +17.17% | IQR 4.12–39.3 |

- Data and design: 65% used synthetic data, 2 studies used real patient data (117 patients), all were single-centre, and there was 1 RCT. GPT-4-family models were used in 75%.
- The review states that "no study provides a comparison of multi-agent to single-agent."
- Internal inconsistency: the listed exclusions add up to 237, but 254 full texts minus 20 included studies gives 234.
- Source: [medRxiv PDF](https://www.medrxiv.org/content/10.1101/2025.08.22.25334232.full.pdf)

Njei et al. [peer-reviewed journal: PLoS One 21(2):e0342182, 2026, DOI 10.1371/journal.pone.0342182; PARTIAL]
- 43 studies included (36 from 2025). Searches covered PubMed, Web of Science, arXiv and medRxiv. There was no risk-of-bias assessment and no registered protocol.
- Evaluations were "predominantly simulated environments or laboratory studies, with few clinical pilots."
- "Clinical outcomes and safety endpoints were rarely addressed."
- Human evaluations were small and rarely used practising clinicians as raters.
- One RCT found low physician engagement because of usability problems.
- The review says some multi-agent triage systems outperformed single-agent counterparts (not traced to the original studies here).
- Internal inconsistency: Table 1 has 44 rows against 43 stated studies, and the type labels do not match the 8/17/18 breakdown. By this researcher's count, about 21 rows are labelled LLM-based multi-agent.
- Source: [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12890167)

Benchmark-level confirmations:
- MedAgentBoard (NeurIPS 2025): no consistent multi-agent advantage over advanced single LLMs. — [NeurIPS](https://neurips.cc/virtual/2025/poster/121792)
- AgentClinic [preprint] and AI Hospital (COLING 2025): static QA overstates interactive performance. — [AgentClinic](https://arxiv.org/abs/2405.07960); [AI Hospital](https://aclanthology.org/2025.coling-main.680)

### Inferences
- The two reviews together support P4 with one peer-reviewed source (Njei) and one systematic but preprint source (Gorenshtein). Cite Njei for "mostly simulated, safety endpoints rare" and Gorenshtein for "no multi-agent versus single-agent comparison", with the preprint label.
- Gorenshtein's subgroup medians (single-agent tool calling +53% vs multi-agent +14–17%) are not head-to-head comparisons. They should be presented as an absence of evidence for multi-agent superiority, not as evidence against it.

### Gaps
- The survey "A Survey of LLM-based Agents in Medicine: How far are we from Baymax?" (Wang et al., Findings of ACL 2025, Anthology ID 2025.findings-acl.539) was identified in search results but not read. Its content and authors are unverified. — [ACL Anthology listing](https://aclanthology.org/2025.findings-acl.539)
- The following were found but not read:
  - "Agentic AI in medicine: architectures, applications, evaluation, and challenges for clinical translation" (arXiv 2607.25489).
  - An F1000Research article (15-1653) that appeared in the review search.
- The review cited in the earlier notes (ScienceDirect PII S1568494625017600) remains unverified.

---

### Synthesis for Chapter 2

**(a) What the evidence establishes**
1. Clinical multi-agent systems with role-play, moderation or hierarchy report gains over single-pass LLMs and prompting baselines. The gains appear on exam-style QA (MedAgents, MDAgents), case-based diagnosis (Ke, MAC), rare disease (DeepRare, peer-reviewed in Nature 2026) and safety benchmarks (TAO, preprint).
2. Interactive or sequential evaluation is much harder than static QA (AgentClinic, AI Hospital, MAI-DxO). Systems that structure information gathering report large gains there (MAI-DxO, preprint).
3. Free multi-agent discussion has documented process failures: anchoring, authority bias and propagation of unsupported observations (MedAgentAudit, preprint). In the MedAgents ablation, discussion adds little on top of structured analysis.
4. Reviews agree that real-data, multi-centre and safety-endpoint evaluations are rare (Njei, peer-reviewed; Gorenshtein, preprint).

**(b) Contradictions**
- Multi-agent gains are inconsistent against strong single-agent methods:
  - MedAgentBoard: no consistent advantage.
  - MDAgents: CoT-SC beat all group baselines on MedQA, and ReConcile beat MDAgents on PubMedQA.
  - MedAgents (GPT-3.5): lower than zero-shot CoT+SC on PubMedQA.
- By contrast, Bedi (real MIMIC-CDM data) and Kim MLHC (real MDT data) report that multi-agent systems beat single agents.
- TAO's headline claim ("best on 4 of 5") conflicts with its own table.
- ConfAgents' MedQA accuracy differs between its own tables (80.21% vs 77.00%).
- The MAC preprint and the npj version differ in sample (150 vs 302), agent count (3 vs 4 doctor agents) and baselines.

**(c) Gaps relevant to P1–P4 and RQ1–RQ4**
- RQ4/P4: no clinical study compares a multi-agent system with a single agent matched for tools, retrieval and compute. MDAgents' moderator and RAG gains are confounded. MAC used more tokens. Cost is reported unevenly (MedAgents, TAO, ConfAgents, MedAgentsBench, MAI-DxO).
- RQ1/P2: no scored abstention or "need more information" status. Escalation is evaluated only through benchmark safety metrics and one 20-case clinician study (TAO).
- RQ2/P3: no claim-to-source verifier with measured unsupported-claim rates in a clinical multi-agent system. DeepRare uses expert agreement; MedAgentAudit measures unsupported observations but not verifier performance.
- RQ3/P1: no robustness test of decision or status under input reordering or rewording in clinical multi-agent systems.
- Architecture: every system uses LLM-based control. A deterministic state-machine orchestrator with a deterministic decision core is not represented.

**(d) Inaccessible or unverified items**
- npj MAC full text (nature.com blocked; PubMed and PMC behind reCAPTCHA): the published numbers are unverified.
- The DeepRare author list was not captured.
- ColaCare's venue (TheWebConf 2025?) was not confirmed on a publisher page.
- The ClinicalAgent ACM BCB 2024 venue is unconfirmed.
- TAO's ICML 2025 listing (icml.cc/virtual/2025/49358) was not opened, so its workshop or main-track status is unknown.
- The MedAgentBoard track is stated only on the authors' GitHub.
- The thoracic tumour-board paper's arXiv ID, year and full author list are unconfirmed.
- Not opened (listed under the first key question's Gaps): oncology and MDT preprints, MeDxAgent, ClinicalAgents (dual memory), the Baymax survey, and the arXiv 2607.25489 survey.
- An aggregator claim that "multi-agent debate improved Top-1 accuracy by 8–12% on hard cases" (attributed to MAC) could not be traced to the source and was not used.

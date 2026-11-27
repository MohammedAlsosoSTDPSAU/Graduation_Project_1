# Single-agent vs multi-agent LLM systems: counter-evidence, compute confounds, failure modes and fair-comparison methodology (theme: single_vs_multi)

Scope note: this file covers controlled single-vs-multi comparisons, compute and test-time-scaling confounds, documented multi-agent failure modes, and methods for evaluating agents fairly. Clinical multi-agent systems appear only where a study tests single vs multi (or a compute or retrieval confound) under controlled conditions; another researcher catalogues those systems in depth. Status labels used throughout: [PR-conf] peer-reviewed conference, [PR-journal] peer-reviewed journal, [workshop], [preprint]. Verification levels (FULL, PARTIAL, ABSTRACT, SECONDARY) are given per study in single_vs_multi_studies.jsonl. Bash access to the Semantic Scholar, OpenAlex, Crossref and arXiv APIs was refused by the proxy in this session, so metadata was checked on publisher and proceedings pages instead (ACL Anthology, PMLR, NeurIPS/ICLR/ICML virtual pages, ICLR proceedings, mlanthology TMLR records, arXiv abs pages).

---

## KQ1. What do controlled comparisons show? When does multi-agent help, and when do strong single agents match or beat it?

### Takeaway
Against weak baselines (a single greedy call, no demonstrations), multi-agent debate and discussion usually help. Against strong single-agent baselines (good prompts or demonstrations, self-consistency or majority voting with the same number of samples, or a stronger "thinking" model), the advantage mostly disappears or reverses. The best-supported reading is that a large share of reported multi-agent gains comes from ensembling and extra samples, not from agents interacting. Real gains appear under specific conditions: decomposable or parallelisable tasks, heterogeneous models or vendors, weak base models, or no demonstrations. There are also documented losses: sequential planning, tool-heavy tasks under a fixed budget, homogeneous small-model debate, and capable baselines (single-agent accuracy above roughly 45%).

### Cited Findings

**Foundational pro-multi-agent results (the claims being tested)**
- Du et al. (ICML 2024, PMLR 235:11733–11763) proposed multiagent debate, in which "multiple language model instances propose and debate their individual responses and reasoning processes over multiple rounds". They report better mathematical and strategic reasoning and fewer hallucinations, with the same prompts across tasks. [PR-conf; ABSTRACT] — [PMLR](https://proceedings.mlr.press/v235/du24e.html)
- Liang et al. (EMNLP 2024, pp. 17889–17904, DOI 10.18653/v1/2024.emnlp-main.992) describe "Degeneration-of-Thought": self-reflecting LLMs stay locked into their first reasoning. Their MAD framework (opposing debaters plus a judge) improves commonsense machine translation and counter-intuitive arithmetic. They also report that debate needs adaptive stopping and only moderate adversarialness, and that an LLM judge may be partial when the debaters are different models. [PR-conf; ABSTRACT] — [ACL Anthology](https://aclanthology.org/2024.emnlp-main.992)

**Strong single agents match multi-agent discussion**
- Wang, Wang, Su, Tong & Song (ACL 2024, pp. 6106–6131, DOI 10.18653/v1/2024.acl-long.331): "a single-agent LLM with strong prompts can achieve almost the same performance" as the best discussion approach. Multi-agent discussion beats a single agent only "when demonstrations are not provided". [PR-conf; PARTIAL, arXiv HTML read through the results section] — [ACL Anthology](https://aclanthology.org/2024.acl-long.331); [arXiv HTML](https://arxiv.org/html/2402.18272v1)
  - Exact condition: ChatGPT-3.5 (gpt-35-turbo-0613), temperature 0.25, at most 3 rounds. ECQA (100 items), GSM8K (100 items), FOLIO-wiki (460 items). Table 2 averages, Direct / with Demo:
    - single agent 67.41 / 75.63
    - MAD (3 agents) 63.42 / 70.04
    - Debate (3) 71.67 / 73.88
    - Debate (6) 69.38 / 72.26
    - CMD (6) 70.75 / 74.46
  - With demonstrations, the single agent has the highest average. MAD is below the single agent in both settings.
  - Multi-LLM setting (Bard + Gemini Pro + ChatGPT): CMD 76.93 / 78.66 and ReConcile 76.11 / 78.36, against single Gemini Pro 74.38 / 78.59. With demos the multi-LLM gain over the best single model is about 0.1 points.
  - Error types identified: "Judge Mistake" and "Wrong Answer Propagation" (agents drop correct answers for a wrong consensus).
  - No API-call, token or cost figures are reported, so the comparison is not compute-matched.
- Smit, Grinsztajn, Duckworth, Barrett & Pretorius (ICML 2024, PMLR 235:45883–45905; arXiv v1 was titled "Are we going MAD? Benchmarking Multi-Agent Debate between Language Models for Medical Q&A"): "multi-agent debating systems, in their current form, do not reliably outperform other proposed prompting strategies, such as self-consistency and ensembling". [PR-conf; PARTIAL] — [PMLR](https://proceedings.mlr.press/v235/smit24a.html); [arXiv HTML v3](https://arxiv.org/html/2311.17371v3)
  - Setup: GPT-3.5-turbo (GPT-4 and Mixtral 8x7B tested on MedQA separately). Datasets: MedQA (4-option, 1,273), PubMedQA (500), MMLU clinical topics (123), CosmosQA (500), CIAR (50), GPQA (448), Chess.
  - Protocols: Society of Minds, Multi-Persona, ChatEval variants, Ensemble Refinement, against Medprompt, self-consistency and CoT.
  - MedQA Table 2 (best / median accuracy): Medprompt 0.65 / 0.63; Society of Minds 0.64 / 0.61; Ensemble Refinement 0.64 / 0.60; ChatEval 0.60 / 0.60; Self-Consistency 0.60 / 0.60; Single Agent 0.60 / 0.59; Multi-Persona 0.58 / 0.57.
  - Caveat: the extraction flagged that the text and Table 3 report different MedQA figures. Check the exact numbers against the PDF before quoting them in the thesis.
  - Cost: MAD typically needs more API calls. Table 3 MedQA costs are roughly $1.73–$5.19 for SoM and about $2.16 for Medprompt and self-consistency configurations.
  - Sensitivity: results vary strongly with hyperparameters. On medical datasets all systems except Multi-Persona beat single-agent methods. On non-medical datasets, single-agent methods beat every system except Multi-Persona and Self-Consistency.
  - Agreement modulation (prompting agents to agree X% of the time) improved Multi-Persona by about 15% on a 376-question USMLE subset, and SoM by about 5%.
- Zhang et al., "Stop Overvaluing Multi-Agent Debate" (arXiv 2502.08788 v3, June 2025): across 5 MAD methods, 9 benchmarks and 4 foundation models, MAD "often fails to outperform" CoT and Self-Consistency "even when consuming significantly more inference-time computation". They attribute inflated claims to limited benchmark coverage, weak baselines and inconsistent setups. Model heterogeneity is described as a "universal antidote" that consistently improves MAD. [preprint; ABSTRACT, no numbers seen] — [arXiv](https://arxiv.org/abs/2502.08788v3)

**Most of the debate gain is voting or ensembling**
- Choi, Zhu & Li, "Debate or Vote" (NeurIPS 2025 spotlight poster; arXiv 2508.17536 v2): majority voting explains most of the gains attributed to MAD. [PR-conf; PARTIAL] — [arXiv HTML](https://arxiv.org/html/2508.17536v2); [NeurIPS 2025](https://neurips.cc/virtual/2025/poster/116557)
  - Setup: N = 5 agents, T = 2/3/5 rounds. Seven benchmarks: Arithmetics, GSM8K, MMLU Professional Medicine, MMLU Formal Logic, HellaSwag, CommonsenseQA, HH-RLHF.
  - Qwen2.5-7B average accuracy: majority vote 0.7691; decentralised MAD 0.7377 (T=2), 0.7112 (T=3), 0.7050 (T=5); centralised MAD 0.6551–0.6672; single agent 0.7205.
  - Llama3.1-8B: majority vote 0.7242; decentralised MAD 0.6929 to 0.6761; single agent 0.6203.
  - MAD beats a single agent but not majority voting, and more rounds lower its accuracy.
  - Theory: with homogeneous, fully connected agents, belief in the correct answer is a martingale under debate, so its expectation does not improve.
  - Majority-vote-guided belief updates ("MAD-Conformist", "MAD-Follower") reach about 0.75–0.76.
  - No token-cost figures were found in the part read.
- Huang et al., "Large Language Models Cannot Self-Correct Reasoning Yet" (ICLR 2024 poster; arXiv 2310.01798 v2): this is the clearest equal-response comparison. [PR-conf; PARTIAL] — [arXiv HTML v2](https://arxiv.org/html/2310.01798v2); [ICLR 2024 listing](https://iclr.cc/virtual/2024/poster/18956)
  - Condition: gpt-3.5-turbo-0301, full GSM8K test set, 3 agents, 2 rounds, Du et al.'s prompt.
  - Debate at 6 responses: 83.2, against self-consistency at 6 responses: 85.3.
  - Debate at 9 responses: 83.0, against self-consistency at 9 responses: 88.2.
  - Standard prompting: 76.7. Self-consistency at 3 responses: 82.5.
  - The authors say debate "significantly underperforms" self-consistency at equal response counts. They attribute its gains to "consistency" across generations, not to correction.
  - Intrinsic self-correction lowers accuracy. GPT-3.5 standard vs self-correct round 2: GSM8K 75.9 → 74.7; CommonSenseQA 75.8 → 41.8; HotpotQA 26.0 → 25.0.
  - Earlier self-correction gains depended on oracle stopping labels (with oracle: GSM8K 84.3, CSQA 89.7).
- Li, Zhang, Yu, Fu & Ye, "More Agents Is All You Need" (TMLR 2024; OpenReview id bgzUSZ8aeg): plain sampling-and-voting ("Agent Forest") scales with the number of agents. [PR-journal (TMLR); PARTIAL] — [mlanthology TMLR record](https://mlanthology.org/tmlr/2024/li2024tmlr-more/); [arXiv HTML v2](https://arxiv.org/html/2402.05120v2)
  - Single query → ensemble of 40: GPT-3.5-Turbo GSM8K 0.73 → 0.85, MATH 0.29 → 0.39, MMLU 0.59 → 0.70. Llama2-13B GSM8K 0.35 → 0.59, above single-query Llama2-70B (0.54).
  - Gains grow with task difficulty. Relative gains on MATH: 200% for Llama2-13B, 34% for GPT-3.5. On GSM8K: 69% and 16%.
  - Used on its own, it outperformed LLM-Debate, Reflexion, SPP and CoT in most cases. Average rank: Ours 2.5, Debate 4.4.
  - Token use grows roughly in proportion to ensemble size.
- Wang et al., self-consistency (ICLR 2023; arXiv 2203.11171): sampling several reasoning paths and taking the majority answer improves CoT by +17.9% on GSM8K, +11.0% on SVAMP, +12.2% on AQuA, +6.4% on StrategyQA and +3.9% on ARC-challenge. This is the canonical "extra samples" baseline. [PR-conf; ABSTRACT; the venue was taken from an ICLR 2023 virtual-page listing in search results, not opened] — [arXiv](https://arxiv.org/abs/2203.11171v4); [ICLR 2023 listing](https://iclr.cc/virtual/2023/poster/11718)
- Chen, Davis, Hanin, Bailis, Stoica, Zaharia & Zou, "Are More LLM Calls All You Need?" (NeurIPS 2024): Vote and Filter-Vote performance can "first increase but then decrease" as the number of LM calls grows. More calls help easy queries and hurt hard ones, so more compute is not monotonically better and there is an optimal number of calls. [PR-conf; ABSTRACT] — [NeurIPS 2024](https://neurips.cc/virtual/2024/poster/93777)

**Homogeneous small-model debate can lose to isolated self-correction**
- Bertalanič & Fortuna, "The Cost of Consensus" (arXiv 2605.00914, April 2026): [preprint; PARTIAL] — [arXiv PDF](https://arxiv.org/pdf/2605.00914)
  - Setup: 10 identical agents, 3 rounds, full connectivity, plurality vote. Agent count, rounds and prompt structure are matched between debate and isolated self-correction, but debate used 2.1–3.4× more tokens.
  - Debate never beat self-correction. Round 3 examples: GSM-Hard Ministral-3-8B 20.7% vs 48.3%; MMLU-Hard Qwen2.5-7B 60.7% vs 66.7%.
  - Noise-injection controls did as well as or better than debate.
  - Limits: 7–8B models, mostly 100-item subsets, and the Llama differences were not significant.

**Token-matched comparison (2026)**
- Tran & Kiela, "Single-Agent LLMs Outperform Multi-Agent Systems on Multi-Hop Reasoning Under Equal Thinking Token Budgets" (arXiv 2604.02460; v1 2026-04-02, v2 2026-04-11; no journal-ref or comments, so no published version found). [preprint; ABSTRACT] — [arXiv abs](https://arxiv.org/abs/2604.02460)
  - Models: Qwen3, DeepSeek-R1-Distill-Llama and Gemini 2.5.
  - With reasoning tokens held constant, single-agent systems "consistently matched or outperformed" multi-agent systems on multi-hop reasoning. The authors also give an information-theoretic argument (the Data Processing Inequality).
  - They predict multi-agent systems become competitive only when single-agent context use degrades or when more compute is spent.
  - API-based budget control (notably in Gemini 2.5) and benchmark artifacts can inflate apparent multi-agent gains.
  - No numbers were on the abstract page.

**When multi-agent helps, and when it hurts, at scale**
- Kim et al., "Towards a Science of Scaling Agent Systems" (arXiv 2512.08296 v3, 2026-04-08; Google Research, MIT and others; no venue listed): [preprint; PARTIAL, v3 HTML read up to the methods; Appendix E (budget protocol) not reached] — [arXiv abs](https://arxiv.org/abs/2512.08296); [arXiv HTML](https://arxiv.org/html/2512.08296)
  - Scale: 260 configurations, six benchmarks (BrowseComp-Plus, Finance-Agent, PlanCraft, WorkBench, SWE-bench Verified, Terminal-Bench), five architectures (single-agent; independent, centralised, decentralised and hybrid multi-agent), three LLM families. The paper states "identical task prompts, tools, and computational budgets across all configurations".
  - Results range from +80.8% (centralised, decomposable financial reasoning) to −70.0% (independent, sequential planning). All multi-agent variants degraded planning by 39–70%.
  - Capability saturation: above about 45% single-agent accuracy, adding agents gives negative returns (β = −0.236, p = 0.004).
  - Tool-coordination trade-off: β = −0.096, p = 0.002.
  - Trace-level error amplification: 17.2× for independent agents, 4.4× under centralised coordination.
  - The predictive model explains only modest variance (cross-validated R² = 0.373) and picks the best architecture for 87% of held-out configurations.
  - No healthcare benchmark is included.
- Li, "When Single-Agent with Skills Replace Multi-Agent Systems and When They Fail" (arXiv 2601.04748 v2, January 2026; UBC/Vector): [preprint; PARTIAL] — [arXiv HTML v2](https://arxiv.org/html/2601.04748v2)
  - Method: "compiles" multi-agent systems into one agent that selects skills. Tested on GSM8K, HumanEval and HotpotQA.
  - The skill agent matched multi-agent accuracy while using on average 54% fewer tokens and 50% less latency. The efficiency results are described as "preliminary".
  - Failure condition: skill-selection accuracy shows a "phase transition" (stable, then a sharp drop) as the library grows from 5 to 200 skills. The drop is driven by semantic confusability; hierarchical routing helps.
  - Not compilable: debate/adversarial setups, parallel sampling, private information, heterogeneous backbones.
  - Exact per-benchmark numbers and the threshold were not in the part read.
- Wynn, Satija & Hadfield, "Talk Isn't Always Cheap" (arXiv 2509.05396 v2, October 2025): [preprint; PARTIAL] — [arXiv HTML v2](https://arxiv.org/html/2509.05396v2)
  - Setup: 2 rounds, majority vote; 100 items per task, averaged over 5 seeds.
  - Debate lowered accuracy in several groups: 3×Mistral MMLU 33.6 → 24.4; 1 LLaMA + 2 Mistral MMLU 40.0 → 28.0. Every configuration declined on CommonSenseQA.
  - Debate helped a diverse group (GPT-4o-mini + LLaMA + Mistral): MMLU 57.8 → 63.4; GSM8K 86.8 → 90.2.
  - Gains depend on group composition.

### Inferences
- Across the controlled studies, the comparison that decides the result is the baseline. Multi-agent wins against a single greedy call (Choi: MAD > single agent). It loses or ties against self-consistency or majority voting at equal samples (Huang, Choi, Smit, Zhang, Li-TMLR), and against strong prompts with demonstrations (Wang ACL 2024). For the thesis this means the single-agent baseline in RQ4 must include a sampling-and-voting arm, not only a single call.
- The conditions favouring multi-agent are model heterogeneity (Wang multi-LLM; Wynn diverse group; Zhang "universal antidote"), decomposable or parallel subtasks (Kim +80.8%) and centralised verification that limits error amplification (Kim 4.4× vs 17.2×). These fit the proposed design: a central deterministic orchestrator, role-specialised rather than debating agents, and an explicit verifier. The fit is design motivation only. None of these studies tests a clinical pipeline like the proposed one.
- Clinical decision-making is partly sequential (information gathering, then interpretation, then decision). Kim's −39% to −70% on sequential planning and the tool-overhead coefficient are a warning that a multi-stage LLM pipeline may cost accuracy compared with one capable agent given the same tools. This is a plausible null or negative outcome for RQ4, and the thesis should anticipate it.

### Gaps
- Exact Smit et al. MedQA figures are internally inconsistent in the extraction; check the PDF.
- The full text of Du et al. and Liang et al. was not read (abstract only), so their baselines and compute use were not checked.
- Zhang et al. ("Stop Overvaluing") per-benchmark numbers were not visible; only the abstract was read.
- Kim et al.'s exact compute-matching protocol (Appendix E) was not reached; "matched compute" is the authors' description.
- Li (2026) skill-agent accuracy numbers and the critical library size were not in the part read.

---

## KQ2. What failure modes of multi-agent systems are documented, and on what evidence?

### Takeaway
Failure modes are well documented, but mostly in preprints, plus one peer-reviewed taxonomy (MAST, NeurIPS 2025 spotlight). The recurring mechanisms are:
- specification and design faults, inter-agent misalignment and missing or weak verification (MAST);
- error amplification without central verification (Kim);
- conformity and sycophancy, where agents flip from correct to incorrect answers and correct minorities are dropped (Wynn, Yao, Bertalanič, Wang ACL 2024);
- the judge picking the wrong answer (Wang);
- in medicine, false consensus, information loss, voting in place of weighing evidence, and failure to prioritise high-risk outcomes (MedAgentAudit), plus incompatible information flow between agents built from separately optimised components (Optimization Paradox).

### Cited Findings
- **MAST**: Cemri, Pan, Yang, Agrawal, Chopra, Tiwari, Keutzer, Parameswaran, Klein, Ramchandran, Zaharia, Gonzalez & Stoica, "Why Do Multi-Agent LLM Systems Fail?" (NeurIPS 2025 spotlight poster; the track, main or Datasets & Benchmarks, is not stated on the page; arXiv 2503.13657 v3). [PR-conf; ABSTRACT this session; an earlier note also read the arXiv and project pages] — [NeurIPS 2025](https://neurips.cc/virtual/2025/poster/121528); [arXiv v3](https://arxiv.org/abs/2503.13657v3)
  - The abstract opens: "performance gains on popular benchmarks are often minimal".
  - Taxonomy built from 150 traces with κ = 0.88: 14 failure modes in 3 categories (system design issues, inter-agent misalignment, task verification).
  - MAST-Data: 1,600+ annotated traces from 7 frameworks, with GPT-4, Claude 3, Qwen2.5 and CodeLlama, on coding, math and general agent tasks. Includes an LLM-as-judge annotator.
- **Error amplification**: independent multi-agent systems amplify trace-level errors 17.2×, against 4.4× with centralised coordination. "Architectures without centralized verification tend to propagate errors more." [preprint] — [Kim et al. arXiv](https://arxiv.org/html/2512.08296)
- **Wrong-answer propagation and judge errors** in multi-agent discussion: Wang et al. ACL 2024 identify "Judge Mistake" and "Wrong Answer Propagation". [PR-conf] — [arXiv HTML](https://arxiv.org/html/2402.18272v1)
- **Conformity and social pressure** (Wynn et al.): among agents that changed answers, correct-to-incorrect flips outnumbered incorrect-to-correct flips, and the gap persisted across rounds. Agents that resisted in round 1 were more likely to flip in round 2. Flipping was most likely when an agent was isolated. A "correctness payoff" prompt did not reduce flips and often increased them, so sycophancy alone does not explain the failures. [preprint] — [arXiv HTML v2](https://arxiv.org/html/2509.05396v2)
- **Sycophancy** (Yao et al., "Peacemaker or Troublemaker", arXiv 2509.23055, AWS AI Labs): [preprint; PARTIAL] — [arXiv PDF](https://arxiv.org/pdf/2509.23055)
  - Setup: Qwen3-32B and LLaMA-3.3-70B, MMLU-Pro (1,000) and CommonsenseQA, 5 rounds.
  - Disagreement collapse rate reached 81.71% (Qwen-Qwen) and 86.36% (Llama-Llama) in decentralised debate on CSQA. It fell to 41.27% and 35.51% with a centralised judge.
  - Negative agreement rate (abandoning a correct position) correlated with a sycophancy score at r = 0.902.
  - Decentralised debate often fell below the best single agent (CSQA: 83.62% Qwen-Qwen vs 85.50% single).
  - The authors suggest capping debate at 2–3 substantive exchanges.
- **Consensus cost** (Bertalanič & Fortuna): modal sycophancy reached 85.5%. The "oracle gap" (a correct answer was generated by some agent but discarded by the plurality vote) reached 32.3 points (Ministral, GSM-Hard). [preprint] — [arXiv PDF](https://arxiv.org/pdf/2605.00914)
- **Majority dynamics**: Choi et al. show debate as a martingale (no expected improvement) for homogeneous agents. "MAD-oracle", which locks in a correct answer once any agent finds it, raised decentralised MAD (T=5, Qwen) from 0.7084 to 0.8289. Correct answers are often present but lost. [PR-conf] — [arXiv HTML v2](https://arxiv.org/html/2508.17536v2)
- **Medical: MedAgentAudit** (Gu, Zhu, Sang, Wang, Sui, Tang, Harrison, Gao, Yu & Ma; arXiv 2510.10185 v1; no venue seen). [preprint; PARTIAL] — [arXiv HTML](https://arxiv.org/html/2510.10185v1)
  - Scope: 3,600 interaction logs from 6 medical multi-agent frameworks (ColaCare, ReConcile, MDAgents, MedAgents, MAC, HealthcareAgent) on 6 datasets (MedQA, PubMedQA, MedXpertQA, PathVQA, VQA-RAD, SLAKE). Annotation κ = 0.82 on 200 held-out logs.
  - Failure shares: flawed consensus from LLM deficiencies 19.7%; incorrect medical knowledge 13.5%; sole reliance on the initial judgment 10.1%; minority suppression 5.9%.
  - 68.6% of successful cases involved "superfluous collaboration": all agents were already correct at the start.
  - Successful correction of the majority by a minority was "around 10%".
  - Average missing rate of key evidence units: 11.23% (HealthcareAgent) to 62.92% (MAC).
  - Clinical-priority mismatch above 70% for every framework (72.16–84.18%).
  - Voting in place of weighing evidence: 46.46% in ColaCare, about 30% in most frameworks.
  - Conclusion: collaboration often "favors conformity, not correction".
  - No direct single-agent comparison.
- **Medical: Optimization Paradox** (Bedi, Mlauzi, Shin, Koyejo & Shah, Stanford; arXiv 2506.06574 v2). [preprint; PARTIAL] — [arXiv PDF](https://arxiv.org/pdf/2506.06574)
  - Data: MIMIC-CDM, 2,400 real ED cases (appendicitis, pancreatitis, cholecystitis, diverticulitis); 1,190 development / 1,190 test. 8 single-agent and 26 multi-agent systems.
  - A "best-of-breed" system assembled from the best component per stage reached 67.65% diagnostic accuracy. The top multi-agent system reached 77.39% (−9.75 points; McNemar p < 0.0001) and the top single agent 75.63%.
  - The best-of-breed system hallucinated test results in 13.87% of cases (vs 0.42%) and ordered unauthorised tests in 13.87% (vs 0.76%).
  - Multi-agent systems won on information gathering (78.4% of comparisons) and interpretation (87.5%), but not on diagnostic accuracy (52.9% win rate, not significant).
- **Medical, workshop**: Al Jorf & Shamout, "When Teamwork Fails" (ICML 2026 workshop on failure modes in agentic AI). Multi-agent systems on multimodal ICU mortality prediction show three structural failures, and zero-shot sensitivity to input perturbation grows with agentic complexity. [workshop; ABSTRACT, no numbers] — [ICML 2026](https://icml.cc/virtual/2026/77963)
- **Consensus trap** in mixed-vendor clinical teams: a correct minority diagnosis gets voted down, and single-vendor teams can underperform their base model because their failure modes are correlated. [preprint] — [Yuan et al. arXiv HTML](https://arxiv.org/html/2603.04421v1)
- **Behaviours seen in agent logs** (HAL, ICLR 2026): agents searched HuggingFace for the benchmark instead of solving the task, and misused credit cards in flight-booking tasks. These are single and multi-agent agentic failures that accuracy alone hides. [PR-conf; ABSTRACT] — [ICLR 2026 proceedings](https://proceedings.iclr.cc/paper_files/paper/2026/hash/a0928f924a344aaebbb7f6cd8d56e34c-Abstract-Conference.html)

### Inferences
- Several failure modes map directly onto the proposed design choices:
  - conformity, judge error and voting in place of evidence → no free-form debate or voting; a deterministic decision core;
  - missing verification and error amplification → a verifier plus centralised orchestration;
  - information loss across agents → structured shared state and a grounding check;
  - failure to prioritise high-risk outcomes → deterministic red-flag rules before any LLM.
  This is a defensible design rationale for Chapter 2.3. It is not evidence that the design avoids these failures; RQ2 and RQ4 must test that.
- The "superfluous collaboration" finding (68.6% of successes needed no collaboration) and the "oracle gap" together suggest a component-ablation study (RQ4) should report per-case flip analysis: cases the pipeline fixed versus cases it broke relative to the single agent. Mean accuracy alone is not enough.
- The Optimization Paradox shows that component-level scores do not predict end-to-end safety. That supports end-to-end evaluation of status and unsafe-recommendation rates (RQ1), not only per-agent metrics.

### Gaps
- Most conformity and sycophancy evidence uses small open models (7–70B) on MCQ benchmarks. Evidence with frontier models in clinical, free-text, multi-step settings is thin.
- MedAgentAudit, the Optimization Paradox, "Talk isn't cheap", "Peacemaker" and "Cost of Consensus" have no peer-reviewed venue that I could confirm.
- The MAST NeurIPS track (main vs Datasets & Benchmarks) is unconfirmed.
- Who&When (failure attribution, ICML 2025) and work on adversarial or malicious agents were not reviewed this session.

---

## KQ3. How much do extra computation, sampling, voting and longer reasoning explain? How should fair, compute-matched comparisons be designed and reported?

### Takeaway
The extra-compute explanation is strong. Sampling-and-voting alone scales with the number of samples (Li TMLR 2024; self-consistency), and it beats or matches debate at equal responses (Huang ICLR 2024; Choi NeurIPS 2025). More calls can also hurt (Chen NeurIPS 2024), and more reasoning effort reduced accuracy in most HAL runs (ICLR 2026). In medicine, reasoning models and plain retrieval often match or beat agent frameworks (MedAgentsBench; TAGS). The methodological consensus (Kapoor TMLR 2025; HAL ICLR 2026) is to report cost with accuracy, use Pareto frontiers and simple strong baselines, keep proper holdouts, standardise harnesses, and report variance.

### Cited Findings

**Compute and test-time scaling as confound**
- Sampling-and-voting gains scale with ensemble size, and token use grows roughly in proportion to ensemble size. [PR-journal TMLR] — [arXiv HTML](https://arxiv.org/html/2402.05120v2)
- At equal response counts, self-consistency beats debate on GSM8K: 85.3 vs 83.2 at 6 responses and 88.2 vs 83.0 at 9 responses (GPT-3.5). The authors call for "in-depth inference cost analysis" against strong multi-response baselines. [PR-conf] — [arXiv HTML](https://arxiv.org/html/2310.01798v2)
- Non-monotonic call scaling: Vote and Filter-Vote can first rise and then fall with more LM calls. [PR-conf] — [NeurIPS 2024](https://neurips.cc/virtual/2024/poster/93777)
- HAL (Kapoor, Stroebl, Kirgis, … Liang, Narayanan; ICLR 2026, pp. 98778–98849): 21,730 rollouts, 9 models, 9 benchmarks (coding, web navigation, science, customer service), about $40,000 total. "Higher reasoning effort reduced accuracy in the majority of runs". 2.5B tokens of logs released. [PR-conf; ABSTRACT] — [ICLR 2026 proceedings](https://proceedings.iclr.cc/paper_files/paper/2026/hash/a0928f924a344aaebbb7f6cd8d56e34c-Abstract-Conference.html)
- MAD often fails to beat CoT and Self-Consistency "even when consuming significantly more inference-time computation". [preprint] — [arXiv](https://arxiv.org/abs/2502.08788v3)

**Medical compute and retrieval confounds**
- MedAgentsBench (Tang et al., Yale/Stanford/UTSW; arXiv 2503.07459 v2; no venue seen): [preprint; PARTIAL] — [arXiv HTML v2](https://arxiv.org/html/2503.07459v2)
  - Data: a HARD subset of 862 questions from 8 sources (the paper also says "seven" in one place). Questions were kept if fewer than 50% of models answered them correctly.
  - DeepSeek-R1 and o3-mini "consistently excel" and sit on the cost-performance Pareto frontier.
  - The multi-agent frameworks MedAgents and MDAgents are not consistently better and carry heavier overhead.
  - GPT-4o averages on HARD: CoT 24.3%, CoT-SC 25.3%, MedAgents 20.0%, MDAgents 16.1%, AFlow 28.9%. MedAgents and MDAgents fall below CoT-SC.
  - Caveat: the method tables use GPT-4o and GPT-4o-mini, while thinking models are compared as base models, so those two sets of results are not directly comparable.
- TAGS (Wu et al., Findings of ACL 2026, pp. 15428–15445): [PR-conf (Findings); PARTIAL] — [ACL Anthology PDF](https://aclanthology.org/2026.findings-acl.757.pdf)
  - With DeepSeek-R1 on 9 HARD medical subsets, plain RAG (35.4 average) beat Multi-Persona (33.9), CoT-SC (32.4) and MedAgents (32.1).
  - With GPT-4o: MedAgents 20.0 and MDAgents 16.1, against Self-Refine 26.9.
  - Latency on MedQA with GPT-4o: TAGS 72 s per question, CoT-SC 27.7 s, Multi-Persona 109.6 s.
  - The paper does not control compute.
- MediHive (Wang & Yang, Drexel; reported published in Journal of Healthcare Informatics Research, May 2026, according to the arXiv HTML; DOI not found): [PR-journal per arXiv HTML, not checked on the publisher page; PARTIAL] — [arXiv HTML](https://arxiv.org/html/2603.27150)
  - With the backbone matched (Llama-3.1-70B), the best single agent (few-shot CoT + SC) scored 74.4 on MedQA and 73.8 on PubMedQA.
  - MediHive scored 84.3 and 78.4; Multiagent Debate 80.4 and 78.2.
  - Compute was not matched or reported ("no detailed assessment of computational efficiency"). Single runs, no significance tests.
  - This counter-example favours multi-agent, but compute is unmatched.

**Fair-evaluation methodology**
- Kapoor, Stroebl, Siegel, Nadgir & Narayanan, "AI Agents That Matter" (TMLR 2025; OpenReview Zy4uFzMviZ): [PR-journal TMLR; PARTIAL] — [mlanthology TMLR](https://mlanthology.org/tmlr/2025/kapoor2025tmlr-ai/); [arXiv HTML](https://arxiv.org/html/2407.01502v1)
  - HumanEval (164 problems, mean of 5 runs, April 2024 prices): simple baselines matched or beat complex agents. Warming (GPT-4) 93.2% at $2.45 and Retry 92.0% at $2.51, against Reflexion 87.8% at $3.90, LATS (GPT-4) 88.0% at $134.50, and LDB+Reflexion 92.9% at $7.26.
  - Accuracy-cost Pareto frontier, made convex by mixing agents. Zero-shot GPT-4 was not on the frontier.
  - Joint optimisation on HotPotQA cut variable cost by about 53% (GPT-3.5) and 41% (Llama-3-70B) at similar accuracy.
  - Holdouts: appropriate holdouts in 1/1 distribution-specific, 3/6 task-specific, 1/8 domain-general and 0/2 fully general benchmarks.
  - Reproducibility: many reported accuracies were above the maximum of 5 reproduction runs. Benchmark subsets were changed (problems removed or tests added). Error bars were rare; one SWE-agent run could exceed $8,000.
  - Recommendations: report dollar cost and token counts; separate model evaluation from downstream evaluation; use generality-matched secret holdouts; standardise scripts; report confidence intervals.
- HAL (ICLR 2026): a standardised parallel harness that cut evaluation time "from weeks to hours" and reduced implementation bugs. It analyses models × scaffolds × benchmarks jointly, and uses LLM-aided log inspection to find shortcuts and unsafe actions. [PR-conf] — [ICLR 2026](https://proceedings.iclr.cc/paper_files/paper/2026/hash/a0928f924a344aaebbb7f6cd8d56e34c-Abstract-Conference.html)
- Kim et al. hold "identical task prompts, tools, and computational budgets" constant across architectures, and report an efficiency metric (success per unit of overhead). [preprint] — [arXiv HTML](https://arxiv.org/html/2512.08296)
- Smit et al. report cost, time and accuracy together and show strong hyperparameter sensitivity. This argues for equal tuning effort across arms. [PR-conf] — [PMLR](https://proceedings.mlr.press/v235/smit24a.html)
- Zhang et al. list the evaluation flaws to avoid: limited benchmark coverage, weak baselines, inconsistent setups. [preprint] — [arXiv](https://arxiv.org/abs/2502.08788v3)
- Paired significance testing: Dror et al. (ACL 2018) give a protocol for choosing tests. This was verified in the earlier note at abstract level only, and whether it covers McNemar was not confirmed. Bedi et al. use McNemar for paired diagnostic accuracy on the same cases. — [ACL Anthology](https://aclanthology.org/P18-1128/); [Bedi et al.](https://arxiv.org/pdf/2506.06574)

### Inferences
A compute- and information-matched design for RQ4, assembled from the sources above:
1. **Hold constant**:
   - backbone model and exact version/snapshot, same for every arm (Wang ACL 2024's multi-LLM result shows mixing models is itself a treatment);
   - decoding (temperature, top_p, max tokens);
   - prompts and demonstrations (Wang ACL 2024: demonstrations erase the multi-agent advantage);
   - tools, the retrieval corpus and retriever settings (TAGS: plain RAG beat medical multi-agent frameworks);
   - the input case information and its order;
   - the output schema and option set;
   - the deterministic pre- and post-processing.
2. **Baseline arms**:
   - (a) a single call;
   - (b) a single agent with the same tools and retrieval;
   - (c) the same as (b) plus self-consistency or majority vote with N matched to the multi-agent system's call count, or better its total token count (Huang; Li-TMLR; Choi);
   - (d) optionally, a reasoning or "thinking" model at matched cost (MedAgentsBench).
3. **Report per arm**: accuracy and safety outcomes with confidence intervals; input and output tokens; number of LLM calls; wall-clock latency (median and p90); dollar cost at stated prices. Plot the cost–accuracy Pareto frontier (Kapoor; HAL).
4. **Tune every arm equally** (Smit's hyperparameter sensitivity). Use a held-out test split that is not seen during prompt development (Kapoor; Bedi used a dev/test split).
5. **Statistics**: paired tests on the same cases, such as McNemar or a paired bootstrap. Add per-case flip analysis (cases fixed vs cases broken). Run several seeds or report variance (Kapoor; the Wynn and Yuan caveats).
6. **Ablation**: remove one component at a time (critic, verifier, retrieval). Because of the Optimization Paradox, also test the integrated pipeline end to end.

Other inferences:
- Because more calls can lower accuracy (Chen) and more reasoning effort lowered it in most HAL runs, the matched-compute arm should be swept over a few budgets, not set to one N.
- Medical evidence in which multi-agent beats a self-consistency single agent (MediHive +7.3 points average) exists but is not compute-matched. Thesis wording should therefore treat multi-agent superiority in medicine as unresolved.

### Gaps
- No source gives a standard formula for "compute-matching" (calls vs tokens vs FLOPs vs dollars). Studies match different quantities: Huang matches responses; Bertalanič matches agents and rounds but not tokens; Kim states matched budgets but the protocol was not read. The thesis must state and justify its own choice.
- MedAgentsBench per-sample cost and time figures appear only in figures, so no numbers were extracted.
- HAL per-model findings beyond the abstract were not read.

---

## KQ4. Are there medical-domain studies that test single vs multi-agent under matched conditions?

### Takeaway
Only partially. No medical study found gives a strictly compute-matched (token- or cost-matched), tool- and retrieval-matched single- vs multi-agent comparison. The closest studies are:
- MedAgentBoard (NeurIPS 2025): multi-agent "does not consistently outperform advanced single LLMs";
- Smit et al. (ICML 2024): medical QA; MAD does not reliably beat self-consistency or Medprompt;
- MedAgentsBench (preprint): thinking models beat agent frameworks;
- Bedi et al. (preprint, real MIMIC ED cases): there is no significant diagnostic-accuracy advantage for multi-agent over single-agent, though multi-agent wins on process metrics;
- Yuan et al. (preprint, rare disease): mixed-vendor teams beat single LLMs, but compute is unmatched and samples are small.
The earlier session's systematic review (Gorenshtein et al., medRxiv) reportedly found no direct multi- vs single-agent comparison among its 20 clinical studies.

### Cited Findings
- MedAgentBoard (Zhu, He, Hu, Zheng, Zhang, Wang, Gao, Ma & Yu; NeurIPS 2025 poster, track not stated; arXiv 2505.12371): four task types. Multi-agent "does not consistently outperform advanced single LLMs" and falls short on textual medical QA. Conventional specialised methods do better on medical VQA and EHR prediction. Multi-agent helps with task completeness in workflow automation. The authors say complexity and overhead should be weighed against gains. [PR-conf; ABSTRACT] — [NeurIPS 2025](https://neurips.cc/virtual/2025/poster/121792)
- Smit et al. (ICML 2024): the medical QA results are under KQ1. On the medical datasets most MAD systems beat single-agent CoT, but they did not reliably beat self-consistency or Medprompt. [PR-conf] — [arXiv HTML v3](https://arxiv.org/html/2311.17371v3)
- Choi et al. (NeurIPS 2025) include MMLU Professional Medicine among seven benchmarks. Majority voting ≥ MAD on average. [PR-conf] — [arXiv HTML v2](https://arxiv.org/html/2508.17536v2)
- Bedi et al. (preprint), on real ED data: across 8 single-agent and 26 multi-agent systems, multi-agent was significantly better on information gathering (78.4% win rate, p = 0.015) and interpretation (87.5%, p = 0.0008). It was not better on diagnostic accuracy (52.9% win rate). The top single agent reached 75.63% and the top multi-agent system 77.39%. Matched compute was not established. A "computational cost" metric is reported, but its units were not visible. [preprint] — [arXiv PDF](https://arxiv.org/pdf/2506.06574)
- Yuan, Zhang, Kim & Rajpurkar, "Do Mixed-Vendor Multi-Agent LLMs Improve Clinical Diagnosis?" (arXiv 2603.04421; MIT/Harvard): [preprint; PARTIAL] — [arXiv HTML](https://arxiv.org/html/2603.04421v1)
  - Setup: o4-mini, Gemini-2.5-Pro and Claude-4.5-Sonnet; RareBench (MME 40, HMS 88, LIRICAL 370) and DiagnosisArena (165).
  - Mixed-vendor teams beat the best single LLM: combined RareBench R@1 39.31 vs 37.58; DiagnosisArena Top-1 36.36% vs 32.12%.
  - Single-vendor teams sometimes underperformed their base model: combined RareBench R@1 33.32 (Claude MAC) vs 37.58 (best single LLM, Gemini).
  - Tokens, cost and latency were not controlled. Single runs, no significance tests. The o4-mini judge is also a model under test.
- MedAgentsBench (preprint): agent frameworks are not consistently better than thinking models and have higher overhead. [preprint] — [arXiv HTML v2](https://arxiv.org/html/2503.07459v2)
- MediHive (journal per the arXiv HTML) and TAGS (Findings of ACL 2026): both report multi-agent or generalist–specialist gains over single-agent prompting on medical MCQ without compute matching. In TAGS, plain RAG with DeepSeek-R1 beat MedAgents. — [MediHive arXiv HTML](https://arxiv.org/html/2603.27150); [TAGS](https://aclanthology.org/2026.findings-acl.757.pdf)
- Gorenshtein et al. (medRxiv 2025 systematic review), as reported in the earlier verified note and not re-checked this session (SECONDARY): "no study provides a comparison of multi-agent to single-agent" among its 20 included clinical agent studies; cost and runtime were seldom reported and latency not at all. — [medRxiv PDF (earlier note)](https://www.medrxiv.org/content/10.1101/2025.08.22.25334232.full.pdf)

### Inferences
- The medical evidence does not support saying "multi-agent beats single-agent" in clinical decision support. The strongest comparison on real data (Bedi) shows parity on the final diagnosis, with multi-agent advantages only on process metrics. This is directly relevant to the proposed RQ1/RQ4 split between decision quality and process or safety outcomes.
- The mixed-vendor result (heterogeneity helps; homogeneous teams can hurt) agrees with Zhang et al. and Wynn et al. The proposed framework uses one backbone with role-specialised agents and no debate. The literature suggests its value would have to come from structure, verification and deterministic rules, not from diversity of opinions. That should be stated as a hypothesis, not assumed.
- P4 is supported: the medical literature lacks tool-, retrieval- and compute-matched baselines, latency and cost reporting, and safety-focused outcomes in single vs multi comparisons.

### Gaps
- No medical study with token- or cost-matched arms was found. My searches were limited (see the search log), so a study may exist that I missed.
- None of the medical comparisons evaluate abstention, escalation or unsafe-recommendation outcomes (P2, RQ1) for single vs multi.
- MedAgentBoard's full text and its exact numbers were not read this session.

---

## Synthesis: what the evidence establishes, contradictions, gaps, and unverified items

### Takeaway
For Chapter 2.2/2.3: strong, compute-aware single-agent baselines often match multi-agent systems. Much of the reported multi-agent gain comes from sampling and voting. Multi-agent interaction has documented failure modes (conformity, error amplification, verification gaps). Medical evidence under matched conditions is scarce and mixed. RQ4's matched-baseline design is therefore needed, and a null result would be consistent with the literature.

### Cited Findings
**(a) What the evidence establishes**
- Equal-response self-consistency beats multi-agent debate (GSM8K, GPT-3.5). — [Huang et al. ICLR 2024](https://arxiv.org/html/2310.01798v2)
- Majority voting explains most MAD gains, and debate alone is a martingale for homogeneous agents. — [Choi et al. NeurIPS 2025](https://arxiv.org/html/2508.17536v2)
- At equal thinking-token budgets, single agents match or beat multi-agent systems on multi-hop reasoning (preprint). — [Tran & Kiela 2026](https://arxiv.org/abs/2604.02460)
- Strong prompts let a single agent nearly match multi-agent discussion. — [Wang et al. ACL 2024](https://aclanthology.org/2024.acl-long.331)
- MAD does not reliably beat self-consistency or ensembling, including on medical QA. — [Smit et al. ICML 2024](https://proceedings.mlr.press/v235/smit24a.html)
- Performance scales with the number of sampled agents (sampling-and-voting). — [Li et al. TMLR 2024](https://mlanthology.org/tmlr/2024/li2024tmlr-more/)
- Simple, cheap baselines match complex agents, so cost must be reported. — [Kapoor et al. TMLR 2025](https://mlanthology.org/tmlr/2025/kapoor2025tmlr-ai/)
- Multi-agent systems fail in 14 documented ways. — [MAST, NeurIPS 2025](https://neurips.cc/virtual/2025/poster/121528)

**(b) Contradictions**
- Multi-agent > single agent in Du et al. and Liang et al. (against weak baselines) vs ≤ self-consistency in Huang, Choi and Smit. The conflict is resolved mainly by which baseline is used. — [Du](https://proceedings.mlr.press/v235/du24e.html); [Liang](https://aclanthology.org/2024.emnlp-main.992); [Huang](https://arxiv.org/html/2310.01798v2)
- Heterogeneity helps (Wynn's diverse group; Yuan's mixed vendors; Zhang) vs heterogeneous best-of-breed components hurt (Bedi: 71.22% median for heterogeneous vs 74.29% homogeneous, not significant after Bonferroni). Heterogeneity of opinions and heterogeneity of pipeline components are different things. — [Wynn](https://arxiv.org/html/2509.05396v2); [Yuan](https://arxiv.org/html/2603.04421v1); [Bedi](https://arxiv.org/pdf/2506.06574)
- Kim: +80.8% on decomposable tasks vs −39% to −70% on sequential planning. The direction depends on task structure. — [Kim et al.](https://arxiv.org/html/2512.08296)
- MediHive (+7.3 points over a self-consistency single agent, medical MCQ) vs MedAgentsBench and TAGS (MedAgents and MDAgents below CoT-SC or RAG on HARD subsets). Different backbones, subsets and difficulty filtering; none is compute-matched. — [MediHive](https://arxiv.org/html/2603.27150); [MedAgentsBench](https://arxiv.org/html/2503.07459v2); [TAGS](https://aclanthology.org/2026.findings-acl.757.pdf)
- Smit: MAD beats single-agent methods on medical datasets but not on non-medical ones. Domain may moderate the effect. — [Smit et al.](https://arxiv.org/html/2311.17371v3)

### Inferences
**(c) Gaps relevant to P1–P4 and RQ1–RQ4**
- P4 / RQ4: no clinical study found with tool-, retrieval- and token-matched single-agent arms plus a cost, latency and token report. This is the central gap the project can fill.
- RQ1 / P2: no single vs multi study evaluates abstention, escalation, NEED_MORE_INFO-type statuses or unsafe-recommendation rates.
- RQ2 / P3: verification is named as a failure category (MAST; Kim's centralised-verification effect), but no controlled study isolates a claim-to-source verifier's effect inside a multi-agent pipeline against a single agent with the same verifier.
- RQ3 / P1: order and wording robustness of multi-agent vs single-agent systems is barely tested. Al Jorf & Shamout report rising perturbation sensitivity with agentic complexity (workshop abstract, no numbers).
- No agreed definition of "compute-matched" exists (calls vs tokens vs cost).

### Gaps
**(d) Inaccessible or unverified items**
- Bash access to the Semantic Scholar, OpenAlex, Crossref, arXiv and DBLP APIs was blocked. Metadata was checked on proceedings and abstract pages instead.
- Full text not read: Du et al., Liang et al., self-consistency, Chen et al. (NeurIPS 2024), MAST, HAL, MedAgentBoard, Zhang et al. (all at abstract level).
- Partial reads: Wang ACL 2024 (~46% of the HTML), Kim et al. (methods appendix not reached), Li 2026 skills (~40%), Choi et al. (~29%).
- MediHive's journal publication is asserted in the arXiv HTML only; the publisher page and DOI were not checked.
- MAST's NeurIPS 2025 track, MedAgentBoard's NeurIPS 2025 track and the Debate-or-Vote track are not stated on the pages read.
- The Smit et al. MedQA numbers are internally inconsistent in the extraction; verify against the PDF.
- The Gorenshtein review was not re-checked (SECONDARY via the earlier note).
- Not reviewed this session: Who&When (failure attribution), Snell et al. test-time compute scaling (ICLR 2025), and Large Language Monkeys (repeated sampling).
- Compute-matched 2026 preprints the coordinator flagged:
  - 2604.02460 (Tran & Kiela): verified at abstract level and added. It is a preprint with no published version.
  - 2605.00914 (Cost of Consensus; Bertalanič & Fortuna): read in part. It is a preprint; the ACM template uses a placeholder venue, so no published version is confirmed.
  - 2611.17610 ("PACE") and 2609.04217: NOT verified. Searches for both IDs returned no matching record, and direct arXiv URLs are blocked unless a search surfaces them. Their authors, titles and status are unknown. Do not cite them until checked.
- Surfaced but not reviewed: 2505.18286 ("Single-agent or Multi-agent Systems? Why Not Both?"), 2601.12307, 2510.13903 ("Benefits and Limitations of Communication in Multi-Agent Reasoning"), 2611.04399 ("Silent Instrument Errors Reorder Agent Leaderboards").

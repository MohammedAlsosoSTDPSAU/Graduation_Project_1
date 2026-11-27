# RAG, Evidence Grounding, Citation/Attribution Faithfulness and Automated Claim Verification for Medical LLMs (theme: rag_verification)

Scope and conventions
- These notes feed Chapter 2 (2.1 review, 2.2 comparative analysis, 2.3 gaps) and map to P3 (citing ≠ being supported) and RQ2 (verifier accuracy and unsupported-claim rate). They also touch P1 (guideline adherence), P2 (abstention when evidence is missing) and P4/RQ4 (compute-matched baselines).
- Verification levels follow the protocol: FULL, PARTIAL, ABSTRACT, SECONDARY. When a number comes from a preprint version of a paper that was later published, this is flagged, because the published numbers may differ.
- Access this session:
  - OpenAlex, Crossref, Semantic Scholar and arXiv APIs were blocked by the proxy, both from the shell and through WebFetch.
  - PMC returned a reCAPTCHA page.
  - Metadata was therefore checked on ACL Anthology pages, NeurIPS proceedings, the PSB proceedings PDF, the npj Digital Medicine PDF (institutional repository copy) and arXiv/ar5iv pages.
- Earlier verified notes (`Agentic clinical decision support evidence/rag_and_evidence_verification.md`) were reused for Lewis 2020, FActScore, Liu et al. 2025 KG-RAG (JAMIA), Amugongo 2025, Gargari 2025, Liang 2026 (JMIR AI) and the Liu/McCoy/Wright 2025 meta-analysis. Their verification levels are carried over and not upgraded.

## Q1. Which medical RAG systems and benchmarks are most relevant, what were their baselines, metrics and results, and what failed?

### Takeaway
Medical RAG reliably beats no-retrieval baselines on accuracy, but the gains depend heavily on configuration: corpus, retriever, document formatting and retrieval design.
- MedRAG: up to +18% over chain-of-thought.
- Kresevic: 43% → 99% accuracy on one guideline after reformatting.
- KG-RAG: F1 0.39 → 0.88.

Almost every system was evaluated on small, author-built question sets, often with a single model and with automated metrics that do not track expert-judged correctness. Several reported failures recur:
- errors of omission;
- clinicians preferring the non-grounded baseline;
- low retrieval precision and recall;
- tables and flowcharts that cannot be retrieved;
- "lost-in-the-middle" effects;
- no-retrieval models beating RAG on some tasks.

### Cited Findings

**MedRAG / MIRAGE (Xiong, Jin, Lu, Zhang; Findings of ACL 2024, pp. 6233–6251, DOI 10.18653/v1/2024.findings-acl.372). Verification: ABSTRACT.**
- MIRAGE has 7,663 questions from five medical QA datasets.
- The study ran 41 combinations of corpora, retrievers and backbone LLMs, using more than 1.8 trillion prompt tokens.
- MedRAG improved six LLMs by **up to 18% over chain-of-thought prompting**, bringing GPT-3.5 and Mixtral to GPT-4 level.
- **Combining various medical corpora and retrievers gave the best results.**
- The authors report log-linear scaling and **"lost-in-the-middle" effects**. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.372)
- The benchmark is multiple-choice QA, so it measures answer accuracy, not citation support or faithfulness. The names of the five datasets, the corpus components and the retriever list were not on the abstract page and were not verified here. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.372)

**Almanac (Zakka, Chaurasia, Shad, Dalal, Kim, Moor, Alexander, Ashley, Boyd, Boyd, Hirsch, Langlotz, Nelson, Hiesinger). Published in NEJM AI 2024 (PMC10857783, PMID 38343631, per search listing). Numbers below are from the arXiv 2303.01229 rendering. Verification: FULL of the preprint; published version not readable (PMC reCAPTCHA).**
- Architecture:
  - A browser restricted to curated domains plus a textbook and web database.
  - Documents are split into 1,000-token chunks and indexed in an HNSW vector store, embedded with text-embedding-ada-002.
  - A match threshold of λ = 0.83 applies. Below it, the system says it lacks sufficient information, which is an abstention mechanism.
  - The generator is text-davinci-003, and answers carry in-text citations. — [ar5iv 2303.01229](https://ar5iv.labs.arxiv.org/html/2303.01229)
- Data and evaluation:
  - ClinicalQA has 130 questions: 25 each in five specialties plus 5 calculation vignettes, written by 5 board-certified physicians.
  - The baseline is ChatGPT (March 23 version), scored by physicians on factuality, completeness and safety. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2303.01229)
- Results:
  - Factuality was on average **18% (absolute) higher** than ChatGPT; in cardiology it was **91% vs 69%** (p = 0.0189).
  - Almanac solved 5/5 calculation vignettes; ChatGPT solved 0/5.
  - Completeness rose by +4.8%, which was not significant. Both systems were weakest in cardiothoracic surgery (33% vs 25%).
  - Adversarial safety was **95% vs 0%**. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2303.01229)
- Failures:
  - Both systems **scored 0% on errors of omission**.
  - **Physicians preferred ChatGPT's answers 57% of the time.**
  - Adversarial resilience weakened with shorter adversarial prompts, and λ needs tuning.
  - Grounded models struggle when the sources contain no clear extractive answer. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2303.01229)
- ChatGPT supported its statements with correct sources only 56% of the time. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2303.01229)

**Clinfo.ai (Lozano, Fleming, Chiang, Shah; Pacific Symposium on Biocomputing 2024, pp. 9–23). Verification: FULL (PSB proceedings PDF).**
- Pipeline:
  - An LLM generates PubMed or Semantic Scholar queries (three seeds, results combined as a union).
  - GPT-3.5 makes a binary relevance filter decision per article, with optional BM25 re-ranking.
  - An LLM summarizes each article, then synthesizes a "Literature Summary" and a TL;DR with IEEE-style citations. — [PSB 2024 PDF](https://d2ygbh7lwzeoju.cloudfront.net/psb-online/proceedings/psb24/lozano.pdf)
- Data: PubMedRS-200 has 200 questions derived from systematic reviews; 146 were evaluated.
- Baselines: GPT-3.5 and GPT-4 without retrieval, Elicit, and StatPearls Semantic Search.
- Metrics: automated only (UniEval, COMET, BERTScore, ROUGE-L, METEOR, etc.). **No human evaluation.** — [PSB PDF](https://d2ygbh7lwzeoju.cloudfront.net/psb-online/proceedings/psb24/lozano.pdf)
- Mixed or negative results:
  - On UniEval Overall, the no-retrieval GPT-3.5 baseline (0.872) scored above most Clinfo.ai variants, for example Synthesis (restricted) at 0.788. Only the unrestricted TL;DR (0.880) exceeded it.
  - **Retrieval precision was 0.162–0.224 and recall 0.052–0.064 against the systematic reviews' reference lists.**
  - In unrestricted search, the source review itself was retrieved in 96.5% of cases, which likely inflates those scores.
  - The query generator hallucinated non-existent MeSH terms. — [PSB PDF](https://d2ygbh7lwzeoju.cloudfront.net/psb-online/proceedings/psb24/lozano.pdf)

**Guideline-grounded RAG: Kresevic et al. (Kresevic, Giuffrè, Ajcevic, Accardo, Crocè, Shung; npj Digital Medicine 7:102, 2024, DOI 10.1038/s41746-024-01091-y). Verification: FULL (repository PDF of the published article).**
- Setup:
  - The guideline was EASL 2020 HCV.
  - The model was GPT-4 Turbo, with a stepwise ablation: in-context guideline; cleaning with tables converted to CSV; tables restructured as text lists; prompt engineering; few-shot learning (54 question–answer pairs).
  - 20 questions (10 text, 5 table, 5 clinical scenarios) were each run 5 times and graded by two hepatologists. — [npj DM PDF](https://arts.units.it/retrieve/5a1fd528-ab48-4f87-bb3f-cfe2c6a11aef/s41746-024-01091-y.pdf)
- Accuracy by step:
  - Baseline: **43%** overall, with 28% on tables and 20% on clinical scenarios.
  - In-context guideline: 67%.
  - Cleaning: 78%.
  - Structuring: 90%.
  - Prompt engineering: **99%** (p < 0.001 vs baseline).
  - Few-shot learning added nothing. — [npj DM PDF](https://arts.units.it/retrieve/5a1fd528-ab48-4f87-bb3f-cfe2c6a11aef/s41746-024-01091-y.pdf)
- Failures:
  - GPT extracted image-based tables with only 16% accuracy, and "information cannot be retrieved accurately from non-text sources".
  - Of 124 inaccurate outputs, 90.3% were fact-conflicting and 9.7% input-conflicting hallucinations.
  - BLEU, ROUGE and METEOR scores did not consistently track expert-graded accuracy. — [npj DM PDF](https://arts.units.it/retrieve/5a1fd528-ab48-4f87-bb3f-cfe2c6a11aef/s41746-024-01091-y.pdf)
- Stated limitations: one disease and one guideline; one model; temperature fixed at 0.9; few runs per question. — [npj DM PDF](https://arts.units.it/retrieve/5a1fd528-ab48-4f87-bb3f-cfe2c6a11aef/s41746-024-01091-y.pdf)

**Knowledge-graph RAG: Liu, Wright, McCoy, Huang, Steitz, Wright (JAMIA 32(6):1032–1039, 2025, DOI 10.1093/jamia/ocaf059). Verification: FULL (reused from earlier notes).**
- Data: 1,020 patient-portal messages, of which 41 were emergencies. Model: GPT-4o. Knowledge source: a nursing telephone-triage book.
- F1 was 0.39 for prompt-only, **0.59 for naive RAG** and **0.88 for KG-RAG (global search)**.
- Stated limitations: a single source; historical data; n = 41 emergencies; "LLMs may give correct answers through flawed reasoning". — [PMC12089757](https://pmc.ncbi.nlm.nih.gov/articles/PMC12089757/)

**Aggregate evidence**
- **Systematic review (Amugongo et al., PLOS Digital Health 2025, DOI 10.1371/journal.pdig.0000877; PARTIAL, reused).**
  - 70 studies were included.
  - 78.9% of the included studies used English data and 21.1% Chinese.
  - The review reports a "lack of standardised evaluation frameworks", and most studies did not assess safety or ethics. — [PMC12157099](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- **Meta-analysis (Liu, McCoy, Wright, JAMIA 2025, DOI 10.1093/jamia/ocaf008).**
  - It pooled 20 studies with OR 1.35 (95% CI 1.19–1.53).
  - **SECONDARY only**: the primary page was not reachable again this session, and the pooled outcome definition is unknown. — [iatrox blog, SECONDARY](https://www.iatrox.com/blog/rag-medical-ai-jamia-meta-analysis-20-studies-iatrox)

**Negative result: BioRAB (Li, Zhan, Yang, Xiao, Huang, Zhang; arXiv 2405.08151). Verification: FULL of the preprint (reused).**
- A PMC record (PMC12637297) with the same title exists, which suggests journal publication, but the venue is not confirmed.
- The best no-retrieval LLMs **beat RAG on MedMCQA**.
- Counterfactual corpora cut ChemProt F1 from 86.91 to 47.79 at 80% wrong labels.
- The models' rate of recognising unhelpful retrieved examples was **0.00–1.07%**. — [arXiv HTML v2](https://arxiv.org/html/2405.08151v2); [PMC listing](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12637297/)

**Origin of RAG (Lewis et al., NeurIPS 2020; ABSTRACT, reused).** The paper names provenance and knowledge updating as open problems that RAG targets. — [NeurIPS](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html)

### Inferences
- Configuration choices drive the size of RAG gains: document formatting (Kresevic), the retrieval structure (KG vs naive), and combining corpora and retrievers (MedRAG). The choice of RAG baseline therefore matters for RQ4. A comparison against a weak "naive RAG" baseline would overstate a multi-agent system's benefit, so a fair single-agent baseline should use the same retrieval stack.
- Almanac's threshold-based "insufficient information" response is an early, explicit abstention design, relevant to P2 and the ABSTAIN/NEED_MORE_INFO statuses. No abstention accuracy was reported.
- Kresevic's finding that tables and flowcharts fail and must be converted to text matters directly for any guideline "pack" corpus.

### Gaps
- None of the medical RAG system papers read here (Almanac, Clinfo.ai, Kresevic, KG-RAG) reports a claim-level citation-support rate for its own outputs. They report accuracy or factuality only.
- The MIRAGE dataset names, the MedCorp composition and the per-retriever results were not verified (abstract page only).
- The Almanac published (NEJM AI) version, its DOI and the final numbers were not readable. Numbers above are from the preprint.
- The primary text of the Liu/McCoy/Wright meta-analysis is still unread.
- No head-to-head comparison of BM25, dense and fused retrieval on guideline corpora was found. VeriFact (Q3) is the closest: hybrid/reranked retrieval beat dense retrieval on EHR facts.

## Q2. What does the evidence say about citation/statement support in medical LLM outputs, and about retrieval noise, conflicting sources and outdated guidelines?

### Takeaway
Citation support is the weakest link.
- In SourceCheckup, even the retrieval-enabled GPT-4 left about 30% of statements unsupported and about half of its responses not fully supported. API-only models were far worse.
- In a retrieve-then-verify study of risk-of-bias assessments, only 60–65% of claims were supported despite 90–98% binary decision accuracy.

Retrieved evidence is also not reliably used well.
- Models adopt wrong retrieved values more than 60% of the time when their own prior was right (ClashEval).
- Contradictory PubMed passages lower answer quality.
- LLMs endorse a large share of outdated guideline recommendations. RAG only partly fixes this.

### Cited Findings

**SourceCheckup: Wu, Wu, Wei, Zhang, Casasola, Nguyen, Riantawan, Shi, Ho, Zou. Published as "An automated framework for assessing how well LLMs cite relevant medical references", Nature Communications 16 (2025), DOI 10.1038/s41467-025-58551-6.**
- **Published-version abstract (ABSTRACT, via the IDEAS record):**
  - 7 LLMs were tested on 800 questions and about 58,000 statement–source pairs.
  - **50–90% of responses were not fully supported, and some were contradicted, by their cited sources.**
  - **GPT-4o with Web Search left ~30% of statements unsupported, and nearly half of its responses were not fully supported.**
  - Doctors' assessments corroborated the results. — [IDEAS/RePEc record](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html)
- **arXiv v1 (FULL of the preprint; the setup differs from the published version):**
  - 1,200 questions from MayoClinic, UpToDate and Reddit r/AskDocs, with more than 40K statement–source pairs.
  - Statement-level support was **0.694 for GPT-4 (RAG, Bing)**, 0.422 for GPT-4 (API), 0.252 for Claude v2.1, 0.219 for Mistral Medium and 0.143 for Gemini Pro.
  - Response-level support was 0.543, 0.227, 0.137, 0.124 and 0.076 respectively.
  - URL validity was 0.992 for GPT-4 (RAG) vs 0.413–0.692 for the API models.
  - GPT-4 (RAG) response-level support was **~80% on MayoClinic questions vs ~30% on r/AskDocs** (p < 0.001), and GPT-4 (RAG) gave no sources in more than 20% of responses. — [arXiv 2402.02008v1](https://arxiv.org/html/2402.02008v1)
  - Stated limitation: the framework measures support, not factual correctness, so verbatim quoting would score well. — [arXiv v1](https://arxiv.org/html/2402.02008v1)

**Retrieve-then-verify (Liang, Sheffield, Butera, Antani; JMIR AI 2026;5:e93761, DOI 10.2196/93761). Verification: ABSTRACT (reused).**
- Task: GPT-5, o3-mini and GPT-3.5 produced RoB 2 assessments for 97 RCTs.
- Method: BM25 retrieved trial-report passages for each claim, and a verifier gave one of four verdicts.
- **Evidence support was 60–65%, with conservative hallucination rates of 34–37%, despite 90–98% binary accuracy.**
- Top-1 BM25 scores were similar across models (~30–31), so the authors attribute the differences to model reasoning rather than retrieval. — [JMIR Preprints page](https://preprints.jmir.org/preprint/93761)

**ALCE (Gao, Yen, Yu, Chen; EMNLP 2023, pp. 6465–6488, DOI 10.18653/v1/2023.emnlp-main.398). Verification: ABSTRACT. General domain.** On ELI5, "even the best models lack complete citation support 50% of the time". — [ACL Anthology](https://aclanthology.org/2023.emnlp-main.398)

**Knowledge conflict: ClashEval (Wu, Wu, Zou; NeurIPS 2024 Datasets and Benchmarks Track). Verification: FULL (proceedings PDF).**
- Data: 1,294 questions in six domains, including **Drug Dosages (n = 249)**. Retrieved documents were perturbed, for example dosages multiplied by 0.1× to 10×.
- Models: GPT-4o, GPT-3.5, Llama-3, Claude Opus, Claude Sonnet and Gemini 1.5 Flash.
- Context adoption:
  - When only the model's prior was correct, **GPT-4o adopted the incorrect retrieved content 60.8% of the time**.
  - Adoption fell as the perturbation grew and as prior confidence rose. — [NeurIPS proceedings PDF](https://proceedings.nips.cc/paper_files/paper/2024/file/3aa291abc426d7a29fb08418c1244177-Paper-Datasets_and_Benchmarks_Track.pdf)
- Mitigation: calibrated token-probability correction raised GPT-4o accuracy from 61.5% to 75.4% and cut context bias from 30.4% to 10.7%, but prior bias rose from 2.1% to 8.5%.
- Adding more documents (k = 5) lowered accuracy and increased "neither" answers.
- The authors caution that the dataset over-represents contextual errors, so its rates are not real-world rates. — [NeurIPS PDF](https://proceedings.nips.cc/paper_files/paper/2024/file/3aa291abc426d7a29fb08418c1244177-Paper-Datasets_and_Benchmarks_Track.pdf)

**Contradictory sources: "Contradictions in Context" (Javadi, Mirabi, Gangar, Ofoghi; arXiv 2511.06668, preprint, venue not stated). Verification: PARTIAL.**
- Data: 8,856 consumer medicine queries built from TGA medicine information, with RAG over 28,873 PubMed documents published 1975–2025. Contradictions were detected with a biomedical NLI model.
- Effect: retrieving the most-contradicted rather than the most-similar documents **lowered average ROUGE-1 by 18.2%**. Mixtral fell from 0.163 to 0.131, about −20%.
- Semantic similarity fell only about 8.3%.
- Caveats: the metrics are lexical and semantic overlap, not judged factuality, and model scale is confounded with architecture. — [arXiv PDF](https://arxiv.org/pdf/2511.06668)

**Outdated guidelines: ConflictMedQA (Wu, Xu, Gao, Diao, Li, Salas, Gui; arXiv 2505.07968v3, preprint). Verification: PARTIAL.**
- Data: 195 pairs of current vs "pseudo-outdated" guideline recommendations, expanded to 4,290 scenario QA items. Seven models were tested.
- Rejection of outdated recommendations was 27.8% for Qwen2.5-72B and 39.5% for GPT-4o, so the models endorsed most outdated advice. The best was Ministral-8B at 80%.
- "More capable or larger-scale models did not consistently exhibit lower" inconsistency rates.
- RAG over an up-to-date 195-item knowledge base (k = 2, retrieval recall 92%) combined with DPO gave the best alignment, rising from 55 to 81–89. The paper reports some mixed RAG-only effects. — [arXiv HTML v3](https://arxiv.org/html/2505.07968v3)
- Limitation: synthetic scenarios. — [arXiv v3](https://arxiv.org/html/2505.07968v3)

**Retrieval noise (BioRAB; FULL preprint, reused).**
- Counterfactual and unlabeled corpora sharply degrade biomedical task F1.
- Models almost never recognise unhelpful retrieved examples (0.00–1.07%). — [arXiv v2](https://arxiv.org/html/2405.08151v2)

**Outdated sources as a challenge (Gargari & Habibi, Digital Health 2025; narrative review, reused).** The review lists outdated sources and heterogeneous guideline formats as open challenges. — [PMC12059965](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)

### Inferences
- Several sources show that being retrieval-enabled or showing citations is not the same as being supported: SourceCheckup, Liang 2026, ALCE, and Almanac's 56% correct-source rate for ChatGPT. This is the core evidence for P3.
- The SourceCheckup gap between MayoClinic and r/AskDocs (~80% vs ~30%) suggests that support rates depend on how far the question sits from the corpus. Real physician queries may resemble the harder, open-ended case.
- ClashEval and ConflictMedQA mean a verifier must be able to check claims against a known-current, versioned corpus. Simply "trusting retrieved text" can propagate wrong dosages or outdated recommendations.

### Gaps
- The verifier-agreement and per-model numbers from the published Nature Communications version of SourceCheckup could not be read (PMC blocked). The arXiv v1 numbers above differ in design: 5 vs 7 models, and 1,200 vs 800 questions.
- No study was found that quantifies guideline version staleness in a deployed clinical RAG system. ConflictMedQA uses synthetic pseudo-outdated pairs.
- No peer-reviewed medical study measuring citation support for guideline-grounded CDS outputs specifically was found.

## Q3. What automated claim-verification or attribution-evaluation methods exist, and how well do they agree with human judgement?

### Takeaway
Methods fall into five families:
1. NLI and fact-checking datasets and models: SciFact, HealthVer.
2. Atomic decomposition with verification: FActScore, MedScore, VeriFact.
3. LLM-as-judge pipelines: SourceCheckup, RAGAS.
4. Citation benchmarks: ALCE, AttributionBench.
5. Small trained checkers: MiniCheck.

Reported agreement with humans ranges from about 78–85% macro-F1 on hard attribution benchmarks (AttributionBench) to about 88–93% agreement with clinicians in medical LLM-as-judge pipelines (SourceCheckup, VeriFact). The medical figures match or exceed inter-clinician agreement, but they come from small validation sets and from single snapshots of the verifier models. Factuality scores also shift heavily with the decomposition method, the verifier LLM and the corpus (MedScore), so the verifier is itself a source of measurement error.

### Cited Findings

**Datasets for evidence-based claim verification**
- **SciFact (Wadden, Lin, Lo, Wang, van Zuylen, Cohan, Hajishirzi; EMNLP 2020, pp. 7534–7550, DOI 10.18653/v1/2020.emnlp-main.609). ABSTRACT.**
  - 1.4K expert-written scientific claims paired with evidence abstracts, plus rationales.
  - Domain adaptation substantially improves over models trained on Wikipedia or political news. — [ACL Anthology](https://aclanthology.org/2020.emnlp-main.609)
- **HealthVer (Sarrouti, Ben Abacha, Mrabet, Demner-Fushman; Findings of EMNLP 2021, pp. 3499–3512, DOI 10.18653/v1/2021.findings-emnlp.297). ABSTRACT.**
  - 14,330 evidence–claim pairs (COVID-19 claims checked against scientific articles), labelled SUPPORT, REFUTE or NEUTRAL.
  - Training on real-world medical claims beat training on synthetic or open-domain claims. — [ACL Anthology](https://aclanthology.org/2021.findings-emnlp.297)

**Atomic decomposition with verification**
- **FActScore (Min et al., EMNLP 2023, DOI 10.18653/v1/2023.emnlp-main.741). ABSTRACT, reused.**
  - It measures the percentage of atomic facts supported by a knowledge source.
  - The automated estimator has an **error rate under 2%** (aggregate score on biographies, not per-claim accuracy).
  - Human-judged ChatGPT scored 58% on biographies. — [ACL Anthology](https://aclanthology.org/2023.emnlp-main.741)
- **MedScore (Huang, DeLucia, Tiyyala, Dredze; Findings of ACL 2026, pp. 14149–14180, DOI 10.18653/v1/2026.findings-acl.693; arXiv 2505.18452). Metadata from ACL Anthology; numbers from arXiv v2. PARTIAL.**
  - Method: medical answers are decomposed into "condition-aware" valid facts and verified against MedCorp (PubMed, StatPearls and textbooks, retrieved with MedCPT, top-10) or against a model's internal knowledge. — [ACL Anthology](https://aclanthology.org/2026.findings-acl.693/); [arXiv v2](https://arxiv.org/html/2505.18452v2)
  - Human evaluation:
    - 10 AskDocsAI responses, 382 claims, two annotators, κ = 0.73.
    - Share of extracted claims judged valid: FActScore 17% (36/212), MedScore 74.4% (64/86), VeriScore 91.7% (22/24, but very few claims extracted). — [arXiv v2](https://arxiv.org/html/2505.18452v2)
  - **Verifier and corpus sensitivity:** the FActScore-decomposed factuality score on AskDocsAI was 31.96% with GPT-4o as verifier, 84.32% with OpenBioLLM, 46.10% with Mistral Small 3 and 50.04% with retrieved documents. The corresponding MedScore figures were 76.54%, 95.64%, 78.09% and 70.07%. — [arXiv v2](https://arxiv.org/html/2505.18452v2)
  - Stated limitations: the method checks only what is present, so it cannot measure omissions, and LLM verifiers may be biased. — [arXiv v2](https://arxiv.org/html/2505.18452v2)
- **VeriFact (Chung et al., about 30 authors, Stanford; arXiv 2501.16672v1, preprint, published version not confirmed). PARTIAL.**
  - Task: verify LLM-written and human-written Brief Hospital Course text against the patient's EHR, using Llama 3.1 70B as judge with BGE-M3 retrieval.
  - Data (VeriFact-BHC): 100 MIMIC-III patients and 13,290 propositions, each labelled by 3 of 25 clinicians.
  - **Agreement on LLM-written text:** VeriFact vs ground truth was 92.7% for sentences and 88.8% for atomic claims. Clinician–clinician agreement was 84.7% and 88.5%.
  - Agreement on human-written text was lower: clinician–clinician 66.6% for sentences and 73.3% for atomic claims.
  - Configuration: the number of retrieved facts mattered most (top-N = 50 was best, not yet plateaued), and hybrid/reranked retrieval beat dense retrieval.
  - Failure modes: Not Supported and Not Addressed were poorly distinguished, omissions cannot be detected, atomic-claim extraction sometimes produced nonsensical claims, and the annotators were study authors. — [arXiv v1](https://arxiv.org/html/2501.16672v1)

**LLM-as-judge pipelines**
- **SourceCheckup verifier (arXiv v1, FULL of the preprint).**
  - The GPT-4 statement–source verifier agreed with doctor consensus **88.0%** of the time vs **85.2%** average inter-doctor agreement (p = 0.21), on 284 annotated pairs.
  - On 110 pairs the verifier labelled unsupported, doctors agreed 95.8% of the time.
  - No chance-corrected statistic (κ) was reported. — [arXiv 2402.02008v1](https://arxiv.org/html/2402.02008v1)
- **RAGAS (Es, James, Espinosa-Anke, Schockaert; EACL 2024 System Demonstrations, ACL Anthology 2024.eacl-demo.16). PARTIAL via ar5iv.**
  - Faithfulness = supported statements / all statements, judged by gpt-3.5-turbo-16k.
  - Pairwise agreement with humans on WikiEval (50 Wikipedia pages, not medical) was **0.95 for faithfulness**, 0.78 for answer relevance and 0.70 for context relevance. The GPT Score baseline scored 0.72, 0.52 and 0.63.
  - The judge model is single, the dataset is small and was built by the authors, and context relevance was "the hardest" dimension. — [ar5iv 2309.15217](https://ar5iv.labs.arxiv.org/html/2309.15217); [ACL Anthology](https://aclanthology.org/2024.eacl-demo.16)

**Attribution benchmarks**
- **AttributionBench (Li, Yue, Liao, Sun; Findings of ACL 2024, ACL Anthology 2024.findings-acl.886; numbers from arXiv v1). PARTIAL.**
  - It combines 7 datasets (ExpertQA, Stanford-GenSearch, AttributedQA, LFQA, BEGIN, AttrEval-GenSearch, HAGRID). None is explicitly medical.
  - Best macro-F1:
    - zero-shot in-distribution: 73.6 (FLAN-UL2);
    - fine-tuned in-distribution: 78.5;
    - fine-tuned out-of-distribution: 84.2.
  - "Even a fine-tuned GPT-3.5 only achieves around 80% macro-F1" (77.7 in the table).
  - Error analysis: **66.0% of errors came from insensitivity to fine-grained information** (numbers, dates, names), and about 26.8% from a mismatch between the evidence humans saw and the evidence models saw. — [arXiv v1](https://arxiv.org/html/2402.15089v1); [ACL Anthology](https://aclanthology.org/2024.findings-acl.886)
- **ALCE.** Its automatic citation metrics are said to have "strong correlation with human judgements" (no numbers on the abstract page). — [ACL Anthology](https://aclanthology.org/2023.emnlp-main.398)

**Small trained checkers**
- **MiniCheck (Tang, Laban, Durrett; EMNLP 2024, pp. 8818–8847, DOI 10.18653/v1/2024.emnlp-main.499). ABSTRACT.**
  - MiniCheck-FT5 (770M parameters) reaches **GPT-4-level accuracy on the LLM-AggreFact benchmark at about 400× lower cost**.
  - It was trained on GPT-4-synthesised hard examples. — [ACL Anthology](https://aclanthology.org/2024.emnlp-main.499)
- **AlignScore (Zha, Yang, Li, Hu; ACL 2023 Long, pp. 11328–11348, DOI 10.18653/v1/2023.acl-long.634). ABSTRACT. An NLI-style alignment verifier.**
  - It is a unified alignment function trained on 4.7M examples from 7 tasks (NLI, QA, paraphrasing, fact verification, IR, semantic similarity, summarisation).
  - It was evaluated on 22 datasets, 19 of them unseen in training.
  - The 355M model "matches or even outperforms metrics based on ChatGPT and GPT-4".
  - The page does not name specific benchmarks or medical data. — [ACL Anthology](https://aclanthology.org/2023.acl-long.634)
- **RAGTruth (Niu, Wu, Zhu, Xu, Shum, Zhong, Song, Zhang; ACL 2024 Long, ACL Anthology 2024.acl-long.585 per listing; abstract from arXiv 2401.00396v2). ABSTRACT.**
  - Nearly 18,000 naturally generated RAG responses from diverse LLMs, annotated at case and word level with hallucination intensity.
  - A relatively small LLM fine-tuned on RAGTruth reached hallucination-detection performance competitive with prompt-based GPT-4.
  - Detector precision, recall and F1, and the task and model list, were not extracted (abstract only). Not medical. — [arXiv v2](https://arxiv.org/abs/2401.00396v2); [ACL Anthology](https://aclanthology.org/2024.acl-long.585)

**NLI-based vs LLM-judge verifiers (synthesis across sources read)**
- Small NLI-style or trained checkers (AlignScore 355M, MiniCheck 770M, the T5-XXL-TRUE and RoBERTa-MNLI baselines in AttributionBench, and a fine-tuned small LLM on RAGTruth) are reported to match GPT-4-based judges on general-domain benchmarks. — [AlignScore](https://aclanthology.org/2023.acl-long.634); [MiniCheck](https://aclanthology.org/2024.emnlp-main.499); [RAGTruth](https://arxiv.org/abs/2401.00396v2)
- On the harder AttributionBench, however, even fine-tuned models top out at 78.5 macro-F1 in-distribution and 84.2 out-of-distribution. — [AttributionBench](https://arxiv.org/html/2402.15089v1)
- **Error modes of LLM-based verifiers reported in the sources read:**
  - insensitivity to fine-grained details such as numbers, dates and names (66% of AttributionBench errors);
  - mismatch between the evidence humans saw and the evidence the model received (about 26.8%);
  - poor separation of "Not Supported" from "Not Addressed" (VeriFact);
  - nonsensical atomic claims produced by decomposition (VeriFact);
  - inability to detect omissions (VeriFact, MedScore);
  - scores that swing with the choice of verifier LLM (MedScore: 31.96% vs 84.32%).
- No head-to-head comparison of NLI checkers and LLM judges on medical guideline claims was found.

**SourceCheckup: which figures come from which version**

| Figure | Version |
|---|---|
| 7 LLMs; 800 questions; ~58,000 statement–source pairs; 50–90% of responses not fully supported; GPT-4o with Web Search ~30% statements unsupported and ~half of responses not fully supported; doctors corroborated | **Nature Communications 2025** (abstract via IDEAS) |
| 5 models; 1,200 questions; >40K pairs; GPT-4 (RAG) statement support 0.694 and response support 0.543; API models 0.143–0.422 (statement) and 0.076–0.227 (response); URL validity 0.992 vs 0.413–0.692; MayoClinic ~80% vs r/AskDocs ~30%; verifier–doctor agreement 88.0% vs inter-doctor 85.2% (p = 0.21, 284 pairs); 95.8% doctor agreement on 110 "unsupported" labels | **arXiv 2402.02008v1 preprint only.** Not checked against the published version, so do not cite these as Nature Communications figures. |

Sources: [IDEAS](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html); [arXiv v1](https://arxiv.org/html/2402.02008v1)

### Inferences
- For RQ2, the closest medical precedents for estimating verifier accuracy are SourceCheckup (88.0% vs 85.2% inter-doctor) and VeriFact (88.8–92.7% vs 84.7–88.5%). Both validate an LLM judge against clinicians and report raw agreement without κ. The project should report κ or per-class precision and recall, especially recall on "unsupported", since raw agreement can hide poor performance on the minority class.
- AttributionBench's dominant error type, fine-grained insensitivity to numbers and names, is the kind of error that matters for thresholds, dosing ranges and score cut-offs in guidelines. A purely LLM-based verifier is therefore likely to be weakest exactly where clinical risk is highest. This supports pairing it with deterministic rule checks, as the proposed design does.
- MedScore's verifier and corpus sensitivity (31.96% vs 84.32% for the same answers) means an "unsupported-claim rate" is only interpretable with the decomposer, verifier model and corpus fixed and reported. This is an RQ2 methods requirement.

### Gaps
- No medical attribution benchmark with human labels at AttributionBench scale was found. The medical validations are small: 284 pairs (SourceCheckup) and 100 patients (VeriFact, albeit 13,290 propositions).
- Fine-tuned medical NLI verifiers (for example SciFact- or HealthVer-trained models) were not compared head-to-head with LLM judges on clinical-guideline claims in any source read.
- The best F1 baselines for HealthVer and SciFact were not on the abstract pages, so they were not extracted.
- MiniCheck's per-dataset balanced accuracy was not extracted. Whether LLM-AggreFact includes medical data was not checked.

## Q4. Do verification or self-correction loops reduce unsupported claims, and are there negative results?

### Takeaway
Verification helps when the feedback is external and reliable: retrieved evidence, tools or oracle-like signals. Intrinsic self-correction (the model critiquing itself with no new information) often fails or harms accuracy.
- Without external feedback, GPT-3.5 fell from 75.8% to 38.1% on CommonSenseQA after self-correction.
- Multi-agent debate did not beat self-consistency at a matched number of responses.

No medical study found here directly measures how much a claim-to-source verify-and-revise loop reduces the unsupported-claim rate. That is a genuine gap for RQ2.

### Cited Findings

**Huang et al. (Huang, Chen, Mishra, Zheng, Yu, Song, Zhou), "Large Language Models Cannot Self-Correct Reasoning Yet".**
- Metadata: arXiv 2310.01798v2, dated 2024-03-14. It is listed as an ICLR 2024 poster on iclr.cc (search listing; the page itself was not opened). Numbers below are from the ar5iv rendering (version not shown). PARTIAL. — [arXiv abs](https://arxiv.org/abs/2310.01798v2); [ICLR 2024 listing](https://iclr.cc/virtual/2024/poster/18956)
- Intrinsic self-correction (standard → round 2):
  - GPT-3.5: GSM8K 75.9 → 74.7; CommonSenseQA **75.8 → 41.8** (round 1: 38.1); HotpotQA 26.0 → 25.0.
  - GPT-4: GSM8K 95.5 → 89.0; HotpotQA 49.0 → 43.0. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2310.01798)
- Gains appear only with **oracle labels**, for example GPT-3.5 CommonSenseQA 75.8 → 89.7, which is unrealistic in deployment. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2310.01798)
- **Multi-agent debate vs self-consistency at matched responses (GSM8K):** 83.2 vs 85.3 at 6 responses, and 83.0 vs 88.2 at 9. Debate trails. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2310.01798)
- Prompt confound on CommonGen-Hard: with a stronger initial prompt (81.8), self-correction lowered the score to 75.1. "Equal effort should be invested in designing the prompts" for initial and corrected responses. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2310.01798)

**Kamoi, Zhang, Zhang, Han, Zhang, "When Can LLMs Actually Correct Their Own Mistakes? A Critical Survey of Self-Correction of LLMs" (TACL 12:1417–1440, 2024, DOI 10.1162/tacl_a_00713). ABSTRACT.**
- "No prior work demonstrates successful self-correction with feedback from prompted LLMs," except in tasks exceptionally suited to it.
- Self-correction works well with **reliable external feedback**, and large-scale fine-tuning enables it.
- Many prior studies use unfair evaluations that overstate the benefits. — [ACL Anthology](https://aclanthology.org/2024.tacl-1.78)

**Chain-of-Verification (Dhuliawala, Komeili, Xu, Raileanu, Li, Celikyilmaz, Weston; Findings of ACL 2024, pp. 3563–3578, DOI 10.18653/v1/2024.findings-acl.212). ABSTRACT.**
- Steps: draft, plan verification questions, answer them independently, then revise.
- It "decreases hallucinations" on Wikidata list questions, closed-book MultiSpanQA and long-form generation. No numbers are on the abstract page.
- It is a general-domain method and uses no external sources. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.212)

**Medical and RAG-adjacent correction evidence**
- **ClashEval.** A calibrated token-probability arbitration between the model's prior and the retrieved context reduced context bias from 30.4% to 10.7% (GPT-4o), at the cost of a rise in prior bias from 2.1% to 8.5%. — [NeurIPS PDF](https://proceedings.nips.cc/paper_files/paper/2024/file/3aa291abc426d7a29fb08418c1244177-Paper-Datasets_and_Benchmarks_Track.pdf)
- **ConflictMedQA.** RAG over current guidance combined with DPO gave the best alignment against outdated recommendations, but on synthetic scenarios. — [arXiv v3](https://arxiv.org/html/2505.07968v3)
- **BioRAB.** Near-zero negative awareness (0.00–1.07%) means generators do not police their own retrieved evidence. — [arXiv v2](https://arxiv.org/html/2405.08151v2)

### Inferences
- The evidence supports the proposed design choice of external, source-based verification with one bounded revision, rather than an intrinsic "critic re-thinks" loop. The literature indicates that intrinsic critique alone can lower accuracy (Huang; Kamoi).
- Huang's debate-vs-self-consistency result is direct evidence for RQ4: multi-agent gains can disappear under compute-matched single-agent baselines such as self-consistency with the same number of samples. This challenges the multi-agent premise and should be cited as such.
- Huang's prompt-confound finding means the project's single-agent baseline needs prompt-engineering effort equal to the multi-agent pipeline's.

### Gaps
- **No medical study was found that reports the unsupported-claim rate before and after a claim-to-source verify-and-revise loop.**
  - CoVe has no external sources and is not medical.
  - SourceCheckup and VeriFact evaluate outputs but do not revise them.
  - Self-BioRAG (PMC11211826) appeared in search results as a medical retrieval-plus-self-reflection model, but it was not opened or verified and is not included.
- CoVe's numeric results were not extracted (abstract page only).
- No evidence was found on whether a second revision round introduces new unsupported claims, which matters for the "one bounded revision" choice.

## Cross-cutting synthesis: what the evidence establishes, contradictions, gaps (P1–P4, RQ1–RQ4), and unverified items

### Takeaway
The evidence firmly establishes P3: retrieval and citations do not ensure support. It also shows that LLM verifiers can roughly match inter-clinician agreement on small medical sets. It does not establish that verify-and-revise loops reduce unsupported claims in clinical settings, or that the gains survive compute- and prompt-matched baselines.

### Cited Findings

**(a) What the evidence establishes**
- RAG improves medical accuracy, but configuration-dependently. — [MedRAG](https://aclanthology.org/2024.findings-acl.372); [Kresevic](https://arts.units.it/retrieve/5a1fd528-ab48-4f87-bb3f-cfe2c6a11aef/s41746-024-01091-y.pdf); [KG-RAG](https://pmc.ncbi.nlm.nih.gov/articles/PMC12089757/)
- Cited or retrieved sources often do not support the statements made. — [SourceCheckup](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html); [Liang 2026](https://preprints.jmir.org/preprint/93761); [ALCE](https://aclanthology.org/2023.emnlp-main.398)
- LLM judges reached about 88–93% agreement with clinicians, against 85–89% inter-clinician agreement, on LLM-written text. — [SourceCheckup v1](https://arxiv.org/html/2402.02008v1); [VeriFact](https://arxiv.org/html/2501.16672v1)
- Intrinsic self-correction is unreliable without external feedback. — [Huang](https://ar5iv.labs.arxiv.org/html/2310.01798); [Kamoi](https://aclanthology.org/2024.tacl-1.78)
- Small NLI-style checkers (AlignScore, MiniCheck, RAGTruth-tuned detectors) are reported to match GPT-4 judges in the general domain, which makes cheap verifiers viable for RQ4 cost matching. — [AlignScore](https://aclanthology.org/2023.acl-long.634); [MiniCheck](https://aclanthology.org/2024.emnlp-main.499); [RAGTruth](https://arxiv.org/abs/2401.00396v2)

**(b) Contradictions**
- Grounding vs clinician preference: Almanac was more factual and safer, yet physicians preferred ChatGPT 57% of the time. — [ar5iv](https://ar5iv.labs.arxiv.org/html/2303.01229)
- RAG vs no-retrieval baselines: no-retrieval GPT-3.5 outscored most Clinfo.ai variants on UniEval Overall. — [PSB](https://d2ygbh7lwzeoju.cloudfront.net/psb-online/proceedings/psb24/lozano.pdf). No-retrieval models also beat RAG on MedMCQA. — [BioRAB](https://arxiv.org/html/2405.08151v2)
- Verifier agreement levels: medical LLM-judge agreement of about 88–93% vs AttributionBench's about 78–84% macro-F1. The tasks, metrics (raw agreement vs macro-F1) and difficulty differ, so the numbers are not directly comparable. — [AttributionBench](https://arxiv.org/html/2402.15089v1)
- Verifier dependence: the same answers scored 31.96% vs 84.32% factuality depending on the verifier LLM. — [MedScore](https://arxiv.org/html/2505.18452v2)
- Multi-agent premise: debate trailed self-consistency at matched compute. — [Huang](https://ar5iv.labs.arxiv.org/html/2310.01798)

**(c) Gaps relevant to P1–P4 and RQ1–RQ4**
- P3/RQ2: there is no clinical study of before-and-after unsupported-claim rates under a claim-to-source verifier with revision.
- RQ2: there is no medical verifier validation that reports κ or recall on the "unsupported" class for guideline claims.
- P1: there is no study of version-controlled guideline corpora or real-world staleness.
- P2: abstention when retrieval is insufficient exists in design (Almanac's λ threshold), but its accuracy is unmeasured.
- RQ4: medical RAG papers mostly compare against no-retrieval or naive-RAG baselines, not against retrieval- and compute-matched single-agent pipelines.
- Evaluation remains non-standardised. — [Amugongo](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)

**(d) Inaccessible or unverified items**
- The full texts of the published Almanac (NEJM AI; DOI not verified) and SourceCheckup (Nature Communications) versions were blocked by the PMC reCAPTCHA. Numbers are from the preprints.
- Liu/McCoy/Wright JAMIA 2025 (ocaf008): SECONDARY only.
- BioRAB's journal venue is unconfirmed (PMC12637297 exists).
- VeriFact's publication status is unconfirmed.
- Huang et al.'s ICLR status is confirmed only through a search listing.
- The RAGAS, AttributionBench and RAGTruth DOIs were not verified; their ACL Anthology IDs were.
- AlignScore and RAGTruth are abstract-only, with no detector numbers extracted.
- SourceCheckup's detailed numbers are from the preprint (see the version table in Q3).
- OpenAlex, Crossref, Semantic Scholar and arXiv APIs were blocked by the proxy.
- Liang 2026 is abstract-only.

### Inferences
- Chapter 2 can justify a claim-to-source verifier backed by deterministic checks as a response to P3. It should present verifier accuracy as an open measurement problem (RQ2) rather than a solved one, and should acknowledge that multi-agent and critic gains may vanish under matched baselines (RQ4).

### Gaps
- See (c) and (d) above.

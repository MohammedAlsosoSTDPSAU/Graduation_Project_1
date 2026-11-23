# RAG and Evidence-Grounded, Verifiable Generation in Medicine (for Ch. 1.1 Background / 1.2 Problem Statement)

Scope note: notes support an undergraduate Research-Track project "Agentic Framework for Healthcare Clinical Decision Support" (hybrid BM25 + dense retrieval with fusion over clinical guidelines; every report claim must cite a guideline passage or calculator result; deterministic checks + LLM claim-to-source entailment before reporting). Verification legend used below:
- **VERIFIED-FT** = I opened the full text (or a large part of it) on the publisher/PMC page.
- **VERIFIED-ABS** = I opened the official abstract/landing page only ("abstract-only").
- **SECONDARY** = numbers seen only in a secondary source (blog / another review) that I opened; primary page could not be opened (PMC/PubMed returned a reCAPTCHA page or an allowlist block). Do not cite these numbers in the thesis until the primary is checked.
- Access problems this session: pmc.ncbi.nlm.nih.gov and pubmed.ncbi.nlm.nih.gov intermittently returned a reCAPTCHA interstitial; academic.oup.com, api.crossref.org and Europe PMC were blocked.

## Q1. Seed sources (7), (8), (9): verification and extraction

### Takeaway
All three seed sources exist. (7) is a PLOS Digital Health 2025 systematic review (70 studies) that finds no standardised RAG evaluation frameworks and little ethics assessment. (8) is Liu, McCoy & Wright, JAMIA 2025 (DOI 10.1093/jamia/ocaf008). Its pooled OR of 1.35 (95% CI 1.19–1.53) across 20 studies is reported here only from a secondary source, because I could not open the primary page. (9) is a real JMIR AI 2026 paper, accepted 11 Aug 2026. It shows that high decision accuracy can sit alongside only 60–65% evidence support.

### Cited Findings

**R7 — Seed (7)**
- Reference ID: R7. Full title: "Retrieval augmented generation for large language models in healthcare: A systematic review". Authors: Lameck Mbangula Amugongo, Pietro Mascheroni, Steven Brooks, Stefan Doering, Jan Seidel. Venue: PLOS Digital Health 4(6): e0000877, published 11 June 2025 (received 16 Aug 2024; accepted 5 May 2025). DOI 10.1371/journal.pdig.0000877; PMID 40498738; PMCID PMC12157099. Status: peer-reviewed, published. — [PMC full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- Note on the seed label: the user's note "(Liu et al.?, JAMIA)" belongs to seed (8), not to this paper. R7 is by Amugongo et al. in PLOS Digital Health. A preprint version exists on Preprints.org (202407.0876). — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/); [Preprints.org listing](https://www.preprints.org/manuscript/202407.0876)
- Competing interests: four authors work for Boehringer Ingelheim Pharma GmbH & Co. KG and one for Boehringer Ingelheim (China). — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- Research problem: the review assesses "RAG-based approaches employed by LLMs in healthcare" across the retrieval, augmentation and generation steps. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- Methodology:
  - PRISMA, with Google Scholar and PubMed searched (title/abstract/full text) for Jan 2020 – Feb 2025.
  - Included English-language papers that propose RAG methods in medicine. Excluded reviews, papers without full text and short conference papers.
  - Screening ran in three stages with multiple reviewers: 2,139 records retrieved, 70 studies included. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- Architecture taxonomy: studies are grouped as Naive, Advanced and Modular RAG. The Modular RAG section expects "a wider and wider adoption of agentic frameworks". — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- Main findings with numbers:
  - Datasets: 78.9% of studies used English datasets and 21.1% used Chinese.
  - Models: proprietary models (GPT-3.5/4) were the most used.
  - Evaluation: the review reports a "lack of standardised evaluation frameworks".
  - Ethics: most studies did not assess ethical considerations (privacy, safety, robustness, bias, trust). — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- Example numbers from included studies, as the review reports them:
  - External knowledge raised GPT-4 average accuracy on multiple-choice medical QA from 73.44% to 79.97%.
  - In one study, GPT-3.5 and GPT-4 produced more hallucinated responses than a custom retrieval-augmented model in all 19 evaluated cases. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- Limitations: I read only the first ~45% of the page (up to Table 3). The Discussion and the stated limitations were not read and still need checking.
  - From the Methods, the English-only scope and the exclusion of short conference papers may under-cover the ACL/NeurIPS literature. This is my inference.
  - Percentages for retriever types and for the share of studies using human evaluation were not visible in what I read. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
- Relevance and Ch.1 use:
  - 1.1 Background: RAG is widely adopted in healthcare, with an architecture taxonomy and a trend toward agentic/modular designs.
  - 1.2 Problem Statement: evaluation is not standardised and ethical/safety assessment is often missing.
- Evidence location: Abstract; Methods (search technique, inclusion criteria); Results (Naive/Advanced/Modular RAG subsections; Table 3).
- Verification status: VERIFIED-FT, partial (Discussion not read).

**R8 — Seed (8)**
- Reference ID: R8. Authors: Siru Liu, Allison B. McCoy, Adam Wright. Venue: Journal of the American Medical Informatics Association (JAMIA), 2025, published online 15 Jan 2025. DOI 10.1093/jamia/ocaf008; PMID 39812777 (from the user's link). — [iatrox blog, SECONDARY](https://www.iatrox.com/blog/rag-medical-ai-jamia-meta-analysis-20-studies-iatrox)
- Independent corroboration: a separate JAMIA 2025 paper by the same group (Liu, Wright, McCoy et al., DOI 10.1093/jamia/ocaf059) cites, as its ref. 24, "a separate systematic review by Liu, McCoy, and Wright" that proposes clinical development guidelines. — [PMC12089757](https://pmc.ncbi.nlm.nih.gov/articles/PMC12089757/)
- Exact title: I could not confirm it from a primary page. The user's title, "Improving large language model applications in biomedicine with retrieval-augmented generation: a systematic review, meta-analysis, and clinical development guidelines", matches the secondary descriptions but still needs checking on PubMed or OUP. Volume and issue are unverified.
- Methodology and results (SECONDARY):
  - 20 studies were included, all published in 2024 (335 records identified; 251 screened after duplicates were removed).
  - Random-effects meta-analysis gave a **pooled OR 1.35 (95% CI 1.19–1.53), P = .001**, I² = 37%, prediction interval 1.01–1.80. — [iatrox blog, SECONDARY](https://www.iatrox.com/blog/rag-medical-ai-jamia-meta-analysis-20-studies-iatrox)
  - The blog did not name the guideline framework, and I did not verify a name (such as "GUIDE-RAG").
- Limitations: not extracted, because the primary text was not accessible.
- Relevance and Ch.1 use:
  - 1.1: quantitative evidence that RAG gives a modest but significant improvement over baseline LLMs.
  - 1.2: the effect is moderate, not transformative. The prediction interval close to 1.0 suggests some settings gain little.
- Evidence location: Abstract/Results (forest plot; figure "ocaf008f2" appears in search results).
- Verification status: **SECONDARY for all numbers; metadata partly corroborated.** Must check on PubMed/OUP before citing.

**R9 — Seed (9)**
- Reference ID: R9. Full title: "Retrieve-Then-Verify for Evaluating Evidence Support and Hallucination in Large Language Model–Generated Medical Information: Empirical Study". Authors: Zhaohui Liang, Cynthia Sheffield, Gisela Butera, Sameer Antani. — [JMIR Preprints #93761 page](https://preprints.jmir.org/preprint/93761)
- Venue: JMIR AI 2026;5:e93761; DOI 10.2196/93761; PMID 42748453; PMCID 13628037 (as listed on the page). — [JMIR Preprints page](https://preprints.jmir.org/preprint/93761)
- Status: submitted 18 Feb 2026, accepted 11 Aug 2026. The page links an accepted manuscript and a published version, but it also still shows the generic "unreviewed preprint" banner. Treat it as **accepted/published in JMIR AI, peer-reviewed**, and confirm the final version at ai.jmir.org. — [JMIR Preprints page](https://preprints.jmir.org/preprint/93761)
- Research problem: measure evidence support and hallucination by checking whether LLM-generated risk-of-bias (RoB 2) assessments are supported by the source trial reports. — [JMIR Preprints page](https://preprints.jmir.org/preprint/93761)
- Methodology and architecture:
  - Models: GPT-5, o3-mini and GPT-3.5.
  - Dataset: 97 RCTs from a Cochrane systematic review, with human annotations and full-text reports.
  - Outputs were constrained to RoB 2 signalling questions and domain judgments.
  - For each generated claim, **Okapi BM25** retrieved trial-report passages. A verifier then gave one of four verdicts (Supported / Contradicted / Not Found / Out of Scope) with minimal supporting quotations.
  - Metrics: evidence support rate; conservative and strict hallucination rates; accuracy, sensitivity, specificity, F1 and Youden's J; Cohen's/Fleiss' κ against human reviewers. — [JMIR Preprints page](https://preprints.jmir.org/preprint/93761)
- Main findings with numbers:
  - Binary accuracy of domain judgments was 90–98%, but exact accuracy was 42–71%.
  - GPT-5 was best overall: perfect binary accuracy on overall RoB and quadratic κ up to 0.81.
  - **Evidence support was 60–65% across models, with conservative hallucination rates of 34–37%.** GPT-5 had mean support of 64.3% and a strict hallucination rate of 35.7%.
  - Mean top-1 BM25 scores were similar across models (~30–31), so the authors attribute the hallucination differences to model reasoning rather than retrieval.
  - Conclusion: high decision-level accuracy can coexist with missing documentary support, and agreement metrics alone miss this. — [JMIR Preprints page](https://preprints.jmir.org/preprint/93761)
- Limitations: none stated in the abstract.
  - The study is a single task (RoB assessment of RCTs) with lexical-only (BM25) retrieval and three OpenAI models. This is my inference from the design.
  - The verifier's own error rate is not reported in the abstract.
- Relevance and Ch.1 use:
  - 1.2: strongest direct support for the project's claim that accuracy is not grounding, and that claim-level verification against retrieved sources is needed.
  - Methodologically close to the project's verifier: claim, then BM25 retrieval, then a support verdict.
- Evidence location: Abstract (Objective/Methods/Results/Conclusions).
- Verification status: VERIFIED-ABS (abstract-only; full text not opened).

### Inferences
- Together, R7–R9 support a three-step problem statement:
  - RAG is widespread (R7).
  - RAG gives a moderate average gain (R8, pending primary check).
  - Even accurate outputs are often not supported by the evidence (R9).
- R8's OR of 1.35 should be described as "modest" and must not be called a general accuracy gain without the primary text, since the outcome definitions behind the OR are unknown to me.

### Gaps
- The R8 primary text (abstract and full text) could not be opened. Still unverified: the exact title, volume/issue, the outcome measure pooled, the guideline framework's name and content, and the limitations.
- R7's Discussion and limitations, the share of studies by retriever type, and the share using human evaluation were not read because of page truncation.
- R9's full text (verifier implementation, verifier accuracy, limitations) was not opened.

## Q2. Foundational and primary sources

### Takeaway
Five of the six foundational papers were checked on primary pages: Lewis 2020, Xiong 2024, Wu 2025, Min 2023 and Cormack 2009. Almanac (Zakka 2024, NEJM AI) was verified only through search listings and a secondary review, because PMC/PubMed blocked access. These papers ground the project's design choices: RAG itself, a medical RAG benchmark that motivates combining retrievers, citation-support auditing, atomic-claim factuality scoring, and RRF fusion.

### Cited Findings

**F1 — Lewis et al. 2020 (RAG)**
- Metadata: "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks". Patrick Lewis, Ethan Perez, Aleksandra Piktus, Fabio Petroni, Vladimir Karpukhin, Naman Goyal, Heinrich Küttler, Mike Lewis, Wen-tau Yih, Tim Rocktäschel, Sebastian Riedel, Douwe Kiela. NeurIPS 33 (2020), pp. 9459–9474. Peer-reviewed. — [NeurIPS proceedings](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html)
- Problem: LLMs store knowledge in their parameters but struggle to access and precisely manipulate it. Providing provenance and updating knowledge are "open research problems". — [NeurIPS](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html)
- Architecture: a parametric seq2seq model combined with non-parametric memory, a "dense vector index of Wikipedia" accessed by a pretrained neural retriever. Two variants: RAG-Sequence (same passages for the whole output) and RAG-Token (different passages per token). — [NeurIPS](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html)
- Findings:
  - State of the art on three open-domain QA tasks.
  - Generation is "more specific, diverse and factual" than a parametric-only baseline.
  - The abstract gives no medical results. — [NeurIPS](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html)
- Ch.1 use: 1.1 (origin and definition of RAG; provenance named as a motivation).
- Verification: VERIFIED-ABS.

**F2 — Xiong et al. 2024 (MIRAGE / MedRAG)**
- Metadata: "Benchmarking Retrieval-Augmented Generation for Medicine". Guangzhi Xiong, Qiao Jin, Zhiyong Lu, Aidong Zhang. Findings of ACL 2024, Bangkok, pp. 6233–6251. DOI 10.18653/v1/2024.findings-acl.372. Peer-reviewed; arXiv 2402.13178. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.372)
- Dataset: MIRAGE, 7,663 questions from five medical QA datasets. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.372)
- Methods: over 1.8 trillion prompt tokens across 41 combinations of corpora, retrievers and backbone LLMs, using the MedRAG toolkit. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.372)
- Findings:
  - MedRAG improved six LLMs by **up to 18%** over chain-of-thought prompting, bringing GPT-3.5 and Mixtral to GPT-4 level.
  - **Combining various medical corpora and retrievers gave the best performance.**
  - The authors observed log-linear scaling and **"lost-in-the-middle"** effects. — [ACL Anthology](https://aclanthology.org/2024.findings-acl.372)
- Limitations: evaluation is multiple-choice QA, which differs from free-text clinical reports (my inference). The full paper was not read.
- Ch.1 use:
  - 1.1: evidence that medical RAG helps and that combining retrievers is best, which motivates hybrid retrieval.
  - 1.2: passage position effects, i.e. retrieval/context quality problems.
- Verification: VERIFIED-ABS.

**F3 — Zakka et al. 2024 (Almanac)**
- Metadata, from search listings only: "Almanac: Retrieval-Augmented Language Models for Clinical Medicine", NEJM AI 2024. PMID 38343631; PMCID PMC10857783; earlier arXiv 2303.01229 (PMC10187428 preprint record). — [search listing: PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC10857783); [arXiv](https://arxiv.org/abs/2303.01229v2)
- Results, as reported by a secondary review:
  - Almanac outperformed ChatGPT on factuality by an average of 18 percentage points (cardiology 91% vs 69%).
  - Almanac scored 95% vs 0% on adversarial safety prompts.
  - Physicians nevertheless **preferred ChatGPT's answers 57% of the time**. — [Gargari & Habibi, Digital Health 2025 (PMC12059965), SECONDARY](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
- Ch.1 use:
  - 1.1: an early clinical RAG system with citations.
  - 1.2: retrieval improved factuality and safety, but clinician preference did not follow, which shows that grounding and perceived quality differ.
- Verification: **SECONDARY for numbers.** The primary PMC page returned a reCAPTCHA. The author list, volume and question count (ClinicalQA) still need checking.

**F4 — Wu et al. 2025 (SourceCheckup)**
- Metadata: "An automated framework for assessing how well LLMs cite relevant medical references". Kevin Wu, Eric Wu, Kevin Wei, Angela Zhang, Allison Casasola, Teresa Nguyen, Sith Riantawan, Patricia Shi, Daniel Ho, James Zou. Nature Communications 16 (2025). DOI 10.1038/s41467-025-58551-6.
  - Earlier arXiv 2402.02008 had a different title: "How well do LLMs cite relevant medical references? An evaluation framework and analyses".
  - Peer-reviewed. PMC record listed as PMC12003634 (could not be opened). — [RePEc/IDEAS record](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html); [arXiv](https://arxiv.org/abs/2402.02008)
- Methods: SourceCheckup is an automated, agent-based pipeline that checks whether the cited sources are relevant and support the claims. Seven popular LLMs were tested on 800 questions and about 58,000 statement–source pairs. — [IDEAS](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html)
- Findings:
  - **50–90% of LLM responses were not fully supported by their cited sources, and some were contradicted by them.**
  - **Even GPT-4o with Web Search (a retrieval-augmented setting) left ~30% of individual statements unsupported, and nearly half its responses were not fully supported.**
  - Doctors' independent assessments corroborated these results. — [IDEAS](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html)
- Limitations: not read (abstract-only). The pipeline uses an LLM as judge; its agreement figures with doctors were not extracted.
- Ch.1 use:
  - 1.2: the core evidence that citations do not guarantee support, even with retrieval. This justifies the project's claim-to-source entailment check.
- Verification: VERIFIED-ABS.

**F5 — Min et al. 2023 (FActScore)**
- Metadata: "FActScore: Fine-grained Atomic Evaluation of Factual Precision in Long Form Text Generation". Sewon Min, Kalpesh Krishna, Xinxi Lyu, Mike Lewis, Wen-tau Yih, Pang Wei Koh, Mohit Iyyer, Luke Zettlemoyer, Hannaneh Hajishirzi. EMNLP 2023 (Main), Singapore, pp. 12076–12100. DOI 10.18653/v1/2023.emnlp-main.741. Peer-reviewed. — [ACL Anthology](https://aclanthology.org/2023.emnlp-main.741)
- Method: split a generation into atomic facts and compute the percentage supported by a reliable knowledge source. — [ACL Anthology](https://aclanthology.org/2023.emnlp-main.741)
- Findings:
  - Human evaluation on biographies: ChatGPT scored 58%.
  - An automated retrieval + LM estimator has an error rate under 2%.
  - The automated estimator was used to evaluate 6,500 generations from 13 LMs; human evaluation of these would have cost about $26K. — [ACL Anthology](https://aclanthology.org/2023.emnlp-main.741)
- Limitations: the domain is biographies grounded in Wikipedia, not medicine (my inference; the full paper was not read).
- Ch.1 use:
  - 1.1: the methodological basis for claim-level (atomic) verification.
  - 1.2: even strong models have substantial unsupported content.
- Verification: VERIFIED-ABS.

**F6 — Cormack, Clarke & Büttcher 2009 (Reciprocal Rank Fusion)**
- Metadata: "Reciprocal Rank Fusion outperforms Condorcet and individual Rank Learning Methods". Gordon V. Cormack, Charles L. A. Clarke (Univ. of Waterloo), Stefan Büttcher (Google). SIGIR '09, Boston, July 2009. DOI 10.1145/1571941.1572114 (from the search listing). Peer-reviewed. — [author PDF](https://cormack.uwaterloo.ca/cormacksigir09-rrf.pdf)
- Method: RRFscore(d) = Σ_r 1/(k + r(d)) with **k = 60**, fixed in a pilot study. — [author PDF](https://cormack.uwaterloo.ca/cormacksigir09-rrf.pdf)
- Findings:
  - RRF "consistently yields better results than any individual system".
  - It beat Condorcet Fuse and CombMNZ on TREC ad hoc and Robust runs by about 4–5% on average.
  - On LETOR 3, a fused meta-learner beat all previously reported methods. — [author PDF](https://cormack.uwaterloo.ca/cormacksigir09-rrf.pdf)
- Limitations: the evaluation is general web/newswire IR, not clinical guidelines (my inference).
- Ch.1 use: 1.1 (foundation for fusing BM25 and dense rankings).
- Verification: VERIFIED-FT (author PDF).

### Inferences
- The project's design maps directly onto this literature:
  - BM25 + dense with RRF: F6, plus F2's finding that combining retrievers is best.
  - Claim-level checking: F5, R9.
  - Citation-support entailment: F4, R9.
- F3 (Almanac) and F4 show that a system can be more factual or retrieval-equipped and still fall short on clinician preference or citation support. This supports a problem statement focused on verifiability rather than accuracy alone.

### Gaps
- Almanac: primary metadata (full author list, volume/issue, ClinicalQA size, evaluator counts) and limitations were not verified because PMC/PubMed returned reCAPTCHA.
- SourceCheckup: agreement between the automated judge and doctors, and per-model figures, were not extracted (abstract-only).
- No foundational medical paper on hybrid BM25 + dense specifically for guidelines was located in this session. F2 is the closest evidence.

## Q3. Remaining limitations of RAG and negative results

### Takeaway
The evidence consistently shows that RAG reduces errors but does not remove them. Remaining problems include unsupported or contradicted citations (F4, R9), sensitivity to noisy, unlabeled or counterfactual retrieved content with near-zero ability to recognise bad evidence (L1), position effects (F2), dependence on the quality and recency of the knowledge source (L3), and non-standard evaluation (R7). One benchmark also reports cases where RAG underperformed no-retrieval baselines (L1).

### Cited Findings

**L1 — Li et al. (BioRAB)**
- Metadata: "Benchmarking Retrieval-Augmented Large Language Models in Biomedical NLP: Application, Robustness, and Self-Awareness". Mingchen Li, Zaifu Zhan, Han Yang, Yongkang Xiao, Jiatan Huang, Rui Zhang (Univ. of Minnesota). arXiv 2405.08151v2; a PMC record (PMC12637297) with the same title appears in search results, which suggests journal publication, but I did not confirm the venue. — [arXiv HTML](https://arxiv.org/html/2405.08151v2)
- Methods: 5 tasks and 9 datasets; LLaMA2-13B, MedLLaMA-13B and LLaMA3-8B; retrievers BM25, Contriever and MedCPT. Four testbeds: unlabeled, counterfactual (20/80/100% wrong labels), diverse (mixed-task corpus) and negative awareness. — [arXiv HTML](https://arxiv.org/html/2405.08151v2)
- Negative results:
  - ChemProt F1 fell from 86.91 (labeled corpus) to 47.79 at an 80% counterfactual rate.
  - ADE F1 was 9.65 with an unlabeled corpus vs 36.06 with a labeled one.
  - MedMCQA F1 was 24.87 with a mixed corpus vs 40.42.
  - **True negative-awareness rate was 0.00% (PHarmKG, BioNLI), 0.26% (MedMCQA) and 1.07% (ADE)**: models "lack the ability to determine which examples could help improve model performance".
  - LLMs **without** retrieval beat retrieval-augmented models on MedMCQA. The authors attribute this to a training-set-only corpus. — [arXiv HTML](https://arxiv.org/html/2405.08151v2)
- Positive results: unlabeled retrieval still raised ChemProt F1 from 42.05 to 72.21, and RAG improved most datasets overall. — [arXiv HTML](https://arxiv.org/html/2405.08151v2)
- Limitations (stated): small corpora, three retrievers each returning a single example, and limited datasets and models. — [arXiv HTML](https://arxiv.org/html/2405.08151v2)
- Ch.1 use: 1.2 (retrieval quality and conflicting/wrong passages; LLMs cannot tell when retrieved evidence is wrong). This motivates a separate verifier.
- Verification: VERIFIED-FT (arXiv). **Publication status needs checking.**

**L2 — Liu et al., JAMIA 2025 (KG-RAG triage)**
- Metadata: "Detecting emergencies in patient portal messages using large language models and knowledge graph-based retrieval-augmented generation". Siru Liu, Aileen P Wright, Allison B McCoy, Sean S Huang, Bryan Steitz, Adam Wright. JAMIA 32(6):1032–1039, 2025. DOI 10.1093/jamia/ocaf059; PMID 40220286. — [PMC12089757](https://pmc.ncbi.nlm.nih.gov/articles/PMC12089757/)
- Methods: 1,020 messages (41 emergency); GPT-4o; knowledge source was a nursing telephone-triage book with 225 protocols. — [PMC12089757](https://pmc.ncbi.nlm.nih.gov/articles/PMC12089757/)
- Results:

  | Approach | F1 | Precision |
  |---|---|---|
  | Prompt-only | 0.39 | 0.27 |
  | Naïve RAG | 0.59 | 0.45 |
  | KG-RAG (global search) | 0.88 | 0.80 |

  All pairwise differences were significant. — [PMC12089757](https://pmc.ncbi.nlm.nih.gov/articles/PMC12089757/)
- Stated limitations:
  - A single knowledge source that may not cover all emergencies.
  - A historical dataset only.
  - "LLMs may give correct answers through flawed reasoning".
  - A small emergency sample (n = 41). — [PMC12089757](https://pmc.ncbi.nlm.nih.gov/articles/PMC12089757/)
- Ch.1 use:
  - 1.2: naive RAG leaves large gaps, and retrieval design matters a lot.
  - The paper's future-work section names multi-agent systems with real-time guideline search, which supports the agentic framing.
- Verification: VERIFIED-FT.

**L3 — Gargari & Habibi 2025 (narrative review)**
- Metadata: "Enhancing medical AI with retrieval-augmented generation: A mini narrative review". Omid Kohandel Gargari, Gholamreza Habibi. Digital Health 11 (2025): 20552076251337177. DOI 10.1177/20552076251337177; PMID 40343063. — [PMC12059965](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
- Challenges listed: knowledge gaps and **outdated sources**; persistent hallucinations and factual errors; complex and heterogeneous guideline formats; relevance and contextual understanding; data quality and bias; difficult evaluation (standard NLP metrics miss medical nuance); limited transparency; workflow integration; ethical, legal and privacy concerns; computational cost; retrieval collapse. — [PMC12059965](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
- Mixed result: the LiVersa hepatology RAG answered 10/10 yes/no questions correctly but gave **incomplete rationales in 3 of 10 clinical scenarios**. — [PMC12059965](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
- Conclusion: RAG effectiveness "depends on the quality, completeness, and recency" of the knowledge source. — [PMC12059965](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
- Ch.1 use: 1.2 (outdated and heterogeneous guidelines; evaluation difficulty).
- Verification: VERIFIED-FT. Narrative review, lower evidence level.

**Cross-source limitations**
- Unfaithful citation persists even with web retrieval (~30% of statements unsupported for GPT-4o with Web Search). — [Wu et al. 2025](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html)
- With BM25 retrieval of the source document, 34–37% conservative hallucination remained despite 90–98% binary accuracy. — [Liang et al. 2026](https://preprints.jmir.org/preprint/93761)
- "Lost-in-the-middle" effects appear in medical RAG. — [Xiong et al. 2024](https://aclanthology.org/2024.findings-acl.372)
- Evaluation frameworks are not standardised, and most studies do not assess ethics or safety. — [Amugongo et al. 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)

### Inferences
- Retrieval errors and model reasoning errors are separate failure sources. R9 attributes its residual hallucination mainly to reasoning, since retrieval scores were similar across models. L1 shows that bad retrieved content also degrades output. A verifier is therefore needed in addition to better retrieval.
- Because LLMs almost never flag bad retrieved evidence (L1 negative awareness ≈0–1.5%), relying on the generator to self-police citations is not supported. An external entailment check plus deterministic checks is justified.

### Gaps
- I found no study in this session that directly measures guideline **staleness** (outdated versions) as a quantified failure mode in clinical RAG. L3 lists it qualitatively only.
- I found no clinical RAG study that directly compares BM25-only, dense-only and fused retrieval on guideline corpora.
- The accuracy of LLM-as-entailment-judge in medicine (the verifier's own error rate) was not quantified in the sources I read. FActScore's under-2% error rate is for biographies.

## Q4. Claim-to-source list and popular claims NOT supported

### Takeaway
The evidence supports "RAG improves accuracy modestly and makes outputs more traceable". It does not support "RAG eliminates hallucination" or "a cited source means the claim is supported". Every Chapter 1 claim below is tied to a verified (or explicitly flagged) source.

### Cited Findings

**Claim-to-source list (status in brackets)**
1. RAG combines parametric memory with a retrieved non-parametric memory and was motivated partly by the need for provenance and updatable knowledge. — F1 [VERIFIED-ABS] [NeurIPS](https://papers.nips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html)
2. RAG is widely adopted in healthcare LLM research (70 studies, 2020–Feb 2025), mostly with proprietary GPT models and English data (78.9%). — R7 [VERIFIED-FT] [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/)
3. Across 20 studies, RAG gave a pooled OR of 1.35 (95% CI 1.19–1.53) over baseline LLMs. — R8 [SECONDARY, check before citing] [iatrox](https://www.iatrox.com/blog/rag-medical-ai-jamia-meta-analysis-20-studies-iatrox)
4. Medical RAG raised accuracy by up to 18% over CoT on MIRAGE (7,663 questions), and combining corpora and retrievers performed best. — F2 [VERIFIED-ABS] [ACL](https://aclanthology.org/2024.findings-acl.372)
5. Fusing multiple rankings with RRF (k = 60) beats individual systems and Condorcet. — F6 [VERIFIED-FT] [PDF](https://cormack.uwaterloo.ca/cormacksigir09-rrf.pdf)
6. Medical LLM responses are often not fully supported by the sources they cite (50–90%), and even retrieval-enabled GPT-4o leaves ~30% of statements unsupported. — F4 [VERIFIED-ABS] [IDEAS](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html)
7. High decision-level accuracy (90–98% binary) can coexist with only 60–65% evidence support, and agreement metrics miss this. — R9 [VERIFIED-ABS] [JMIR](https://preprints.jmir.org/preprint/93761)
8. Atomic-fact decomposition with retrieval-based support checking is an established factuality metric (FActScore; estimator error under 2% on biographies). — F5 [VERIFIED-ABS] [ACL](https://aclanthology.org/2023.emnlp-main.741)
9. Retrieval-augmented models are sensitive to wrong or irrelevant retrieved content and almost never recognise it (negative awareness 0–1.5%). — L1 [VERIFIED-FT, preprint/venue unconfirmed] [arXiv](https://arxiv.org/html/2405.08151v2)
10. RAG design matters: naive RAG F1 0.59 vs KG-RAG 0.88 for emergency triage. — L2 [VERIFIED-FT] [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12089757/)
11. RAG effectiveness depends on source quality, completeness and recency, and guideline formats are heterogeneous. — L3 [VERIFIED-FT, narrative review] [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
12. Standardised evaluation frameworks for medical RAG are lacking. — R7 [VERIFIED-FT] [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12157099/); L3 [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
13. A retrieval-augmented clinical system improved factuality and adversarial safety over ChatGPT, yet physicians preferred ChatGPT's answers 57% of the time. — F3 [SECONDARY] [PMC12059965](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)

**Popular claims NOT supported by the evidence (avoid in Ch. 1)**
- "RAG eliminates hallucination."
  - Contradicted: 34–37% conservative hallucination with retrieval-based checking. — [R9](https://preprints.jmir.org/preprint/93761)
  - Contradicted: ~30% unsupported statements for GPT-4o with Web Search. — [F4](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html)
  - L3 lists persistent hallucinations as a remaining challenge. — [L3](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
- "If an answer cites a source, the claim is supported."
  - Contradicted: 50–90% of responses were not fully supported, and some were contradicted by their own citations. — [F4](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-025-58551-6.html)
- "High accuracy / agreement with experts implies the output is evidence-grounded."
  - Contradicted. — [R9](https://preprints.jmir.org/preprint/93761)
- "RAG always improves performance."
  - The pooled effect is modest, with a prediction interval lower bound of ~1.01 ([R8, SECONDARY](https://www.iatrox.com/blog/rag-medical-ai-jamia-meta-analysis-20-studies-iatrox)).
  - No-retrieval LLMs beat RAG on MedMCQA in BioRAB, and noisy or counterfactual corpora degrade results. — [L1](https://arxiv.org/html/2405.08151v2)
- "LLMs will ignore wrong or irrelevant retrieved passages."
  - Contradicted: negative awareness ≈0–1.5%. — [L1](https://arxiv.org/html/2405.08151v2)
- "More factual / grounded answers will be preferred by clinicians."
  - Not supported: Almanac vs ChatGPT preference of 57% for ChatGPT. — [SECONDARY](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)
- "RAG solves outdated knowledge automatically."
  - RAG helps only if the corpus itself is current. — [L3](https://pmc.ncbi.nlm.nih.gov/articles/PMC12059965/)

### Inferences
- Suggested wording for 1.2: "RAG yields moderate accuracy gains but does not guarantee that generated statements are supported by the cited evidence. Citation support and claim-level verification therefore remain open problems for physician-facing CDS." Each part is backed by R8 (pending), F4 and R9.

### Gaps
- Claims 3 and 13 rest on secondary sources until the R8 and Almanac primary pages are checked on PubMed, OUP or NEJM AI.
- I did not verify any "hybrid retrieval improves medical RAG" claim beyond F2's "combining retrievers" result. Avoid stronger wording.

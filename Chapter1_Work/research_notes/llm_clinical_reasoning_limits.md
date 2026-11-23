# Capabilities and limitations of single LLMs for clinical reasoning and clinical decision-making (evidence ledger for Chapter 1: 1.1 Background, 1.2 Problem Statement)

Verification legend used throughout:
- VERIFIED-FT = numbers read from the full text (publisher, accepted manuscript or arXiv full PDF)
- VERIFIED-ABS = only the abstract / publisher landing metadata was readable ("abstract-only")
- SECONDARY = numbers taken from a news or indexing page, not the paper itself; must be checked in the paper before citing
- PARTIAL = bibliographic record only partly confirmed
- PubMed and PMC pages returned a reCAPTCHA wall for every attempt in this session, so PMC full texts could not be read directly; other hosts (Nature PDF, institutional repositories, arXiv, ACL Anthology, Cureus) were used instead.

## Q1. What do the landmark studies report? (benchmark strength vs weakness in realistic decision-making)

### Takeaway
Single LLMs reach expert-level scores on exam-style medical QA (Flan-PaLM 67.6% and Med-PaLM 2 86.5% on MedQA), but when the same kind of model has to act as an autonomous clinical decision-maker on real patient records (MIMIC-CDM), it diagnoses worse than physicians, does not follow guidelines, orders tests poorly, makes errors every few patients, and changes its answer when the order or amount of information changes. One caveat for balance: Hager et al. tested only open Llama-2-70B-based models, not GPT-4.

### Cited Findings

**[L1] Singhal et al., "Large language models encode clinical knowledge"**
- Bibliographic data: Karan Singhal, Shekoofeh Azizi, Tao Tu, … Alan Karthikesalingam, Vivek Natarajan. *Nature* 2023;620(7972):172–180. DOI 10.1038/s41586-023-06291-2. PMID 37438534. Peer-reviewed. There is a publisher correction (Nature 2023;620(7973):E19). arXiv preprint 2212.13138. — [PMC full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962) (read once, before the captcha wall)
- Research problem and method: introduces the MultiMedQA benchmark (6 existing datasets plus HealthSearchQA). Evaluates Flan-PaLM 540B and the instruction-prompt-tuned Med-PaLM. Uses a clinician and lay-user human-evaluation framework. Architecture: single LLM. — [same](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
- Benchmark numbers: Flan-PaLM reached 67.6% on MedQA (USMLE, 4-option), more than 17% above the prior state of the art (Results; Extended Data Table 4). It scored 57.6% on MedMCQA, 79.0% on PubMedQA, 83.8% on MMLU professional medicine and 80.4% on MMLU clinical knowledge (Fig. 2; Extended Data Fig. 2). — [same](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
- Human evaluation of long-form answers to 140 questions (Figs. 4–6), reported as Flan-PaLM / Med-PaLM / clinicians:
  - aligned with scientific consensus: 61.9% / 92.6% / 92.9%
  - potential for harmful outcomes: 29.7% / 5.9% / 5.7%
  - incorrect or inappropriate content: 16.1% / **18.7%** / 1.4%
  - omitted important content: 47.6% / 15.3% / 11.1%
  - biased content: 7.9% / 0.8% / 1.4%
  - correct knowledge retrieval: 76.3% / 95.4% / 97.8%
  - rated helpful by lay users: 60.6% / 80.3% / 91.1%
  — [same](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
- Even the tuned model was roughly 13 times more likely than clinicians to include incorrect or inappropriate content (18.7% vs 1.4%). The authors attribute this partly to longer answers. — [same](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
- The abstract says Med-PaLM "remains inferior to clinicians". The authors also note that scale alone was not enough to produce safe answers. — [same](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
- Uncertainty: self-consistency agreement over 41 decodes was used as an uncertainty proxy. Deferring low-confidence answers raised MedQA accuracy to 82.5% at a deferral fraction of 0.45 (Fig. 3, "Uncertainty and selective prediction"). The authors call this "preliminary". — [same](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
- Limitations stated by the authors: one rater per answer, only 140 questions, English only, subjective axes, and limited evaluation of fairness. — [same](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
- Relevance and use: 1.1 (benchmark capability) and 1.2 (an exam score does not mean safety; abstaining or deferring helps).
- Status: VERIFIED-FT.

**[L2] Singhal et al., Med-PaLM 2: "Toward expert-level medical question answering with large language models"**
- Publication status: published in *Nature Medicine* (2025), DOI 10.1038/s41591-024-03423-7, with about 35 authors (affiliations mainly Google LLC). — [Nature Index record](https://www.nature.com/nature-index/article/10.1038/s41591-024-03423-7)
- Preprint: arXiv 2305.09617, "Towards Expert-Level Medical Question Answering with Large Language Models", v1, 16 May 2023. — [arXiv](https://arxiv.org/abs/2305.09617)
- A PMC record exists (PMC11922739) but could not be read because of the captcha wall. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/11922739/)
- Volume and pages were NOT verified. Do not cite pages until they are checked.
- Numbers from the arXiv v1 abstract (the published version may differ slightly):
  - Med-PaLM 2 scored up to 86.5% on MedQA, versus 67.2% for Med-PaLM, a gain of more than 19%.
  - In pairwise ranking of 1,066 consumer medical questions, physicians preferred Med-PaLM 2 answers over physician answers on 8 of 9 axes (p < 0.001).
  - On 240 adversarial long-form questions, it improved significantly over Med-PaLM on every axis.
  - The authors say real-world validation is still needed.
  — [arXiv](https://arxiv.org/abs/2305.09617)
- Architecture: single LLM, with ensemble-refinement prompting.
- Relevance and use: 1.1 (strongest "benchmark capability" evidence) and 1.2 ("still needs real-world validation").
- Status: VERIFIED-ABS (arXiv abstract). Journal metadata only from Nature Index. Figures and tables were not read.

**[L3] Hager et al., "Evaluation and mitigation of the limitations of large language models in clinical decision-making"**
- Bibliographic data: Paul Hager, Friederike Jungmann, Robbie Holland, Kunal Bhagat, Inga Hubrecht, Manuel Knauer, Jakob Vielhauer, Marcus Makowski, Rickmer Braren, Georgios Kaissis, Daniel Rueckert. *Nature Medicine* 2024. DOI 10.1038/s41591-024-03097-1. Peer-reviewed (PMC11405275). The medRxiv preprint is 10.1101/2024.01.26.24301810. The version read was the author/accepted version in the Helmholtz Munich repository; volume and pages are not shown there. — [Helmholtz repository full text](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Dataset: MIMIC-CDM, built from MIMIC-IV. It covers 2,400 emergency department patients with acute abdominal pain: appendicitis 957, cholecystitis 648, pancreatitis 538 and diverticulitis 257. MIMIC-CDM-FI gives all information at once. — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Models: only Llama-2-70B-based open models (Llama 2 Chat, OASST, WizardLM, Clinical Camel, Meditron; Table 1). GPT-4, ChatGPT and Med-PaLM were not tested because of MIMIC data-use rules. Clinical Camel and Meditron could not complete the autonomous task. — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Architecture: a single LLM acting as an agent that iteratively requests exams, labs and imaging through tools.
- Diagnosis vs clinicians (MIMIC-CDM-FI reader subset, Fig. 2): LLMs were significantly worse than doctors (P < 0.001). The extraction reported gaps of about 16–25 percentage points, with doctors' means around 87.5–92.5%. Read the exact values from Fig. 2 before citing. — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Autonomous information-gathering setting (Fig. 3): mean accuracy fell compared with full information.
  - Llama 2 Chat: 58.8% → 45.5%
  - OASST: 67.8% → 54.9%
  - WizardLM: 65.1% → 53.9%
  — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Information gathering (Extended Data Figs. 2–3):
  - Physical examination was the first action in 97.1% of cases for Llama 2 Chat, but only 79.8% for OASST and 53.1% for WizardLM.
  - No model consistently ordered all required lab categories. For example, OASST ordered pancreatic enzymes for pancreatitis only 56.5% of the time.
  — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Lab interpretation (Extended Data Fig. 4), reported as Chat / OASST / WizardLM: low results 26.5% / 70.2% / 45.8%; high results 50.1% / 77.2% / 24.1%. — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Guideline adherence (Fig. 4): models often missed case-specific treatments such as colectomy or drainage, especially in severe cases. Section title: "Current LLMs are hasty and unsafe clinical decision-makers". — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Instruction following (Extended Data Fig. 6): action errors occurred every 2–4 patients, and invented (hallucinated) tools every 2–5 patients. — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Robustness:
  - Small wording changes moved accuracy by up to about 16 points (Extended Data Fig. 7).
  - Reordering the same information changed accuracy by up to 18.0% (Fig. 6).
  - More information was not always better: a single exam often beat full information. Chat's cholecystitis accuracy rose 18.5% with the radiology report alone (Fig. 5).
  — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Mitigations: summarization and filtering to abnormal labs helped modestly (Extended Data Fig. 8). — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Conclusion: current LLMs are not ready for autonomous clinical decision-making and need extensive clinician supervision. — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Limitations stated by the authors:
  - four pathologies and one chief complaint
  - one US hospital, English only
  - a small clinician comparison (the extraction said "four hospitalists and 100 cases", while the Methods reader subset is n = 80; check this inconsistency in the paper)
  - quantized models with a 4,096-token context
  - no closed models
  — [same](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Relevance and use: the core 1.2 evidence. It motivates the project's deterministic guideline/safety rules, its order-independent fact aggregation, a decision core that can return NEED_MORE_INFO, and tool-call validation.
- Status: VERIFIED-FT (accepted version). Exact Fig. 2 values and the reader-study size need confirming against the final version.

**[L4] Thirunavukarasu et al., "Large language models in medicine"**
- Bibliographic data: Arun James Thirunavukarasu, Darren Shu Jeng Ting, Kabilan Elangovan, Laura Gutierrez, Ting Fang Tan, Daniel Shu Wei Ting. *Nature Medicine* 2023;29:1930–1940. DOI 10.1038/s41591-023-02448-8. Review article. Published 17 July 2023, August 2023 issue. — [Nature PDF landing](https://www.nature.com/articles/s41591-023-02448-8.pdf); [Cambridge repository (CC BY 4.0 accepted version)](https://www.repository.cam.ac.uk/handle/1810/349830)
- Content that could be verified: the abstract says LLM chatbots "have already been deployed in a range of biomedical contexts, with impressive but mixed results". The review weighs strengths and limitations and is framed as a primer for clinicians. Fig. 3 covers "Limitations, priorities for research and development and potential use-cases". — [same](https://www.nature.com/articles/s41591-023-02448-8.pdf)
- Architecture: review (not an experiment).
- Relevance and use: 1.1 framing citation ("impressive but mixed results").
- Status: VERIFIED-ABS. The body text was not readable, so do NOT attribute specific claims about hallucination, bias or privacy to this paper until the body is checked.

### Inferences
- The project's central contrast, "high exam scores ≠ safe autonomous decision-making", is directly supported by L1/L2 on one side and L3 on the other.
- Most of the failure modes in Hager et al. are the ones a deterministic orchestration layer can target:
  - guideline non-adherence → rule engine
  - missing tests → NEED_MORE_INFO / test-request logic
  - order and quantity sensitivity → structured fact extraction before reasoning
  - invented tools → tool-call validation
- Hager et al. tested 2023-era open 70B models only. Chapter 1 should state that explicitly, rather than implying the result holds for GPT-4-class or 2025 reasoning models.

### Gaps
- Published Med-PaLM 2 (Nature Medicine 2025) volume and pages, and any changes from the arXiv numbers, were not verified because PMC was captcha-blocked.
- Exact clinician-vs-LLM accuracies per pathology in Hager Fig. 2 were not captured verbatim.
- No study was found in this session that replicates MIMIC-CDM with GPT-4o or o1-class reasoning models.

## Q2. Verification of seed source (6): "Large language models for clinical artificial intelligence in healthcare: a systematic review" (2025)

### Takeaway
The title and URL are real: the Springer page with DOI suffix s44163-025-00784-x appears in search indexing under the title "Large language models for clinical artificial intelligence in healthcare a systematic review", and a ResearchGate record exists. Every attempt to open the article (Springer HTML, Springer PDF, doi.org, Crossref, ResearchGate) failed. Authors, volume, article number, scope and findings are therefore UNVERIFIED and must not be written into Chapter 1 from this session's notes.

### Cited Findings
- A search index lists "Large language models for clinical artificial intelligence in healthcare a systematic review" at link.springer.com/article/10.1007/s44163-025-00784-x. — [Springer (listing only; fetch failed)](https://link.springer.com/article/10.1007/s44163-025-00784-x)
- A ResearchGate record (publication 400166518) carries the same title. The fetch failed. — [ResearchGate](https://www.researchgate.net/publication/400166518_Large_language_models_for_clinical_artificial_intelligence_in_healthcare_a_systematic_review)
- Reference ID L5. Status: PARTIAL (title and DOI string only).

### Inferences
- The "s44163" journal code in the DOI is the code used by Springer's *Discover Artificial Intelligence*. That fits the coordinator's guess, but it is an inference from the DOI pattern and was not confirmed on the page.
- A ResearchGate ID around 400 million suggests the record was created around late 2025 or early 2026, so the online date may differ from the "2025" volume year. This is unconfirmed.

### Gaps
- Authors, volume/article number, publication date, number of included studies, databases, PRISMA details and findings are all unknown.
- Action for the user: open the Springer page in a browser and copy the "Cite this article" block and the abstract. Until then, cite it only as "(6) [details to be confirmed]".
- Different, verified systematic reviews surfaced in the same searches and could replace or supplement it, but they were NOT read in this session:
  - "A systematic review of large language model (LLM) evaluations in clinical medicine", BMC Med Inform Decis Mak 2025 — [Springer](https://link.springer.com/article/10.1186/s12911-025-02954-4)
  - "Reasoning with large language models in medicine: a systematic review…", Health Inf Sci Syst 2025 — [Springer](https://link.springer.com/article/10.1007/s13755-025-00403-0)

## Q3. How do LLMs handle incomplete or conflicting information, ask for more information, or abstain?

### Takeaway
Three independent lines of evidence (MIMIC-CDM, CRAFT-MD, MediQ) show that LLMs lose substantial accuracy when they must gather information themselves instead of receiving a complete vignette. They often fail to ask the key history questions or order the needed tests. Explicit abstention or confidence-gating strategies recover part of the gap. This directly motivates a NEED_MORE_INFO / ABSTAIN output.

### Cited Findings

**[L6] Li et al., "MediQ: Question-Asking LLMs and a Benchmark for Reliable Interactive Clinical Reasoning"**
- Bibliographic data: Shuyue Stella Li, Vidhisha Balachandran, Shangbin Feng, Jonathan S. Ilgen, Emma Pierson, Pang Wei Koh, Yulia Tsvetkov. arXiv 2406.00922 (v1 3 June 2024; v3 7 November 2024). Presented as a NeurIPS 2024 poster (virtual page 94856). The page does not say whether it was the main track or the Datasets & Benchmarks track. — [arXiv](https://arxiv.org/abs/2406.00922); [NeurIPS 2024 poster page](https://neurips.cc/virtual/2024/poster/94856)
- Method: a Patient System and an adaptive Expert System. Single-turn medical benchmarks are converted into an interactive format, and the Expert System may abstain from diagnosing when it is not confident. Architecture: single LLM with abstention / confidence modules. — [arXiv](https://arxiv.org/abs/2406.00922)
- Findings:
  - LLMs are trained to answer even when the context is incomplete.
  - Directly prompting state-of-the-art LLMs to ask questions *degrades* performance.
  - Abstention strategies improve diagnostic accuracy by 22.3%, yet performance still lags the full-information upper bound.
  - Filtering irrelevant context and reformatting conversations help further.
  — [arXiv](https://arxiv.org/abs/2406.00922)
- Relevance and use: 1.2. It directly supports a project design that separates "decide whether there is enough information" from "answer".
- Status: VERIFIED-ABS. The size of the drop and the datasets (MedQA, Craft-MD) are not confirmed from the abstract. Peer-reviewed: accepted at NeurIPS 2024 (poster).

**[L7] Johri et al., "An evaluation framework for clinical use of large language models in patient interaction tasks" (CRAFT-MD)**
- Bibliographic data: Shreya Johri, Jaehwan Jeong, Benjamin A. Tran, Daniel I. Schlessinger, Shannon Wongvibulsin, Leandra A. Barnes, Hong-Yu Zhou, Zhuo Ran Cai, Eliezer M. Van Allen, David Kim, Roxana Daneshjou, Pranav Rajpurkar. *Nature Medicine* 2025. DOI 10.1038/s41591-024-03328-5. Accepted 1 October 2024. Peer-reviewed. Earlier medRxiv preprint 10.1101/2023.09.12.23295399. The copy read showed placeholder dates, and volume and pages were not shown. — [full-text copy](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Dataset: 2,000 vignettes (1,800 MedQA-USMLE across 12 specialties, 100 Derm-Public, 100 Derm-Private). Each conversation was repeated 5 times, giving 10,000 conversations. Architecture: a single LLM as "doctor" plus a simulated patient agent and a grader agent. — [same](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Accuracy, vignette → multi-turn conversation, with 4 multiple-choice options (Fig. 2):
  - GPT-4: 0.820 → 0.627
  - GPT-3.5: 0.657 → 0.467
  - Mistral-v2-7b: 0.637 → 0.426
  - LLaMA-2-7b: 0.395 → 0.319
  — [same](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Accuracy with free response and no options: GPT-4 fell from 0.486 to 0.264. — [same](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Summarizing the conversation helped (GPT-4 MCQ 0.627 → 0.669) but stayed below vignette accuracy. — [same](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Expert-rated history-taking (Fig. 3, skin-disease case study):
  - GPT-4 gathered complete relevant history in 71.11% of conversations, GPT-3.5 in 31.11%, Mistral-v2-7b in 8.88% and LLaMA-2-7b in 51.11%.
  - The authors say LLMs "frequently missed critical details during history-taking".
  — [same](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- The paper gives 10 evaluation recommendations (Table 1), including continuous monitoring across model versions (Fig. 5, Mistral v1 vs v2). — [same](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Relevance and use: 1.2 (information gathering; the vignette-to-conversation gap).
- Status: VERIFIED-FT (version with placeholder date; probably the final text). Confirm volume and pages.

**[L3] Hager et al.** (details under Q1)
- Accuracy dropped about 11–13 points when models had to gather information themselves, compared with full information.
- Models failed to consistently request physical examination or the required lab categories.
— [Hager, Helmholtz repository](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)

**[L1] Singhal et al.** (details under Q1)
- Deferring low-confidence answers (self-consistency) raised MedQA accuracy to 82.5% at a 0.45 deferral fraction. This is early evidence that abstention improves reliability.
— [Singhal 2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)

### Inferences
- Across L3, L6 and L7, the "complete vignette" format of common benchmarks overstates clinical readiness. Performance drops in every interactive or incomplete-information setting tested, roughly 10–20 accuracy points in the cited numbers.
- Evidence on *conflicting* information (as opposed to missing or reordered information) is thin. The closest finding is Hager et al.'s order and quantity sensitivity, where extra or reordered facts change the decision.

### Gaps
- No peer-reviewed study read in this session tested LLMs on explicitly contradictory patient data, such as discordant lab and history findings. This should be stated as a gap or a project contribution rather than claimed from the literature.
- MediQ's exact datasets and the size of the drop need the full paper.

## Q4. Hallucination, fabricated references, calibration and overconfidence, and inconsistency across runs

### Takeaway
Hallucination is measurable and substantial:
- medical-exam-based tests (Med-HALT)
- fabricated or inaccurate citations (93% of ChatGPT-3.5 references were fabricated or inaccurate in one study)
- tool invention in agentic settings (Hager)

Medical LLMs are poorly calibrated: 12 models almost never recognized unanswerable questions (MetaMedQA). Outputs also vary across repeated runs (Omiye; CRAFT-MD repeated each conversation 5 times for this reason).

### Cited Findings

**[L8] Pal, Umapathi & Sankarasubbu, "Med-HALT: Medical Domain Hallucination Test for Large Language Models"**
- Bibliographic data: Proceedings of CoNLL 2023, pp. 314–334. DOI 10.18653/v1/2023.conll-1.21. Peer-reviewed. arXiv 2307.15343. — [ACL Anthology](https://aclanthology.org/2023.conll-1.21)
- Design: built from multinational medical exams. It has two test families: reasoning hallucination tests (False Confidence Test, None-of-the-Above, Fake Questions) and memory hallucination tests (PubMed ID/title/link retrieval). Models evaluated: Text-Davinci, GPT-3.5, Llama-2, MPT and Falcon. Architecture: single LLMs. — [ACL Anthology](https://aclanthology.org/2023.conll-1.21)
- Reasoning results (Table 2):
  - The best reasoning average was Llama-2-70B at 72.33% accuracy. GPT-3.5 averaged 44.48% and Llama-2-70B-Chat 11.26%.
  - False Confidence Test accuracy was at most 42.21% (Llama-2-70B), and the paper says no model reached acceptable FCT accuracy.
  — [arXiv PDF](https://arxiv.org/pdf/2307.15343)
- Memory results (Table 3):
  - Average accuracy was at most 30.36% (Falcon-40B). GPT-3.5 averaged 19.96%.
  - GPT-3.5 scored 0.29% on PMID-to-title.
  — [arXiv PDF](https://arxiv.org/pdf/2307.15343)
- Caveat: the text claims that open models beat commercial ones on all tasks, but Table 3 partly contradicts that. — [arXiv PDF](https://arxiv.org/pdf/2307.15343)
- Status: VERIFIED-FT (arXiv). The models are from 2023.

**[L9] Bhattacharyya, Miller, Bhattacharyya & Miller, "High Rates of Fabricated and Inaccurate References in ChatGPT-Generated Medical Content"**
- Bibliographic data: *Cureus* 2023;15(5):e39238. DOI 10.7759/cureus.39238. Peer-reviewed (Cureus). — [Cureus PDF](https://assets.cureus.com/uploads/original_article/pdf/158289/20230618-23404-wiiz34.pdf)
- Method: ChatGPT-3.5 generated 30 short medical papers. Two researchers checked the 115 references against Medline, Google Scholar and DOAJ. — [same](https://assets.cureus.com/uploads/original_article/pdf/158289/20230618-23404-wiiz34.pdf)
- Results:
  - 47% (54/115) of references were fabricated.
  - 46% (53/115) were authentic but inaccurate.
  - Only 7% (8/115) were authentic and accurate.
  - An incorrect PMID appeared in 93% of papers.
  — [same](https://assets.cureus.com/uploads/original_article/pdf/158289/20230618-23404-wiiz34.pdf)
- Limitations: GPT-3.5 only, results were sensitive to prompt wording, and the text of the papers was not checked. — [same](https://assets.cureus.com/uploads/original_article/pdf/158289/20230618-23404-wiiz34.pdf)
- Relevance and use: 1.2 (lack of traceability). It motivates retrieval-grounded claims and claim verification.
- Status: VERIFIED-FT.

**[L10] Griot, Hemptinne, Vanderdonckt & Yuksel, "Large Language Models lack essential metacognition for reliable medical reasoning"**
- Bibliographic data: *Nature Communications* 2025;16. DOI 10.1038/s41467-024-55628-6. Published around 2 February 2025 (RePEc metadata). Peer-reviewed. — [RePEc/IDEAS record with abstract](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-024-55628-6.html)
- Method: the MetaMedQA benchmark (multiple-choice questions plus confidence scores, missing-answer and unknown-answer tasks). 12 models were tested. Architecture: single LLMs. — [same](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-024-55628-6.html)
- Abstract findings: models achieved high accuracy on multiple-choice questions but "consistently failed to recognize their knowledge limitations" and gave confident answers even when the correct option was absent. — [same](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-024-55628-6.html)
- Numbers from a SECONDARY news report only:
  - 1,373 six-option questions
  - GPT-4o-2024-05-13 had the highest accuracy, 73.3%
  - unknown recall was 0% for all but three models, with GPT-4o best at only 3.7%
  - most models reported maximum confidence (5 of 5) regardless of accuracy
  — [News-Medical (secondary)](https://www.news-medical.net/news/20250115/Study-reveals-AIe28099s-critical-flaw-in-medical-decision-making.aspx)
- Relevance and use: 1.2 (calibration and overconfidence). It supports deterministic abstention rather than relying on the LLM's self-reported confidence.
- Status: VERIFIED-ABS. The numbers are SECONDARY; confirm them in the paper's results before citing.

**[L11] Omiye, Lester, Spichak, Rotemberg & Daneshjou, "Large language models propagate race-based medicine"**
- Bibliographic data: *npj Digital Medicine* 2023;6:195. DOI 10.1038/s41746-023-00939-z. Brief Communication. Peer-reviewed. — [Nature PDF](https://www.nature.com/articles/s41746-023-00939-z.pdf)
- Method: ChatGPT, GPT-4, Bard and Claude were tested in May–August 2023 on 9 questions. The abstract says 8; this discrepancy is in the paper itself. Each question was asked 5 times. — [same](https://www.nature.com/articles/s41746-023-00939-z.pdf)
- Findings:
  - Every model produced some race-based content (eGFR, lung capacity, skin thickness).
  - Models sometimes invented equations.
  - "The concerning outputs do not always appear in all five runs of the same question", which is evidence of run-to-run inconsistency.
  — [same](https://www.nature.com/articles/s41746-023-00939-z.pdf)
- Relevance and use: 1.2 (bias, fabrication, inconsistency across runs).
- Status: VERIFIED-FT.

**[L3] Hager et al.** (details under Q1)
- Invented, non-existent tools appeared every 2–5 patients.
- Accuracy changed by up to about 16 points under minor prompt rewording.
— [Hager](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)

**[L1] Singhal et al.** (details under Q1)
- Med-PaLM still produced incorrect or inappropriate content in 18.7% of answers, versus 1.4% for clinicians.
— [Singhal 2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)

### Inferences
- Ungrounded generation (L8, L9) and overconfidence (L10) together argue for:
  1. retrieval with traceable sources
  2. claim-level verification
  3. abstention decided by deterministic rules rather than by the LLM's own confidence
- Run-to-run variability (L11, L3) supports evaluating each configuration over repeated runs and reporting variance in the project's experiments.

### Gaps
- Exact MetaMedQA numbers have not been read from the primary paper.
- No 2025–2026 study on reference fabrication by current frontier models in medicine was read. L9 covers GPT-3.5 only, so Chapter 1 should not claim current models fabricate at 47%.
- A dedicated study quantifying test-retest consistency of clinical LLM decisions (for example, an agreement statistic across runs) was not found or read in this session.

## Q5. Documented risks of general-purpose models in healthcare (bias, data quality, outdated knowledge, lack of traceability)

### Takeaway
Peer-reviewed evidence verified in this session covers bias and race-based content (L11), fabrication and lack of traceable sources (L9, L8), unsafe or non-guideline decisions and dependence on data presentation (L3), and the general "impressive but mixed" framing (L4). Specific claims about outdated knowledge and privacy could not be tied to verified text in this session.

### Cited Findings
- Bias: all four commercial LLMs tested produced race-based medical content. — [Omiye 2023](https://www.nature.com/articles/s41746-023-00939-z.pdf)
- Bias in tuned models: Flan-PaLM answers were rated biased in 7.9% of cases, Med-PaLM in 0.8% and clinicians in 1.4%. — [Singhal 2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962)
- Traceability: 93% of references in ChatGPT-3.5 medical text were fabricated or inaccurate. — [Bhattacharyya 2023](https://assets.cureus.com/uploads/original_article/pdf/158289/20230618-23404-wiiz34.pdf)
- Traceability: LLMs scored near zero on most PubMed link/ID retrieval tasks. — [Med-HALT](https://arxiv.org/pdf/2307.15343)
- Data quality and presentation: decisions changed with the order and amount of information. Filtering to abnormal labs improved results, which shows sensitivity to how the input is curated. — [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)
- Model drift: CRAFT-MD recommends continuous monitoring because model versions differ (Mistral v1 vs v2, Fig. 5). — [Johri 2025](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)
- Scope of evidence: Hager's single-hospital, English-only data limits generalizability, and the authors note this explicitly. — [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883)

### Inferences
- "Outdated knowledge" follows logically from fixed training cut-offs. In this session it is not backed by a verified quotation, so phrase it as a design rationale for retrieval, or find a citable source first.

### Gaps
- Thirunavukarasu et al.'s body text on privacy, explainability and outdated training data was not readable. Do not cite it for those specific points until checked.
- No peer-reviewed quantification of the harm from outdated clinical knowledge in LLMs, such as superseded guidelines, was read.

## Q6. Contradictory and balancing evidence (LLMs matching or exceeding physicians)

### Takeaway
Several rigorous studies show LLMs alone outperforming physicians in specific, mostly text-vignette, diagnostic tasks:
- AMIE: 59.1% vs 33.6% top-10 accuracy on hard cases
- GPT-4 in Goh et al.: 16 points above physicians using conventional resources
- Med-PaLM 2 answers preferred over physician answers on 8 of 9 axes

The problem statement should therefore say "strong on curated diagnostic and QA tasks, unreliable in realistic, incomplete, interactive decision workflows", not "LLMs are bad at medicine".

### Cited Findings

**[L12] Goh et al., "Large Language Model Influence on Diagnostic Reasoning: A Randomized Clinical Trial"**
- Bibliographic data: *JAMA Network Open* 2024;7(10):e2440969. DOI 10.1001/jamanetworkopen.2024.40969. Peer-reviewed. — [PSNet summary](https://psnet.ahrq.gov/issue/large-language-model-influence-diagnostic-reasoning-randomized-clinical-trial); [UMN abstract](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/)
- Design: 50 physicians (26 attendings, 24 residents) were randomized to GPT-4 plus conventional resources or conventional resources alone. Each worked through 4–6 vignettes. — [UMN abstract](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/)
- Physician scores: the median diagnostic-reasoning score was 76% with the LLM vs 74% without (difference 2 points, 95% CI −4 to 8, P = .60). Time per case did not differ significantly. — [same](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/)
- LLM alone: scored 16 points higher than the conventional-resources group (95% CI 2–30, P = .03). This was a secondary, exploratory analysis. — [same](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/)
- Status: VERIFIED-ABS.

**[L13] McDuff et al., "Towards accurate differential diagnosis with large language models" (AMIE)**
- Bibliographic data: *Nature* 2025;642(8067):451–457. DOI 10.1038/s41586-025-08869-4. Peer-reviewed. arXiv 2312.00164. — [RePEc/IDEAS record](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08869-4.html)
- Design: 302 challenging real-world case reports (NEJM clinicopathological conferences) and 20 clinicians. — [same](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08869-4.html)
- Top-10 differential-diagnosis accuracy:
  - AMIE alone: 59.1%
  - unassisted clinicians: 33.6% (P = 0.04 vs AMIE alone)
  - clinicians with search and standard resources: 36.1%
  - clinicians assisted by AMIE: 51.7%
  — [same](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08869-4.html)
- Status: VERIFIED-ABS (the abstract was truncated on the page).

**[L2] Med-PaLM 2** (details under Q1)
- Physicians preferred Med-PaLM 2 answers over physician answers on 8 of 9 axes (1,066 questions).
— [arXiv 2305.09617](https://arxiv.org/abs/2305.09617)

**[L7] CRAFT-MD** (details under Q3)
- With a complete vignette, GPT-4 reached 0.820 accuracy on 4-option questions. Its weakness appeared in conversational information gathering, not in knowledge itself.
— [Johri 2025](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf)

### Inferences
- The pro-LLM results come from curated, complete case descriptions (vignettes, case reports) with a diagnosis-only endpoint. The negative results come from settings that require information gathering, guideline-conformant treatment, or robustness to presentation.
- The two sets of findings are compatible: they measure different things. Chapter 1 can present this as the precise research gap.
- Goh et al. also show that physicians did not improve simply by having an LLM available. This supports structured system design over "just give clinicians a chatbot".

### Gaps
- No study read here compares a multi-agent orchestrated clinical system against single-LLM baselines. Evidence on multi-agent systems is outside this researcher's scope and should come from the parallel notes.

## Q7. Claim-to-source list for candidate Chapter 1 claims, and claims the evidence does NOT support

### Takeaway
Most problem-statement claims can be anchored to verified, peer-reviewed sources (L1, L3, L6–L13). Several popular sweeping claims are not supported and should be avoided or qualified.

### Cited Findings (candidate claim → source; strength)

**1.1 Background**
- C1. LLMs achieve high accuracy on medical licensing-style benchmarks (67.6% → 86.5% on MedQA within a few months, 2022–2023). — [Singhal 2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962); [Med-PaLM 2 arXiv](https://arxiv.org/abs/2305.09617). Strong; cite the published Nat Med 2025 version for Med-PaLM 2.
- C2. LLM chatbots have been deployed in many biomedical contexts with "impressive but mixed results". — [Thirunavukarasu 2023](https://www.nature.com/articles/s41591-023-02448-8.pdf). Strong (abstract).
- C3. On difficult diagnostic case reports and vignettes, some LLMs outperform unassisted physicians. — [McDuff 2025](https://ideas.repec.org/a/nat/nature/v642y2025i8067d10.1038_s41586-025-08869-4.html); [Goh 2024](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/). Strong, task-specific.

**1.2 Problem Statement**
- C4. High exam accuracy does not equal clinical safety: even a tuned model gave incorrect or inappropriate content in 18.7% of long-form answers, vs 1.4% for clinicians. — [Singhal 2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962). Strong.
- C5. In realistic autonomous decision-making on real patient records, LLMs diagnosed significantly worse than physicians and did not follow treatment guidelines. — [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883). Strong, but limited to open Llama-2-based models.
- C6. LLMs fail to request necessary examinations and tests consistently. — [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883); [Johri 2025](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf). Strong.
- C7. Accuracy drops when information must be gathered interactively rather than given upfront (GPT-4 0.820 → 0.627). — [Johri 2025](https://gwern.net/doc/ai/nn/transformer/gpt/4/nonfiction/2025-johri.pdf); [Li 2024 MediQ](https://arxiv.org/abs/2406.00922); [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883). Strong.
- C8. LLM decisions are sensitive to the order and amount of information and to prompt wording (up to 18% from reordering). — [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883). Strong (one study).
- C9. LLMs tend to answer even when information is insufficient. Abstention or confidence-gating improves accuracy (MediQ +22.3%; Med-PaLM deferral reached 82.5%). — [Li 2024](https://arxiv.org/abs/2406.00922); [Singhal 2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC10396962). Strong.
- C10. Medical LLMs are overconfident and rarely recognize unanswerable questions. — [Griot 2025](https://ideas.repec.org/a/nat/natcom/v16y2025i1d10.1038_s41467-024-55628-6.html). Strong qualitatively; get numbers from the primary paper.
- C11. LLMs hallucinate medical facts and references (Med-HALT). With ChatGPT-3.5, 47% of references were fabricated and 46% inaccurate. — [Med-HALT](https://aclanthology.org/2023.conll-1.21); [Bhattacharyya 2023](https://assets.cureus.com/uploads/original_article/pdf/158289/20230618-23404-wiiz34.pdf). Strong for 2023-era models.
- C12. In agentic settings, LLMs make instruction-following errors and invent non-existent tools every few patients. — [Hager 2024](https://push-zb.helmholtz-munich.de/deliver.php?id=35883). Strong.
- C13. Outputs vary across repeated runs, and harmful content may appear in only some runs. — [Omiye 2023](https://www.nature.com/articles/s41746-023-00939-z.pdf). Moderate (qualitative).
- C14. General-purpose LLMs can reproduce biased, race-based medical content. — [Omiye 2023](https://www.nature.com/articles/s41746-023-00939-z.pdf). Strong.
- C15. Simply giving physicians access to an LLM did not significantly improve their diagnostic reasoning. — [Goh 2024](https://experts.umn.edu/en/publications/large-language-model-influence-on-diagnostic-reasoning-a-randomiz/). Strong (one RCT, 50 physicians).

### Inferences — popular claims the evidence does NOT support (avoid or qualify)
- "LLMs are unreliable in all clinical tasks." Contradicted by L2, L12 and L13, where LLMs equal or beat physicians on curated diagnostic and QA tasks.
- "LLMs outperform doctors at clinical decision-making." Only shown for vignette or case-report diagnosis (L12, L13). L3 shows the opposite for autonomous, guideline-based decision-making on real records.
- "GPT-4 / current frontier models fail MIMIC-CDM." Not tested; Hager evaluated only Llama-2-70B-based models (L3).
- "LLMs fabricate about half of all medical references." Shown only for ChatGPT-3.5 in 2023 on 115 references (L9). Do not generalize to current models.
- "More patient information always improves LLM decisions." Contradicted by L3 (a single exam sometimes beat full information).
- "LLMs know when they don't know" or "LLM self-reported confidence can gate safety." Contradicted by L10. Self-consistency deferral (L1) is a different, sampling-based signal and was called preliminary.
- "Prompting an LLM to ask clarifying questions solves missing information." Contradicted by L6 (naive question-asking degraded performance).
- "Med-PaLM 2 is only an arXiv preprint." Outdated; it was published in Nature Medicine 2025 (DOI 10.1038/s41591-024-03423-7) (L2).
- Any specific statement attributed to seed source (6) or to the body of Thirunavukarasu et al. on privacy or outdated knowledge. Not verified in this session.

### Gaps
- No verified source yet quantifies LLM behaviour under explicitly *conflicting* patient information. Present this as an open problem the project addresses, not as an established finding.
- Seed source (6) metadata still needs manual verification by the user.
- Published Med-PaLM 2 volume and pages, MetaMedQA primary numbers, Hager Fig. 2 exact values and CRAFT-MD volume and pages still need checking. These sources were reachable only through captcha-blocked PMC or partial pages in this session.

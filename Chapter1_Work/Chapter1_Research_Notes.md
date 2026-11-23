# Chapter 1 · Research Notes and Methodological Decisions

Project: **Agentic Framework for Healthcare Clinical Decision Support** (Research Track)
Started: 2026-11-23 · Companion files: `Stage1_Project_Audit.md`, `Research_Matrix.xlsx`, `Claim_Source_Ledger.xlsx`, `Verified_References.bib`, `Critical_Review.md`

---

## Stage 2 · Research strategy

### Research questions the literature must answer for Chapter 1
1. What is CDSS, how did it evolve, and what limits conventional systems? (1.1, 1.3)
2. What can LLMs do in clinical reasoning, and where do single-model approaches fail (evidence grounding, hallucination, incomplete information, consistency)? (1.1, 1.2)
3. What do LLM multi-agent systems add in clinical decision support, what are their reported results, and do gains persist against strong, compute-matched single-agent baselines? (1.1, 1.2, 1.3)
4. What does RAG improve, what does it leave unsolved (retrieval errors, citation faithfulness)? (1.1, 1.2)
5. How are such systems evaluated fairly, on which data, and with which safety mechanisms (guardrails, abstention, human oversight, auditability)? (1.2, 1.4, 1.5)
6. Which institutional guidance sets the boundaries of AI CDS (WHO, FDA CDS guidance, NIST AI RMF, DECIDE-AI, SFDA)? (1.1, 1.5)

### Themes and search strings (adapted per database)

| Theme | Core search string |
|---|---|
| A · CDSS | ("clinical decision support system" OR CDSS) AND (overview OR review OR limitations OR "alert fatigue") |
| B · LLMs in clinical reasoning | ("large language model" OR LLM) AND ("clinical decision" OR "clinical reasoning" OR diagnosis) AND (evaluation OR limitations OR hallucination) |
| C · Multi-agent / agentic | ("multi-agent" OR "LLM agents" OR "agentic") AND (clinical OR medical OR healthcare) AND ("decision support" OR diagnosis); ("single-agent" AND "multi-agent") AND (comparison OR "compute") |
| D · RAG and evidence | ("retrieval-augmented generation" OR RAG) AND (medical OR clinical OR biomedical) AND (review OR benchmark OR citation OR faithfulness) |
| E · Safety and trust | (hallucination OR "unsupported claims" OR abstention OR "selective prediction" OR guardrail) AND (medical OR clinical) AND LLM |
| F · Evaluation and data | (benchmark OR evaluation framework OR "human evaluation") AND (clinical LLM); MIMIC AND LLM AND (policy OR "data use") |

### Databases
Google Scholar, PubMed / PMC, IEEE Xplore, ACM Digital Library, ScienceDirect, SpringerLink, Semantic Scholar, ACL Anthology, arXiv (preprints, labelled); institutional: WHO, FDA, NIST, AHRQ, SFDA, PhysioNet.

### Inclusion criteria
- Peer-reviewed systematic reviews, meta-analyses, empirical studies, established conference papers; official institutional guidance.
- 2021–2026 for the current state; older foundational works only for definitions and established concepts (labelled foundational).
- Directly addresses CDSS, LLM clinical reasoning, LLM agents/MAS in medicine, medical RAG, clinical AI safety or evaluation.

### Exclusion criteria
- Opinion pieces, blogs, vendor material (except official guidance) as evidence of effectiveness.
- Sources known only from a title or search snippet.
- Preprints for central claims when a peer-reviewed alternative exists; any preprint used is labelled as such.
- Non-English sources (practical constraint).

### Source hierarchy
Systematic reviews and meta-analyses > peer-reviewed empirical studies > established conference papers > official guidance (definitions and requirements only, not effectiveness) > preprints (labelled).

### Verification protocol (per source)
Title, authors, year, venue, DOI/PMID/URL, publication status, objective, method, architecture, dataset, findings, limitations, relevance, Chapter 1 subsection, evidence location, verification level (full text / abstract only / page opened). Automated DOI APIs are blocked in this environment, so metadata is checked against the publisher or PubMed page directly.

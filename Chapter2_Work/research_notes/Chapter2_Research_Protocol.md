# Shared protocol for all Chapter 2 researchers (read fully before searching)

## Project context
Graduation Project 1 (Research Track, CS, Prince Sattam bin Abdulaziz University): **"An Agentic Framework for Clinical Decision Support in Healthcare"**. The proposed system is a centrally orchestrated (deterministic LangGraph state machine) multi-agent LLM framework for physician-facing clinical decision support:
- deterministic pack selection, red-flag rules and required-field checks run before any LLM;
- LLM agents: case understanding (grounded fact extraction, with a deterministic grounding check), hypothesis generation, critic, guideline evidence retrieval (RAG), option appraisal over a closed, pack-defined option set, and a verifier (rule checks plus claim-to-source verification, one bounded revision);
- deterministic decision core: hard constraints, weighted ranking, robustness test across missing-value ranges;
- five statuses: RECOMMEND, RECOMMEND_WITH_CAVEATS, NEED_MORE_INFO, ESCALATE_NOW, ABSTAIN;
- the physician decides; no prescribing or dosing.
The target clinical presentation is NOT yet chosen, so do not search for one specialty only.

## Chapter 1 problem statement (map every study to these)
- **P1** Reliability drops in realistic decision-making (information gathering, order/wording sensitivity, guideline adherence).
- **P2** Missing information and uncertainty are handled poorly (abstention, escalation, asking for information, overconfidence).
- **P3** Citing a source does not mean being supported by it (RAG faithfulness, citation support, claim verification).
- **P4** Evaluation practice cannot yet answer whether multi-agent designs help (weak or unmatched baselines, compute confound, few real-data or safety-focused evaluations).

## Research questions
- **RQ1** Decision quality and status accuracy, including unsafe recommendations and unnecessary abstention or escalation.
- **RQ2** Verifier accuracy and unsupported-claim rate, with and without claim-to-source verification.
- **RQ3** Robustness of status and ranking to reordering and rewording.
- **RQ4** Whether differences survive tool-, retrieval- and compute-matched single-agent baselines; component ablation; latency and cost.

## Chapter 2 structure the evidence will feed (Seminar.pdf, Research Track)
2.1 Review of existing work; 2.2 Comparative analysis (methods, datasets, evaluation metrics, reported results); 2.3 Identified gaps; 2.4 Summary and justification.

## Integrity rules (non-negotiable)
- Never invent papers, authors, DOIs, datasets, metrics or numbers. If you cannot confirm a detail, leave the field empty and say so.
- Verify bibliographic details on the publisher page, DOI record (you may WebFetch https://api.crossref.org/works/<DOI>), ACL Anthology, PMLR, NeurIPS proceedings, OpenReview, PubMed/PMC, Semantic Scholar (https://api.semanticscholar.org/graph/v1/paper/DOI:<doi>?fields=title,venue,year,authors,externalIds,publicationTypes) or OpenAlex (https://api.openalex.org/works/doi:<doi>), whichever is reachable. Record which one you used.
- Label each source's peer-review status: peer-reviewed journal / peer-reviewed conference / preprint (arXiv, medRxiv, etc.) / report or guidance / other. Check whether a preprint has since been published, and record the venue if so.
- Record verification level: FULL (full text read), PARTIAL (substantial parts read), ABSTRACT (abstract or record only), SECONDARY (only via another source; avoid citing numbers from these).
- Numbers must come from the source itself. Note the exact condition each number applies to (model, dataset, split, metric).
- Include contradictory or null findings, especially studies where single agents match or beat multi-agent systems. Do not select papers to flatter the proposed framework.
- Prefer original studies over reviews. When you use a review, trace its key claims to the original studies where possible.
- Seminal and older work is welcome where it is foundational. For LLM work, focus on 2022 to November 2026.
- Databases that need a login (Scopus, Web of Science, IEEE Xplore full text, ACM full text, Saudi Digital Library) are not available. Use WebSearch and WebFetch on publicly reachable pages. Record any inaccessible full text.

## Required output files (in this folder)
1. `<theme>.md`: narrative notes, organised by sub-theme, critical rather than descriptive. End with: (a) what the evidence establishes, (b) contradictions, (c) gaps relevant to P1–P4 and RQ1–RQ4, (d) inaccessible or unverified items.
2. `<theme>_studies.jsonl`: one JSON object per line per included study, with exactly these keys (use "" when unknown; never guess):
   `{"id": "<short key, e.g. hager2024>", "theme": "<theme>", "title": "", "authors": "<first author et al. or full list if verified>", "year": "", "venue": "", "doi": "", "url": "", "peer_review": "journal|conference|preprint|report|guidance|other", "published_version_checked": "<yes/no + venue if a preprint was later published>", "verification": "FULL|PARTIAL|ABSTRACT|SECONDARY", "metadata_verified_via": "", "study_type": "<original empirical | benchmark | system paper | systematic review | survey | guidance>", "objective": "", "architecture_methods": "", "models_tools": "", "dataset_sample": "", "metrics": "", "baselines": "", "main_findings": "<with exact numbers and conditions>", "limitations": "", "relevance": "<P1-P4 / RQ1-RQ4 codes and a one-line reason>", "supports_or_contradicts": "<what it supports, and whether it challenges the multi-agent premise>", "claims_supported": ""}`
3. `<theme>_searchlog.jsonl`: one object per search, with exactly these keys:
   `{"date": "2026-11-27", "tool": "WebSearch|WebFetch|...", "source_targeted": "<e.g. Google Scholar via web search, PubMed, ACL Anthology>", "query": "", "results_screened": "", "included": "", "excluded_with_reason": ""}`

Aim for depth over breadth: about 12–25 well-extracted, relevant studies per theme, with key studies read in full where accessible.

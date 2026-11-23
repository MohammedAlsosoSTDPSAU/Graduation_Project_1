# Stage 1 · Project Audit — Chapter 1 (Research Track)

Date: 2026-11-23 · Status: **D1–D4 resolved on 2026-11-23 (see Section 12); D5 open; Stage 2 started**

---

## 1. Materials inspected

| File | What it actually is | Key finding |
|---|---|---|
| `Seminar.pdf` (CS Dept, 7 pp.) | Official FYP document requirements for the three tracks | Defines a **5-chapter** report with fixed sections per track. Chapter 1 (Research Track) is on p. 3. |
| `02-Research Track Form 3.docx` | **Proposal Approval Form**, byte-identical to the Form 2 file attached earlier | It is **not** a report template. Contains team names/IDs, supervisor, title, abstract, 3 objectives, methodology, technologies, contributions. |
| `Decision workflow · English.pdf` | Exported decision-workflow board (6 stages, 8 design principles) | Matches the current architecture: centralised deterministic orchestrator, shared CaseState, packs, decision core, physician final authority. |
| `472 FYP2 - 5019 - Report 2.pdf` | Sample report of a previous team (supervisor's reference) | LaTeX-produced FYP2 report; useful for writing tone, agent-description style and methodology presentation. Not a Word template, and its chapter plan predates `Seminar.pdf`. |
| `GP1_Proposed_Report_Structure.pdf` | 9-chapter structure we prepared on 2026-11-06 | **Does not follow `Seminar.pdf`** (see C1). |
| Research proposal (Form 2/3 content) | Approved proposal text | Describes a broader, operational, decentralised system than the workflow (see C3–C5). |

## 2. Official requirements (Seminar.pdf, Research Track)

**Chapter 1 · Introduction** (mandatory sections)

| Section | Required content |
|---|---|
| 1.1 Background | Research area and technical background |
| 1.2 Problem Statement | Unresolved research problem or limitations of existing methods |
| 1.3 Motivation | Need for further research |
| 1.4 Objectives | Research objectives or research questions |
| 1.5 Scope | Research scope, assumptions and study boundaries |

The rest of the official structure matters for Chapter 1, because Chapter 1 must promise only what Chapters 2–5 can deliver:

- **Ch 2 Literature Review**: 2.1 Review of existing work · 2.2 Comparative analysis (methods, datasets, evaluation metrics, reported results; Google Scholar / SDL) · 2.3 Identified gaps · 2.4 Summary and justification
- **Ch 3 Research Requirement Analysis**: 3.1 Dataset / testbed requirements · 3.2 Baseline methods for comparison · 3.3 Evaluation metrics · 3.4 Experimental requirements
- **Ch 4 Design Considerations**: 4.1 Selection and justification of algorithms/models · 4.2 Computational resource requirements · 4.3 Model performance and computational efficiency · 4.4 Experimental feasibility and reproducibility · 4.5 Research challenges, risks and limitations
- **Ch 5**: 5.1 Proposed framework · 5.2 Methodology workflow · 5.3 Data exploratory analysis · 5.4 AI model design · 5.5 Algorithms for key components · 5.6 Deployment design

## 3. Project understanding (one paragraph)

The project investigates whether a structured, evidence-grounded multi-agent architecture improves the safety, traceability and decision quality of LLM-based clinical decision support compared with single-LLM approaches. A deterministic orchestrator runs a fixed six-stage flow over a shared CaseState: scope and safety checks before any LLM call; case understanding with grounded extraction; parallel hypothesis generation (with a critic) and score calculation; evidence retrieval; option appraisal and claim verification (one bounded revision); and a deterministic decision core that filters options by hard constraints, ranks them by weighted criteria and tests the ranking's robustness to missing data. Every path ends in one report with one of five statuses (RECOMMEND, RECOMMEND_WITH_CAVEATS, NEED_MORE_INFO, ESCALATE_NOW, ABSTAIN); the physician keeps final authority. Condition-specific knowledge lives in versioned packs, so the engine is condition-agnostic.

## 4. Workflow interpretation — what it commits Chapter 1 to

1. **Task type**: decision support on a *closed set of pack-defined options* (e.g., next-step / disposition options), not free-form diagnosis or prescribing.
2. **Safety precedence**: deterministic rules > calculators and pack rules > retrieved evidence > LLM judgment.
3. **Uncertainty handling is a research object**: NEED_MORE_INFO and ABSTAIN are outputs to be evaluated, not failures.
4. **Human oversight**: the physician accepts, edits or rejects; nothing is executed automatically.
5. **Generality claim**: a new condition = a new pack, no engine change. This is a testable claim (transfer to a second pack), not an assumption.

## 5. Conflicts and weaknesses

Severity: **Major** (affects direction or other chapters) · **Recommended revision** · **Minor**

### C1 · Report structure does not match the official structure — Major
The 9-chapter `GP1_Proposed_Report_Structure.pdf` contradicts `Seminar.pdf` (5 chapters, fixed sections). Official requirements outrank our recommendation.
**Recommendation**: replace it with the official Research Track structure; our extra material maps cleanly (requirements → Ch 3; risks, ethics, resources → Ch 4; design, agents, decision core, data → Ch 5; background theory → 1.1 and Ch 2). If the 9-chapter version was already sent to the supervisor, send the corrected version with a one-line note.

### C2 · Official Word report template is missing — Blocking for Stage 7 only
The folder's `.docx` is the proposal approval form. No report template is present, and the sample report is a LaTeX PDF.
**Recommendation**: obtain the department's Word template (or written confirmation that none exists and which format to follow). Stages 2–6 can proceed; I will not claim template compliance without inspecting it.

### C3 · Proposal scope (operational management) vs workflow scope (clinical decision support) — Major
The workflow implements no triage logistics, resource allocation, scheduling or routing, and the supervisor stated the system must be about decision making rather than management (supervisor decisions outrank the proposal). Keeping operational claims would create promises Chapters 3–5 cannot evaluate.

| Proposal statement | Problem | Proposed revision |
|---|---|---|
| "holistically manage and automate both the clinical decision-making process and the broader operational workflow" | Operational automation is not built | Clinical decision support only |
| "triage, resource allocation, and appointment scheduling" | Not in workflow | Remove; keep "safety screening (red flags)" as part of CDS |
| "eliminating operational bottlenecks", "optimizing healthcare resource utilization" | Not measured | Remove |
| "enhancing patient outcomes", "error-resistant medical ecosystem" | Not measurable in a simulated study | Replace with measurable decision-quality and safety outcomes |
| Objective 1: "decentralized ... autonomously managing the end-to-end healthcare workflow ... operational logistics" | Contradicts centralised orchestrator and physician authority | Design and implement a centrally orchestrated multi-agent CDS framework with deterministic safety and decision logic |
| Objective 2: "reducing diagnostic errors ... in real-time" | Clinical diagnostic error cannot be validly measured on simulated cases; "real-time" not required | Measure unsafe-recommendation rate, must-not-miss recall and unsupported-claim rate on annotated test cases |
| Objective 3: "operational efficiency ... resource allocation" | Not part of the system | Keep latency, cost per case and decision accuracy vs baselines |
| KPI: "resilience in managing complex clinical pathways **without human intervention**" | Contradicts human-in-the-loop design and safety guidance | Replace with abstention/escalation quality under incomplete information |
| "State Machine and Message Brokering protocols ... asynchronous agent communication" | Workflow uses a synchronous state machine with a shared state | "State-machine orchestration over a shared case state" |
| "empirically proving that decentralized AI agents can ... safely orchestrate" | Overclaim; "proving" and "safely" not supportable | "empirically evaluating whether ..." |

**Effect on Chapter 1**: title (C5), 1.1 framing, 1.2 problem, 1.4 objectives and 1.5 scope all narrow to clinical decision support.

### C4 · "Decentralized" vs centralised orchestration — Recommended revision
The proposal says decentralised; the workflow is deliberately centralised (deterministic orchestrator) for auditability and safety. Chapter 1 must describe the actual design and can justify centralisation (traceability, bounded loops, deterministic safety precedence) as a design choice, citing literature on orchestration patterns in Stage 2.

### C5 · Title inconsistency — Minor (typo) / needs supervisor approval (rewording)
- Approved form: "Agentic framework for healthcare **care** decision support system" (duplicated word).
- Harness: "Agentic Framework for Healthcare Clinical Decision Support System".
- Our earlier cover pages: "Agentic Framework for a Healthcare Decision Support System".
**Recommendation**: use one title everywhere. Minimal, low-risk fix: **"An Agentic Framework for Healthcare Clinical Decision Support"** (typo fixed, scope made explicit). Any stronger rewording needs supervisor/department approval.

### C6 · Is "Agentic Framework" accurate? — Recommended revision
Most control flow is deterministic; LLM agents act as bounded, typed steps, with limited autonomy (critic exchange, verifier feedback). This is closer to an orchestrated multi-agent *workflow* than to autonomous agents. The term is defensible if Chapter 1 defines it precisely (LLM agents with specialised roles, tools and structured outputs, coordinated by a deterministic orchestrator) and avoids claims of autonomy.

### C7 · Objectives are not measurable as written — Recommended revision
SMART check of the proposal objectives: O1 is not specific to the built system; O2 and O3 measure outcomes the design cannot validly produce (diagnostic error in practice, operational efficiency). Draft direction for Stage 4 (not final): RQ1 safety and evidence support vs baselines; RQ2 handling of incomplete information (NEED_MORE_INFO / ABSTAIN quality, robustness); RQ3 transfer to a second pack without engine changes; plus latency and cost as efficiency measures. Chain to enforce: problem → RQs → objectives → Ch 3 baselines/metrics → Ch 5 workflow.

### C8 · Fairness of the single-LLM comparison — Recommended revision
Comparing a full pipeline (retrieval, calculators, rules, several calls) with a bare single LLM confounds architecture with extra information and compute. A fair design needs: same base model and settings; a strong single-agent baseline with the **same** retrieval, calculators and safety rules; a compute-matched variant (same number of LLM calls, e.g. self-consistency); and ablations removing one component at a time. Stage 2 will look for evidence on whether multi-agent gains persist against compute-matched single-agent baselines (negative results included).

### C9 · Test data are undefined, yet Ch 3.1 and Ch 5.3 require a dataset/testbed and exploratory analysis — Major risk
The proposal mentions "de-identified, publicly available healthcare datasets" without naming one. Options to assess in Stage 2: published clinical vignettes; exam-style benchmarks (knowledge, not decision with abstention); clinician-reviewed synthetic vignettes with controlled missing-information variants (directly tests RQ2); credentialed EHR data such as MIMIC (access requirements and third-party API restrictions must be checked). Scope in 1.5 depends on this choice.

### C10 · The supported clinical decision is not yet named — Major
"Clinical decision support" must be concretised for 1.5: which decision type (e.g., next-step management / disposition over a pack's closed option set), which conditions (packs) and which users. The earlier working assumption was one deeply evaluated pack plus one transfer pack; this needs confirmation.

## 6. Feasibility and complexity

- Six LLM roles plus a deterministic decision core is ambitious for four students without medical background over two semesters. Critical path: **test data + clinician reviewer**, not code.
- Recommended scoping (to confirm in Stage 4): GP1 = Chapters 1–5 + a walking skeleton (orchestrator, CaseState, one pack, safety rules, one agent, report); GP2 = remaining agents, full evaluation, transfer pack, paper.
- Every component kept must be justified by an ablation; components that cannot be evaluated should be marked future work.

## 7. Capability check

| Capability | Status |
|---|---|
| Official Word template | **Missing** (C2) |
| Web search and reading open-access full text (PMC, arXiv, publisher OA pages) | Available |
| Paywalled full text (e.g., ScienceDirect, IEEE) | **Not available** from here; abstracts only. Needs PDFs from the Saudi Digital Library placed in this folder |
| Automated DOI/PMID validation (Crossref, PubMed E-utilities, arXiv API) | **Blocked** by network allowlist; manual verification via publisher/PubMed pages works but is slower. Optional fix: allow `api.crossref.org`, `eutils.ncbi.nlm.nih.gov`, `export.arxiv.org`, `api.semanticscholar.org` in Claude settings › Capabilities |
| Edit Word documents, export PDF (python-docx, LibreOffice) | Available |
| Research matrix / ledger (.xlsx), bibliography (.bib for Zotero) | Available |
| Zotero connector | Not connected; a `.bib` file will be produced for import |

## 8. Missing information

1. Official Word report template.
2. Which chapters are due for GP1, and the submission date.
3. Whether the 9-chapter structure was sent to the supervisor.
4. Supervisor confirmation of scope narrowing (C3) and title (C5).
5. Test-data decision and clinician-reviewer availability (C9).
6. Target conditions/packs and decision type (C10).

## 9. Initial research risks

- Literature on LLM multi-agent CDS is young and preprint-heavy (2023–2026); peer-reviewed status must be checked per source.
- Evidence that multi-agent setups beat strong single-agent baselines is mixed; the problem statement must not assume superiority.
- Several reference starting points are 2025–2026 reviews; bibliographic details and publication status must be verified before citation.
- Regulatory sources (FDA CDS guidance, WHO, NIST AI RMF) are guidance, not evidence of effectiveness; they will be cited only for definitions and requirements.

## 10. Decisions needed before Stage 2

- **D1** Report structure: adopt the official Seminar.pdf Research Track structure (recommended).
- **D2** Scope: narrow to clinical decision support; drop operational management claims (recommended; aligns with supervisor).
- **D3** Title wording (C5).
- **D4** Template: provide it, or confirm none exists.
- **D5** Test data and clinician reviewer (can be decided with the supervisor during Stage 2).

## 11. Decision log

| # | Date | Decision / assumption | Source | Status |
|---|---|---|---|---|
| 1 | 2026-11-23 | Chapter 1 follows Seminar.pdf Research Track sections 1.1–1.5 | Official requirements | Adopted |
| 2 | 2026-11-23 | No claim of template compliance until the template is inspected | Harness §3 | Adopted |
| 3 | 2026-11-23 | Physician keeps final authority; no prescribing or dose adjustment; safety checks precede LLM reasoning | Workflow, harness §10 | Adopted |
| 4 | 2026-11-23 | Scope narrowed to clinical decision support; operational management claims dropped | Team approval, supervisor remark, workflow | Adopted |
| 5 | 2026-11-23 | Title: "Agentic Framework for Healthcare Clinical Decision Support" | Team approval | Adopted |
| 6 | 2026-11-23 | Report structure = Seminar.pdf Research Track (5 chapters); the supervisor reviewed the 9-chapter draft and adopted this structure | Supervisor decision | Adopted |
| 7 | 2026-11-23 | Template #F15-CS-1.docx governs cover and front matter (declaration, approval, acknowledgements, abstracts EN/AR, TOC, lists, keywords); body: 12 pt, 1.5 line spacing; its sample TOC lists older generic chapter names, so Seminar.pdf governs chapter and section titles | Template + supervisor decision | Adopted |


## 12. Resolution of Stage 1 decisions (2026-11-23)

- **D1** The 9-chapter draft was shown to the supervisor, who amended it and adopted the 5-chapter structure in `Seminar.pdf`. C1 closed.
- **D2** Scope narrowed to clinical decision support. C3 closed; proposal statements in the C3 table will be rewritten in Chapter 1.
- **D3** Title: **Agentic Framework for Healthcare Clinical Decision Support**. C5 closed.
- **D4** Official template provided: `#F15-CS-1.docx` (Letter size; margins 90 pt left/right, 27 pt top, 72 pt bottom; Times New Roman / Book Antiqua; instructions: font size 12, 1.5 line spacing). It covers the cover and front matter only; it defines no Heading 1–3 styles, so chapter headings will be added as proper Word heading styles so the TOC can be generated. Its sample TOC (Purpose of the project, System Analysis, ...) belongs to an older generic structure and is superseded by `Seminar.pdf` for chapter content. C2 closed.
- **D5** Test data and clinician reviewer: open; examined in Stage 2.

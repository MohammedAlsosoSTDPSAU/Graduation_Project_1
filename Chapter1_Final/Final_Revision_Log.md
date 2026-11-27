# Chapter 1 — Final Revision Log

**Date:** 27 November 2026
**Scope:** targeted pre-submission revision of `Chapter1_Final.docx`. The chapter was not rewritten. The previous version is preserved in the repository history.

## 1. FDA regulatory accuracy

The 29 January 2026 FDA guidance was re-read in full.

| Before | After | Source (page) |
|---|---|---|
| "Under the FDA guidance, software intended for critical, time-sensitive tasks or decisions does not meet the criterion that separates decision support from a device." | States the scope (HCP-facing, United States, non-binding). Quotes FDA's position that functions for a *critical, time-sensitive task or decision* do not meet **Criterion 4**, and adds that time-criticality and level of automation are *factors* in that assessment. Criterion 4 is now described as one of four criteria, not as "the" dividing criterion. | pp. 4–6, 14–15 |
| ESCALATE_NOW "does not itself guide a time-critical intervention" (relevance not explained) | Explains that FDA's device examples include software that identifies patient deterioration and alerts an HCP. This is the function closest to ESCALATE_NOW. States that the prototype's escalation is rule-based, carries no treatment directive and is evaluated offline, and that its regulatory treatment if deployed would depend on intended use and an assessment this project does not perform. | p. 24 |
| 1.1.5: FDA "distinguishes decision support from a regulated device in part by…" | "Sets out four criteria…; the fourth requires that an HCP be able to independently review the basis…". Automation-bias definition cited to its page. | pp. 6, 15 |
| SFDA: "makes regulatory treatment depend on the software's intended use" | Restated in the guidance's own terms: intended use determines whether AI/ML technologies are regulated as medical devices. | SFDA MDS-G010 v1.0 |
| "It does not claim regulatory compliance" (repeated in 1.3) | Removed from 1.3. The disclaimer remains once in 1.5.7 and once in Table 1.4. | — |

**Result:** the chapter makes no regulatory classification or compliance claim about the prototype.

## 2. Fair baseline comparison

| Location | Change |
|---|---|
| SO4 | The "call-matched" baseline is now a variant whose computational budget is "brought as close as practicable" to the framework's. Single agents get the same *retrieval corpus*, rules and calculators. |
| RQ4 | Fairness is now defined by the same model, decoding settings, tools and retrieval access, plus a comparable computational budget. Token usage is added to the reported cost measures. |
| New paragraph after the RQs | Explains why equal call counts are not enough, citing sampling-and-voting gains [33]. Fixes what is held constant: model version, decoding settings, tools, retrieval access and input format. Treats budget as a *reported* variable (calls, input and output tokens, latency, cost per case) and says mismatches will be reported, not hidden. Defers the exact configurations to Chapter 3, so no unapproved setting is presented as decided. |
| Table 1.3 (SO4), 1.5.3 | Aligned with the paragraph above. |

## 3. Scope consistency with the approved workflow

Checked against `Decision workflow · English.pdf`.

- **Removed an invented element.** The "order of precedence (rules → calculators → evidence → LLM)" does not appear in the workflow. It was replaced by what the workflow actually shows:
  - red-flag rules run before any LLM and before the completeness check;
  - the LLM agents contribute content;
  - deterministic steps set the final status.
- **Added** the six-stage flow, the agent roles and the decision core. The core applies hard constraints, then weighted ranking, then a robustness test across missing-value ranges.
- **Added** the verifier loop: issues go back to the appraisal agent at most once, and a second failure ends in ABSTAIN.
- **Added** the exact mapping from the **seven outcomes to the five statuses**:
  - ABSTAIN ×2: no pack covers the complaint, or verification failed twice
  - ESCALATE_NOW: red flag
  - NEED_MORE_INFO ×2: missing required fields, or an unstable top option
  - RECOMMEND_WITH_CAVEATS: a serious risk remains open
  - RECOMMEND
- **Corrected** two status definitions:
  - RECOMMEND_WITH_CAVEATS: previously "flags uncertainties"; now "states an open serious risk".
  - ABSTAIN: previously "evidence insufficient"; now "no pack, or verification failed twice".
- **Corrected** Table 1.2: "rules and agreement signals" (not in the workflow) became the actual decision gates.
- **Physician oversight:** "accepts, edits or rejects *with a reason*; the choice is logged", as the workflow shows.
- **SO1** now lists all the deterministic components: pack selection, safety and completeness checks, calculators, decision core and report composer.
- **Unchanged:** the target clinical pathway is still described as not selected, and no specialty is named. Generality across conditions is still a design intention, not a claim.

## 4. Citation audit (all 40 references)

| Change | References |
|---|---|
| Bibliographic details completed after verification | [4] Singhal 2025: *Nat. Med.* 31(3):943–950<br>[8] Amugongo: DOI confirmed<br>[7] Lewis: pages confirmed<br>[19], [21] WHO: ISBNs (LMM page dated 25 Mar 2025)<br>[26] Wu: Art. no. 3615<br>[32] Smit: pp. 45883–45905<br>[39] SFDA: number, version and date from the PDF cover |
| Author order corrected | [32] Smit et al.: Grinsztajn second, Duckworth third (PMLR) |
| Publication status updated from preprint to peer-reviewed | [36] Cemri et al.: NeurIPS 2025<br>[37] Kapoor et al.: TMLR 2025<br>In-text "preprint" labels removed for both |
| Status confirmed as preprint, label kept | [29] HealthBench<br>[30] Gorenshtein et al. (no journal version found) |
| Claim corrected to match its source | [8] the review reports a lack of *ethics* assessment, not "ethics or safety" |
| Page locators added | FDA [20], at the five places it is cited |

Every numerical claim was re-checked against the source records gathered during verification. No number changed except where noted above. The claim-to-source audit is in `Final_Citation_Audit.xlsx`, which has three sheets: the reference audit, the claim–source ledger (57 rows) and notes.

## 5. Editorial

- Removed the repeated compliance disclaimer from 1.3.
- Adjusted wording where the regulatory and fairness passages changed. All other text is unchanged.
- Length grew from about 5,200 to about 5,800 words, mainly in 1.1.4 (the workflow description) and 1.4.3 (the fairness paragraph). The chapter is now 21 pages including the reference list.
- Long tables now continue across pages with a repeated header row instead of leaving half-page gaps.

## 6. Fixes from the final independent check

A separate reviewer re-read the FDA PDF and the approved workflow and checked the revised chapter. It found no critical issues, two major issues and eight minor ones. All were fixed except F9, which is optional, and F10, the remaining reference gaps.

| # | Issue | Fix |
|---|---|---|
| F1 (Major) | The chapter said every status is set deterministically, but the verifier is an LLM agent | Now says that routing is deterministic and five of the seven outcomes are rule-decided, while the verification outcome depends on the verifier's judgement, which RQ2 evaluates. Table 1.2 is aligned with this. |
| F2 (Major) | "Same tool and retrieval access for all systems" contradicted the no-tools baseline | Tool and retrieval access is now the same "for all tool-using systems". |
| F3 | Identical decoding settings would rule out a sampling-based, budget-matched baseline | Adds a sampling exception, which must be stated and reported. A4 and 1.5.3 are aligned. |
| F4 | Grounding was attributed to the LLM agent | A deterministic grounding check now turns any unquoted fact into a gap, and it is added to SO1. |
| F5 | The report's contents differed from the workflow | The report now carries the status, ranked options, reasons and sources. Steps go to a separate append-only audit log. |
| F6 | The FDA deterioration example was incomplete | Now states that the example analyses vital-sign patterns, which the prototype does not, and that only its time-critical element relates to ESCALATE_NOW. |
| F7 | "Sets out four criteria" | Now "explains the four statutory criteria". |
| F8 | 1.5.1 and Table 1.2 omitted the verification-failure ABSTAIN and the role of pack selection | Both added. |

## 7. Remaining unresolved items

These items do not block submission, but they must be disclosed and resolved later.

1. **Supervisor decisions still open.** Each is stated openly in the chapter:
   - the test-set strategy
   - the target clinical presentation
   - the GP1/GP2 split
   - a clinician reviewer
   - claim-support labelling by two team members
2. **Incomplete reference details.** These were not found on accessible publisher pages and were not guessed:
   - [16] MediQ: volume and pages
   - [34] MedAgentBoard: track and pages
   - [35] Hakim: article number
   - [36] Cemri: proceedings pages
   - [22] SDAIA: official URL
3. **Optional (F9).** The workflow description in 1.1.4 could be shortened and moved to 1.4 or Chapter 5.
4. **Cover page.** Student names and IDs are omitted at the team's request. They must be added when the chapter is merged into the full report.
5. **Formatting choices to confirm with the supervisor.** These are not specified by the template:
   - a 1-inch top margin on body pages
   - reference text at 10.5 pt and table text at about 9.5 pt
   - "Table 1.x:" caption style

## 8. Verdict

**Ready to submit to the supervisor as the GP1 Chapter 1 draft. Not yet final for the complete report.** No critical or major issue remains. The open decisions in item 1 above are project decisions, not defects in the chapter, and the text discloses each of them. Before the full report is bound, the reference gaps in item 2 and the cover details in item 4 must be completed.

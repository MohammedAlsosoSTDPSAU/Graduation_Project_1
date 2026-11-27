# Chapter 1 — Open Decisions

This file lists only decisions that need the supervisor or the team. Editing or source checks cannot resolve them. Chapter 1 states openly that each one is still open, so none of them makes the chapter inaccurate. Each one does constrain the chapters that follow.

| # | Decision | Who decides | Why it is still open | Where Chapter 1 says so | Impact on later chapters |
|---|---|---|---|---|---|
| D1 | **Target clinical presentation** (the first pathway pack) | Supervisor, with the team | The approved workflow is condition-agnostic by design, and no presentation has been chosen. The project audit records this as C10. | 1.5.2: four selection criteria are given; the decision type excludes choosing or dosing medication | **Blocks Chapter 3.1** (dataset/testbed), the pack contents, the choice of guidelines and Chapter 5.3 (data exploration). It should be the first decision. |
| D2 | **Test-set strategy** | Supervisor | No public dataset is labelled for the five statuses. The team-built, guideline-anchored vignettes are only a proposal; the audit records this as D5. | 1.5.4: "proposed… subject to supervisor approval" | Defines Chapter 3.1, 3.3 and 5.3. A change, for example to credentialed EHR data, would require PhysioNet credentialing and locally run models. |
| D3 | **Clinician reviewer** | Team (supervisor may help identify one) | No clinician has been approached. The team has no clinical background. | 1.5.4 and 1.5.7 (limitation) | Affects how valid the test labels are (assumption A1) and the limitations in Chapters 3 and 6. Without a reviewer, labels rest on published guidelines alone. |
| D4 | **Claim-support labelling procedure** (two team members, agreement reported) | Team, with supervisor approval | Introduced during this revision as a proposal; not yet in the decision log | 1.5.4 ("It is further proposed…") and 1.5.7 (limitation) | RQ2 (verifier accuracy) depends on these labels. Chapter 3.3 must define the labelling protocol and the agreement statistic. |
| D5 | **GP1/GP2 division of work** | Supervisor | The plan of design plus an initial core implementation in GP1, with full implementation and evaluation in GP2, has not been confirmed. | 1.5.3 and Table 1.4 | Sets how much of Chapter 5.4–5.6 must be implemented, rather than designed, by the end of GP1. |
| D6 | **Base model(s) and hosting** (local or API) | Team, with supervisor | It depends on cost, data-handling conditions and the hardware available. The team expects API costs. | 1.5.3 (deferred to Chapter 4) | Chapter 4.1–4.3, the fairness controls in Chapter 3.2, and the data-use conditions if any credentialed data is used. |
| D7 | **Student names and IDs on the cover** | Team | Left off at the team's request for this draft | Cover page | They must be added before the chapter goes into the full report. They do not affect the content. |

## Items not listed here

These do not need a decision. They are handled through editing or source checks, and are tracked in `Chapter1_Final_Citation_Audit.xlsx` and `Chapter1_Final_Acceptance_Report.md`:

- incomplete reference details
- formatting choices that follow the template
- newer Saudi AI policy documents to cover in Chapter 2

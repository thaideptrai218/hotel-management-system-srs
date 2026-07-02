# Figure 1: DFD-Style System Context Diagram

- Document target: SRS
- Source evidence: `C:\Users\pgb31\Downloads\hotel_management_system_srs_v1_2_staff_screen_mockup_ready.usecase-layout-compact-table-caption-below.docx`, DGM-CTX-001, section 2.1 Actors
- Scope: Hotel Marketplace Management System MVP+Staff v1.2 context
- Assumptions: Notification Service may be mocked; System Scheduler is a time-based actor
- Open questions: None for diagram boundary
- Traceability: ACT-001 to ACT-011; DGM-CTX-001

## Explanation

The system is shown as one DFD-style process circle. All human actors, external systems, and the timer are outside the system and shown as external entity rectangles. Data/control flows are labeled using the v1.2 context diagram delegation block. Exchanges that have different data in opposite directions are modeled as separate one-way flows, such as Search Criteria, Approved Hotel Results, Payment Request, Payment Result, Notification Request, and Notification Status.

## Validation checklist

- [x] Opens in diagrams.net/draw.io.
- [x] Diagram elements match the source evidence.
- [x] No SRS/SDD boundary violation.
- [x] Naming and IDs are consistent with the document.
- [x] The system is shown as one DFD-style circular/ellipse process.
- [x] All external entities are outside the boundary.
- [x] Human actors are drawn as external entity rectangles, not UML actor stick figures.
- [x] Every arrow has a data/control flow label.
- [x] Request/result and request/status exchanges use separate one-way data flows.

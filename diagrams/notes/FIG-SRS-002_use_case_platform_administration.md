# Figure 2: Use Case Diagram - Platform Administration

- Document target: SRS
- Source evidence: `C:\Users\pgb31\Downloads\hotel_management_system_srs_v1_2_staff_screen_mockup_ready.usecase-layout-compact-table-caption-below.docx`, sections 2.2.1 DGM-UC-006, 2.2.2 Use Case List, 3.2.11, 3.2.12, 3.2.13
- Scope: Platform approval, commission, payment reconciliation, refund, settlement, and dashboard use cases
- Assumptions: Notification Service may be mocked
- Open questions: None
- Traceability: ACT-002, ACT-003, ACT-008, ACT-009, ACT-010; UC-018, UC-019, UC-020, UC-021, UC-022, UC-023

## Explanation

This diagram shows platform administration use cases. Platform Administrator performs hotel approval, commission management, payment reconciliation, refund status processing, settlement marking, and dashboard viewing. payOS participates in payment reconciliation. Customer participates in refund status processing. Property Owner participates in approval outcome and settlement-related interaction. Notification Service participates in approval, refund, and settlement notifications.

## Validation checklist

- [x] Every use case is inside the system boundary.
- [x] Every actor is outside the system boundary.
- [x] Every use case has at least one actor association or justified include/extend relationship.
- [x] Use case names are goal-level and verb-object.
- [x] No screen, button, controller, service, repository, API, table, or code method appears.
- [x] Include/extend arrows point in the correct direction.
- [x] Diagram is split because the full SRS has more than 12 use cases.

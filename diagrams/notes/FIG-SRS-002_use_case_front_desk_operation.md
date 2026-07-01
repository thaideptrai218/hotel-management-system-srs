# Figure 2: Use Case Diagram - Front Desk Operation

- Document target: SRS
- Source evidence: `C:\Users\pgb31\Downloads\hotel_management_system_srs_v1_2_staff_screen_mockup_ready.usecase-layout-compact-table-caption-below.docx`, sections 2.2.1 DGM-UC-004, 2.2.2 Use Case List, 3.2.8 FEAT-FRONTDESK
- Scope: Front desk booking, room assignment, check-in, check-out, no-show, and pay-at-property recording
- Assumptions: Notification Service may be mocked
- Open questions: None
- Traceability: ACT-002, ACT-003, ACT-004, ACT-005, ACT-010; UC-014, UC-015, UC-016, UC-017, UC-028, UC-029, UC-030, UC-031

## Explanation

This diagram shows front desk operation use cases. Receptionist is the primary operational actor. Hotel Manager and Property Owner are supervisory actors for selected hotel booking views and operational oversight. Customer participates in check-in, check-out, and walk-in booking. The only formal include relationship shown is `Check In Customer` including `Assign Physical Room`, because the v1.2 document explicitly preserves this relationship as mandatory.

## Validation checklist

- [x] Every use case is inside the system boundary.
- [x] Every actor is outside the system boundary.
- [x] Every use case has at least one actor association or justified include/extend relationship.
- [x] Use case names are goal-level and verb-object.
- [x] No screen, button, controller, service, repository, API, table, or code method appears.
- [x] Include/extend arrows point in the correct direction.
- [x] Diagram is split because the full SRS has more than 12 use cases.

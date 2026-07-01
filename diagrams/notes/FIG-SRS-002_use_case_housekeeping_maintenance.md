# Figure 2: Use Case Diagram - Housekeeping and Maintenance

- Document target: SRS
- Source evidence: `C:\Users\pgb31\Downloads\hotel_management_system_srs_v1_2_staff_screen_mockup_ready.usecase-layout-compact-table-caption-below.docx`, sections 2.2.1 DGM-UC-005, 2.2.2 Use Case List, 3.2.9, 3.2.10
- Scope: Housekeeping tasks, room issue reporting, and maintenance request handling
- Assumptions: Notification Service may be mocked
- Open questions: None
- Traceability: ACT-004, ACT-005, ACT-006, ACT-007, ACT-010; UC-032, UC-033, UC-034, UC-035, UC-036, UC-037

## Explanation

This diagram shows housekeeping and maintenance use cases. Housekeeping Staff views cleaning tasks, updates room cleaning status, and reports room issues. Maintenance Staff views, updates, and resolves maintenance requests by releasing rooms from maintenance. Hotel Manager supervises both housekeeping and maintenance use cases. Receptionist can report room issues. Notification Service participates in issue, maintenance update, and release events. No extend relationship is drawn for Report Room Issue because the source document says not to model it as extend without a formal extension point.

## Validation checklist

- [x] Every use case is inside the system boundary.
- [x] Every actor is outside the system boundary.
- [x] Every use case has at least one actor association or justified include/extend relationship.
- [x] Use case names are goal-level and verb-object.
- [x] No screen, button, controller, service, repository, API, table, or code method appears.
- [x] Include/extend arrows point in the correct direction.
- [x] Diagram is split because the full SRS has more than 12 use cases.

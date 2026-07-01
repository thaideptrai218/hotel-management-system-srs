# Figure 2: Use Case Diagram - Account and Marketplace

- Document target: SRS
- Source evidence: `C:\Users\pgb31\Downloads\hotel_management_system_srs_v1_2_staff_screen_mockup_ready.usecase-layout-compact-table-caption-below.docx`, sections 2.1 Actors, 2.2.1 DGM-UC-001, 2.2.2 Use Case List, 3.2.1 FEAT-AUTH, 3.2.2 FEAT-MKT
- Scope: Account and marketplace discovery use cases
- Assumptions: None
- Open questions: None
- Traceability: ACT-001 to ACT-008; UC-001, UC-002, UC-003, UC-004, UC-025; FEAT-AUTH, FEAT-MKT

## Explanation

This diagram shows account and marketplace use cases. Guest can register, search hotels, and view hotel details. Customer can search/view hotels and use authenticated account functions. All authenticated user roles can log in and manage their own profile. Login is modeled as a standalone use case, not as an include relationship for every protected function.

## Validation checklist

- [x] Every use case is inside the system boundary.
- [x] Every actor is outside the system boundary.
- [x] Every use case has at least one actor association or justified include/extend relationship.
- [x] Use case names are goal-level and verb-object.
- [x] No screen, button, controller, service, repository, API, table, or code method appears.
- [x] Include/extend arrows point in the correct direction.
- [x] Diagram is split because the full SRS has more than 12 use cases.

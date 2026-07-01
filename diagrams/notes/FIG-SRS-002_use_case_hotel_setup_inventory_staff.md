# Figure 2: Use Case Diagram - Hotel Setup, Room Inventory, and Staff Management

- Document target: SRS
- Source evidence: `C:\Users\pgb31\Downloads\hotel_management_system_srs_v1_2_staff_screen_mockup_ready.usecase-layout-compact-table-caption-below.docx`, sections 2.2.1 DGM-UC-003, 2.2.2 Use Case List, 3.2.5, 3.2.6, 3.2.7
- Scope: Hotel profile setup, room inventory, availability, and staff role management
- Assumptions: Notification Service may be mocked
- Open questions: None
- Traceability: ACT-003, ACT-004, ACT-008, ACT-010; UC-009, UC-010, UC-011, UC-012, UC-013, UC-018, UC-026, UC-027

## Explanation

This diagram separates hotel setup and staff management from front desk operation. Property Owner can register hotel property, manage hotel profile, room inventory, availability, and staff accounts/permissions. Hotel Manager has hotel-scoped delegated permissions for profile, room, availability, and staff operations. Platform Administrator performs hotel approval. Notification Service participates in hotel registration and approval outcomes.

## Validation checklist

- [x] Every use case is inside the system boundary.
- [x] Every actor is outside the system boundary.
- [x] Every use case has at least one actor association or justified include/extend relationship.
- [x] Use case names are goal-level and verb-object.
- [x] No screen, button, controller, service, repository, API, table, or code method appears.
- [x] Include/extend arrows point in the correct direction.
- [x] Diagram is split because the full SRS has more than 12 use cases.

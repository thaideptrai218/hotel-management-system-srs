# Figure FIG-SRS-006: Activity Diagram - UC-016 Check Out Customer

- Document target: SRS
- Source evidence: `hotel_management_system_srs_v1_2_staff_screen_mockup_ready.md`, Section 2.2.1 activity diagram list, UC-016 Check Out Customer, UC-030 payment collection, NSF-008 housekeeping task creation, FEAT-FRONTDESK, SCR-021, SCR-026, checkout and housekeeping business rules
- Scope: Front desk actor checks out a checked-in stay, handles pay-at-property collection, finalizes receipt/folio, releases room to housekeeping, records notification, and handles checkout exceptions
- Assumptions: ASSUMP-014; Notification Service may be mocked
- Open questions: None
- Traceability: UC-016, UC-030, UC-032, NSF-008, ACT-005, ACT-006, ACT-010, SCR-021, SCR-026, BR-STAY-002, BR-STAY-003, BR-STAY-006, BR-HK-001, BR-FIN-006, BR-AUDIT-001

## Explanation

The diagram covers the main checkout flow plus important exception paths: booking not checked in or actor denied, pay-at-property payment missing, and room-release/task-creation failure. A successful path finalizes the receipt/folio, changes the room to Dirty, creates/receives the housekeeping task, records the checkout notification, and reaches the final state.

## Validation checklist

- [x] Opens in diagrams.net/draw.io.
- [x] Diagram elements match the source evidence.
- [x] No SRS boundary violation.
- [x] Decision branches have guard labels.
- [x] Alternative paths merge, loop for correction, or terminate correctly.
- [x] No unsupported parallel flow or dead-end activity remains.
- [x] Main and important alternative/exception flows are represented.
- [x] Final state matches the use case postcondition.
- [x] `.drawio` XML is well-formed, has no duplicate IDs, and every edge has a valid source and target.
- [x] Naming and IDs are consistent with the SRS.

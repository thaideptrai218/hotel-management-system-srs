# Figure FIG-SRS-006: Activity Diagram - UC-015 Check In Customer

- Document target: SRS
- Source evidence: `hotel_management_system_srs_v1_2_staff_screen_mockup_ready.md`, Section 2.2.1 activity diagram list, UC-015 Check In Customer, UC-029 Assign Physical Room, FEAT-FRONTDESK, SCR-021, SCR-024, SCR-025, stay and room business rules
- Scope: Front desk actor checks in a confirmed booking, validates hotel scope, records identity information, assigns physical room, updates stay/room status, and records notification
- Assumptions: ASSUMP-014; Notification Service may be mocked
- Open questions: OQ-003
- Traceability: UC-015, UC-029, ACT-005, ACT-010, SCR-021, SCR-024, SCR-025, BR-STAY-001, BR-STAY-005, BR-ROOM-001, BR-STAFF-002, BR-STAFF-003, BR-AUDIT-001

## Explanation

The diagram covers the main check-in flow plus important exception paths: hotel-scope access denied, booking not confirmed, missing or invalid identity information, and invalid/no room assignment. A successful path records check-in, sets the assigned room to Occupied, records the check-in notification, and reaches the final state.

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

# Figure FIG-SRS-006: Activity Diagram - Automated Notification

- Document target: SRS
- Source evidence: `hotel_management_system_srs_v1_2_staff_screen_mockup_ready.md`, Section 2.2.1 activity diagram list, FEAT-AUTO-NOTI, UC-024, NSF-002, NSF-003, NSF-008, NSF-009, INT-NOTI-001, notification business rules
- Scope: Business-event or scheduled notification trigger, recipient validation, notification record creation, mocked or external delivery, status update, audit, and observable result
- Assumptions: ASSUMP-014; external notification delivery may be mocked while notification records remain required
- Open questions: None
- Traceability: UC-024, NSF-002, NSF-003, NSF-008, NSF-009, ACT-010, ACT-011, ENT-023, BR-NOTI-001, INT-NOTI-001

## Explanation

The diagram models the black-box SRS flow for automated notification handling. It distinguishes business-event triggers from scheduled triggers, validates recipient and permission, records or suppresses the notification, sends or mocks delivery through the configured channel, updates status, records audit evidence, and ends in an observable delivered/failed/suppressed result.

## Validation checklist

- [x] Opens in diagrams.net/draw.io.
- [x] Diagram elements match the source evidence.
- [x] No SRS boundary violation.
- [x] Decision branches have guard labels.
- [x] Alternative paths merge or terminate correctly.
- [x] No unsupported parallel flow or dead-end activity remains.
- [x] Time/scheduled trigger is connected through a decision path.
- [x] Main and important alternative/exception flows are represented.
- [x] Final state matches the flow outcome.
- [x] `.drawio` XML is well-formed, has no duplicate IDs, and every edge has a valid source and target.
- [x] Naming and IDs are consistent with the SRS.

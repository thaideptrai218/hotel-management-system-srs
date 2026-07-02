# Figure FIG-SRS-006: Activity Diagram - UC-007 Cancel Booking

- Document target: SRS
- Source evidence: `hotel_management_system_srs_v1_2_staff_screen_mockup_ready.md`, Section 2.2.1 activity diagram list, UC-007 Cancel Booking, FEAT-CUST-MYBOOK, SCR-010, SCR-013, refund business rules
- Scope: Customer requests cancellation, System validates eligibility, releases availability, records refund status when required, records notification, and displays the result
- Assumptions: Refund execution is manual/admin-recorded in MVP+Staff scope
- Open questions: None
- Traceability: UC-007, UC-021, ACT-002, ACT-008, ACT-010, SCR-010, SCR-013, BR-BOOK-008, BR-REF-001, BR-REF-002, BR-REF-003, BR-FIN-002

## Explanation

The diagram shows cancellation request validation, rejection when cancellation is not allowed, successful cancellation and availability release, refund-required versus no-refund branching, cancellation notification recording, and final status feedback. Gateway refund automation is intentionally excluded because refund processing is manual/admin-recorded in the source SRS.

## Validation checklist

- [x] Opens in diagrams.net/draw.io.
- [x] Diagram elements match the source evidence.
- [x] No SRS boundary violation.
- [x] Decision branches have guard labels.
- [x] Alternative paths merge or terminate correctly.
- [x] No unsupported parallel flow or dead-end activity remains.
- [x] Main and important alternative/exception flows are represented.
- [x] Final state matches the use case postcondition.
- [x] `.drawio` XML is well-formed, has no duplicate IDs, and every edge has a valid source and target.
- [x] Naming and IDs are consistent with the SRS.

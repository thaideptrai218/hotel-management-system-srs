# Figure FIG-SRS-006: Activity Diagram - UC-005 Create Booking

- Document target: SRS
- Source evidence: `hotel_management_system_srs_v1_2_staff_screen_mockup_ready.md`, Section 2.2.1 activity diagram list, UC-005 Create Booking, FEAT-CUST-BOOK, SCR-007, SCR-008, booking business rules
- Scope: Customer creates a booking, validates availability, chooses Platform Collect or Pay at Property, receives confirmation/payment instruction, and records notification
- Assumptions: ASSUMP-014; Notification Service may record rather than externally send events
- Open questions: None
- Traceability: UC-005, UC-006, UC-024, ACT-002, ACT-010, SCR-007, SCR-008, BR-BOOK-001, BR-BOOK-003, BR-BOOK-004, BR-BOOK-005, BR-BOOK-006, BR-BOOK-011, BR-BOOK-012, BR-FIN-001, BR-FIN-003

## Explanation

The diagram shows Customer input, System validation, availability checking, room-price-only amount calculation, booking creation, payment-mode branching, booking notification recording, and final visible confirmation or payment instruction. Validation and unavailable-room paths return to the Customer for correction instead of creating a booking.

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

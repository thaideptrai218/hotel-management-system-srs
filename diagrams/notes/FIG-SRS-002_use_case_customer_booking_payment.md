# Figure 2: Use Case Diagram - Customer Booking and Payment

- Document target: SRS
- Source evidence: `C:\Users\pgb31\Downloads\hotel_management_system_srs_v1_2_staff_screen_mockup_ready.usecase-layout-compact-table-caption-below.docx`, sections 2.2.1 DGM-UC-002, 2.2.2 Use Case List, 3.2.3 FEAT-CUST-BOOK, 3.2.4 FEAT-CUST-MYBOOK
- Scope: Customer booking, payment, booking management, and unpaid booking expiration
- Assumptions: Notification Service may be mocked; System Scheduler is a time-based actor
- Open questions: None; v1.2 source basis confirms payment timeout is 15 minutes
- Traceability: ACT-002, ACT-009, ACT-010, ACT-011; UC-005, UC-006, UC-007, UC-008, UC-024; FEAT-CUST-BOOK, FEAT-CUST-MYBOOK

## Explanation

This diagram shows customer-facing booking and payment use cases. Customer can create a booking, pay online, view bookings, and cancel a booking. payOS participates only in online payment. Notification Service participates in booking, cancellation, and expiration events. System Scheduler initiates unpaid booking expiration. The document explicitly says Create Booking may continue to Pay Online only when Platform Collect is selected, so no mandatory include relationship is drawn.

## Validation checklist

- [x] Every use case is inside the system boundary.
- [x] Every actor is outside the system boundary.
- [x] Every use case has at least one actor association or justified include/extend relationship.
- [x] Use case names are goal-level and verb-object.
- [x] No screen, button, controller, service, repository, API, table, or code method appears.
- [x] Include/extend arrows point in the correct direction.
- [x] Diagram is split because the full SRS has more than 12 use cases.

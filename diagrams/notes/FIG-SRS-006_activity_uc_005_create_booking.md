# Figure 6: Activity Diagram - UC-005 Create Booking

- Document target: SRS
- Source evidence: v1.2 DOCX DGM-ACT-BOOK-001, UC-005
- Scope: Customer creates a booking and chooses Platform Collect or Pay at Property
- Assumptions: Notification Service may record rather than send events
- Open questions: None
- Traceability: UC-005, UC-006, UC-024, ACT-002, ACT-010

## Explanation

The activity diagram shows Customer input, System validation and availability checking, room-price-only calculation, booking creation, availability reservation, notification recording, and the final branch to payment instruction or Pay-at-Property confirmation.

## Validation checklist

- [x] Activity diagram maps to one use case or one coherent workflow.
- [x] Actor/system boundary is preserved.
- [x] Decision branches have clear guard conditions.
- [x] Main and important alternative flows are represented.
- [x] No code-level/internal design elements appear.
- [x] The final state matches the use case post-condition.

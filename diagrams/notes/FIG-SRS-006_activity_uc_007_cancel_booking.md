# Figure 6: Activity Diagram - UC-007 Cancel Booking

- Document target: SRS
- Source evidence: v1.2 DOCX DGM-ACT-CANCEL-001, UC-007
- Scope: Customer cancellation and refund-status preparation
- Assumptions: Refund execution is manual/admin-recorded in MVP
- Open questions: None
- Traceability: UC-007, UC-021, ACT-002, ACT-008, ACT-010

## Explanation

The diagram shows cancellation request validation, rejection or cancellation, availability release, refund-record creation when required, and cancellation notification recording. It avoids gateway refund automation as required by the source diagram block.

## Validation checklist

- [x] Activity diagram maps to one use case or one coherent workflow.
- [x] Actor/system boundary is preserved.
- [x] Decision branches have clear guard conditions.
- [x] Main and important alternative flows are represented.
- [x] No code-level/internal design elements appear.
- [x] The final state matches the use case post-condition.

# Figure 6: Activity Diagram - UC-016 Check Out Customer

- Document target: SRS
- Source evidence: v1.2 DOCX DGM-ACT-CHECKOUT-001, UC-016, UC-030
- Scope: Receptionist checkout, pay-at-property collection, receipt, room dirty status, and housekeeping task
- Assumptions: Notification Service may be mocked
- Open questions: None
- Traceability: UC-016, UC-030, UC-032, ACT-005, ACT-006, ACT-010

## Explanation

The activity diagram shows checkout from a checked-in stay, optional pay-at-property collection, folio/receipt finalization, room status update to Dirty, housekeeping task creation, and notification event recording. It intentionally excludes platform settlement logic and full customer invoice/folio display.

## Validation checklist

- [x] Activity diagram maps to one use case or one coherent workflow.
- [x] Actor/system boundary is preserved.
- [x] Decision branches have clear guard conditions.
- [x] Main and important alternative flows are represented.
- [x] No code-level/internal design elements appear.
- [x] The final state matches the use case post-condition.

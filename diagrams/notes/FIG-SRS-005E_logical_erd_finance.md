# Figure 5E: Logical ERD - Finance

- Document target: SRS
- Source evidence: SRS v1.2 DGM-ERD-001; payment, refund, commission, settlement requirements
- Scope: Booking finance records, payment modes, refund, invoice, commission, settlement
- Assumptions: SettlementItem can reference a commission item and related booking context
- Open questions: None
- Traceability: UC-006, UC-008, UC-016, UC-017, UC-025, UC-026, UC-027, UC-028, UC-029

## Explanation

This slice isolates finance so Platform Collect, Pay at Property, refund handling, commission receivable, and settlement/collection records can be reviewed without room inventory noise.

## Validation checklist

- [x] Opens in diagrams.net/draw.io.
- [x] Diagram elements match the source evidence.
- [x] No SRS boundary violation.
- [x] Naming and IDs are consistent with the document.

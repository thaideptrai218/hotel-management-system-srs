# Figure 7: Business State Diagram - Booking Lifecycle

- Document target: SRS
- Source evidence: v1.2 DOCX DGM-STATE-BOOK-001
- Scope: Booking business status lifecycle
- Assumptions: Completed is shown as optional/reporting state because the source marks it as derived/reporting
- Open questions: None
- Traceability: UC-005, UC-006, UC-007, UC-015, UC-016, UC-017, UC-024

## Explanation

This state diagram shows booking lifecycle states and transitions. Admin refund/settlement actions are intentionally not shown as direct booking stay-status transitions.

## Validation checklist

- [x] State names are business states, not actions.
- [x] Events are verbs or event phrases.
- [x] Guards and actions are business-level.
- [x] All status values used in SRS are covered.
- [x] Invalid transitions are not silently allowed.
- [x] The diagram references related UC IDs.

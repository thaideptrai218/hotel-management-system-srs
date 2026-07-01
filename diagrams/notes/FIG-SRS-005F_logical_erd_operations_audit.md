# Figure 5F: Logical ERD - Operations, Notification, and Audit

- Document target: SRS
- Source evidence: SRS v1.2 DGM-ERD-001; housekeeping, maintenance, notification, audit requirements
- Scope: Room operations, housekeeping task, maintenance request, room status history, notifications, audit
- Assumptions: NotificationRecord may be related to events from housekeeping/maintenance as an event source; exact polymorphic link is logical at SRS level
- Open questions: None
- Traceability: UC-020, UC-021, UC-022, UC-023, UC-024, UC-033, UC-034, UC-035

## Explanation

This slice keeps operational room work and observability records together. It shows the entities affected by housekeeping and maintenance flows plus notification and audit records without dragging finance or booking detail into the same figure.

## Validation checklist

- [x] Opens in diagrams.net/draw.io.
- [x] Diagram elements match the source evidence.
- [x] No SRS boundary violation.
- [x] Naming and IDs are consistent with the document.

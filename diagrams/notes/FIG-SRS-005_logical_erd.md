# Figure 5: Logical Entity Relationship Diagram

- Document target: SRS
- Source evidence: v1.2 DOCX DGM-ERD-001, sections 3.1.6 Entity Details and 3.1.7 Entity Origin Traceability
- Scope: Logical/domain ERD for hotel marketplace, staff, booking, front desk, housekeeping, maintenance, payment, refund, commission, settlement, notification, and audit records
- Assumptions: Cardinalities follow the DGM-ERD-001 required relationships
- Open questions: None
- Traceability: ENT items listed in DGM-ERD-001; UC-001 to UC-037

## Explanation

This ERD keeps the model logical rather than physical. It shows the main business entities, key identifiers, status-bearing entities, and required relationships. Technical indexes, ORM fields, repositories, services, and packages are intentionally omitted.

## Validation checklist

- [x] Every entity is a business/domain concept.
- [x] Entities are traceable to use cases, screens, or source SRS sections.
- [x] Cardinalities are present and plausible.
- [x] Entity names match Entity Details and DGM-ERD-001.
- [x] No physical-only database implementation detail dominates the diagram.

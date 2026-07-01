# Figure 5B: Logical ERD - Account and Staff

- Document target: SRS
- Source evidence: SRS v1.2 DGM-ERD-001; staff/account features and use cases
- Scope: Authentication, roles, hotel ownership, hotel-scoped staff assignment, staff invitation
- Assumptions: StaffInvitation may optionally link to a UserAccount after acceptance
- Open questions: None
- Traceability: UC-001, UC-002, UC-003, UC-030, UC-031, UC-032, staff actor model

## Explanation

This slice isolates account and staff concepts so hotel-scoped staff access is readable without mixing booking, inventory, and finance records.

## Validation checklist

- [x] Opens in diagrams.net/draw.io.
- [x] Diagram elements match the source evidence.
- [x] No SRS boundary violation.
- [x] Naming and IDs are consistent with the document.

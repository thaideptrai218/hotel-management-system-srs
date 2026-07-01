# FIG-SRS-007 - Business State Diagram: Payment, Refund, and Settlement Lifecycle

- Source block: DGM-STATE-FIN-001.
- Skill used: `srs-drawio-business-state-diagram`.
- Output: `FIG-SRS-007_business_state_finance_lifecycle.drawio`.

This diagram separates payment, refund, and settlement/commission state groups while showing their key dependencies.

Payment states covered: Pending, Processing, Paid, Failed, Cancelled, Expired, Reconciled, Exception.

Refund states covered: Not Required, Requested, Approved, Rejected, Processing, Refunded, Failed.

Settlement states covered: Pending, Partially Settled, Settled, Commission Receivable, Commission Collected, Exception.

The refund path is explicitly approval-driven; the diagram does not imply automated gateway refund approval.

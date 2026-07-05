# 3.20 UC-020 - Reconcile Payment

## 3.20.1 Design Purpose

This section describes the detailed design for **UC-020 Reconcile Payment**. The use case covers Review payment transaction status and mark reconciliation result. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-ADMIN-FINANCE, UC-020, SCR-039, NSF-001, ENT-016, ENT-024, BR-PAY-003, BR-FIN-002, BR-ADMIN-003, BR-AUDIT-001, MSG-FIN-003, MSG-FIN-004, TR-020, AT-UC020-06A, AT-UC020-06B.

**Precondition:** Platform Administrator authenticated; payment transaction exists.

**Trigger:** Admin opens Payment Reconciliation.

**Post-condition:** POS-01: Payment transaction reconciliation status is updated and audit is recorded.

The flow must:

- Main step 1: Platform Administrator admin opens Payment Reconciliation.
- Main step 2: System displays transactions with filters.
- Main step 3: Platform Administrator admin selects transaction.
- Main step 4: System displays payment, booking, provider reference, amount, and reconciliation status.
- Main step 5: Platform Administrator admin marks Reconciled or Exception and enters note if required.
- Main step 6: System validates decision, required note, current reconciliation state, and duplicate/concurrent update guard.
- Main step 7: System records reconciliation status and audit if the transaction is still eligible for update.
- Continue through the remaining SRS main-flow steps until the UC-020 post-condition is reached.
- Enforce related business rules: BR-PAY-003, BR-FIN-002, BR-ADMIN-003, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC020-06A, AT-UC020-06B.

## 3.20.2 Class Diagram

This part presents the class diagram for UC-020 Reconcile Payment.

![DGM-CLS-UC-020 - Reconcile Payment Class Diagram](./dgm-cls-uc-020-reconcile-payment-class-diagram.png)

**Figure 3.20-1: Class Diagram of UC-020 Reconcile Payment**

## 3.20.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### PaymentReconciliationScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-020 Reconcile Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ReconcilePaymentController Class

**Description:** API/application entry controller for UC-020 Reconcile Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ReconcilePaymentRequest Class

**Description:** Request DTO carrying input for UC-020 Reconcile Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ReconcilePaymentService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Reconcile Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `reconcilePayment(request)` | Executes the UC-020 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### PaymentTransactionRepository Class

**Description:** Repository abstraction for loading and saving data required by Reconcile Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### PayOsReconciliationClient Class

**Description:** Supporting service or integration used by UC-020 Reconcile Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ReconcilePaymentResponse Class

**Description:** Response DTO returned by UC-020 Reconcile Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### PaymentTransaction Class

**Description:** Online payment transaction for Platform Collect booking.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### AuditRecord Class

**Description:** Administrative, financial, staff, booking, room, housekeeping, and maintenance action audit record.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.20.4 Sequence Diagram

This part presents the sequence diagrams for UC-020 Reconcile Payment. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-020 - Reconcile Payment Main Flow](./dgm-seq-uc-020-reconcile-payment-main-flow.png)

**Figure 3.20-2: Sequence Diagram of UC-020 Reconcile Payment - Main Flow**

### AT-UC020-06A - Amount/status mismatch

- **Branch from Main Step:** 6
- **Condition:** Amount/status mismatch
- **Expected Response:** Payment reconciliation status has been updated successfully.

![DGM-SEQ-UC-020 - Amount/status mismatch](./dgm-seq-uc-020-reconcile-payment-at-uc020-06a-amount-status-mismatch.png)

**Figure 3.20-3: Sequence Diagram of UC-020 Reconcile Payment - AT-UC020-06A Amount/status mismatch**

### AT-UC020-06B - Duplicate reconciliation action or already reconciled transaction

- **Branch from Main Step:** 6
- **Condition:** Duplicate reconciliation action or already reconciled transaction
- **Expected Response:** Payment reconciliation status has been updated successfully.

![DGM-SEQ-UC-020 - Duplicate reconciliation action or already reconciled transaction](./dgm-seq-uc-020-reconcile-payment-at-uc020-06b-duplicate-reconciliation-action-or-already-reconciled-transaction.png)

**Figure 3.20-4: Sequence Diagram of UC-020 Reconcile Payment - AT-UC020-06B Duplicate reconciliation action or already reconciled transaction**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Platform Administrator according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC020-06A returns "Payment reconciliation status has been updated successfully."; AT-UC020-06B returns "Payment reconciliation status has been updated successfully.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC020-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC020-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC020-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

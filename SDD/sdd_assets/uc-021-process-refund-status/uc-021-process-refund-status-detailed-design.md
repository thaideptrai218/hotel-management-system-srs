# 3.21 UC-021 - Process Refund Status

## 3.21.1 Design Purpose

This section describes the detailed design for **UC-021 Process Refund Status**. The use case covers Record manual refund decision and refund status. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-ADMIN-FINANCE, UC-021, SCR-040, ENT-018, ENT-024, BR-REF-001, BR-REF-002, BR-FIN-002, BR-AUDIT-001, MSG-REF-001, MSG-REF-003, MSG-REF-004, TR-021, AT-UC021-06A, AT-UC021-06B.

**Precondition:** Platform Administrator authenticated; RefundRecord exists.

**Trigger:** Admin opens Refund Management.

**Post-condition:** POS-01: Refund status is updated and customer-visible refund status changes accordingly.

The flow must:

- Main step 1: Platform Administrator admin opens Refund Management.
- Main step 2: System displays refund request list.
- Main step 3: Platform Administrator admin selects refund.
- Main step 4: System displays booking, payment, policy, paid amount, requested amount, and current refund status.
- Main step 5: Platform Administrator admin approves/rejects/marks processed and enters amount/note if required.
- Main step 6: System validates amount and transition.
- Main step 7: System updates refund status and records audit.
- Continue through the remaining SRS main-flow steps until the UC-021 post-condition is reached.
- Enforce related business rules: BR-REF-001, BR-REF-002, BR-FIN-002, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC021-06A, AT-UC021-06B.

## 3.21.2 Class Diagram

This part presents the class diagram for UC-021 Process Refund Status.

![DGM-CLS-UC-021 - Process Refund Status Class Diagram](./dgm-cls-uc-021-process-refund-status-class-diagram.png)

**Figure 3.21-1: Class Diagram of UC-021 Process Refund Status**

## 3.21.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### RefundManagementScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-021 Process Refund Status.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ProcessRefundStatusController Class

**Description:** API/application entry controller for UC-021 Process Refund Status.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ProcessRefundStatusRequest Class

**Description:** Request DTO carrying input for UC-021 Process Refund Status.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ProcessRefundStatusService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Process Refund Status.

| No | Method | Description |
|---:|---|---|
| 1 | `processRefundStatus(request)` | Executes the UC-021 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### RefundRecordRepository Class

**Description:** Repository abstraction for loading and saving data required by Process Refund Status.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### NotificationService Class

**Description:** Supporting service or integration used by UC-021 Process Refund Status.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ProcessRefundStatusResponse Class

**Description:** Response DTO returned by UC-021 Process Refund Status.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### RefundRecord Class

**Description:** Refund eligibility, decision, and manual processing status.

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

## 3.21.4 Sequence Diagram

This part presents the sequence diagrams for UC-021 Process Refund Status. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-021 - Process Refund Status Main Flow](./dgm-seq-uc-021-process-refund-status-main-flow.png)

**Figure 3.21-2: Sequence Diagram of UC-021 Process Refund Status - Main Flow**

### AT-UC021-06A - Invalid transition

- **Branch from Main Step:** 6
- **Condition:** Invalid transition
- **Expected Response:** The selected refund status transition is not allowed.

![DGM-SEQ-UC-021 - Invalid transition](./dgm-seq-uc-021-process-refund-status-at-uc021-06a-invalid-transition.png)

**Figure 3.21-3: Sequence Diagram of UC-021 Process Refund Status - AT-UC021-06A Invalid transition**

### AT-UC021-06B - Amount exceeds paid amount

- **Branch from Main Step:** 6
- **Condition:** Amount exceeds paid amount
- **Expected Response:** Refund amount cannot exceed the paid amount.

![DGM-SEQ-UC-021 - Amount exceeds paid amount](./dgm-seq-uc-021-process-refund-status-at-uc021-06b-amount-exceeds-paid-amount.png)

**Figure 3.21-4: Sequence Diagram of UC-021 Process Refund Status - AT-UC021-06B Amount exceeds paid amount**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Platform Administrator according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC021-06A returns "The selected refund status transition is not allowed."; AT-UC021-06B returns "Refund amount cannot exceed the paid amount.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC021-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC021-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC021-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

# 3.22 UC-022 - Mark Settlement

## 3.22.1 Design Purpose

This section describes the detailed design for **UC-022 Mark Settlement**. The use case covers Mark hotel payable settlement or commission collection as completed. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-ADMIN-FINANCE, UC-022, SCR-041, ENT-021, ENT-022, ENT-024, BR-FIN-002, BR-FIN-003, BR-FIN-005, BR-FIN-007, BR-AUDIT-001, MSG-FIN-001, MSG-FIN-005, MSG-FIN-006, MSG-FIN-007, TR-022, AT-UC022-07A, AT-UC022-07B, AT-UC022-07C, AT-UC022-07D.

**Precondition:** Platform Administrator authenticated; settlement or commission candidate records exist.

**Trigger:** Admin opens Settlement Management.

**Post-condition:** POS-01: Settlement or commission collection status is updated and notification/audit is recorded.

The flow must:

- Main step 1: Platform Administrator admin opens Settlement Management.
- Main step 2: System calculates eligibility by Settlement Type: Hotel Settlement uses Platform Collect reconciliation/refund/stay status, while Commission Collection uses CommissionRecord and Pay-at-Property collection/receivable status without requiring payOS reconciliation.
- Main step 3: System displays eligible hotel settlement records and eligible commission collection records only.
- Main step 4: Platform Administrator admin selects record/batch.
- Main step 5: System displays expected amount, settlement type, related items, hotel, applicable reconciliation/refund/commission/collection state, exception state, and current settlement status.
- Main step 6: Platform Administrator admin enters settlement date, amount, reference, and note.
- Main step 7: System validates selected settlement type eligibility, amount, required reference/date, applicable unresolved refund/reconciliation/commission/collection state, and exception state.
- Continue through the remaining SRS main-flow steps until the UC-022 post-condition is reached.
- Enforce related business rules: BR-FIN-002, BR-FIN-003, BR-FIN-005, BR-FIN-007, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC022-07A, AT-UC022-07B, AT-UC022-07C, AT-UC022-07D.

## 3.22.2 Class Diagram

This part presents the class diagram for UC-022 Mark Settlement.

![DGM-CLS-UC-022 - Mark Settlement Class Diagram](./dgm-cls-uc-022-mark-settlement-class-diagram.png)

**Figure 3.22-1: Class Diagram of UC-022 Mark Settlement**

## 3.22.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### SettlementManagementScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-022 Mark Settlement.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### MarkSettlementController Class

**Description:** API/application entry controller for UC-022 Mark Settlement.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### MarkSettlementRequest Class

**Description:** Request DTO carrying input for UC-022 Mark Settlement.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### MarkSettlementService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Mark Settlement.

| No | Method | Description |
|---:|---|---|
| 1 | `markSettlement(request)` | Executes the UC-022 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### SettlementRecordRepository Class

**Description:** Repository abstraction for loading and saving data required by Mark Settlement.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### SettlementEligibilityService Class

**Description:** Supporting service or integration used by UC-022 Mark Settlement.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### MarkSettlementResponse Class

**Description:** Response DTO returned by UC-022 Mark Settlement.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### SettlementRecord Class

**Description:** Manual hotel settlement or commission collection header.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### SettlementItem Class

**Description:** Line item linking settlement to booking/commission/payment records.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.22.4 Sequence Diagram

This part presents the sequence diagrams for UC-022 Mark Settlement. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-022 - Mark Settlement Main Flow](./dgm-seq-uc-022-mark-settlement-main-flow.png)

**Figure 3.22-2: Sequence Diagram of UC-022 Mark Settlement - Main Flow**

### AT-UC022-07A - Ineligible record

- **Branch from Main Step:** 7
- **Condition:** Ineligible record
- **Expected Response:** This record is not eligible for settlement or collection.

![DGM-SEQ-UC-022 - Ineligible record](./dgm-seq-uc-022-mark-settlement-at-uc022-07a-ineligible-record.png)

**Figure 3.22-3: Sequence Diagram of UC-022 Mark Settlement - AT-UC022-07A Ineligible record**

### AT-UC022-07B - Amount mismatch

- **Branch from Main Step:** 7
- **Condition:** Amount mismatch
- **Expected Response:** The entered amount does not match the expected amount.

![DGM-SEQ-UC-022 - Amount mismatch](./dgm-seq-uc-022-mark-settlement-at-uc022-07b-amount-mismatch.png)

**Figure 3.22-4: Sequence Diagram of UC-022 Mark Settlement - AT-UC022-07B Amount mismatch**

### AT-UC022-07C - Missing settlement date or reference

- **Branch from Main Step:** 7
- **Condition:** Missing settlement date or reference
- **Expected Response:** Please enter the settlement or collection date.

![DGM-SEQ-UC-022 - Missing settlement date or reference](./dgm-seq-uc-022-mark-settlement-at-uc022-07c-missing-settlement-date-or-reference.png)

**Figure 3.22-5: Sequence Diagram of UC-022 Mark Settlement - AT-UC022-07C Missing settlement date or reference**

### AT-UC022-07D - Unresolved required prerequisite or finance exception

- **Branch from Main Step:** 7
- **Condition:** Unresolved required prerequisite or finance exception
- **Expected Response:** This record is not eligible for settlement or collection.

![DGM-SEQ-UC-022 - Unresolved required prerequisite or finance exception](./dgm-seq-uc-022-mark-settlement-at-uc022-07d-unresolved-required-prerequisite-or-finance-exception.png)

**Figure 3.22-6: Sequence Diagram of UC-022 Mark Settlement - AT-UC022-07D Unresolved required prerequisite or finance exception**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Platform Administrator according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC022-07A returns "This record is not eligible for settlement or collection."; AT-UC022-07B returns "The entered amount does not match the expected amount."; AT-UC022-07C returns "Please enter the settlement or collection date."; AT-UC022-07D returns "This record is not eligible for settlement or collection.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC022-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC022-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC022-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

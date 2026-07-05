# 3.19 UC-019 - Manage Commission Rate

## 3.19.1 Design Purpose

This section describes the detailed design for **UC-019 Manage Commission Rate**. The use case covers Set commission rate per approved hotel. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-ADMIN-FINANCE, UC-019, SCR-038, ENT-020, BR-FIN-001, BR-ADMIN-002, BR-AUDIT-001, MSG-FIN-002, MSG-ADMIN-002, MSG-ADMIN-005, TR-019, AT-UC019-04A, AT-UC019-04B.

**Precondition:** Platform Administrator authenticated; hotel exists.

**Trigger:** Admin opens Commission Management.

**Post-condition:** POS-01: Commission rate is saved for future booking snapshots.

The flow must:

- Main step 1: Platform Administrator admin opens Commission Management.
- Main step 2: System displays hotels and current rates.
- Main step 3: Platform Administrator admin selects hotel and enters new rate/note/effective date.
- Main step 4: System validates rate range and effective date.
- Main step 5: System records rate for future bookings.
- Main step 6: System preserves existing booking snapshots.
- Main step 7: System records audit and displays success.
- Enforce related business rules: BR-FIN-001, BR-ADMIN-002, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC019-04A, AT-UC019-04B.

## 3.19.2 Class Diagram

This part presents the class diagram for UC-019 Manage Commission Rate.

![DGM-CLS-UC-019 - Manage Commission Rate Class Diagram](./dgm-cls-uc-019-manage-commission-rate-class-diagram.png)

**Figure 3.19-1: Class Diagram of UC-019 Manage Commission Rate**

## 3.19.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### CommissionManagementScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-019 Manage Commission Rate.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ManageCommissionRateController Class

**Description:** API/application entry controller for UC-019 Manage Commission Rate.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ManageCommissionRateRequest Class

**Description:** Request DTO carrying input for UC-019 Manage Commission Rate.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ManageCommissionRateService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Manage Commission Rate.

| No | Method | Description |
|---:|---|---|
| 1 | `manageCommissionRate(request)` | Executes the UC-019 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### CommissionRecordRepository Class

**Description:** Repository abstraction for loading and saving data required by Manage Commission Rate.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### CommissionPolicyService Class

**Description:** Supporting service or integration used by UC-019 Manage Commission Rate.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ManageCommissionRateResponse Class

**Description:** Response DTO returned by UC-019 Manage Commission Rate.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### CommissionRecord Class

**Description:** Platform commission calculated for a booking.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.19.4 Sequence Diagram

This part presents the sequence diagrams for UC-019 Manage Commission Rate. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-019 - Manage Commission Rate Main Flow](./dgm-seq-uc-019-manage-commission-rate-main-flow.png)

**Figure 3.19-2: Sequence Diagram of UC-019 Manage Commission Rate - Main Flow**

### AT-UC019-04A - Invalid rate

- **Branch from Main Step:** 4
- **Condition:** Invalid rate
- **Expected Response:** Please enter a valid commission rate.

![DGM-SEQ-UC-019 - Invalid rate](./dgm-seq-uc-019-manage-commission-rate-at-uc019-04a-invalid-rate.png)

**Figure 3.19-3: Sequence Diagram of UC-019 Manage Commission Rate - AT-UC019-04A Invalid rate**

### AT-UC019-04B - Hotel not approved

- **Branch from Main Step:** 4
- **Condition:** Hotel not approved
- **Expected Response:** Commission rate can be configured only for an approved hotel.

![DGM-SEQ-UC-019 - Hotel not approved](./dgm-seq-uc-019-manage-commission-rate-at-uc019-04b-hotel-not-approved.png)

**Figure 3.19-4: Sequence Diagram of UC-019 Manage Commission Rate - AT-UC019-04B Hotel not approved**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Platform Administrator according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC019-04A returns "Please enter a valid commission rate."; AT-UC019-04B returns "Commission rate can be configured only for an approved hotel.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC019-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC019-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC019-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

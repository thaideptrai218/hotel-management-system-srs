# 3.35 UC-035 - View Maintenance Requests

## 3.35.1 Design Purpose

This section describes the detailed design for **UC-035 View Maintenance Requests**. The use case covers View open, assigned, and resolved maintenance requests for assigned hotels. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-MAINTENANCE, UC-035, SCR-033, ENT-026, BR-MAINT-001, BR-STAFF-002, BR-STAFF-006, MSG-MAINT-003, MSG-AUTH-007, TR-035, AT-UC035-03A, AT-UC035-02A.

**Precondition:** Actor authenticated; hotel assignment and maintenance visibility can be validated before list display.

**Trigger:** Actor opens Maintenance Request List.

**Post-condition:** POS-01: Authorized maintenance requests are displayed.

The flow must:

- Main step 1: Actor opens Maintenance Request List.
- Main step 2: System validates actor role, hotel assignment, and maintenance visibility scope.
- Main step 3: System displays requests by room, severity, status, assignee, and date.
- Main step 4: Actor filters or selects request.
- Main step 5: System validates selected request access and displays request detail with allowed actions.
- Enforce related business rules: BR-MAINT-001, BR-STAFF-002, BR-STAFF-006.
- Return a separate scenario response for each alternative/error flow: AT-UC035-03A, AT-UC035-02A.

## 3.35.2 Class Diagram

This part presents the class diagram for UC-035 View Maintenance Requests.

![DGM-CLS-UC-035 - View Maintenance Requests Class Diagram](./dgm-cls-uc-035-view-maintenance-requests-class-diagram.png)

**Figure 3.35-1: Class Diagram of UC-035 View Maintenance Requests**

## 3.35.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### MaintenanceRequestListScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-035 View Maintenance Requests.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ViewMaintenanceRequestsController Class

**Description:** API/application entry controller for UC-035 View Maintenance Requests.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ViewMaintenanceRequestsRequest Class

**Description:** Request DTO carrying input for UC-035 View Maintenance Requests.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ViewMaintenanceRequestsService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for View Maintenance Requests.

| No | Method | Description |
|---:|---|---|
| 1 | `viewMaintenanceRequests(request)` | Executes the UC-035 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### MaintenanceRequestRepository Class

**Description:** Repository abstraction for loading and saving data required by View Maintenance Requests.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### MaintenanceAuthorizationService Class

**Description:** Supporting service or integration used by UC-035 View Maintenance Requests.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ViewMaintenanceRequestsResponse Class

**Description:** Response DTO returned by UC-035 View Maintenance Requests.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### MaintenanceRequest Class

**Description:** Room maintenance issue/request.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.35.4 Sequence Diagram

This part presents the sequence diagrams for UC-035 View Maintenance Requests. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-035 - View Maintenance Requests Main Flow](./dgm-seq-uc-035-view-maintenance-requests-main-flow.png)

**Figure 3.35-2: Sequence Diagram of UC-035 View Maintenance Requests - Main Flow**

### AT-UC035-03A - No request

- **Branch from Main Step:** 3
- **Condition:** No request
- **Expected Response:** No maintenance requests match the selected filters.

![DGM-SEQ-UC-035 - No request](./dgm-seq-uc-035-view-maintenance-requests-at-uc035-03a-no-request.png)

**Figure 3.35-3: Sequence Diagram of UC-035 View Maintenance Requests - AT-UC035-03A No request**

### AT-UC035-02A - Unauthorized hotel

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-035 - Unauthorized hotel](./dgm-seq-uc-035-view-maintenance-requests-at-uc035-02a-unauthorized-hotel.png)

**Figure 3.35-4: Sequence Diagram of UC-035 View Maintenance Requests - AT-UC035-02A Unauthorized hotel**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Maintenance Staff, Hotel Manager according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Read-only query transaction; no persistent state is changed in the successful display flow. |
| Error Handling | AT-UC035-03A returns "No maintenance requests match the selected filters."; AT-UC035-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC035-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC035-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC035-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

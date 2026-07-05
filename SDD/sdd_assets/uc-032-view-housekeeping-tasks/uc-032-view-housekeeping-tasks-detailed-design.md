# 3.32 UC-032 - View Housekeeping Tasks

## 3.32.1 Design Purpose

This section describes the detailed design for **UC-032 View Housekeeping Tasks**. The use case covers View assigned or hotel-level housekeeping tasks by room, date, priority, and status. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-HOUSEKEEPING, UC-032, SCR-030, SCR-031, ENT-025, BR-HK-001, BR-HK-002, BR-STAFF-002, BR-STAFF-005, MSG-HK-001, MSG-AUTH-007, TR-032, AT-UC032-03A, AT-UC032-02A.

**Precondition:** Actor authenticated; hotel assignment and task visibility can be validated before list display.

**Trigger:** Actor opens Housekeeping Task List.

**Post-condition:** POS-01: Authorized housekeeping tasks are displayed.

The flow must:

- Main step 1: Actor opens Housekeeping Task List.
- Main step 2: System validates actor role, hotel assignment, and task visibility scope.
- Main step 3: System displays assigned tasks or hotel-level tasks according to role.
- Main step 4: Actor filters by room, date, status, priority, or task type.
- Main step 5: System refreshes list.
- Main step 6: Actor selects task.
- Main step 7: System validates selected task access and displays task detail with allowed actions.
- Enforce related business rules: BR-HK-001, BR-HK-002, BR-STAFF-002, BR-STAFF-005.
- Return a separate scenario response for each alternative/error flow: AT-UC032-03A, AT-UC032-02A.

## 3.32.2 Class Diagram

This part presents the class diagram for UC-032 View Housekeeping Tasks.

![DGM-CLS-UC-032 - View Housekeeping Tasks Class Diagram](./dgm-cls-uc-032-view-housekeeping-tasks-class-diagram.png)

**Figure 3.32-1: Class Diagram of UC-032 View Housekeeping Tasks**

## 3.32.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### HousekeepingDashboard Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-032 View Housekeeping Tasks.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ViewHousekeepingTasksController Class

**Description:** API/application entry controller for UC-032 View Housekeeping Tasks.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ViewHousekeepingTasksRequest Class

**Description:** Request DTO carrying input for UC-032 View Housekeeping Tasks.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ViewHousekeepingTasksService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for View Housekeeping Tasks.

| No | Method | Description |
|---:|---|---|
| 1 | `viewHousekeepingTasks(request)` | Executes the UC-032 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### HousekeepingTaskRepository Class

**Description:** Repository abstraction for loading and saving data required by View Housekeeping Tasks.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### HousekeepingAuthorizationService Class

**Description:** Supporting service or integration used by UC-032 View Housekeeping Tasks.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ViewHousekeepingTasksResponse Class

**Description:** Response DTO returned by UC-032 View Housekeeping Tasks.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### HousekeepingTask Class

**Description:** Cleaning or inspection task for a physical room.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.32.4 Sequence Diagram

This part presents the sequence diagrams for UC-032 View Housekeeping Tasks. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-032 - View Housekeeping Tasks Main Flow](./dgm-seq-uc-032-view-housekeeping-tasks-main-flow.png)

**Figure 3.32-2: Sequence Diagram of UC-032 View Housekeeping Tasks - Main Flow**

### AT-UC032-03A - No tasks

- **Branch from Main Step:** 3
- **Condition:** No tasks
- **Expected Response:** No housekeeping tasks match the selected filters.

![DGM-SEQ-UC-032 - No tasks](./dgm-seq-uc-032-view-housekeeping-tasks-at-uc032-03a-no-tasks.png)

**Figure 3.32-3: Sequence Diagram of UC-032 View Housekeeping Tasks - AT-UC032-03A No tasks**

### AT-UC032-02A - Unauthorized hotel

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-032 - Unauthorized hotel](./dgm-seq-uc-032-view-housekeeping-tasks-at-uc032-02a-unauthorized-hotel.png)

**Figure 3.32-4: Sequence Diagram of UC-032 View Housekeeping Tasks - AT-UC032-02A Unauthorized hotel**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Housekeeping Staff, Hotel Manager according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Read-only query transaction; no persistent state is changed in the successful display flow. |
| Error Handling | AT-UC032-03A returns "No housekeeping tasks match the selected filters."; AT-UC032-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC032-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC032-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC032-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

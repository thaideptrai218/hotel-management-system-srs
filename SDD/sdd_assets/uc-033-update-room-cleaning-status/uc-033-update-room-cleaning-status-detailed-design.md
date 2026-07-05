# 3.33 UC-033 - Update Room Cleaning Status

## 3.33.1 Design Purpose

This section describes the detailed design for **UC-033 Update Room Cleaning Status**. The use case covers Update cleaning task and room cleaning status. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-HOUSEKEEPING, UC-033, SCR-032, SCR-035, ENT-025, ENT-027, BR-HK-001, BR-HK-002, BR-HK-003, BR-STAFF-002, BR-STAFF-005, BR-AUDIT-001, MSG-HK-002, MSG-HK-003, MSG-AUTH-007, TR-033, AT-UC033-05A, AT-UC033-04A, AT-UC033-07A, AT-UC033-02A.

**Precondition:** Actor authenticated; housekeeping task exists or room requires cleaning, and task access can be validated before detail display.

**Trigger:** Actor updates cleaning status.

**Post-condition:** POS-01: Housekeeping task and room cleaning status are updated according to allowed workflow.

The flow must:

- Main step 1: Actor opens housekeeping task detail.
- Main step 2: System validates actor role, hotel assignment, and selected task access before showing task details.
- Main step 3: System displays room, task status, checklist, notes, and allowed transitions.
- Main step 4: Actor selects new cleaning status and enters notes if required.
- Main step 5: System validates status transition and permission.
- Main step 6: System updates housekeeping task.
- Main step 7: System updates room status according to rule and records RoomStatusHistory.
- Continue through the remaining SRS main-flow steps until the UC-033 post-condition is reached.
- Enforce related business rules: BR-HK-001, BR-HK-002, BR-HK-003, BR-STAFF-002, BR-STAFF-005, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC033-05A, AT-UC033-04A, AT-UC033-07A, AT-UC033-02A.

## 3.33.2 Class Diagram

This part presents the class diagram for UC-033 Update Room Cleaning Status.

![DGM-CLS-UC-033 - Update Room Cleaning Status Class Diagram](./dgm-cls-uc-033-update-room-cleaning-status-class-diagram.png)

**Figure 3.33-1: Class Diagram of UC-033 Update Room Cleaning Status**

## 3.33.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### HousekeepingTaskDetailScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-033 Update Room Cleaning Status.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### UpdateRoomCleaningStatusController Class

**Description:** API/application entry controller for UC-033 Update Room Cleaning Status.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### UpdateRoomCleaningStatusRequest Class

**Description:** Request DTO carrying input for UC-033 Update Room Cleaning Status.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### UpdateRoomCleaningStatusService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Update Room Cleaning Status.

| No | Method | Description |
|---:|---|---|
| 1 | `updateRoomCleaningStatus(request)` | Executes the UC-033 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### HousekeepingTaskRepository Class

**Description:** Repository abstraction for loading and saving data required by Update Room Cleaning Status.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### RoomStatusWorkflowService Class

**Description:** Supporting service or integration used by UC-033 Update Room Cleaning Status.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### UpdateRoomCleaningStatusResponse Class

**Description:** Response DTO returned by UC-033 Update Room Cleaning Status.

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

### RoomStatusHistory Class

**Description:** History of room operational status changes.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.33.4 Sequence Diagram

This part presents the sequence diagrams for UC-033 Update Room Cleaning Status. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-033 - Update Room Cleaning Status Main Flow](./dgm-seq-uc-033-update-room-cleaning-status-main-flow.png)

**Figure 3.33-2: Sequence Diagram of UC-033 Update Room Cleaning Status - Main Flow**

### AT-UC033-05A - Invalid transition

- **Branch from Main Step:** 5
- **Condition:** Invalid transition
- **Expected Response:** The selected cleaning status transition is not allowed.

![DGM-SEQ-UC-033 - Invalid transition](./dgm-seq-uc-033-update-room-cleaning-status-at-uc033-05a-invalid-transition.png)

**Figure 3.33-3: Sequence Diagram of UC-033 Update Room Cleaning Status - AT-UC033-05A Invalid transition**

### AT-UC033-04A - Issue found

- **Branch from Main Step:** 4
- **Condition:** Issue found
- **Expected Response:** The selected cleaning status transition is not allowed.

![DGM-SEQ-UC-033 - Issue found](./dgm-seq-uc-033-update-room-cleaning-status-at-uc033-04a-issue-found.png)

**Figure 3.33-4: Sequence Diagram of UC-033 Update Room Cleaning Status - AT-UC033-04A Issue found**

### AT-UC033-07A - Inspection required

- **Branch from Main Step:** 7
- **Condition:** Inspection required
- **Expected Response:** The selected cleaning status transition is not allowed.

![DGM-SEQ-UC-033 - Inspection required](./dgm-seq-uc-033-update-room-cleaning-status-at-uc033-07a-inspection-required.png)

**Figure 3.33-5: Sequence Diagram of UC-033 Update Room Cleaning Status - AT-UC033-07A Inspection required**

### AT-UC033-02A - Unauthorized hotel or task

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel or task
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-033 - Unauthorized hotel or task](./dgm-seq-uc-033-update-room-cleaning-status-at-uc033-02a-unauthorized-hotel-or-task.png)

**Figure 3.33-6: Sequence Diagram of UC-033 Update Room Cleaning Status - AT-UC033-02A Unauthorized hotel or task**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Housekeeping Staff, Hotel Manager according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC033-05A returns "The selected cleaning status transition is not allowed."; AT-UC033-04A returns "The selected cleaning status transition is not allowed."; AT-UC033-07A returns "The selected cleaning status transition is not allowed."; AT-UC033-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC033-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC033-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC033-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

# 3.34 UC-034 - Report Room Issue

## 3.34.1 Design Purpose

This section describes the detailed design for **UC-034 Report Room Issue**. The use case covers Report room issue and create maintenance request. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-HOUSEKEEPING, FEAT-MAINTENANCE, UC-034, SCR-032, ENT-026, ENT-027, BR-MAINT-001, BR-MAINT-002, BR-HK-004, BR-STAFF-002, BR-STAFF-005, BR-AUDIT-001, MSG-MAINT-001, MSG-MAINT-002, MSG-AUTH-007, TR-034, AT-UC034-05A, AT-UC034-07A, AT-UC034-02A.

**Precondition:** Actor authenticated; room exists and hotel assignment can be validated before issue form display.

**Trigger:** Actor selects Report Issue.

**Post-condition:** POS-01: Maintenance request is created and room status is updated if issue severity requires blocking.

The flow must:

- Main step 1: Actor opens room issue report form.
- Main step 2: System validates actor role, hotel assignment, room access, and issue-report permission before showing room details.
- Main step 3: System displays room, issue type, severity, description, photo/note fields if enabled.
- Main step 4: Actor enters issue details and submits report.
- Main step 5: System validates required issue information and hotel assignment.
- Main step 6: System creates maintenance request.
- Main step 7: System updates room status to Maintenance or Out of Service if severity requires blocking and records RoomStatusHistory.
- Continue through the remaining SRS main-flow steps until the UC-034 post-condition is reached.
- Enforce related business rules: BR-MAINT-001, BR-MAINT-002, BR-HK-004, BR-STAFF-002, BR-STAFF-005, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC034-05A, AT-UC034-07A, AT-UC034-02A.

## 3.34.2 Class Diagram

This part presents the class diagram for UC-034 Report Room Issue.

![DGM-CLS-UC-034 - Report Room Issue Class Diagram](./dgm-cls-uc-034-report-room-issue-class-diagram.png)

**Figure 3.34-1: Class Diagram of UC-034 Report Room Issue**

## 3.34.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### HousekeepingTaskDetailScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-034 Report Room Issue.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ReportRoomIssueController Class

**Description:** API/application entry controller for UC-034 Report Room Issue.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ReportRoomIssueRequest Class

**Description:** Request DTO carrying input for UC-034 Report Room Issue.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ReportRoomIssueService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Report Room Issue.

| No | Method | Description |
|---:|---|---|
| 1 | `reportRoomIssue(request)` | Executes the UC-034 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### MaintenanceRequestRepository Class

**Description:** Repository abstraction for loading and saving data required by Report Room Issue.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### MaintenanceNotificationService Class

**Description:** Supporting service or integration used by UC-034 Report Room Issue.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ReportRoomIssueResponse Class

**Description:** Response DTO returned by UC-034 Report Room Issue.

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

### RoomStatusHistory Class

**Description:** History of room operational status changes.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.34.4 Sequence Diagram

This part presents the sequence diagrams for UC-034 Report Room Issue. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-034 - Report Room Issue Main Flow](./dgm-seq-uc-034-report-room-issue-main-flow.png)

**Figure 3.34-2: Sequence Diagram of UC-034 Report Room Issue - Main Flow**

### AT-UC034-05A - Missing issue details

- **Branch from Main Step:** 5
- **Condition:** Missing issue details
- **Expected Response:** Please enter required room issue information.

![DGM-SEQ-UC-034 - Missing issue details](./dgm-seq-uc-034-report-room-issue-at-uc034-05a-missing-issue-details.png)

**Figure 3.34-3: Sequence Diagram of UC-034 Report Room Issue - AT-UC034-05A Missing issue details**

### AT-UC034-07A - Low severity issue

- **Branch from Main Step:** 7
- **Condition:** Low severity issue
- **Expected Response:** Please enter required room issue information.

![DGM-SEQ-UC-034 - Low severity issue](./dgm-seq-uc-034-report-room-issue-at-uc034-07a-low-severity-issue.png)

**Figure 3.34-4: Sequence Diagram of UC-034 Report Room Issue - AT-UC034-07A Low severity issue**

### AT-UC034-02A - Unauthorized hotel or room

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel or room
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-034 - Unauthorized hotel or room](./dgm-seq-uc-034-report-room-issue-at-uc034-02a-unauthorized-hotel-or-room.png)

**Figure 3.34-5: Sequence Diagram of UC-034 Report Room Issue - AT-UC034-02A Unauthorized hotel or room**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Housekeeping Staff, Receptionist, Hotel Manager according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC034-05A returns "Please enter required room issue information."; AT-UC034-07A returns "Please enter required room issue information."; AT-UC034-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC034-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC034-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC034-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

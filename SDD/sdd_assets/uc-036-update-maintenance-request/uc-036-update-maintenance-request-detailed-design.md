# 3.36 UC-036 - Update Maintenance Request

## 3.36.1 Design Purpose

This section describes the detailed design for **UC-036 Update Maintenance Request**. The use case covers Update diagnosis, work status, note, and completion result. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-MAINTENANCE, UC-036, SCR-034, ENT-026, ENT-024, BR-MAINT-001, BR-MAINT-002, BR-STAFF-002, BR-STAFF-006, BR-AUDIT-001, MSG-MAINT-004, MSG-MAINT-005, MSG-MAINT-006, MSG-AUTH-007, TR-036, AT-UC036-05A, AT-UC036-05B, AT-UC036-02A.

**Precondition:** Actor authenticated/assigned; maintenance request exists.

**Trigger:** Actor updates maintenance request detail.

**Post-condition:** POS-01: Maintenance request status, notes, or completion result is updated and audited.

The flow must:

- Main step 1: Actor opens maintenance request detail.
- Main step 2: System validates actor role, hotel assignment, and selected request access before showing request details.
- Main step 3: System displays room, issue information, current status, assignee, priority, notes, and allowed transitions.
- Main step 4: Actor updates diagnosis, status, note, assignee, or completion information.
- Main step 5: System validates status transition and permission.
- Main step 6: System updates maintenance request.
- Main step 7: System records audit and sends/records notification if required.
- Continue through the remaining SRS main-flow steps until the UC-036 post-condition is reached.
- Enforce related business rules: BR-MAINT-001, BR-MAINT-002, BR-STAFF-002, BR-STAFF-006, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC036-05A, AT-UC036-05B, AT-UC036-02A.

## 3.36.2 Class Diagram

This part presents the class diagram for UC-036 Update Maintenance Request.

![DGM-CLS-UC-036 - Update Maintenance Request Class Diagram](./dgm-cls-uc-036-update-maintenance-request-class-diagram.png)

**Figure 3.36-1: Class Diagram of UC-036 Update Maintenance Request**

## 3.36.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### MaintenanceRequestDetailScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-036 Update Maintenance Request.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### UpdateMaintenanceRequestController Class

**Description:** API/application entry controller for UC-036 Update Maintenance Request.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### UpdateMaintenanceRequestRequest Class

**Description:** Request DTO carrying input for UC-036 Update Maintenance Request.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### UpdateMaintenanceRequestService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Update Maintenance Request.

| No | Method | Description |
|---:|---|---|
| 1 | `updateMaintenanceRequest(request)` | Executes the UC-036 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### MaintenanceRequestRepository Class

**Description:** Repository abstraction for loading and saving data required by Update Maintenance Request.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### MaintenanceWorkflowService Class

**Description:** Supporting service or integration used by UC-036 Update Maintenance Request.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### UpdateMaintenanceRequestResponse Class

**Description:** Response DTO returned by UC-036 Update Maintenance Request.

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

### AuditRecord Class

**Description:** Administrative, financial, staff, booking, room, housekeeping, and maintenance action audit record.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.36.4 Sequence Diagram

This part presents the sequence diagrams for UC-036 Update Maintenance Request. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-036 - Update Maintenance Request Main Flow](./dgm-seq-uc-036-update-maintenance-request-main-flow.png)

**Figure 3.36-2: Sequence Diagram of UC-036 Update Maintenance Request - Main Flow**

### AT-UC036-05A - Invalid transition

- **Branch from Main Step:** 5
- **Condition:** Invalid transition
- **Expected Response:** The selected maintenance status transition is not allowed.

![DGM-SEQ-UC-036 - Invalid transition](./dgm-seq-uc-036-update-maintenance-request-at-uc036-05a-invalid-transition.png)

**Figure 3.36-3: Sequence Diagram of UC-036 Update Maintenance Request - AT-UC036-05A Invalid transition**

### AT-UC036-05B - Missing completion note

- **Branch from Main Step:** 5
- **Condition:** Missing completion note
- **Expected Response:** Please enter a completion or resolution note.

![DGM-SEQ-UC-036 - Missing completion note](./dgm-seq-uc-036-update-maintenance-request-at-uc036-05b-missing-completion-note.png)

**Figure 3.36-4: Sequence Diagram of UC-036 Update Maintenance Request - AT-UC036-05B Missing completion note**

### AT-UC036-02A - Unauthorized hotel or request

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel or request
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-036 - Unauthorized hotel or request](./dgm-seq-uc-036-update-maintenance-request-at-uc036-02a-unauthorized-hotel-or-request.png)

**Figure 3.36-5: Sequence Diagram of UC-036 Update Maintenance Request - AT-UC036-02A Unauthorized hotel or request**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Maintenance Staff, Hotel Manager according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC036-05A returns "The selected maintenance status transition is not allowed."; AT-UC036-05B returns "Please enter a completion or resolution note."; AT-UC036-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC036-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC036-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC036-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

# 3.37 UC-037 - Release Room from Maintenance

## 3.37.1 Design Purpose

This section describes the detailed design for **UC-037 Release Room from Maintenance**. The use case covers Mark maintenance completed and return room to cleaning/available path according to room status rule. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-MAINTENANCE, UC-037, SCR-034, SCR-035, ENT-026, ENT-025, ENT-027, BR-MAINT-002, BR-HK-002, BR-ROOM-006, BR-STAFF-002, BR-STAFF-006, BR-AUDIT-001, MSG-MAINT-007, MSG-MAINT-008, MSG-MAINT-009, MSG-AUTH-007, TR-037, AT-UC037-05A, AT-UC037-05B, AT-UC037-02A.

**Precondition:** Actor authenticated/assigned; maintenance request exists and may be ready for release.

**Trigger:** Actor selects Release Room.

**Post-condition:** POS-01: Room is released from maintenance to Dirty, Inspection Required, or Available according to policy, and follow-up housekeeping task is created if required.

The flow must:

- Main step 1: Actor opens completed maintenance request detail.
- Main step 2: System validates actor role, hotel assignment, request access, and room release permission before showing release options.
- Main step 3: System displays room status, completion information, and release options.
- Main step 4: Actor confirms release and selects next room status if required.
- Main step 5: System validates maintenance completion and permission.
- Main step 6: System updates maintenance request as Resolved if not already.
- Main step 7: System updates room status to Dirty, Inspection Required, or Available according to policy and records RoomStatusHistory.
- Continue through the remaining SRS main-flow steps until the UC-037 post-condition is reached.
- Enforce related business rules: BR-MAINT-002, BR-HK-002, BR-ROOM-006, BR-STAFF-002, BR-STAFF-006, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC037-05A, AT-UC037-05B, AT-UC037-02A.

## 3.37.2 Class Diagram

This part presents the class diagram for UC-037 Release Room from Maintenance.

![DGM-CLS-UC-037 - Release Room from Maintenance Class Diagram](./dgm-cls-uc-037-release-room-from-maintenance-class-diagram.png)

**Figure 3.37-1: Class Diagram of UC-037 Release Room from Maintenance**

## 3.37.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### MaintenanceRequestDetailScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-037 Release Room from Maintenance.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ReleaseRoomFromMaintenanceController Class

**Description:** API/application entry controller for UC-037 Release Room from Maintenance.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ReleaseRoomFromMaintenanceRequest Class

**Description:** Request DTO carrying input for UC-037 Release Room from Maintenance.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ReleaseRoomFromMaintenanceService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Release Room from Maintenance.

| No | Method | Description |
|---:|---|---|
| 1 | `releaseRoomFromMaintenance(request)` | Executes the UC-037 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### MaintenanceRequestRepository Class

**Description:** Repository abstraction for loading and saving data required by Release Room from Maintenance.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### RoomReleasePolicyService Class

**Description:** Supporting service or integration used by UC-037 Release Room from Maintenance.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ReleaseRoomFromMaintenanceResponse Class

**Description:** Response DTO returned by UC-037 Release Room from Maintenance.

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

### HousekeepingTask Class

**Description:** Cleaning or inspection task for a physical room.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.37.4 Sequence Diagram

This part presents the sequence diagrams for UC-037 Release Room from Maintenance. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-037 - Release Room from Maintenance Main Flow](./dgm-seq-uc-037-release-room-from-maintenance-main-flow.png)

**Figure 3.37-2: Sequence Diagram of UC-037 Release Room from Maintenance - Main Flow**

### AT-UC037-05A - Maintenance not complete

- **Branch from Main Step:** 5
- **Condition:** Maintenance not complete
- **Expected Response:** Maintenance must be completed before the room can be released.

![DGM-SEQ-UC-037 - Maintenance not complete](./dgm-seq-uc-037-release-room-from-maintenance-at-uc037-05a-maintenance-not-complete.png)

**Figure 3.37-3: Sequence Diagram of UC-037 Release Room from Maintenance - AT-UC037-05A Maintenance not complete**

### AT-UC037-05B - Manager approval required

- **Branch from Main Step:** 5
- **Condition:** Manager approval required
- **Expected Response:** Manager approval is required before releasing this room.

![DGM-SEQ-UC-037 - Manager approval required](./dgm-seq-uc-037-release-room-from-maintenance-at-uc037-05b-manager-approval-required.png)

**Figure 3.37-4: Sequence Diagram of UC-037 Release Room from Maintenance - AT-UC037-05B Manager approval required**

### AT-UC037-02A - Unauthorized hotel, request, or room

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel, request, or room
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-037 - Unauthorized hotel, request, or room](./dgm-seq-uc-037-release-room-from-maintenance-at-uc037-02a-unauthorized-hotel-request-or-room.png)

**Figure 3.37-5: Sequence Diagram of UC-037 Release Room from Maintenance - AT-UC037-02A Unauthorized hotel, request, or room**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Maintenance Staff, Hotel Manager according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC037-05A returns "Maintenance must be completed before the room can be released."; AT-UC037-05B returns "Manager approval is required before releasing this room."; AT-UC037-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC037-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC037-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC037-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

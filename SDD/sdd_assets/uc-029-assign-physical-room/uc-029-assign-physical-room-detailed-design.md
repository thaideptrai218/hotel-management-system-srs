# 3.29 UC-029 - Assign Physical Room

## 3.29.1 Design Purpose

This section describes the detailed design for **UC-029 Assign Physical Room**. The use case covers Assign or change physical room for a confirmed booking before or during check-in. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-FRONTDESK, UC-029, SCR-024, SCR-021, ENT-011, ENT-015, ENT-027, BR-ROOM-001, BR-ROOM-002, BR-STAY-001, BR-BOOK-009, BR-STAFF-002, BR-STAFF-003, BR-AUDIT-001, MSG-STAY-004, MSG-ROOM-007, MSG-ROOM-009, MSG-AUTH-007, TR-029, AT-UC029-05A, AT-UC029-05B, AT-UC029-05C, AT-UC029-02A.

**Precondition:** Actor authenticated; booking exists and assignment permission/status can be validated before room options are displayed.

**Trigger:** Actor selects Assign Room.

**Post-condition:** POS-01: Physical room assignment is recorded without overlap conflict.

The flow must:

- Main step 1: Actor opens booking detail or room assignment board.
- Main step 2: System validates actor role, booking hotel scope, booking status, and assignment permission before showing room options.
- Main step 3: System displays booking room requirement and available physical rooms.
- Main step 4: Actor selects/changes physical room.
- Main step 5: System validates room type match, status, overlap, and hotel assignment.
- Main step 6: System records room assignment.
- Main step 7: System displays success.
- Enforce related business rules: BR-ROOM-001, BR-ROOM-002, BR-STAY-001, BR-BOOK-009, BR-STAFF-002, BR-STAFF-003, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC029-05A, AT-UC029-05B, AT-UC029-05C, AT-UC029-02A.

## 3.29.2 Class Diagram

This part presents the class diagram for UC-029 Assign Physical Room.

![DGM-CLS-UC-029 - Assign Physical Room Class Diagram](./dgm-cls-uc-029-assign-physical-room-class-diagram.png)

**Figure 3.29-1: Class Diagram of UC-029 Assign Physical Room**

## 3.29.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### RoomAssignmentBoard Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-029 Assign Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### AssignPhysicalRoomController Class

**Description:** API/application entry controller for UC-029 Assign Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### AssignPhysicalRoomRequest Class

**Description:** Request DTO carrying input for UC-029 Assign Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### AssignPhysicalRoomService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Assign Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `assignPhysicalRoom(request)` | Executes the UC-029 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### PhysicalRoomRepository Class

**Description:** Repository abstraction for loading and saving data required by Assign Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### RoomAssignmentPolicyService Class

**Description:** Supporting service or integration used by UC-029 Assign Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### AssignPhysicalRoomResponse Class

**Description:** Response DTO returned by UC-029 Assign Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### PhysicalRoom Class

**Description:** Individual private hotel room under a room type.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### BookingRoomAssignment Class

**Description:** Physical room assignment for booking/stay.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.29.4 Sequence Diagram

This part presents the sequence diagrams for UC-029 Assign Physical Room. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-029 - Assign Physical Room Main Flow](./dgm-seq-uc-029-assign-physical-room-main-flow.png)

**Figure 3.29-2: Sequence Diagram of UC-029 Assign Physical Room - Main Flow**

### AT-UC029-05A - Room unavailable

- **Branch from Main Step:** 5
- **Condition:** Room unavailable
- **Expected Response:** Selected physical room is not available for assignment.

![DGM-SEQ-UC-029 - Room unavailable](./dgm-seq-uc-029-assign-physical-room-at-uc029-05a-room-unavailable.png)

**Figure 3.29-3: Sequence Diagram of UC-029 Assign Physical Room - AT-UC029-05A Room unavailable**

### AT-UC029-05B - Room type mismatch

- **Branch from Main Step:** 5
- **Condition:** Room type mismatch
- **Expected Response:** Selected physical room does not match the required room type.

![DGM-SEQ-UC-029 - Room type mismatch](./dgm-seq-uc-029-assign-physical-room-at-uc029-05b-room-type-mismatch.png)

**Figure 3.29-4: Sequence Diagram of UC-029 Assign Physical Room - AT-UC029-05B Room type mismatch**

### AT-UC029-05C - Overlap

- **Branch from Main Step:** 5
- **Condition:** Overlap
- **Expected Response:** This physical room is already assigned to another active stay for the selected dates.

![DGM-SEQ-UC-029 - Overlap](./dgm-seq-uc-029-assign-physical-room-at-uc029-05c-overlap.png)

**Figure 3.29-5: Sequence Diagram of UC-029 Assign Physical Room - AT-UC029-05C Overlap**

### AT-UC029-02A - Unauthorized hotel, booking, or assignment action

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel, booking, or assignment action
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-029 - Unauthorized hotel, booking, or assignment action](./dgm-seq-uc-029-assign-physical-room-at-uc029-02a-unauthorized-hotel-booking-or-assignment-action.png)

**Figure 3.29-6: Sequence Diagram of UC-029 Assign Physical Room - AT-UC029-02A Unauthorized hotel, booking, or assignment action**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Receptionist, Hotel Manager, Property Owner according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC029-05A returns "Selected physical room is not available for assignment."; AT-UC029-05B returns "Selected physical room does not match the required room type."; AT-UC029-05C returns "This physical room is already assigned to another active stay for the selected dates."; AT-UC029-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC029-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC029-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC029-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

# 3.12 UC-012 - Manage Physical Room

## 3.12.1 Design Purpose

This section describes the detailed design for **UC-012 Manage Physical Room**. The use case covers create and update individual private rooms under room types. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-ROOM-INV, UC-012, SCR-018, ENT-011, ENT-027, BR-ROOM-001, BR-ROOM-005, BR-ROOM-006, BR-OWNER-001, BR-STAFF-002, MSG-ROOM-003, MSG-ROOM-004, MSG-ROOM-006, MSG-AUTH-007, TR-012, AT-UC012-05A, AT-UC012-05B, AT-UC012-02A.

**Precondition:** Actor authenticated; hotel/room type owned or assigned.

**Trigger:** Actor opens Physical Room Management.

**Post-condition:** POS-01: Physical room information is created or updated for an owned/assigned hotel.

The flow must:

- Main step 1: Actor selects hotel and room type.
- Main step 2: System validates actor permission for the selected hotel and room type before displaying room data.
- Main step 3: System displays physical rooms, lifecycle status, and allowed actions.
- Main step 4: Actor creates or updates room number/name, floor, notes, or requests an allowed lifecycle action.
- Main step 5: System validates duplicate room number, room data, lifecycle transition, and active booking conflicts.
- Main step 6: System records physical room changes and RoomStatusHistory when lifecycle status changes.
- Main step 7: System displays success message.
- Enforce related business rules: BR-ROOM-001, BR-ROOM-005, BR-ROOM-006, BR-OWNER-001, BR-STAFF-002.
- Return a separate scenario response for each alternative/error flow: AT-UC012-05A, AT-UC012-05B, AT-UC012-02A.

## 3.12.2 Class Diagram

This part presents the class diagram for UC-012 Manage Physical Room.

![DGM-CLS-UC-012 - Manage Physical Room Class Diagram](./dgm-cls-uc-012-manage-physical-room-class-diagram.png)

**Figure 3.12-1: Class Diagram of UC-012 Manage Physical Room**

## 3.12.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### PhysicalRoomManagementScreen Class

**Description:** Boundary object for the user-visible entry point of UC-012 Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or user-visible entry state described by the SRS. |
| 2 | `collectInput()` | Collects actor input before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### PhysicalRoomController Class

**Description:** API/application entry controller for UC-012 Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case within role or hotel scope. |

### ManagePhysicalRoomRequest Class

**Description:** Request DTO carrying input for UC-012 Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or guest context needed for authorization. |

### PhysicalRoomService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `managephysicalroom(request)` | Executes the UC-012 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### PhysicalRoomRepository Class

**Description:** Repository abstraction for loading and saving data required by Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### RoomStatusWorkflowService Class

**Description:** Supporting service or integration used by UC-012 Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ManagePhysicalRoomResponse Class

**Description:** Response DTO returned by UC-012 Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### PhysicalRoom Class

**Description:** Primary domain entity affected or displayed by UC-012 Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### RoomStatusHistory Class

**Description:** Supporting domain entity affected or displayed by UC-012 Manage Physical Room.

| No | Method | Description |
|---:|---|---|
| 1 | `isLinkedToUseCase()` | Determines whether the entity is related to the current use-case operation. |
| 2 | `updateStatus()` | Updates status or lifecycle information when the validated flow requires it. |
| 3 | `getAuditSummary()` | Provides auditable summary data for protected state changes. |

## 3.12.4 Sequence Diagram

This part presents the sequence diagrams for UC-012 Manage Physical Room. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-012 - Manage Physical Room Main Flow](./dgm-seq-uc-012-manage-physical-room-main-flow.png)

**Figure 3.12-2: Sequence Diagram of UC-012 Manage Physical Room - Main Flow**

### AT-UC012-05A - Duplicate room number

- **Branch from Main Step:** 5
- **Condition:** Duplicate room number
- **Expected Response:** Room number must be unique within the hotel.

![DGM-SEQ-UC-012 - Duplicate room number](./dgm-seq-uc-012-manage-physical-room-at-uc012-05a-duplicate-room-number.png)

**Figure 3.12-3: Sequence Diagram of UC-012 Manage Physical Room - AT-UC012-05A Duplicate room number**

### AT-UC012-05B - Inactivate occupied room

- **Branch from Main Step:** 5
- **Condition:** Inactivate occupied room
- **Expected Response:** This room cannot be inactivated because it is currently occupied.

![DGM-SEQ-UC-012 - Inactivate occupied room](./dgm-seq-uc-012-manage-physical-room-at-uc012-05b-inactivate-occupied-room.png)

**Figure 3.12-4: Sequence Diagram of UC-012 Manage Physical Room - AT-UC012-05B Inactivate occupied room**

### AT-UC012-02A - Unauthorized hotel or room type

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel or room type
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-012 - Unauthorized hotel or room type](./dgm-seq-uc-012-manage-physical-room-at-uc012-02a-unauthorized-hotel-or-room-type.png)

**Figure 3.12-5: Sequence Diagram of UC-012 Manage Physical Room - AT-UC012-02A Unauthorized hotel or room type**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only the SRS actor scope for Property Owner / Hotel Manager; enforce role, ownership, hotel-scope, or platform-scope preconditions before protected data is displayed or changed. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. Read-only flows do not create domain records. |
| Error Handling | AT-UC012-05A returns "Room number must be unique within the hotel."; AT-UC012-05B returns "This room cannot be inactivated because it is currently occupied."; AT-UC012-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC012-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC012-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC012-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

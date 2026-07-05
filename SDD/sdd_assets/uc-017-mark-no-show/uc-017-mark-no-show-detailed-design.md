# 3.17 UC-017 - Mark No-show

## 3.17.1 Design Purpose

This section describes the detailed design for **UC-017 Mark No-show**. The use case covers mark confirmed booking as no-show when customer does not arrive within allowed operational window. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-FRONTDESK, UC-017, SCR-021, SCR-023, ENT-013, ENT-028, BR-STAY-004, BR-BOOK-009, BR-FIN-004, BR-STAY-007, BR-STAFF-002, BR-STAFF-003, MSG-STAY-006, MSG-STAY-007, MSG-STAY-008, MSG-AUTH-007, TR-017, AT-UC017-05A, AT-UC017-05B, AT-UC017-05C, AT-UC017-02A.

**Precondition:** Actor authenticated; booking exists for a hotel the actor owns or is assigned to.

**Trigger:** Actor selects Mark No-show.

**Post-condition:** POS-01: Booking status becomes No-show and financial traceability is preserved.

The flow must:

- Main step 1: Actor opens booking detail or no-show candidate action.
- Main step 2: System validates actor role, hotel scope, and booking access before showing no-show details.
- Main step 3: System displays no-show eligibility, policy summary, and reason field.
- Main step 4: Actor selects Mark No-show and enters reason.
- Main step 5: System validates booking status, no-show eligibility, and required reason.
- Main step 6: System updates booking to No-show.
- Main step 7: System releases reserved availability, releases any pre-assigned physical room, and keeps finance records according to policy.
- Main step 8: System records audit and sends or records notification.
- Main step 9: System displays no-show success.
- Enforce related business rules: BR-STAY-004, BR-BOOK-009, BR-FIN-004, BR-STAY-007, BR-STAFF-002, BR-STAFF-003.
- Return a separate scenario response for each alternative/error flow: AT-UC017-05A, AT-UC017-05B, AT-UC017-05C, AT-UC017-02A.

## 3.17.2 Class Diagram

This part presents the class diagram for UC-017 Mark No-show.

![DGM-CLS-UC-017 - Mark No-show Class Diagram](./dgm-cls-uc-017-mark-no-show-class-diagram.png)

**Figure 3.17-1: Class Diagram of UC-017 Mark No-show**

## 3.17.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### NoShowActionScreen Class

**Description:** Boundary object for the user-visible entry point of UC-017 Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or user-visible entry state described by the SRS. |
| 2 | `collectInput()` | Collects actor input before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### NoShowController Class

**Description:** API/application entry controller for UC-017 Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case within role or hotel scope. |

### MarkNoShowRequest Class

**Description:** Request DTO carrying input for UC-017 Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or guest context needed for authorization. |

### NoShowService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `marknoshow(request)` | Executes the UC-017 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### BookingRepository Class

**Description:** Repository abstraction for loading and saving data required by Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### NoShowPolicyService Class

**Description:** Supporting service or integration used by UC-017 Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### MarkNoShowResponse Class

**Description:** Response DTO returned by UC-017 Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### Booking Class

**Description:** Primary domain entity affected or displayed by UC-017 Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### BookingRoomAssignment Class

**Description:** Supporting domain entity affected or displayed by UC-017 Mark No-show.

| No | Method | Description |
|---:|---|---|
| 1 | `isLinkedToUseCase()` | Determines whether the entity is related to the current use-case operation. |
| 2 | `updateStatus()` | Updates status or lifecycle information when the validated flow requires it. |
| 3 | `getAuditSummary()` | Provides auditable summary data for protected state changes. |

## 3.17.4 Sequence Diagram

This part presents the sequence diagrams for UC-017 Mark No-show. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-017 - Mark No-show Main Flow](./dgm-seq-uc-017-mark-no-show-main-flow.png)

**Figure 3.17-2: Sequence Diagram of UC-017 Mark No-show - Main Flow**

### AT-UC017-05A - Too early

- **Branch from Main Step:** 5
- **Condition:** Too early
- **Expected Response:** This booking is not eligible to be marked as no-show yet.

![DGM-SEQ-UC-017 - Too early](./dgm-seq-uc-017-mark-no-show-at-uc017-05a-too-early.png)

**Figure 3.17-3: Sequence Diagram of UC-017 Mark No-show - AT-UC017-05A Too early**

### AT-UC017-05B - Invalid booking status

- **Branch from Main Step:** 5
- **Condition:** Invalid booking status
- **Expected Response:** This action is not allowed for the current booking status.

![DGM-SEQ-UC-017 - Invalid booking status](./dgm-seq-uc-017-mark-no-show-at-uc017-05b-invalid-booking-status.png)

**Figure 3.17-4: Sequence Diagram of UC-017 Mark No-show - AT-UC017-05B Invalid booking status**

### AT-UC017-05C - Missing no-show reason

- **Branch from Main Step:** 5
- **Condition:** Missing no-show reason
- **Expected Response:** Please enter a no-show reason before marking this booking.

![DGM-SEQ-UC-017 - Missing no-show reason](./dgm-seq-uc-017-mark-no-show-at-uc017-05c-missing-no-show-reason.png)

**Figure 3.17-5: Sequence Diagram of UC-017 Mark No-show - AT-UC017-05C Missing no-show reason**

### AT-UC017-02A - Unauthorized hotel or booking

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel or booking
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-017 - Unauthorized hotel or booking](./dgm-seq-uc-017-mark-no-show-at-uc017-02a-unauthorized-hotel-or-booking.png)

**Figure 3.17-6: Sequence Diagram of UC-017 Mark No-show - AT-UC017-02A Unauthorized hotel or booking**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only the SRS actor scope for Receptionist / Hotel Manager / Property Owner; enforce role, ownership, hotel-scope, or platform-scope preconditions before protected data is displayed or changed. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. Read-only flows do not create domain records. |
| Error Handling | AT-UC017-05A returns "This booking is not eligible to be marked as no-show yet."; AT-UC017-05B returns "This action is not allowed for the current booking status."; AT-UC017-05C returns "Please enter a no-show reason before marking this booking."; AT-UC017-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC017-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC017-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC017-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

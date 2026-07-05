# 3.28 UC-028 - View Arrival and Departure List

## 3.28.1 Design Purpose

This section describes the detailed design for **UC-028 View Arrival and Departure List**. The use case covers View today/upcoming arrivals, departures, no-show candidates, and operational status. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-FRONTDESK, UC-028, SCR-022, SCR-023, ENT-013, BR-STAFF-002, BR-STAFF-003, BR-BOOK-009, MSG-FD-001, MSG-AUTH-007, TR-028, AT-UC028-03A, AT-UC028-02A.

**Precondition:** Actor authenticated; hotel assignment and front desk list visibility can be validated before list display.

**Trigger:** Actor opens Arrival/Departure List.

**Post-condition:** POS-01: Arrival/departure/in-house/no-show candidate list is displayed for assigned hotel scope.

The flow must:

- Main step 1: Actor opens Arrival/Departure List.
- Main step 2: System validates actor role, hotel assignment, and front desk list visibility scope.
- Main step 3: System displays hotel/date filters and lists for arrivals, in-house stays, departures, and no-show candidates.
- Main step 4: Actor filters by date, room type, status, or keyword.
- Main step 5: System refreshes list.
- Main step 6: Actor selects booking.
- Main step 7: System validates selected booking access and displays booking detail with allowed actions.
- Enforce related business rules: BR-STAFF-002, BR-STAFF-003, BR-BOOK-009.
- Return a separate scenario response for each alternative/error flow: AT-UC028-03A, AT-UC028-02A.

## 3.28.2 Class Diagram

This part presents the class diagram for UC-028 View Arrival and Departure List.

![DGM-CLS-UC-028 - View Arrival and Departure List Class Diagram](./dgm-cls-uc-028-view-arrival-and-departure-list-class-diagram.png)

**Figure 3.28-1: Class Diagram of UC-028 View Arrival and Departure List**

## 3.28.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### FrontDeskDashboard Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-028 View Arrival and Departure List.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ViewArrivalAndDepartureListController Class

**Description:** API/application entry controller for UC-028 View Arrival and Departure List.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ViewArrivalAndDepartureListRequest Class

**Description:** Request DTO carrying input for UC-028 View Arrival and Departure List.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ViewArrivalAndDepartureListService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for View Arrival and Departure List.

| No | Method | Description |
|---:|---|---|
| 1 | `viewArrivalAndDepartureList(request)` | Executes the UC-028 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### BookingRepository Class

**Description:** Repository abstraction for loading and saving data required by View Arrival and Departure List.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### FrontDeskAuthorizationService Class

**Description:** Supporting service or integration used by UC-028 View Arrival and Departure List.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ViewArrivalAndDepartureListResponse Class

**Description:** Response DTO returned by UC-028 View Arrival and Departure List.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### Booking Class

**Description:** Customer reservation for selected hotel and room/date range.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.28.4 Sequence Diagram

This part presents the sequence diagrams for UC-028 View Arrival and Departure List. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-028 - View Arrival and Departure List Main Flow](./dgm-seq-uc-028-view-arrival-and-departure-list-main-flow.png)

**Figure 3.28-2: Sequence Diagram of UC-028 View Arrival and Departure List - Main Flow**

### AT-UC028-03A - No arrivals/departures

- **Branch from Main Step:** 3
- **Condition:** No arrivals/departures
- **Expected Response:** No arrivals or departures match the selected filters.

![DGM-SEQ-UC-028 - No arrivals/departures](./dgm-seq-uc-028-view-arrival-and-departure-list-at-uc028-03a-no-arrivals-departures.png)

**Figure 3.28-3: Sequence Diagram of UC-028 View Arrival and Departure List - AT-UC028-03A No arrivals/departures**

### AT-UC028-02A - Unauthorized hotel

- **Branch from Main Step:** 2
- **Condition:** Unauthorized hotel
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-028 - Unauthorized hotel](./dgm-seq-uc-028-view-arrival-and-departure-list-at-uc028-02a-unauthorized-hotel.png)

**Figure 3.28-4: Sequence Diagram of UC-028 View Arrival and Departure List - AT-UC028-02A Unauthorized hotel**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Receptionist, Hotel Manager, Property Owner according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Read-only query transaction; no persistent state is changed in the successful display flow. |
| Error Handling | AT-UC028-03A returns "No arrivals or departures match the selected filters."; AT-UC028-02A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC028-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC028-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC028-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

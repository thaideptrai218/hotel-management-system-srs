# 3.31 UC-031 - Create Walk-in Booking

## 3.31.1 Design Purpose

This section describes the detailed design for **UC-031 Create Walk-in Booking**. The use case covers Create booking for guest arriving directly at hotel if room is available. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-FRONTDESK, UC-031, SCR-027, ENT-013, ENT-014, ENT-015, BR-BOOK-001, BR-BOOK-002, BR-BOOK-003, BR-BOOK-013, BR-FD-001, BR-STAFF-003, MSG-BOOK-002, MSG-FD-002, MSG-FD-003, TR-031, AT-UC031-06A, AT-UC031-05A, AT-UC031-02A, AT-UC031-07A, AT-UC031-07B.

**Precondition:** Actor authenticated; walk-in booking enabled for owned or assigned hotel.

**Trigger:** Actor selects Create Walk-in Booking.

**Post-condition:** POS-01: Walk-in booking is created with booking source Walk-in if availability exists.

The flow must:

- Main step 1: Actor opens Walk-in Booking Screen.
- Main step 2: System validates actor role, hotel scope, walk-in enablement, and available payment modes before showing booking fields.
- Main step 3: System displays hotel, date, room type, guest information, price summary, and payment mode fields.
- Main step 4: Actor enters guest/stay information and selects payment mode.
- Main step 5: System validates date range, guest information, payment mode, and price.
- Main step 6: System atomically validates availability and reserves requested room type quantity for the date range.
- Main step 7: System branches by selected payment mode and creates booking with source Walk-in and correct initial status.
- Continue through the remaining SRS main-flow steps until the UC-031 post-condition is reached.
- Enforce related business rules: BR-BOOK-001, BR-BOOK-002, BR-BOOK-003, BR-BOOK-013, BR-FD-001, BR-STAFF-003.
- Return a separate scenario response for each alternative/error flow: AT-UC031-06A, AT-UC031-05A, AT-UC031-02A, AT-UC031-07A, AT-UC031-07B.

## 3.31.2 Class Diagram

This part presents the class diagram for UC-031 Create Walk-in Booking.

![DGM-CLS-UC-031 - Create Walk-in Booking Class Diagram](./dgm-cls-uc-031-create-walk-in-booking-class-diagram.png)

**Figure 3.31-1: Class Diagram of UC-031 Create Walk-in Booking**

## 3.31.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### WalkInBookingScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-031 Create Walk-in Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### CreateWalkInBookingController Class

**Description:** API/application entry controller for UC-031 Create Walk-in Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### CreateWalkInBookingRequest Class

**Description:** Request DTO carrying input for UC-031 Create Walk-in Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### CreateWalkInBookingService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Create Walk-in Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `createWalkInBooking(request)` | Executes the UC-031 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### BookingRepository Class

**Description:** Repository abstraction for loading and saving data required by Create Walk-in Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### AvailabilityReservationService Class

**Description:** Supporting service or integration used by UC-031 Create Walk-in Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### CreateWalkInBookingResponse Class

**Description:** Response DTO returned by UC-031 Create Walk-in Booking.

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

### BookingRoom Class

**Description:** Booking line item representing room type and quantity.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.31.4 Sequence Diagram

This part presents the sequence diagrams for UC-031 Create Walk-in Booking. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-031 - Create Walk-in Booking Main Flow](./dgm-seq-uc-031-create-walk-in-booking-main-flow.png)

**Figure 3.31-2: Sequence Diagram of UC-031 Create Walk-in Booking - Main Flow**

### AT-UC031-06A - Room unavailable

- **Branch from Main Step:** 6
- **Condition:** Room unavailable
- **Expected Response:** The selected room is no longer available for the selected dates.

![DGM-SEQ-UC-031 - Room unavailable](./dgm-seq-uc-031-create-walk-in-booking-at-uc031-06a-room-unavailable.png)

**Figure 3.31-3: Sequence Diagram of UC-031 Create Walk-in Booking - AT-UC031-06A Room unavailable**

### AT-UC031-05A - Missing guest contact

- **Branch from Main Step:** 5
- **Condition:** Missing guest contact
- **Expected Response:** Please enter required guest contact information.

![DGM-SEQ-UC-031 - Missing guest contact](./dgm-seq-uc-031-create-walk-in-booking-at-uc031-05a-missing-guest-contact.png)

**Figure 3.31-4: Sequence Diagram of UC-031 Create Walk-in Booking - AT-UC031-05A Missing guest contact**

### AT-UC031-02A - Walk-in disabled

- **Branch from Main Step:** 2
- **Condition:** Walk-in disabled
- **Expected Response:** Walk-in booking is not enabled for this hotel or role.

![DGM-SEQ-UC-031 - Walk-in disabled](./dgm-seq-uc-031-create-walk-in-booking-at-uc031-02a-walk-in-disabled.png)

**Figure 3.31-5: Sequence Diagram of UC-031 Create Walk-in Booking - AT-UC031-02A Walk-in disabled**

### AT-UC031-07A - Platform Collect

- **Branch from Main Step:** 7
- **Condition:** Platform Collect
- **Expected Response:** The selected room is no longer available for the selected dates.

![DGM-SEQ-UC-031 - Platform Collect](./dgm-seq-uc-031-create-walk-in-booking-at-uc031-07a-platform-collect.png)

**Figure 3.31-6: Sequence Diagram of UC-031 Create Walk-in Booking - AT-UC031-07A Platform Collect**

### AT-UC031-07B - Pay at Property

- **Branch from Main Step:** 7
- **Condition:** Pay at Property
- **Expected Response:** The selected room is no longer available for the selected dates.

![DGM-SEQ-UC-031 - Pay at Property](./dgm-seq-uc-031-create-walk-in-booking-at-uc031-07b-pay-at-property.png)

**Figure 3.31-7: Sequence Diagram of UC-031 Create Walk-in Booking - AT-UC031-07B Pay at Property**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Receptionist, Hotel Manager, Property Owner according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC031-06A returns "The selected room is no longer available for the selected dates."; AT-UC031-05A returns "Please enter required guest contact information."; AT-UC031-02A returns "Walk-in booking is not enabled for this hotel or role."; AT-UC031-07A returns "The selected room is no longer available for the selected dates."; AT-UC031-07B returns "The selected room is no longer available for the selected dates.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC031-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC031-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC031-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

# 3.24 UC-024 - Expire Unpaid Booking

## 3.24.1 Design Purpose

This section describes the detailed design for **UC-024 Expire Unpaid Booking**. The use case covers Expire pending-payment bookings when payment timeout is reached. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-AUTO-NOTI, UC-024, NSF-002, ENT-013, ENT-023, BR-BOOK-006, BR-BOOK-007, BR-ROOM-002, BR-PAY-003, BR-PAY-005, MSG-BOOK-006, TR-024, AT-UC024-03A, AT-UC024-03B.

**Precondition:** Pending Payment booking exists; timeout configured.

**Trigger:** Payment timeout is reached.

**Post-condition:** POS-01: Expired Pending Payment bookings are marked Expired and reserved availability is released.

The flow must:

- Main step 1: System triggers expiration check.
- Main step 2: System identifies Pending Payment bookings past deadline.
- Main step 3: System atomically verifies booking is still Pending Payment, no successful payment exists, and expiration lock can be acquired.
- Main step 4: System marks eligible locked bookings Expired.
- Main step 5: System releases availability.
- Main step 6: System records notification event.
- Main step 7: System skips records that did not pass the atomic eligibility check.
- Enforce related business rules: BR-BOOK-006, BR-BOOK-007, BR-ROOM-002, BR-PAY-003, BR-PAY-005.
- Return a separate scenario response for each alternative/error flow: AT-UC024-03A, AT-UC024-03B.

## 3.24.2 Class Diagram

This part presents the class diagram for UC-024 Expire Unpaid Booking.

![DGM-CLS-UC-024 - Expire Unpaid Booking Class Diagram](./dgm-cls-uc-024-expire-unpaid-booking-class-diagram.png)

**Figure 3.24-1: Class Diagram of UC-024 Expire Unpaid Booking**

## 3.24.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### ExpireUnpaidBookingJob Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-024 Expire Unpaid Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ExpireUnpaidBookingJobController Class

**Description:** API/application entry controller for UC-024 Expire Unpaid Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ExpireUnpaidBookingRequest Class

**Description:** Request DTO carrying input for UC-024 Expire Unpaid Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ExpireUnpaidBookingService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Expire Unpaid Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `expireUnpaidBooking(request)` | Executes the UC-024 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### BookingRepository Class

**Description:** Repository abstraction for loading and saving data required by Expire Unpaid Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### AvailabilityReservationService Class

**Description:** Supporting service or integration used by UC-024 Expire Unpaid Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ExpireUnpaidBookingResponse Class

**Description:** Response DTO returned by UC-024 Expire Unpaid Booking.

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

### NotificationRecord Class

**Description:** Notification event sent or recorded.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.24.4 Sequence Diagram

This part presents the sequence diagrams for UC-024 Expire Unpaid Booking. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-024 - Expire Unpaid Booking Main Flow](./dgm-seq-uc-024-expire-unpaid-booking-main-flow.png)

**Figure 3.24-2: Sequence Diagram of UC-024 Expire Unpaid Booking - Main Flow**

### AT-UC024-03A - Payment success already exists

- **Branch from Main Step:** 3
- **Condition:** Payment success already exists
- **Expected Response:** This pending payment booking has expired. Please create a new booking.

![DGM-SEQ-UC-024 - Payment success already exists](./dgm-seq-uc-024-expire-unpaid-booking-at-uc024-03a-payment-success-already-exists.png)

**Figure 3.24-3: Sequence Diagram of UC-024 Expire Unpaid Booking - AT-UC024-03A Payment success already exists**

### AT-UC024-03B - Booking status changed

- **Branch from Main Step:** 3
- **Condition:** Booking status changed
- **Expected Response:** This pending payment booking has expired. Please create a new booking.

![DGM-SEQ-UC-024 - Booking status changed](./dgm-seq-uc-024-expire-unpaid-booking-at-uc024-03b-booking-status-changed.png)

**Figure 3.24-4: Sequence Diagram of UC-024 Expire Unpaid Booking - AT-UC024-03B Booking status changed**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only System Scheduler according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC024-03A returns "This pending payment booking has expired. Please create a new booking."; AT-UC024-03B returns "This pending payment booking has expired. Please create a new booking.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC024-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC024-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC024-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

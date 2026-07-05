# 3.7 UC-007 - Cancel Booking

## 3.7.1 Design Purpose

This section describes the detailed design for **UC-007 Cancel Booking**. The use case covers cancel own booking according to policy and initiate refund status if applicable. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-CUST-MYBOOK, UC-007, SCR-010, SCR-013, ENT-013, ENT-018, BR-BOOK-008, BR-REF-001, BR-REF-002, BR-REF-003, BR-FIN-002, MSG-BOOK-005, MSG-BOOK-007, MSG-REF-002, TR-007, AT-UC007-04A, AT-UC007-06A, AT-UC007-06B.

**Precondition:** Customer authenticated; booking exists and belongs to customer.

**Trigger:** Customer selects Cancel Booking.

**Post-condition:** POS-01: Eligible booking is cancelled; reserved availability is released; refund status is recorded if applicable.

The flow must:

- Main step 1: Customer opens own Booking Detail.
- Main step 2: System displays booking status, policy, refund eligibility, and cancel action if allowed.
- Main step 3: Customer selects Cancel Booking and enters reason if required.
- Main step 4: System validates ownership, status, and policy.
- Main step 5: System cancels booking and releases reserved availability if applicable.
- Main step 6: System determines refund eligibility.
- Main step 7: System creates or updates RefundRecord if review is required.
- Main step 8: System sends or records cancellation notification.
- Main step 9: System displays cancellation result and refund status.
- Enforce related business rules: BR-BOOK-008, BR-REF-001, BR-REF-002, BR-REF-003, BR-FIN-002.
- Return a separate scenario response for each alternative/error flow: AT-UC007-04A, AT-UC007-06A, AT-UC007-06B.

## 3.7.2 Class Diagram

This part presents the class diagram for UC-007 Cancel Booking.

![DGM-CLS-UC-007 - Cancel Booking Class Diagram](./dgm-cls-uc-007-cancel-booking-class-diagram.png)

**Figure 3.7-1: Class Diagram of UC-007 Cancel Booking**

## 3.7.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### CustomerBookingDetailScreen Class

**Description:** Boundary object for the user-visible entry point of UC-007 Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or user-visible entry state described by the SRS. |
| 2 | `collectInput()` | Collects actor input before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### CancelBookingController Class

**Description:** API/application entry controller for UC-007 Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case within role or hotel scope. |

### CancelBookingRequest Class

**Description:** Request DTO carrying input for UC-007 Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or guest context needed for authorization. |

### CancelBookingService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `cancelbooking(request)` | Executes the UC-007 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### BookingRepository Class

**Description:** Repository abstraction for loading and saving data required by Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### RefundPolicyService Class

**Description:** Supporting service or integration used by UC-007 Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### CancelBookingResponse Class

**Description:** Response DTO returned by UC-007 Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### Booking Class

**Description:** Primary domain entity affected or displayed by UC-007 Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### RefundRecord Class

**Description:** Supporting domain entity affected or displayed by UC-007 Cancel Booking.

| No | Method | Description |
|---:|---|---|
| 1 | `isLinkedToUseCase()` | Determines whether the entity is related to the current use-case operation. |
| 2 | `updateStatus()` | Updates status or lifecycle information when the validated flow requires it. |
| 3 | `getAuditSummary()` | Provides auditable summary data for protected state changes. |

## 3.7.4 Sequence Diagram

This part presents the sequence diagrams for UC-007 Cancel Booking. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-007 - Cancel Booking Main Flow](./dgm-seq-uc-007-cancel-booking-main-flow.png)

**Figure 3.7-2: Sequence Diagram of UC-007 Cancel Booking - Main Flow**

### AT-UC007-04A - Cancellation not allowed

- **Branch from Main Step:** 4
- **Condition:** Cancellation not allowed
- **Expected Response:** This booking cannot be cancelled according to its current status or policy.

![DGM-SEQ-UC-007 - Cancellation not allowed](./dgm-seq-uc-007-cancel-booking-at-uc007-04a-cancellation-not-allowed.png)

**Figure 3.7-3: Sequence Diagram of UC-007 Cancel Booking - AT-UC007-04A Cancellation not allowed**

### AT-UC007-06A - Refund not required

- **Branch from Main Step:** 6
- **Condition:** Refund not required
- **Expected Response:** Refund is marked Not Required and cancellation continues.

![DGM-SEQ-UC-007 - Refund not required](./dgm-seq-uc-007-cancel-booking-at-uc007-06a-refund-not-required.png)

**Figure 3.7-4: Sequence Diagram of UC-007 Cancel Booking - AT-UC007-06A Refund not required**

### AT-UC007-06B - Refund review required

- **Branch from Main Step:** 6
- **Condition:** Refund review required
- **Expected Response:** Refund request is under review by the platform.

![DGM-SEQ-UC-007 - Refund review required](./dgm-seq-uc-007-cancel-booking-at-uc007-06b-refund-review-required.png)

**Figure 3.7-5: Sequence Diagram of UC-007 Cancel Booking - AT-UC007-06B Refund review required**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only the SRS actor scope for Customer; enforce role, ownership, hotel-scope, or platform-scope preconditions before protected data is displayed or changed. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. Read-only flows do not create domain records. |
| Error Handling | AT-UC007-04A returns "This booking cannot be cancelled according to its current status or policy."; AT-UC007-06A returns "Refund is marked Not Required and cancellation continues."; AT-UC007-06B returns "Refund request is under review by the platform.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC007-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC007-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC007-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

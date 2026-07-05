# 3.30 UC-030 - Record Pay-at-Property Payment

## 3.30.1 Design Purpose

This section describes the detailed design for **UC-030 Record Pay-at-Property Payment**. The use case covers Record amount collected directly at hotel for Pay at Property booking. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-FRONTDESK, UC-030, SCR-026, ENT-017, ENT-019, ENT-024, BR-PAY-004, BR-PAY-006, BR-PAY-007, BR-FIN-003, BR-STAFF-003, BR-AUDIT-001, MSG-PAY-005, MSG-PAY-006, MSG-PAY-007, MSG-PAY-008, TR-030, AT-UC030-05A, AT-UC030-02A, AT-UC030-05B, AT-UC030-05C.

**Precondition:** Actor authenticated; booking exists for a hotel the actor owns or is assigned to.

**Trigger:** Actor selects Record Payment Collection.

**Post-condition:** POS-01: Pay-at-Property collection is recorded and hotel-visible balance/receipt is updated.

The flow must:

- Main step 1: Actor opens booking detail or checkout screen.
- Main step 2: System validates booking access and payment mode before displaying expected amount, prior collections, and balance.
- Main step 3: System displays expected amount, prior collections, and remaining balance.
- Main step 4: Actor enters amount, method, date, note/reference.
- Main step 5: System validates amount, required collection fields, remaining balance, and duplicate/concurrent collection guard.
- Main step 6: System atomically records collection and updates payment/collection status and invoice balance.
- Main step 7: System records audit and displays success.
- Enforce related business rules: BR-PAY-004, BR-PAY-006, BR-PAY-007, BR-FIN-003, BR-STAFF-003, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC030-05A, AT-UC030-02A, AT-UC030-05B, AT-UC030-05C.

## 3.30.2 Class Diagram

This part presents the class diagram for UC-030 Record Pay-at-Property Payment.

![DGM-CLS-UC-030 - Record Pay-at-Property Payment Class Diagram](./dgm-cls-uc-030-record-property-payment-collection-class-diagram.png)

**Figure 3.30-1: Class Diagram of UC-030 Record Pay-at-Property Payment**

## 3.30.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### CheckOutPaymentCollectionScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-030 Record Pay-at-Property Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### RecordPayAtPropertyPaymentController Class

**Description:** API/application entry controller for UC-030 Record Pay-at-Property Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### RecordPayAtPropertyPaymentRequest Class

**Description:** Request DTO carrying input for UC-030 Record Pay-at-Property Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### RecordPayAtPropertyPaymentService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Record Pay-at-Property Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `recordPayAtPropertyPayment(request)` | Executes the UC-030 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### PaymentCollectionRecordRepository Class

**Description:** Repository abstraction for loading and saving data required by Record Pay-at-Property Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### PaymentCollectionPolicyService Class

**Description:** Supporting service or integration used by UC-030 Record Pay-at-Property Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### RecordPayAtPropertyPaymentResponse Class

**Description:** Response DTO returned by UC-030 Record Pay-at-Property Payment.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### PaymentCollectionRecord Class

**Description:** Hotel-side collection record for Pay at Property booking.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### Invoice Class

**Description:** Basic invoice/folio for booking and checkout.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.30.4 Sequence Diagram

This part presents the sequence diagrams for UC-030 Record Pay-at-Property Payment. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-030 - Record Pay-at-Property Payment Main Flow](./dgm-seq-uc-030-record-property-payment-collection-main-flow.png)

**Figure 3.30-2: Sequence Diagram of UC-030 Record Pay-at-Property Payment - Main Flow**

### AT-UC030-05A - Invalid amount

- **Branch from Main Step:** 5
- **Condition:** Invalid amount
- **Expected Response:** Please enter a valid collection amount.

![DGM-SEQ-UC-030 - Invalid amount](./dgm-seq-uc-030-record-property-payment-collection-at-uc030-05a-invalid-amount.png)

**Figure 3.30-3: Sequence Diagram of UC-030 Record Pay-at-Property Payment - AT-UC030-05A Invalid amount**

### AT-UC030-02A - Wrong payment mode

- **Branch from Main Step:** 2
- **Condition:** Wrong payment mode
- **Expected Response:** This action is allowed only for Pay at Property bookings.

![DGM-SEQ-UC-030 - Wrong payment mode](./dgm-seq-uc-030-record-property-payment-collection-at-uc030-02a-wrong-payment-mode.png)

**Figure 3.30-4: Sequence Diagram of UC-030 Record Pay-at-Property Payment - AT-UC030-02A Wrong payment mode**

### AT-UC030-05B - Amount exceeds expected

- **Branch from Main Step:** 5
- **Condition:** Amount exceeds expected
- **Expected Response:** The collection amount cannot exceed the expected balance unless exception handling is allowed.

![DGM-SEQ-UC-030 - Amount exceeds expected](./dgm-seq-uc-030-record-property-payment-collection-at-uc030-05b-amount-exceeds-expected.png)

**Figure 3.30-5: Sequence Diagram of UC-030 Record Pay-at-Property Payment - AT-UC030-05B Amount exceeds expected**

### AT-UC030-05C - Duplicate or concurrent collection

- **Branch from Main Step:** 5
- **Condition:** Duplicate or concurrent collection
- **Expected Response:** Please enter a valid collection amount.

![DGM-SEQ-UC-030 - Duplicate or concurrent collection](./dgm-seq-uc-030-record-property-payment-collection-at-uc030-05c-duplicate-or-concurrent-collection.png)

**Figure 3.30-6: Sequence Diagram of UC-030 Record Pay-at-Property Payment - AT-UC030-05C Duplicate or concurrent collection**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Receptionist, Hotel Manager, Property Owner according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC030-05A returns "Please enter a valid collection amount."; AT-UC030-02A returns "This action is allowed only for Pay at Property bookings."; AT-UC030-05B returns "The collection amount cannot exceed the expected balance unless exception handling is allowed."; AT-UC030-05C returns "Please enter a valid collection amount.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC030-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC030-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC030-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

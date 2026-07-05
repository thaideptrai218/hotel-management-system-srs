# 3.3 UC-003 - Register Account

## 3.3.1 Design Purpose

This section describes the detailed design for **UC-003 Register Account**. The use case covers register a Customer or Property Owner account. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-AUTH, UC-003, SCR-001, ENT-001, ENT-002, BR-AUTH-001, BR-AUTH-002, BR-AUTH-003, MSG-AUTH-003, MSG-AUTH-004, MSG-AUTH-005, TR-003, AT-UC003-04A, AT-UC003-04B.

**Precondition:** Guest is not authenticated.

**Trigger:** Guest selects Register.

**Post-condition:** POS-01: A Customer or Property Owner account is created after valid registration, or registration is rejected with a clear reason.

The flow must:

- Main step 1: Guest opens Register Screen.
- Main step 2: System displays account type, full name, email, phone, password, confirmation, and terms fields.
- Main step 3: Guest enters required information.
- Main step 4: System validates mandatory fields, format, password confirmation, and uniqueness.
- Main step 5: System creates account with selected role.
- Main step 6: System sends or records registration notification.
- Main step 7: System displays registration success message.
- Enforce related business rules: BR-AUTH-001, BR-AUTH-002, BR-AUTH-003.
- Return a separate scenario response for each alternative/error flow: AT-UC003-04A, AT-UC003-04B.

## 3.3.2 Class Diagram

This part presents the class diagram for UC-003 Register Account.

![DGM-CLS-UC-003 - Register Account Class Diagram](./dgm-cls-uc-003-register-account-class-diagram.png)

**Figure 3.3-1: Class Diagram of UC-003 Register Account**

## 3.3.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### RegisterScreen Class

**Description:** Boundary object for the user-visible entry point of UC-003 Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or user-visible entry state described by the SRS. |
| 2 | `collectInput()` | Collects actor input before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### RegistrationController Class

**Description:** API/application entry controller for UC-003 Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case within role or hotel scope. |

### RegisterAccountRequest Class

**Description:** Request DTO carrying input for UC-003 Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or guest context needed for authorization. |

### RegistrationService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `registeraccount(request)` | Executes the UC-003 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### UserAccountRepository Class

**Description:** Repository abstraction for loading and saving data required by Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### PasswordPolicyService Class

**Description:** Supporting service or integration used by UC-003 Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### RegisterAccountResponse Class

**Description:** Response DTO returned by UC-003 Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### UserAccount Class

**Description:** Primary domain entity affected or displayed by UC-003 Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### UserRole Class

**Description:** Supporting domain entity affected or displayed by UC-003 Register Account.

| No | Method | Description |
|---:|---|---|
| 1 | `isLinkedToUseCase()` | Determines whether the entity is related to the current use-case operation. |
| 2 | `updateStatus()` | Updates status or lifecycle information when the validated flow requires it. |
| 3 | `getAuditSummary()` | Provides auditable summary data for protected state changes. |

## 3.3.4 Sequence Diagram

This part presents the sequence diagrams for UC-003 Register Account. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-003 - Register Account Main Flow](./dgm-seq-uc-003-register-account-main-flow.png)

**Figure 3.3-2: Sequence Diagram of UC-003 Register Account - Main Flow**

### AT-UC003-04A - Duplicate email phone

- **Branch from Main Step:** 4
- **Condition:** Duplicate email/phone
- **Expected Response:** Email or phone number is already in use.

![DGM-SEQ-UC-003 - Duplicate email phone](./dgm-seq-uc-003-register-account-at-uc003-04a-duplicate-email-phone.png)

**Figure 3.3-3: Sequence Diagram of UC-003 Register Account - AT-UC003-04A Duplicate email phone**

### AT-UC003-04B - Invalid data

- **Branch from Main Step:** 4
- **Condition:** Invalid data
- **Expected Response:** Please complete all required account information.

![DGM-SEQ-UC-003 - Invalid data](./dgm-seq-uc-003-register-account-at-uc003-04b-invalid-data.png)

**Figure 3.3-4: Sequence Diagram of UC-003 Register Account - AT-UC003-04B Invalid data**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only the SRS actor scope for Guest; enforce role, ownership, hotel-scope, or platform-scope preconditions before protected data is displayed or changed. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. Read-only flows do not create domain records. |
| Error Handling | AT-UC003-04A returns "Email or phone number is already in use."; AT-UC003-04B returns "Please complete all required account information.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC003-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC003-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC003-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

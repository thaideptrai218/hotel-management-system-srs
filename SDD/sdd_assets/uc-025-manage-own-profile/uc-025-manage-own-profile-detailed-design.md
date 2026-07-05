# 3.25 UC-025 - Manage Own Profile

## 3.25.1 Design Purpose

This section describes the detailed design for **UC-025 Manage Own Profile**. The use case covers View and update own basic profile where allowed. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-AUTH, UC-025, SCR-003, ENT-001, BR-AUTH-004, BR-STAFF-002, MSG-AUTH-003, MSG-AUTH-007, MSG-AUTH-009, TR-025, AT-UC025-04A, AT-UC025-01A.

**Precondition:** Actor authenticated.

**Trigger:** Actor opens User Profile.

**Post-condition:** POS-01: Actor own profile is updated after validation, or rejected with a clear reason.

The flow must:

- Main step 1: Actor opens User Profile Screen.
- Main step 2: System validates own-profile access and displays own profile fields with read-only role/assignment information.
- Main step 3: Actor updates editable profile information.
- Main step 4: System validates formats and uniqueness if changed.
- Main step 5: System records updated profile information.
- Main step 6: System displays update success.
- Enforce related business rules: BR-AUTH-004, BR-STAFF-002.
- Return a separate scenario response for each alternative/error flow: AT-UC025-04A, AT-UC025-01A.

## 3.25.2 Class Diagram

This part presents the class diagram for UC-025 Manage Own Profile.

![DGM-CLS-UC-025 - Manage Own Profile Class Diagram](./dgm-cls-uc-025-manage-own-profile-class-diagram.png)

**Figure 3.25-1: Class Diagram of UC-025 Manage Own Profile**

## 3.25.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### UserProfileScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-025 Manage Own Profile.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ManageOwnProfileController Class

**Description:** API/application entry controller for UC-025 Manage Own Profile.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ManageOwnProfileRequest Class

**Description:** Request DTO carrying input for UC-025 Manage Own Profile.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ManageOwnProfileService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Manage Own Profile.

| No | Method | Description |
|---:|---|---|
| 1 | `manageOwnProfile(request)` | Executes the UC-025 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### UserAccountRepository Class

**Description:** Repository abstraction for loading and saving data required by Manage Own Profile.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### ProfileAuthorizationService Class

**Description:** Supporting service or integration used by UC-025 Manage Own Profile.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ManageOwnProfileResponse Class

**Description:** Response DTO returned by UC-025 Manage Own Profile.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### UserAccount Class

**Description:** Registered account for Customer, Property Owner, Hotel Manager, Receptionist, Housekeeping Staff, Maintenance Staff, or Platform Administrator.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.25.4 Sequence Diagram

This part presents the sequence diagrams for UC-025 Manage Own Profile. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-025 - Manage Own Profile Main Flow](./dgm-seq-uc-025-manage-own-profile-main-flow.png)

**Figure 3.25-2: Sequence Diagram of UC-025 Manage Own Profile - Main Flow**

### AT-UC025-04A - Duplicate email/phone

- **Branch from Main Step:** 4
- **Condition:** Duplicate email/phone
- **Expected Response:** Email or phone number is already in use.

![DGM-SEQ-UC-025 - Duplicate email/phone](./dgm-seq-uc-025-manage-own-profile-at-uc025-04a-duplicate-email-phone.png)

**Figure 3.25-3: Sequence Diagram of UC-025 Manage Own Profile - AT-UC025-04A Duplicate email/phone**

### AT-UC025-01A - Unauthorized profile access

- **Branch from Main Step:** 1
- **Condition:** Unauthorized profile access
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-025 - Unauthorized profile access](./dgm-seq-uc-025-manage-own-profile-at-uc025-01a-unauthorized-profile-access.png)

**Figure 3.25-4: Sequence Diagram of UC-025 Manage Own Profile - AT-UC025-01A Unauthorized profile access**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Customer, Property Owner, Hotel Manager, Receptionist, Housekeeping Staff, Maintenance Staff, Platform Administrator according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC025-04A returns "Email or phone number is already in use."; AT-UC025-01A returns "You are not authorized to perform this action.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC025-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC025-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC025-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

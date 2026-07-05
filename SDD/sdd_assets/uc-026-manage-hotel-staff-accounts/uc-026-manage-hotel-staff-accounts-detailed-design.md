# 3.26 UC-026 - Manage Hotel Staff Accounts

## 3.26.1 Design Purpose

This section describes the detailed design for **UC-026 Manage Hotel Staff Accounts**. The use case covers Invite, create, update, deactivate, and view staff accounts for assigned hotels. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-STAFF, UC-026, SCR-028, ENT-001, ENT-003, ENT-004, ENT-024, BR-STAFF-001, BR-STAFF-002, BR-AUTH-002, BR-AUDIT-001, MSG-STAFF-001, MSG-STAFF-004, MSG-AUTH-003, MSG-AUTH-007, TR-026, AT-UC026-05A, AT-UC026-02A, AT-UC026-05B.

**Precondition:** Actor authenticated; staff management authority can be validated for selected hotel scope.

**Trigger:** Actor opens Staff Management.

**Post-condition:** POS-01: Staff account and hotel assignment are created, updated, invited, or deactivated according to permission.

The flow must:

- Main step 1: Actor opens Staff Management for selected hotel scope.
- Main step 2: System validates actor staff-management authority for selected hotel scope before displaying staff data.
- Main step 3: System displays staff list, roles, assignment status, and actions allowed by actor authority.
- Main step 4: Actor creates, invites, updates, or deactivates staff account.
- Main step 5: System validates staff data, duplicate email/phone, hotel permission, role availability, and manager authority limits.
- Main step 6: System creates/updates staff account and assignment.
- Main step 7: System records audit and sends/records notification.
- Continue through the remaining SRS main-flow steps until the UC-026 post-condition is reached.
- Enforce related business rules: BR-STAFF-001, BR-STAFF-002, BR-AUTH-002, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC026-05A, AT-UC026-02A, AT-UC026-05B.

## 3.26.2 Class Diagram

This part presents the class diagram for UC-026 Manage Hotel Staff Accounts.

![DGM-CLS-UC-026 - Manage Hotel Staff Accounts Class Diagram](./dgm-cls-uc-026-manage-hotel-staff-accounts-class-diagram.png)

**Figure 3.26-1: Class Diagram of UC-026 Manage Hotel Staff Accounts**

## 3.26.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### StaffManagementScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-026 Manage Hotel Staff Accounts.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ManageHotelStaffAccountsController Class

**Description:** API/application entry controller for UC-026 Manage Hotel Staff Accounts.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ManageHotelStaffAccountsRequest Class

**Description:** Request DTO carrying input for UC-026 Manage Hotel Staff Accounts.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ManageHotelStaffAccountsService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Manage Hotel Staff Accounts.

| No | Method | Description |
|---:|---|---|
| 1 | `manageHotelStaffAccounts(request)` | Executes the UC-026 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### UserAccountRepository Class

**Description:** Repository abstraction for loading and saving data required by Manage Hotel Staff Accounts.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### NotificationService Class

**Description:** Supporting service or integration used by UC-026 Manage Hotel Staff Accounts.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ManageHotelStaffAccountsResponse Class

**Description:** Response DTO returned by UC-026 Manage Hotel Staff Accounts.

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

### HotelStaffAssignment Class

**Description:** Mapping between a staff user, hotel, and hotel-scoped role.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.26.4 Sequence Diagram

This part presents the sequence diagrams for UC-026 Manage Hotel Staff Accounts. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-026 - Manage Hotel Staff Accounts Main Flow](./dgm-seq-uc-026-manage-hotel-staff-accounts-main-flow.png)

**Figure 3.26-2: Sequence Diagram of UC-026 Manage Hotel Staff Accounts - Main Flow**

### AT-UC026-05A - Duplicate staff email/phone

- **Branch from Main Step:** 5
- **Condition:** Duplicate staff email/phone
- **Expected Response:** Email or phone number is already in use.

![DGM-SEQ-UC-026 - Duplicate staff email/phone](./dgm-seq-uc-026-manage-hotel-staff-accounts-at-uc026-05a-duplicate-staff-email-phone.png)

**Figure 3.26-3: Sequence Diagram of UC-026 Manage Hotel Staff Accounts - AT-UC026-05A Duplicate staff email/phone**

### AT-UC026-02A - No permission

- **Branch from Main Step:** 2
- **Condition:** No permission
- **Expected Response:** You are not authorized to perform this action.

![DGM-SEQ-UC-026 - No permission](./dgm-seq-uc-026-manage-hotel-staff-accounts-at-uc026-02a-no-permission.png)

**Figure 3.26-4: Sequence Diagram of UC-026 Manage Hotel Staff Accounts - AT-UC026-02A No permission**

### AT-UC026-05B - Staff has open tasks

- **Branch from Main Step:** 5
- **Condition:** Staff has open tasks
- **Expected Response:** Please reassign or resolve open tasks before deactivating this staff account.

![DGM-SEQ-UC-026 - Staff has open tasks](./dgm-seq-uc-026-manage-hotel-staff-accounts-at-uc026-05b-staff-has-open-tasks.png)

**Figure 3.26-5: Sequence Diagram of UC-026 Manage Hotel Staff Accounts - AT-UC026-05B Staff has open tasks**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Property Owner, Hotel Manager according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC026-05A returns "Email or phone number is already in use."; AT-UC026-02A returns "You are not authorized to perform this action."; AT-UC026-05B returns "Please reassign or resolve open tasks before deactivating this staff account.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC026-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC026-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC026-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

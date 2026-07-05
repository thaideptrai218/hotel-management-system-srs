# 3.27 UC-027 - Assign Staff Roles and Permissions

## 3.27.1 Design Purpose

This section describes the detailed design for **UC-027 Assign Staff Roles and Permissions**. The use case covers Assign hotel-scoped staff roles and permissions. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-STAFF, UC-027, SCR-029, ENT-002, ENT-003, ENT-024, BR-STAFF-001, BR-STAFF-002, BR-AUTH-002, BR-AUDIT-001, MSG-STAFF-002, MSG-STAFF-003, MSG-STAFF-005, TR-027, AT-UC027-05A, AT-UC027-05B.

**Precondition:** Actor authenticated; staff account exists; actor has role management permission.

**Trigger:** Actor opens Staff Role Assignment.

**Post-condition:** POS-01: Hotel-scoped staff role and permission assignment is updated and audited.

The flow must:

- Main step 1: Actor selects staff member.
- Main step 2: System validates actor authority over the selected staff member and hotel scope before displaying assignments.
- Main step 3: System displays current hotel assignments, role permissions, and only role options allowed by actor authority.
- Main step 4: Actor selects/changes staff role and hotel scope.
- Main step 5: System validates role, hotel assignment, and actor authority.
- Main step 6: System updates hotel-scoped staff role assignment.
- Main step 7: System records audit and displays success.
- Enforce related business rules: BR-STAFF-001, BR-STAFF-002, BR-AUTH-002, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC027-05A, AT-UC027-05B.

## 3.27.2 Class Diagram

This part presents the class diagram for UC-027 Assign Staff Roles and Permissions.

![DGM-CLS-UC-027 - Assign Staff Roles and Permissions Class Diagram](./dgm-cls-uc-027-assign-staff-roles-and-permissions-class-diagram.png)

**Figure 3.27-1: Class Diagram of UC-027 Assign Staff Roles and Permissions**

## 3.27.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### StaffRoleAssignmentScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-027 Assign Staff Roles and Permissions.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### AssignStaffRolesAndPermissionsController Class

**Description:** API/application entry controller for UC-027 Assign Staff Roles and Permissions.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### AssignStaffRolesAndPermissionsRequest Class

**Description:** Request DTO carrying input for UC-027 Assign Staff Roles and Permissions.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### AssignStaffRolesAndPermissionsService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Assign Staff Roles and Permissions.

| No | Method | Description |
|---:|---|---|
| 1 | `assignStaffRolesAndPermissions(request)` | Executes the UC-027 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### UserRoleRepository Class

**Description:** Repository abstraction for loading and saving data required by Assign Staff Roles and Permissions.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### StaffAuthorizationService Class

**Description:** Supporting service or integration used by UC-027 Assign Staff Roles and Permissions.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### AssignStaffRolesAndPermissionsResponse Class

**Description:** Response DTO returned by UC-027 Assign Staff Roles and Permissions.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### UserRole Class

**Description:** Role definition for platform or hotel-scoped access.

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

## 3.27.4 Sequence Diagram

This part presents the sequence diagrams for UC-027 Assign Staff Roles and Permissions. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-027 - Assign Staff Roles and Permissions Main Flow](./dgm-seq-uc-027-assign-staff-roles-and-permissions-main-flow.png)

**Figure 3.27-2: Sequence Diagram of UC-027 Assign Staff Roles and Permissions - Main Flow**

### AT-UC027-05A - Invalid role

- **Branch from Main Step:** 5
- **Condition:** Invalid role
- **Expected Response:** Please select a valid staff role.

![DGM-SEQ-UC-027 - Invalid role](./dgm-seq-uc-027-assign-staff-roles-and-permissions-at-uc027-05a-invalid-role.png)

**Figure 3.27-3: Sequence Diagram of UC-027 Assign Staff Roles and Permissions - AT-UC027-05A Invalid role**

### AT-UC027-05B - Staff not assigned to hotel

- **Branch from Main Step:** 5
- **Condition:** Staff not assigned to hotel
- **Expected Response:** Staff must be assigned to at least one hotel before accessing staff functions.

![DGM-SEQ-UC-027 - Staff not assigned to hotel](./dgm-seq-uc-027-assign-staff-roles-and-permissions-at-uc027-05b-staff-not-assigned-to-hotel.png)

**Figure 3.27-4: Sequence Diagram of UC-027 Assign Staff Roles and Permissions - AT-UC027-05B Staff not assigned to hotel**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Property Owner, Hotel Manager according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC027-05A returns "Please select a valid staff role."; AT-UC027-05B returns "Staff must be assigned to at least one hotel before accessing staff functions.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC027-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC027-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC027-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

# 3.18 UC-018 - Approve Hotel Property

## 3.18.1 Design Purpose

This section describes the detailed design for **UC-018 Approve Hotel Property**. The use case covers Approve or reject submitted hotel properties. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-ADMIN-APPROVAL, UC-018, SCR-037, ENT-005, ENT-024, BR-MKT-001, BR-ADMIN-001, BR-AUDIT-001, MSG-ADMIN-001, MSG-ADMIN-003, MSG-ADMIN-004, TR-018, AT-UC018-06A, AT-UC018-06B.

**Precondition:** Platform Administrator authenticated; hotel submission exists.

**Trigger:** Admin opens Hotel Approval.

**Post-condition:** POS-01: Hotel approval status is updated and marketplace visibility follows the new approval state.

The flow must:

- Main step 1: Platform Administrator admin opens Hotel Approval Screen.
- Main step 2: System displays pending hotel submissions.
- Main step 3: Platform Administrator admin selects a hotel.
- Main step 4: System displays submitted hotel data, images, amenities, policies, owner info, and review notes.
- Main step 5: Platform Administrator admin approves/rejects and enters reason if required.
- Main step 6: System validates decision.
- Main step 7: System updates status and records audit.
- Continue through the remaining SRS main-flow steps until the UC-018 post-condition is reached.
- Enforce related business rules: BR-MKT-001, BR-ADMIN-001, BR-AUDIT-001.
- Return a separate scenario response for each alternative/error flow: AT-UC018-06A, AT-UC018-06B.

## 3.18.2 Class Diagram

This part presents the class diagram for UC-018 Approve Hotel Property.

![DGM-CLS-UC-018 - Approve Hotel Property Class Diagram](./dgm-cls-uc-018-approve-hotel-property-class-diagram.png)

**Figure 3.18-1: Class Diagram of UC-018 Approve Hotel Property**

## 3.18.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### HotelApprovalScreen Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-018 Approve Hotel Property.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ApproveHotelPropertyController Class

**Description:** API/application entry controller for UC-018 Approve Hotel Property.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ApproveHotelPropertyRequest Class

**Description:** Request DTO carrying input for UC-018 Approve Hotel Property.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ApproveHotelPropertyService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for Approve Hotel Property.

| No | Method | Description |
|---:|---|---|
| 1 | `approveHotelProperty(request)` | Executes the UC-018 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### HotelPropertyRepository Class

**Description:** Repository abstraction for loading and saving data required by Approve Hotel Property.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### NotificationService Class

**Description:** Supporting service or integration used by UC-018 Approve Hotel Property.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ApproveHotelPropertyResponse Class

**Description:** Response DTO returned by UC-018 Approve Hotel Property.

| No | Method | Description |
|---:|---|---|
| 1 | `includeSummary()` | Adds the display or operation summary needed by the screen. |
| 2 | `includeUserMessage()` | Adds the user-facing success, empty-state, or validation message. |
| 3 | `includeNextAction()` | Adds the next available action when the SRS flow continues or returns for correction. |

### HotelProperty Class

**Description:** Hotel listed and managed on the platform.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

### AuditRecord Class

**Description:** Administrative, financial, staff, booking, room, housekeeping, and maintenance action audit record.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.18.4 Sequence Diagram

This part presents the sequence diagrams for UC-018 Approve Hotel Property. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-018 - Approve Hotel Property Main Flow](./dgm-seq-uc-018-approve-hotel-property-main-flow.png)

**Figure 3.18-2: Sequence Diagram of UC-018 Approve Hotel Property - Main Flow**

### AT-UC018-06A - Missing rejection reason

- **Branch from Main Step:** 6
- **Condition:** Missing rejection reason
- **Expected Response:** Please enter a rejection reason.

![DGM-SEQ-UC-018 - Missing rejection reason](./dgm-seq-uc-018-approve-hotel-property-at-uc018-06a-missing-rejection-reason.png)

**Figure 3.18-3: Sequence Diagram of UC-018 Approve Hotel Property - AT-UC018-06A Missing rejection reason**

### AT-UC018-06B - Already reviewed

- **Branch from Main Step:** 6
- **Condition:** Already reviewed
- **Expected Response:** This hotel submission has already been reviewed. Please refresh the page.

![DGM-SEQ-UC-018 - Already reviewed](./dgm-seq-uc-018-approve-hotel-property-at-uc018-06b-already-reviewed.png)

**Figure 3.18-4: Sequence Diagram of UC-018 Approve Hotel Property - AT-UC018-06B Already reviewed**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Platform Administrator according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Use a single application transaction for validated state changes, persistence updates, audit records, and notification records where applicable. |
| Error Handling | AT-UC018-06A returns "Please enter a rejection reason."; AT-UC018-06B returns "This hotel submission has already been reviewed. Please refresh the page.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC018-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC018-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC018-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

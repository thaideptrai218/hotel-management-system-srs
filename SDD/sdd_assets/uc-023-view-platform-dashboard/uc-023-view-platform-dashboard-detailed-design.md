# 3.23 UC-023 - View Platform Dashboard

## 3.23.1 Design Purpose

This section describes the detailed design for **UC-023 View Platform Dashboard**. The use case covers View platform booking, revenue, commission, payment, refund, and settlement metrics. The design is based on the SRS/SDD only; class names and methods are conceptual design assumptions because no implementation codebase was inspected.

**Related SRS items:** FEAT-ADMIN-REPORT, UC-023, SCR-036, NSF-007, ENT-013, ENT-016, ENT-020, ENT-022, BR-ADMIN-004, MSG-RPT-001, TR-023, AT-UC023-04A.

**Precondition:** Platform Administrator authenticated.

**Trigger:** Admin opens Dashboard.

**Post-condition:** POS-01: Platform dashboard metrics are displayed for the selected filters.

The flow must:

- Main step 1: Platform Administrator admin opens dashboard.
- Main step 2: System displays date/hotel filters and metrics for bookings, revenue, commission, refund, settlement, and hotel approval.
- Main step 3: Platform Administrator admin applies filters.
- Main step 4: System refreshes metrics.
- Main step 5: Platform Administrator admin navigates to details if needed.
- Enforce related business rules: BR-ADMIN-004.
- Return a separate scenario response for each alternative/error flow: AT-UC023-04A.

## 3.23.2 Class Diagram

This part presents the class diagram for UC-023 View Platform Dashboard.

![DGM-CLS-UC-023 - View Platform Dashboard Class Diagram](./dgm-cls-uc-023-view-platform-dashboard-class-diagram.png)

**Figure 3.23-1: Class Diagram of UC-023 View Platform Dashboard**

## 3.23.3 Class Specifications

This part explains the key methods shown in the class diagram. The classes are conceptual design assumptions unless source code is inspected.

### AdminDashboard Class

**Description:** Boundary object for the user-visible or scheduled entry point of UC-023 View Platform Dashboard.

| No | Method | Description |
|---:|---|---|
| 1 | `openOrDisplay()` | Displays the use-case screen or activates the scheduled entry point described by the SRS. |
| 2 | `collectInput()` | Collects actor input or scheduler criteria before request submission. |
| 3 | `renderResult(response)` | Displays the result, validation message, or next action to the actor. |

### ViewPlatformDashboardController Class

**Description:** API/application entry controller for UC-023 View Platform Dashboard.

| No | Method | Description |
|---:|---|---|
| 1 | `handleRequest(request)` | Receives the request from the boundary and delegates the business operation to the service. |
| 2 | `validateRequest(request)` | Checks required request shape before business rule execution. |
| 3 | `authorizeActor(actorContext)` | Verifies that the current actor may execute this use case and related hotel/platform scope. |

### ViewPlatformDashboardRequest Class

**Description:** Request DTO carrying input for UC-023 View Platform Dashboard.

| No | Method | Description |
|---:|---|---|
| 1 | `hasRequiredFields()` | Returns whether mandatory fields from the SRS screen/use-case step are present. |
| 2 | `normalizeInput()` | Normalizes filter, status, note, amount, date, or reference input before service validation. |
| 3 | `containsActorContext()` | Confirms the request carries the authenticated actor or scheduler context needed for authorization. |

### ViewPlatformDashboardService Class

**Description:** Application service that coordinates the main flow, business rules, persistence, and response creation for View Platform Dashboard.

| No | Method | Description |
|---:|---|---|
| 1 | `viewPlatformDashboard(request)` | Executes the UC-023 main flow and returns a response for the boundary. |
| 2 | `applyBusinessRules(request)` | Applies the related SRS business rules and state-transition constraints. |
| 3 | `buildResponse(result)` | Builds success, empty-state, or validation responses without exposing unauthorized data. |

### DashboardMetricRepository Class

**Description:** Repository abstraction for loading and saving data required by View Platform Dashboard.

| No | Method | Description |
|---:|---|---|
| 1 | `findForUseCase(criteria)` | Loads the entity state required for validation and display. |
| 2 | `findById(id)` | Retrieves a specific record within actor, hotel, or platform scope. |
| 3 | `saveChanges(entity)` | Persists allowed state changes when the use case modifies data. |

### DashboardMetricService Class

**Description:** Supporting service or integration used by UC-023 View Platform Dashboard.

| No | Method | Description |
|---:|---|---|
| 1 | `verifyRuleContext(entity)` | Checks specialized policy, authorization, calculation, notification, or external status context. |
| 2 | `performSupportingAction(entity)` | Performs notification, calculation, audit, or external reconciliation support when required. |
| 3 | `returnResult()` | Returns the supporting result to the application service for final response composition. |

### ViewPlatformDashboardResponse Class

**Description:** Response DTO returned by UC-023 View Platform Dashboard.

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

### PaymentTransaction Class

**Description:** Online payment transaction for Platform Collect booking.

| No | Method | Description |
|---:|---|---|
| 1 | `isInAllowedState()` | Determines whether the entity state allows the requested use-case operation. |
| 2 | `applyUseCaseChange()` | Applies the state or data change permitted by the validated flow. |
| 3 | `getDisplaySummary()` | Provides safe summary data for the response or audit record. |

## 3.23.4 Sequence Diagram

This part presents the sequence diagrams for UC-023 View Platform Dashboard. The main-flow diagram shows only the successful scenario. Each alternative/error scenario has its own diagram.

![DGM-SEQ-UC-023 - View Platform Dashboard Main Flow](./dgm-seq-uc-023-view-platform-dashboard-main-flow.png)

**Figure 3.23-2: Sequence Diagram of UC-023 View Platform Dashboard - Main Flow**

### AT-UC023-04A - No data

- **Branch from Main Step:** 4
- **Condition:** No data
- **Expected Response:** No data is available for the selected filters.

![DGM-SEQ-UC-023 - No data](./dgm-seq-uc-023-view-platform-dashboard-at-uc023-04a-no-data.png)

**Figure 3.23-3: Sequence Diagram of UC-023 View Platform Dashboard - AT-UC023-04A No data**

### Validation, Authorization, Transaction, and Error Handling Notes

| Area | Design |
|---|---|
| Validation | Validate required input, current entity status, date/amount/reference values, and SRS business rules before any state change. |
| Authorization | Allow only Platform Administrator according to the SRS actor, role, hotel-scope, or platform-scope precondition. |
| Transaction | Read-only query transaction; no persistent state is changed in the successful display flow. |
| Error Handling | AT-UC023-04A returns "No data is available for the selected filters.". |
| Privacy | Return only fields allowed for the current role and scope; staff roles must not receive unrelated customer, platform finance, or cross-hotel data. |

## Assumptions and Open Issues

- ASSUMP-UC023-001: Controller, service, repository, DTO, and entity class names are conceptual SDD design names because no source implementation was inspected.
- ASSUMP-UC023-002: Final API routes, database column names, and UI widget names may differ from these SDD class names but must preserve the traced SRS behavior.
- OQ-UC023-001: Confirm final implementation class/package names before treating the conceptual design as code-level documentation.

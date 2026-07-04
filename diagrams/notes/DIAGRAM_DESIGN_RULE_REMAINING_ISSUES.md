# Diagram Design Rule Remaining Issues - Verified Update

Scope: SRS diagram image/design-rule audit after re-checking against SWD392/COMET modeling guidance and the project-specific diagram rules for business state, activity, screen-flow, logical ERD, use case, and context diagrams. The shared draw.io foundation rule was not used for this verification pass.

Legend:

- Fixed: verified as a real rule violation and corrected in `.drawio` plus exported `.png`.
- Advisory: valid readability/design concern, but not a hard UML/SRS semantic error.
- Deferred: real improvement, but requires a broader design decision or larger re-layout than a local correction.
- Not an issue: the original report was too strict or no longer applies.

## Fixed in This Pass

| Diagram | Verification result | Fix applied |
|---|---|---|
| `FIG-SRS-007_business_state_finance_lifecycle` | Fixed. All 27 transitions were unlabeled, which violates the statechart rule that transitions should show the event/condition/action. | Added concise transition labels directly on edges. |
| `FIG-SRS-007_business_state_physical_room` | Fixed. All 20 transitions were unlabeled. | Added concise transition labels for activation, assignment, check-in/out, cleaning, inspection, maintenance, inactive, and final retirement flows. |
| `FIG-SRS-007_business_state_booking` | Fixed. Four terminal transitions to final state were unlabeled. | Added final transition labels: close expired/cancelled/no-show/completed booking. Existing user layout changes were preserved. |
| `FIG-SRS-006_activity_uc_007_cancel_booking` | Fixed. Edges `e11` and `e13` were genuinely disconnected in XML. | Connected `Create refund record for admin review -> merge_refund` and `merge_refund -> Record cancellation notification event`. |
| Activity diagrams with swimlane divider lines | Fixed as XML-QA issue. Lane dividers were decorative edges without semantic source/target. | Converted activity lane dividers to decorative rectangle vertices so activity control-flow edges all have valid source and target. |
| `FIG-SRS-003_screen_flow_housekeeping_operation` | Fixed. `Report Room Issue action` was an action node, not a screen/modal/tab. | Removed the pseudo-screen node and represented `Submit room issue` as an arrow label from `SCR-032` to `SCR-035`. |
| `FIG-SRS-003_screen_flow_maintenance_operation` | Fixed. `Update Maintenance Status action` and `Release Room action` were action nodes, not screens/modals/tabs. | Removed pseudo-screen nodes and represented `Update status` and `Release room` as navigation/action labels from `SCR-034` to `SCR-035`. |
| `FIG-SRS-003_screen_flow_customer_search_booking_payment` | Fixed. Some labels described conditions/results rather than user navigation. | Renamed labels to user actions such as `Select Platform Collect`, `View Pay-at-Property confirmation`, and `Submit cancellation`. |
| `FIG-SRS-003_screen_flow_front_desk_operation` | Fixed. Some labels described system outcomes rather than navigation. | Renamed room-board transitions to `Open room status board`. |
| `FIG-SRS-003_screen_flow_platform_administration` | Fixed. Some finance flow labels were workflow terms rather than navigation labels. | Renamed to `Open refund management` and `Open settlement management`. |
| `FIG-SRS-005F_logical_erd_operations_audit` | Fixed. Dashed notification relationships were unlabeled. | Added optional event-reference labels/cardinalities for housekeeping and maintenance notification relationships. |
| Logical ERD module views `FIG-SRS-005B` to `FIG-SRS-005F` | Fixed. Entity boxes did not show the `<<entity>>` stereotype expected by the logical ERD rule. | Added rendered `<<entity>>` stereotypes to module-view entity boxes. |

## Advisory or Deferred

| Diagram | Verification result | Current decision |
|---|---|---|
| `FIG-SRS-007_business_state_finance_lifecycle` packed lifecycle concern | Advisory/deferred. The concern is valid for readability because payment, refund, commission, settlement, and collection are separate lifecycles. However, the current diagram already separates them into visible subpanels, and splitting it into multiple figures would require document-structure and figure-numbering changes. | Kept combined figure, fixed the hard transition-label violation. Split later only if the reviewer requires one lifecycle per figure. |
| Use case diagrams: `account_marketplace`, `front_desk_operation`, `housekeeping_maintenance`, `platform_administration` | Advisory. Crossing association lines reduce readability, but the diagrams remain valid UML use case diagrams and each module stays within the expected use-case count. | No automatic re-layout in this pass. Re-layout should be done as a visual polish pass if required. |
| Logical ERD module views cardinality placement | Advisory. Cardinality content is present in compact labels such as `1 : *`; the stricter endpoint-placement style would improve readability but is not a missing-cardinality semantic error. | Not changed in this pass beyond adding missing dashed relationship labels. |
| Screen flow diagrams back/cancel paths | Advisory. Back/cancel arrows are recommended for major forms, but absence is not always a rule violation unless the SRS requires those navigation paths. | Not added without explicit screen behavior evidence. |
| Activity diagrams long labels | Advisory. Some labels are long but still represent observable SRS behavior. | Not shortened globally to avoid changing use-case semantics; only layout-breaking issues were fixed. |

## Not Counted As Remaining Issue

- ERD split rule: The ERD is already split into `FIG-SRS-005A` to `FIG-SRS-005F`; therefore the large canonical ERD is treated as a reference/canonical view, not as the primary readability view.
- Context diagram broad actor labels: The earlier true ambiguity for opposite-direction flows was fixed by separating search result, payment, and notification request/status/result flows. Remaining broad role interaction labels are high-level context summaries, not definite UML/SWD392 errors unless a stricter DFD decomposition is requested.

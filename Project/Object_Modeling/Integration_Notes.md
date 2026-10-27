# Class Model Integration Notes

Diagram: [Consolidated Class Diagram](Class_Diagram/Consolidated_Class_Diagram.png) | [Class Inventory](Class_Inventory.md) | [Consistency Check](Class_Model_Consistency_Check.md)

## Inputs Reviewed

| Model | Owner | Files |
|---|---|---|
| Accounts, Roles, Notifications & Dashboard | Ahmed Abdelmohsen | Ahmed class inventory and UML diagram |
| Budget Requests | Abdelrahman Ahmed Elsayed | [Abdelrahman_Class_Inventory.md](Individual_Models/Abdelrahman_Class_Inventory.md), [Abdelrahman_Class_Diagram.drawio](Individual_Models/Abdelrahman_Class_Diagram.drawio) |
| Event Creation, Cancellation & Approval | Haziq Mohammad Khalid | [Haziq_Class_Inventory.md](Individual_Models/Haziq_Class_Inventory.md), [Haziq_Class_Diagram.drawio](Individual_Models/Haziq_Class_Diagram.drawio) |

## Integration Steps

1. Listed every class from the three models, including stubs, and matched classes that appear in more than one model.
2. Kept the full version of each duplicated class (the owner's version) and removed the stubs.
3. Looked for classes that only existed as stubs with no owner (`Club`, `FacultyMember`) and added them in full.
4. Compared how the three models represent people and roles, and chose one design for the whole system.
5. Lined up repeated structures (decision records, status enums) and data types.
6. Re-checked each association's name and multiplicity once its ends pointed at the merged classes.
7. Checked the result against the requirements, use cases and DFD (see [Class_Model_Consistency_Check.md](Class_Model_Consistency_Check.md)).

## Modification Log

| # | Area | Before (individual models) | After (consolidated model) | Reason |
|---|---|---|---|---|
| M1 | Accounts | `UserAccount` was a concrete class used only for club executives. Faculty had no account class, only Haziq's `FacultyMember` stub. | `UserAccount` is abstract, with subclasses `ClubMember` and `FacultyMember`. `hasAccess()` is abstract and each subclass implements it. | The Level-0 DFD shows Faculty Member sending login credentials to P1, so faculty need accounts (FR1, NFR1, NFR12). |
| M2 | Roles | `UserAccount 0..* — 1 ClubRole` ("assigned role"). | `ClubMember 0..* — holds — 1 ClubRole`. | Only club members hold club roles; faculty are not club roles. |
| M3 | Roles | Abdelrahman modeled `President` and `Treasurer` as subclasses of `ClubMember`. | Removed. The President and Treasurer are `ClubMember`s holding `PresidentRole` / `TreasurerRole`. | FR5 changes a member's role when positions change. With subclasses, a role change would replace the object (identity), which Ahmed's role design avoids. |
| M4 | Roles | Haziq's stub had `ClubMember.isPresidentOrVicePresidentOf(club)`. Ahmed's `PresidentRole` had no event permission. | `ClubRole.canManageEvents()` (overridden in `PresidentRole` and `VicePresidentRole`) plus `ClubMember.getClub()`. | FR11 and FR15 let the President and Vice President create and cancel events; NFR12 needs a server-side check. |
| M5 | Roles | Each role class overrode `hasPermission()`. `TreasurerRole.canAccessBudgetRequests()`. Nothing covered budget decisions. | `hasPermission()` stays only in `ClubRole`, working from the `permissions` set. Added `canSubmitBudgetRequests()` (`TreasurerRole`) and `canDecideBudgetRequests()` (`PresidentRole`). | Removes repeated overrides. Covers FR6 (Treasurer submits) and FR7 (President decides). |
| M6 | Club | Only a stub in Haziq's model. Abdelrahman's `ClubBudget` belonged to "a club" with no class for it. | New `Club` class: `ClubMember 1..* — member of — 1 Club`, `Club 1 — organizes — 0..* Event`, `Club 0..* — advised by — 1 FacultyMember`, `Club 1 ◆— has — 1 ClubBudget`. | The system serves multiple clubs (NFR9), and every event, budget and member belongs to one. |
| M7 | Faculty | Stub in Haziq's model, with its owner left undecided. | Full class in the accounts package: `getPendingProposals()` and `getAdvisedClubs()`. It keeps Haziq's associations ("accompanied by", "rooms allocated by"). | UC12 and UC13 actor. FacultyMember is a user type, so it sits with the other account classes. |
| M8 | Decisions | `EventReview` (`reviewId`, `decision: ReviewDecision`, `comments`, `reviewedAt`) and `BudgetDecisionRecord` (`recordId`, `decision: String`, `comments`, `timestamp`). | New abstract `DecisionRecord` with `recordId`, `decision: Decision`, `comments`, `decidedAt` and `decidedBy: UserAccount`, all `{readOnly}`. `EventReview` and `BudgetDecisionRecord` extend it. | The same structure appeared twice. NFR7 needs an unchangeable record of who decided and when, and FR7, FR13 and FR14 share the same three outcomes. |
| M9 | Decisions | Haziq's `ReviewDecision` enum; Abdelrahman's `decision: String`. | One `Decision` enumeration: APPROVED, REJECTED, CHANGES_REQUESTED. | A typed value instead of a free-text string; one enum instead of two. |
| M10 | Decisions | Haziq: `FacultyMember 1 — makes — 0..* EventReview`. Abdelrahman: `President 1 — makes — 0..* BudgetDecisionRecord`. | `DecisionRecord.decidedBy: UserAccount {readOnly}`, with the constraint in a note: an EventReview's decider is `event.getApprover()`, and a BudgetDecisionRecord's decider holds `PresidentRole`. | One rule in the superclass instead of two parallel associations. Drawn as an attribute to keep the diagram readable. |
| M11 | Budget | `BudgetRequest.decisionComments: String`. | Removed. Comments are kept in the `BudgetDecisionRecord` history. | Duplicate data. The history already keeps every comment (FR9, NFR7). |
| M12 | Budget | `approve(comments)`, `reject(reason)` and `requestChanges(comments)` returned `void` and didn't record who decided. | Each takes `president: ClubMember` and returns the new `BudgetDecisionRecord`. | Records the decider for the audit trail (NFR7). Matches how event decisions create an `EventReview`. |
| M13 | Budget | `BudgetRequest 1 — has history — 0..* BudgetDecisionRecord` (plain association). | Composition, matching `Event 1 ◆— has — 0..* EventReview`. | A decision record cannot exist without its request. Both kinds of history are now modeled the same way. |
| M14 | Budget | Money was `double`; `remainingBalance` was stored. | Money is `Decimal`. `/remainingBalance` is a derived attribute (`totalBudget − allocatedAmount`). | NFR6 needs exact budget calculations, and floating-point rounding is not exact. A derived value cannot drift out of sync. |
| M15 | Event ↔ Budget | Haziq: `BudgetRequest 0..* — funds — 1 Event`. Abdelrahman: `Event 1 — requested for — 0..* BudgetRequest`. | Kept Abdelrahman's "requested for", with the constraint `{event.status = APPROVED}`. | Same association drawn twice; the budget owner's name is used. FR6 allows requests only for approved events. |
| M16 | Notifications | Haziq: `Event 1 — triggers — 0..* Notification`. Ahmed: `Notification 0..* — about event — 0..1 Event`. | Kept Ahmed's "about event" and "about budget". The static factory methods `createForEvent` / `createForBudget` now take `recipient: UserAccount`. | Same relationship drawn twice. Ahmed's factory methods had no way to say who receives the notification. |
| M17 | Notifications | `NotificationType` was named but its values weren't listed. | Values: EVENT_DECISION, EVENT_UPDATED, EVENT_CANCELLED, BUDGET_DECISION, ACTION_REQUIRED. | The four kinds of alert listed in FR4. |
| M18 | Dashboard | `generateDashboard(events: List, budgets: List, notes: List)` on `UserAccount`; `DashboardSummary` had no attributes. | Moved to `ClubMember` with typed lists. `DashboardSummary` lists the FR3 items. Dependency `ClubMember ⇢ «create» DashboardSummary`. | UC2 actors are the four club roles. Typed parameters and listed content make FR3 traceable. |
| M19 | Types | IDs were `UUID` (Ahmed), `int` (Haziq) and `String` (Abdelrahman). Ahmed used `Boolean`, `Void`, `Set`, `List` and `DateTime?`. | IDs are `String` everywhere. Primitives are `boolean` and `void`, collections are `List<T>` / `Set<String>`, and the optional value is `DateTime [0..1]`. | One set of conventions across the whole model (Exercise 3: "Review attributes and methods for consistency"). |
| M20 | Notation | Abdelrahman's preview PNG drew `composed of` with a filled triangle. | Drawn with a filled diamond at the `BudgetRequest` end. | His `.drawio` source already used a composition diamond; only the exported preview was wrong. |

## Assumptions and Open Points

- **One club per member account.** `ClubMember 1..* — 1 Club` assumes a member account belongs to one club. If students in several clubs must be supported, a `Membership` association class (club + role) should replace the direct link.
- **Executive responsibilities.** FR2 says Executives work on events "depending on their assigned responsibilities". The model only has `ExecutiveRole.canViewAssignedEvents()`. An assignment between `ClubMember` and `Event` can be added if the team specifies it.
- **Earlier-lab updates.** Faculty Member was added as an actor of UC-01 Log In (use case diagram and Use_Cases.md), matching the Level-0 DFD. Rooms and buildings are recorded as reference data in D2 Event Proposals in the DFD consistency check and Core Processes Matrix.

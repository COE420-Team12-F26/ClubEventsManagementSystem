# Lab 8 – Class Distribution

[Design Goals](Architectural_Design_Goals.md) | [Style Selection](Architectural_Style_Selection.md) | [Component Descriptions](Component_Descriptions.md) | [Component Diagram](Component_Diagram/Software_Architecture_Component_Diagram.pdf)

All 23 classes and 4 enumerations of the consolidated Lab 5 class diagram ([Class_Inventory.md](../Object_Modeling/Class_Inventory.md)) are assigned to exactly one component.

## Summary

| ID | Component Name | Responsibilities | Included Classes |
|---|---|---|---|
| CO1 | Web Presentation | Web pages and controllers; calls the business components and builds the dashboard. | DashboardSummary «derived view» (pages and controllers are added during implementation) |
| CO2 | Accounts & Roles | Accounts, clubs, roles, permission checks, role changes. | UserAccount (abstract), ClubMember, FacultyMember, Club, ClubRole (abstract), PresidentRole, VicePresidentRole, ExecutiveRole, TreasurerRole |
| CO3 | Event Proposal Management | Event proposals, clash check, routing and faculty decisions, cancellation. | Event (abstract), OnCampusEvent, OffCampusTrip, Room, Building, ClashCheck, EventReview, EventStatus «enumeration» |
| CO4 | Budget Management | Budget requests, decisions, allocation, balance, history, summary. | BudgetRequest, CostBreakdownItem, ClubBudget, BudgetDecisionRecord, BudgetStatus «enumeration» |
| CO5 | Notification & Audit Services | Notifications and the shared, read-only decision audit trail. | Notification, DecisionRecord (abstract), NotificationType «enumeration», Decision «enumeration» |

**Totals:** CO1 – 1 class, CO2 – 9 classes, CO3 – 7 classes + 1 enumeration, CO4 – 4 classes + 1 enumeration, CO5 – 2 classes + 2 enumerations = 23 classes + 4 enumerations.

## Class-by-Class Placement

| Class ID | Class | Component | Reason |
|---|---|---|---|
| C01 | UserAccount (abstract) | CO2 | Login account and authentication (FR1, P1, D1). |
| C02 | ClubMember | CO2 | Student account that holds a club role (FR2, FR5); subclass of UserAccount. |
| C03 | FacultyMember | CO2 | Faculty login account; subclass of UserAccount. |
| C04 | ClubRole (abstract) | CO2 | Permission set used by every server-side role check (FR2, NFR12). |
| C05 | PresidentRole | CO2 | Subclass of ClubRole. |
| C06 | VicePresidentRole | CO2 | Subclass of ClubRole. |
| C07 | ExecutiveRole | CO2 | Subclass of ClubRole. |
| C08 | TreasurerRole | CO2 | Subclass of ClubRole. |
| C09 | Club | CO2 | Groups members and names the faculty advisor; every club-scoped query starts from it (NFR9). |
| C10 | Notification | CO5 | Created by both workflows (FR4), so it sits in the core layer below them (D5). |
| C11 | DashboardSummary «derived view» | CO1 | Built per request from the events, budget and notifications that CO1 already gathers, and never stored (FR3, UC2). |
| C12 | Event (abstract) | CO3 | Proposal lifecycle: validate, submit, edit, resubmit, cancel (FR11, FR14, FR15, P2, D2). |
| C13 | OnCampusEvent | CO3 | Subclass of Event; runs the clash check (FR12). |
| C14 | OffCampusTrip | CO3 | Subclass of Event; routed to the faculty advisor (FR11). |
| C15 | Room | CO3 | Room reference data used by the clash check (D2). |
| C16 | Building | CO3 | Contains rooms and names the room allocator used for routing (FR11, FR13). |
| C17 | ClashCheck | CO3 | Part of OnCampusEvent (composition); one clash check run (FR12). |
| C18 | EventReview | CO3 | Part of Event (composition); faculty decision on a proposal (FR13, FR14, P3). Extends DecisionRecord from CO5. |
| C19 | BudgetRequest | CO4 | Request for funds and its approval workflow (FR6, FR7, P4, D3). |
| C20 | CostBreakdownItem | CO4 | Part of BudgetRequest (composition) (FR6). |
| C21 | ClubBudget | CO4 | Changed only by budget logic: fund check, allocation, remaining balance (FR8, FR10, NFR6, P5, D4). |
| C22 | BudgetDecisionRecord | CO4 | Part of BudgetRequest (composition); the President's decision (FR7, FR9). Extends DecisionRecord from CO5. |
| C23 | DecisionRecord (abstract) | CO5 | Shared read-only audit record for event and budget decisions (NFR7); both workflows build on it. |
| E1 | EventStatus «enumeration» | CO3 | Used by Event. |
| E2 | BudgetStatus «enumeration» | CO4 | Used by BudgetRequest. |
| E3 | Decision «enumeration» | CO5 | Used by DecisionRecord. |
| E4 | NotificationType «enumeration» | CO5 | Used by Notification. |

## Grouping Rules Used

- Wholes and their parts stay together: five of the six Lab 5 compositions are inside one component (Building–Room, OnCampusEvent–ClashCheck, Event–EventReview in CO3; BudgetRequest–CostBreakdownItem, BudgetRequest–BudgetDecisionRecord in CO4).
- Inheritance hierarchies stay together: UserAccount and ClubRole in CO2, Event in CO3.
- Types shared by both workflows (DecisionRecord, Decision, Notification) go to the core services layer (CO5), below both. EventReview and BudgetDecisionRecord extend DecisionRecord from the layer below, so dependencies still point downward.
- Club–ClubBudget is the one composition that is split: ClubBudget stays with the budget logic that changes it (FR8) and is linked to its club by `clubId` (`IBudgetService.getBudget(clubId)`). Likewise, a club's events and a faculty member's pending proposals are read through `IEventService` (`getEventsForClub`, `getPendingFor`), so CO2 never calls the domain layer.
- Each enumeration goes with the class that uses it.

**Revision to earlier artifacts:** `ClubMember.generateDashboard()` is replaced by the static `DashboardSummary.generate(role, events, budgets, budget, notes)` in CO1, so CO2 no longer calls into CO3 or CO4. The Lab 5 class diagram ([Integration_Notes.md](../Object_Modeling/Integration_Notes.md), M21) and the Lab 6 UC02 sequence diagram are updated to match.

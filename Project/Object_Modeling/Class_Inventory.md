# Consolidated Class Inventory

Diagram: [Consolidated Class Diagram](Class_Diagram/Consolidated_Class_Diagram.png) ([draw.io source](Class_Diagram/Consolidated_Class_Diagram.drawio)) | [Integration Notes](Integration_Notes.md) | [Consistency Check](Class_Model_Consistency_Check.md) | [Individual Models](Individual_Models/)

The team model has **23 classes and 4 enumerations**. It merges the three individual models:

| Component | Owner | Individual model |
|---|---|---|
| Accounts, Roles, Notifications & Dashboard | Ahmed Abdelmohsen | 7 classes + DashboardSummary view; stubs: Event, BudgetRequest |
| Budget Requests | Abdelrahman Ahmed Elsayed | 4 classes; stubs: Event, ClubMember, Treasurer, President |
| Event Creation, Cancellation & Approval | Haziq Mohammad Khalid | 7 classes + 2 enumerations; stubs: ClubMember, FacultyMember, Club, Notification, BudgetRequest |

## Approved Classes

### Accounts, Roles & Notifications (P1, P6 · D1, D5)

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Origin |
|---|---|---|---|---|
| C01 | UserAccount *(abstract)* | Login account for anyone who uses the system. Stores AUS email, password hash and active status. Authenticates the user and lists their notifications. | UC1, UC3, UC4 | Ahmed C1 (made abstract) |
| C02 | ClubMember | A student account in a club. Holds one club role at a time, can be given a new role, and builds the role-based dashboard. | UC1–UC11, UC14, UC15 | Ahmed C1 + ClubMember stubs (Abdelrahman C06, Haziq) |
| C03 | FacultyMember | A faculty account. Reviews event proposals as a building's room allocator or as a club's faculty advisor, and goes on approved trips. | UC1, UC12, UC13 | Haziq stub, promoted to a full class |
| C04 | ClubRole *(abstract)* | Common permission set and permission checks shared by all club roles. | UC1, UC4, UC5 | Ahmed C2 |
| C05 | PresidentRole | Manages roles and events, views the budget and decides on budget requests. | UC5, UC7, UC11, UC15 | Ahmed C3 + Abdelrahman President stub |
| C06 | VicePresidentRole | Manages events alongside the President. | UC11, UC14, UC15 | Ahmed C4 |
| C07 | ExecutiveRole | Views the events they are assigned to. | UC2, UC4 | Ahmed C5 |
| C08 | TreasurerRole | Views the budget and submits budget requests. | UC6, UC8, UC9, UC10 | Ahmed C6 + Abdelrahman Treasurer stub |
| C09 | Club | A student club. Groups its members, events and budget, and names its faculty advisor. | UC2, UC6, UC8, UC11 | Haziq stub, promoted to a full class |
| C10 | Notification | An alert sent to one account about an event or a budget request, with its read status. | UC3, UC10 | Ahmed C7 + Haziq Notification stub |
| C11 | DashboardSummary *«derived view»* | The role-filtered dashboard content (upcoming events, budget requests, remaining budget, recent notifications). Built on demand and never stored. | UC2 | Ahmed (modeling note) |

### Event Proposals (P2, P3 · D2)

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Origin |
|---|---|---|---|---|
| C12 | Event *(abstract)* | An event proposal and its status lifecycle: validate, submit, edit/resubmit, cancel. | UC11–UC15 | Haziq C01 |
| C13 | OnCampusEvent | An event booked in a university room. Runs the clash check and is routed to the building's room allocator. | UC11, UC12, UC14 | Haziq C02 |
| C14 | OffCampusTrip | An event outside the university. Routed to the club's faculty advisor, who then goes with the students. | UC11, UC12, UC14 | Haziq C03 |
| C15 | Room | A bookable university room. Provides the approved bookings used by the clash check. | UC11 | Haziq C04 |
| C16 | Building | Contains rooms and names the faculty member in charge of allocating them. | UC11, UC12 | Haziq C05 |
| C17 | ClashCheck | One run of the room clash check, and whether the booker chose to submit anyway. | UC11, UC12 | Haziq C06 |
| C18 | EventReview | A faculty decision on an event proposal (a kind of DecisionRecord). | UC12, UC13, UC14 | Haziq C07 |

### Budget Requests (P4, P5 · D3, D4)

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Origin |
|---|---|---|---|---|
| C19 | BudgetRequest | A request for funds for an approved event. Has an amount, a status and a cost breakdown. | UC6, UC7, UC9 | Abdelrahman C01 |
| C20 | CostBreakdownItem | One line of a budget request's cost breakdown. | UC6 | Abdelrahman C02 |
| C21 | ClubBudget | The club's total, allocated and remaining budget. Allocates funds when a request is approved. | UC8 | Abdelrahman C03 |
| C22 | BudgetDecisionRecord | The President's decision on a budget request (a kind of DecisionRecord). | UC7, UC9, UC10 | Abdelrahman C04 |

### Shared

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Origin |
|---|---|---|---|---|
| C23 | DecisionRecord *(abstract)* | Read-only audit record of any approve / reject / request-changes decision: who decided, when, and why. | UC7, UC9, UC12, UC13 | New: generalizes Haziq C07 and Abdelrahman C04 |

### Enumerations

| ID | Name | Values | Used by |
|---|---|---|---|
| E1 | EventStatus | PENDING, CHANGES_REQUESTED, APPROVED, REJECTED, CANCELLED | Event |
| E2 | BudgetStatus | PENDING, APPROVED, REJECTED, CHANGES_REQUESTED | BudgetRequest |
| E3 | Decision | APPROVED, REJECTED, CHANGES_REQUESTED | DecisionRecord |
| E4 | NotificationType | EVENT_DECISION, EVENT_UPDATED, EVENT_CANCELLED, BUDGET_DECISION, ACTION_REQUIRED | Notification |

## Duplicate Classes Identified and Resolved

| Duplicate | Appeared In | Resolution |
|---|---|---|
| Event | Haziq (full), Ahmed (stub, `eventId: UUID`), Abdelrahman (stub, `eventId`, `title`) | Kept Haziq's full class; both stubs removed. |
| BudgetRequest | Abdelrahman (full), Ahmed (stub with `BudgetRequestStatus`), Haziq (stub) | Kept Abdelrahman's full class; stubs removed; enum named `BudgetStatus` everywhere. |
| Notification | Ahmed (full), Haziq (stub with `create(recipient, message)`) | Kept Ahmed's class; its factory methods now take the recipient. |
| ClubMember | Abdelrahman (stub superclass), Haziq (stub), Ahmed (as `UserAccount`) | One `ClubMember` class, a subclass of `UserAccount`, that holds a `ClubRole`. |
| President / Treasurer | Abdelrahman (stub subclasses of ClubMember), Ahmed (`PresidentRole`, `TreasurerRole`) | Kept Ahmed's role classes; Abdelrahman's person subclasses removed (see decision D2). |
| FacultyMember | Haziq (stub only, no owner) | Promoted to a full subclass of `UserAccount`. |
| Club | Haziq (stub only); implied by Abdelrahman's "club's total budget" | Promoted to a full class. |
| EventReview / BudgetDecisionRecord | Haziq, Abdelrahman: both stored id, decision, comments, timestamp | Common fields moved to new abstract `DecisionRecord`; both kept as subclasses. |
| ReviewDecision / `decision: String` | Haziq enum, Abdelrahman string | Replaced by one shared `Decision` enumeration. |

## Removed or Renamed Classes

| Class | Model | Reason |
|---|---|---|
| Event «stub» | Ahmed, Abdelrahman | Duplicate of C12 |
| BudgetRequest «stub» | Ahmed, Haziq | Duplicate of C19 |
| Notification «stub» | Haziq | Duplicate of C10 |
| ClubMember «stub» | Abdelrahman, Haziq | Merged into C02 |
| President «stub», Treasurer «stub» | Abdelrahman | Replaced by role objects C05, C08 |
| FacultyMember «stub», Club «stub» | Haziq | Promoted to C03, C09 |
| ReviewDecision | Haziq | Renamed to the shared `Decision` (E3) |
| BudgetRequestStatus | Ahmed (stub type) | Renamed to `BudgetStatus` (E2), the owner's name |

## Major Integration Decisions

| # | Decision | Summary |
|---|---|---|
| D1 | Single account hierarchy | `UserAccount` is abstract, with `ClubMember` and `FacultyMember` as subclasses, because faculty also log in through P1 (Level-0 DFD). |
| D2 | Roles are objects, not person subclasses | A `ClubMember` holds one `ClubRole`. Role changes (FR5) keep the same account and identity, which rules out Abdelrahman's `ClubMember ← President/Treasurer` subclasses. |
| D3 | Shared decision history | `EventReview` and `BudgetDecisionRecord` extend the read-only `DecisionRecord` (NFR7), with the shared `Decision` enum. |
| D4 | `Club` added | It ties members, the faculty advisor, events and the club budget together (needed for NFR9 multi-club support). |
| D5 | Common types | IDs are `String`, money is `Decimal`, and the primitives `boolean`/`void` and typed collections are used throughout. |

All 20 changes, with the before/after state and the reason for each, are logged in [Integration_Notes.md](Integration_Notes.md).

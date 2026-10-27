# Class Model Consistency and Traceability Check

Diagram: [Consolidated Class Diagram](Class_Diagram/Consolidated_Class_Diagram.png) | [Class Inventory](Class_Inventory.md) | [Integration Notes](Integration_Notes.md) | [Functional Requirements](../Requirements/Functional_Requirements.md) | [Use Cases](../Requirements/Use_Cases.md) | [Level-0 DFD](../Data_Flow_Modeling/Level_0_DFD/Level_0_DFD.png)

## Functional Requirements vs. Classes

| FR | Requirement | Classes and Members |
|---|---|---|
| FR1 | User Login | `UserAccount.authenticate()`, `ClubMember`, `FacultyMember` |
| FR2 | Role-Based Access | `ClubRole` + `PresidentRole`, `VicePresidentRole`, `ExecutiveRole`, `TreasurerRole`; `ClubMember.hasAccess()` |
| FR3 | Personalized Dashboard | `ClubMember.generateDashboard()`, `DashboardSummary`, `Event`, `BudgetRequest`, `ClubBudget`, `Notification` |
| FR4 | Notifications | `Notification`, `NotificationType`, `Notification.createForEvent()` / `createForBudget()` |
| FR5 | Account and Role Management | `ClubMember.assignRole()`, `ClubMember.roleAssignedAt`, `PresidentRole.canManageRoles()` |
| FR6 | Submit Budget Request | `BudgetRequest`, `CostBreakdownItem`, `TreasurerRole.canSubmitBudgetRequests()`, "submits", "requested for" `{event.status = APPROVED}` |
| FR7 | Budget Approval Workflow | `BudgetRequest.approve()` / `reject()` / `requestChanges()`, `BudgetDecisionRecord`, `Decision`, `PresidentRole.canDecideBudgetRequests()` |
| FR8 | Running Budget Balance | `ClubBudget.allocate()`, `ClubBudget./remainingBalance`, `ClubBudget.hasSufficientFunds()` |
| FR9 | Budget History Log | `BudgetRequest.status`, "has history" → `BudgetDecisionRecord`, "requested for" → `Event` |
| FR10 | Budget Summary View | `ClubBudget` (total, allocated, remaining), `canViewBudget()` (`PresidentRole`, `TreasurerRole`) |
| FR11 | Create Event Proposal | `Event`, `OnCampusEvent`, `OffCampusTrip`, `Room`, `Building`, `Event.getApprover()`, "proposes", "organizes", "advised by" |
| FR12 | Location Clash Check | `ClashCheck`, `OnCampusEvent.runClashCheck()`, `Room.getApprovedEvents()`, `Event.overlaps()` |
| FR13 | Approve or Reject Event Proposal | `EventReview`, `DecisionRecord.isValid()`, `OffCampusTrip.assignAccompanyingFaculty()`, `Building.isRoomAllocator()` |
| FR14 | Request Changes to Event Proposal | `Decision.CHANGES_REQUESTED`, `EventStatus.CHANGES_REQUESTED`, `Event.resubmit()`, `Event.getReviewHistory()` |
| FR15 | Cancel Event | `Event.cancel()`, `Event.canBeCancelled()`, `Event.cancellationReason`, `EventStatus.CANCELLED` |

## Use Cases vs. Classes

| Use Case | Actor(s) | Classes |
|---|---|---|
| UC1 Log In | Club roles, Faculty Member | `UserAccount`, `ClubMember`, `FacultyMember`, `ClubRole` |
| UC2 View Dashboard | President / VP / Executive / Treasurer | `ClubMember`, `DashboardSummary`, `Event`, `BudgetRequest`, `ClubBudget`, `Notification` |
| UC3 View Notifications | President / VP / Executive / Treasurer | `UserAccount.listNotifications()`, `Notification` |
| UC4 View Role-Based Features | President / VP / Executive / Treasurer | `ClubRole` hierarchy, `ClubMember.hasAccess()` |
| UC5 Manage Club Member Roles | President | `ClubMember.assignRole()`, `PresidentRole` |
| UC6 Submit Budget Request | Treasurer | `BudgetRequest`, `CostBreakdownItem`, `TreasurerRole`, `Event` |
| UC7 Review Budget Request | President | `BudgetRequest`, `BudgetDecisionRecord`, `ClubBudget`, `PresidentRole` |
| UC8 View Budget Summary | Treasurer / President | `ClubBudget`, `Club` |
| UC9 View Budget Request History | Treasurer | `BudgetRequest`, `BudgetDecisionRecord` |
| UC10 Receive Budget Decision Notification | Treasurer | `Notification` ("about budget"), `BudgetDecisionRecord` |
| UC11 Create Event Proposal | President / VP | `Event`, `OnCampusEvent`, `OffCampusTrip`, `Room`, `ClashCheck`, `Club` |
| UC12 Review Event Proposal | Faculty Member | `FacultyMember.getPendingProposals()`, `EventReview`, `ClashCheck` |
| UC13 Request Changes to Event Proposal | Faculty Member | `EventReview` (`CHANGES_REQUESTED`) |
| UC14 Revise Returned Event Proposal | President / VP | `Event.updateDetails()`, `Event.resubmit()`, `Event.getReviewHistory()` |
| UC15 Cancel Event | President / VP | `Event.cancel()` |

Every use case maps to at least one class, and every class supports at least one use case.

## DFD Data Stores vs. Classes

| Data Store | Classes | Notes |
|---|---|---|
| D1 User Accounts & Roles | `UserAccount`, `ClubMember`, `FacultyMember`, `ClubRole` (+4 subclasses), `Club` | Club membership is stored with the accounts. |
| D2 Event Proposals | `Event`, `OnCampusEvent`, `OffCampusTrip`, `ClashCheck`, `EventReview`, `Room`, `Building` | `Room` and `Building` are the room and building reference data held in D2 and read by P2 and P3. |
| D3 Budget Requests & History | `BudgetRequest`, `CostBreakdownItem`, `BudgetDecisionRecord` | |
| D4 Club Budget | `ClubBudget` | |
| D5 Notifications | `Notification` | |
| (none) | `DashboardSummary` | «derived view» built by P6 and not stored, consistent with the DFD having no dashboard store. |

## DFD Processes vs. Classes

| Process | Main Classes and Operations |
|---|---|
| P1 Authenticate Users & Manage Roles | `UserAccount.authenticate()`, `ClubMember.assignRole()`, `ClubRole.hasPermission()` |
| P2 Manage Event Proposals | `Event.validate()` / `submit()` / `updateDetails()` / `cancel()`, `OnCampusEvent.runClashCheck()`, `ClashCheck` |
| P3 Review Event Proposals | `FacultyMember.getPendingProposals()`, `EventReview.save()`, `Event.updateStatus()` |
| P4 Manage Budget Requests | `BudgetRequest.submit()`, `CostBreakdownItem` |
| P5 Review Budget Requests & Update Club Budget | `BudgetRequest.approve()` / `reject()` / `requestChanges()`, `BudgetDecisionRecord`, `ClubBudget.hasSufficientFunds()` / `allocate()` |
| P6 Generate Dashboard & Notifications | `ClubMember.generateDashboard()`, `DashboardSummary`, `Notification.createForEvent()` / `createForBudget()` |

## Non-Functional Requirements Reflected in the Structure

| NFR | How the class model supports it |
|---|---|
| NFR1, NFR12 Security / Access Control | `hasAccess()`, `ClubRole` permission checks, `Building.isRoomAllocator()`, `Event.getApprover()` (checked on the server) |
| NFR6 Accuracy | Money attributes use `Decimal`; `/remainingBalance` is derived |
| NFR7 Auditability | `DecisionRecord` attributes are `{readOnly}` and records are never edited |
| NFR9 Scalability (multiple clubs) | `Club` separates members, events and budgets per club |
| NFR14 Robustness | `Event.validate()` returns one error per field |
| NFR15 Data Integrity | `Event.version` with `expectedVersion` on `updateStatus()`, `cancel()` and `EventReview.save()` |

The other NFRs (performance, usability, compatibility, availability, response time, learnability, maintainability) are about runtime behaviour or the user interface, not class structure. The class model does not need to show them.

## Verification of Associations and Multiplicities

| # | Association | End A | Mult. | End B | Mult. | Type | Check |
|---|---|---|---|---|---|---|---|
| 1 | receives | UserAccount | 1 | Notification | 0..* | Association | Each notification has one recipient (FR4). |
| 2 | holds | ClubMember | 0..* | ClubRole | 1 | Association | One role at a time; many members can share a role object (FR2, FR5). |
| 3 | member of | ClubMember | 1..* | Club | 1 | Association | A club has at least one member (its President). |
| 4 | advised by | Club | 0..* | FacultyMember | 1 | Association | Every club has one advisor; an advisor can advise several clubs (Scenario 2). |
| 5 | has | Club | 1 | ClubBudget | 1 | Composition | Each club has exactly one budget (FR8, FR10). |
| 6 | about event | Notification | 0..* | Event | 0..1 | Association | A notification refers to at most one event (FR4). |
| 7 | about budget | Notification | 0..* | BudgetRequest | 0..1 | Association | A notification refers to at most one budget request (UC10). |
| 8 | organizes | Club | 1 | Event | 0..* | Association | Each event belongs to one club (FR11). |
| 9 | proposes | ClubMember | 1 | Event | 0..* | Association | Created by one President/VP (FR11, NFR12). |
| 10 | has | Event | 1 | EventReview | 0..* | Composition | Reviews are kept across return/resubmit cycles (FR14). |
| 11 | accompanied by | OffCampusTrip | 0..* | FacultyMember | 0..1 | Association | Set only when the trip is approved (FR13). |
| 12 | rooms allocated by | Building | 0..* | FacultyMember | 1 | Association | One room allocator per building (FR11). |
| 13 | contains | Building | 1 | Room | 1..* | Composition | A room belongs to one building. |
| 14 | booked in | OnCampusEvent | 0..* | Room | 1 | Association | One room per on-campus event (FR11, FR12). |
| 15 | is checked by | OnCampusEvent | 1 | ClashCheck | 0..* | Composition | One check per submission or resubmission (FR12). |
| 16 | conflicts with | ClashCheck | 0..* | OnCampusEvent | 0..* | Association | The approved events that overlap (FR12). |
| 17 | allocates from | ClubBudget | 1 | BudgetRequest | 0..* | Association | Approved requests draw on the club budget (FR8). |
| 18 | requested for | BudgetRequest | 0..* | Event | 1 | Association | `{event.status = APPROVED}` (FR6). |
| 19 | composed of | BudgetRequest | 1 | CostBreakdownItem | 1..* | Composition | A request needs a cost breakdown (FR6). |
| 20 | has history | BudgetRequest | 1 | BudgetDecisionRecord | 0..* | Composition | Every decision is kept (FR9, NFR7). |
| 21 | submits | ClubMember | 1 | BudgetRequest | 0..* | Association | Submitted by the member holding `TreasurerRole` (FR6). |

Every association has a name, a reading direction and multiplicities at both ends.

## Verification of Generalizations

| Superclass | Subclasses | "Is-a" check |
|---|---|---|
| UserAccount *(abstract)* | ClubMember, FacultyMember | Both are kinds of user who log in through P1, and neither ever changes into the other. |
| ClubRole *(abstract)* | PresidentRole, VicePresidentRole, ExecutiveRole, TreasurerRole | Each is a club position with its own permissions (FR2). |
| Event *(abstract)* | OnCampusEvent, OffCampusTrip | Same lifecycle; different location data and reviewer (FR11). |
| DecisionRecord *(abstract)* | EventReview, BudgetDecisionRecord | Same audit data and outcomes; different subject and decider (FR7, FR13, NFR7). |

## Checklist

- [x] Every approved class traces to at least one use case and one requirement.
- [x] Every DFD data store (D1–D5) is represented by at least one class.
- [x] Every DFD process (P1–P6) is carried out by class operations.
- [x] Duplicate classes from individual models are merged; no stubs remain.
- [x] Attributes and methods show visibility, data types, parameters and return types.
- [x] Data types follow one convention (String IDs, Decimal money, boolean/void, typed collections).
- [x] All associations are named and have multiplicities at both ends.
- [x] Generalizations represent genuine "is-a" relationships.
- [x] UML notes document the main constraints (role assignment, event lifecycle, budget rules, immutable decisions).

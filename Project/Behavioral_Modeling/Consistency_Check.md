# Lab 6 – Consistency Checks

This document records the three consistency checks required by the Lab 6 brief for the three sequence diagrams: `SD_UC11_CreateEventProposal`, `SD_UC07_ReviewBudgetRequest`, and `SD_UC02_ViewDashboard`.

## Check A — vs. Lab 3 Use Cases and Requirements

- **UC11 (Create Event Proposal):** The diagram's flow (create → validate → submit → run clash check → branch on clash → notify approver) matches UC11's main success scenario and its clash-handling alternate flow, and implements FR11 (on-campus events must be checked for room/date conflicts before approval) and the general proposal-submission requirement that invalid proposals are rejected client-side before being submitted (`errors.isEmpty()` guard).
- **UC07 (Review Budget Request):** The diagram's three-way decision (approve / reject / request changes) matches UC07's decision step exactly, and the nested `sufficientFunds` guard implements the requirement that a request cannot be approved if it would exceed the club's remaining budget.
- **UC02 (View Dashboard):** The diagram matches UC02's single main success scenario (member requests dashboard → system aggregates events, budget, and notifications → returns summary); no alternate flow is required by the use case beyond the empty-collection case, which the `loop`/`opt` fragments naturally degrade to (zero iterations).

All three diagrams use the same actor and object vocabulary as `Project/Requirements/Use_Cases.md` (President/VP, Club Member, Treasurer) and introduce no actor or flow that is not already present in the Lab 3 use case descriptions.

## Check B — vs. Lab 5 Class Diagram

Every message in all three diagrams was checked one-for-one against the operation signatures in `Project/Object_Modeling/Class_Inventory.md` and `Consolidated_Class_Diagram.png`. All messages reuse the exact method names, parameter lists, and return types defined there:

| Sequence message | Class Diagram operation |
|---|---|
| `validate() : List<String>` | `Event.validate(): List<String>` |
| `submit()` | `Event.submit(): void` |
| `runClashCheck()` | `OnCampusEvent.runClashCheck(): ClashCheck` |
| `getApprovedEvents(eventDate)` | `Room.getApprovedEvents(date): List<OnCampusEvent>` |
| `overlaps(approvedEvent) : boolean` | `Event.overlaps(other): boolean` |
| `run() : boolean` | `ClashCheck.run(): boolean` |
| `getRoomAllocator() : FacultyMember` | `Building.getRoomAllocator(): FacultyMember` |
| `updateStatus(PENDING, expectedVersion)` | `Event.updateStatus(newStatus, expectedVersion): boolean` |
| `createForEvent(...)` | `Notification.createForEvent(recipient, event, message): Notification` (static) |
| `getWarningMessage()` / `overrideAndSubmit()` / `hasClash()` | `ClashCheck.getWarningMessage(): String` / `overrideAndSubmit(): void` / `hasClash(): boolean` |
| `approve(president, comments) : BudgetDecisionRecord` | `BudgetRequest.approve(president, comments): BudgetDecisionRecord` |
| `reject(president, reason) : BudgetDecisionRecord` | `BudgetRequest.reject(president, reason): BudgetDecisionRecord` |
| `requestChanges(president, comments) : BudgetDecisionRecord` | `BudgetRequest.requestChanges(president, comments): BudgetDecisionRecord` |
| `hasSufficientFunds(requestedAmount) : boolean` | `ClubBudget.hasSufficientFunds(amount): boolean` |
| `allocate(requestedAmount) : void` | `ClubBudget.allocate(amount): void` |
| `/remainingBalance = totalBudget - allocatedAmount` | `ClubBudget./remainingBalance` (derived attribute) |
| `createForBudget(...)` | `Notification.createForBudget(recipient, request, message): Notification` (static) |
| `generateDashboard(events, budgets, notes) : DashboardSummary` | `ClubMember.generateDashboard(events, budgets, notes): DashboardSummary` |
| `getStatus()` | `Event.getStatus()` |
| `getRemainingBalance() : Decimal` | `ClubBudget.getRemainingBalance(): Decimal` |
| `isUnread() : boolean` | `Notification.isUnread(): boolean` |
| `DashboardSummary(upcomingEvents, budgetRequests, remainingBudget, recentNotifications)` | `DashboardSummary` attributes (derived view) |

No new operations, attributes, or classes were introduced by the sequence diagrams; every participating object is one of the 23 classes already defined and integrated in the team's Lab 5 Class Diagram.

## Check C — vs. the Overall System

- **Naming consistency:** actor names (`President / VP`, `President`, `Club Member`), object names (`:OnCampusEvent`, `:ClashCheck`, `:Room`, `:Building`, `:BudgetRequest`, `:ClubBudget`, `:BudgetDecisionRecord`, `:Notification`, `:ClubMember`, `:Event`, `:DashboardSummary`), and enumeration literals (`PENDING`, `APPROVED`, `REJECTED`, `CHANGES_REQUESTED`) are identical across all three diagrams and match the Class Diagram exactly — no synonyms or renamed classes were introduced.
- **Cross-component coherence:** UC11 and UC07 both create a `Notification` through the same two static factory methods (`createForEvent` / `createForBudget`) already defined on the shared `Notification` class, and UC02 reads notifications created by either flow, confirming the three diagrams describe one consistent system rather than three isolated ones.
- **No contradictions with team integration decisions:** the diagrams respect the Major Integration Decisions (D1–D5) recorded in `Integration_Notes.md` (e.g., `Notification` and `DashboardSummary` as shared/promoted classes, `ClubBudget./remainingBalance` as a derived attribute) and the associations/multiplicities verified in `Class_Model_Consistency_Check.md`.

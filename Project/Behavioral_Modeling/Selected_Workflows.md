# Lab 7 – Selected Workflows (UML Activity Diagrams)

As required by Lab 7, **no new use cases were selected**. The three Activity Diagrams model the *same* three use cases that were modeled with Sequence Diagrams in Lab 6 (see `Selected_Use_Cases.md`). The Sequence Diagrams show *who sends which message to whom*; the Activity Diagrams show the *workflow* — the order of actions, the decisions, the loops, the parallel work, and which participant is responsible for each step.

| Use Case | Activity Diagram | Lab 6 Sequence Diagram | Swimlanes | Component owner |
|---|---|---|---|---|
| UC11 – Create Event Proposal (On-Campus) | `Activity_Diagrams/AD_UC11_CreateEventProposal` (.pdf / .png / .drawio) | `SD_UC11_CreateEventProposal` | President / VP · System · Faculty Member (Room Allocator) | Haziq |
| UC07 – Review Budget Request | `Activity_Diagrams/AD_UC07_ReviewBudgetRequest` (.pdf / .png / .drawio) | `SD_UC07_ReviewBudgetRequest` | President · System · Treasurer | Abdelrahman |
| UC02 – View Dashboard | `Activity_Diagrams/AD_UC02_ViewDashboard` (.pdf / .png / .drawio) | `SD_UC02_ViewDashboard` | Club Member · System | Ahmed |

The `.drawio` files are the native editable models (open in diagrams.net / draw.io); the `.pdf` and `.png` files are readable exports of the same diagrams.

---

## AD_UC11 – Create Event Proposal (On-Campus)

**Workflow summary**
1. The President/VP fills in the event form (title, description, date, time, room, expected attendees). A merge node sits above this action because the flow can come back here later.
2. The System creates the `OnCampusEvent` and validates it.
   - `[errors found]` → validation errors are displayed and the flow ends (**proposal not submitted**).
   - `[no errors]` → the President/VP submits the proposal.
3. The System runs the clash check: it retrieves the approved events for the room/date, then loops (merge → decision) over them — `[more approved events to check]` → *Check overlap with next approved event* → back to the merge; `[all checked]` → *Determine clash result*.
4. Decision on the result:
   - `[no clash]` → goes straight to the merge node before routing.
   - `[clash found]` → the System shows a warning with the clashing event's name and time, then the President/VP decides:
     - `[change details]` → back to the form (the merge node at the top).
     - `[submit anyway]` → the System overrides the check and flags the clash on the proposal, then joins the merge.
5. The System gets the building's room allocator and sets the status to **PENDING**.
6. **Fork** — two things happen in parallel: the System notifies the room allocator (who receives the proposal for review in the Faculty lane), while the President/VP views the proposal with status *Pending*.
7. **Join** → final node (**proposal submitted, Pending**).

A note explains that off-campus trips follow the same workflow but enter a destination, skip the clash check, and go to the club's faculty advisor (FR11).

**Elements used:** 1 initial node, 2 final nodes, 15 actions, 4 decision nodes, 3 merge nodes, 1 fork, 1 join, 3 swimlanes, 8 guarded flows.

## AD_UC07 – Review Budget Request

**Workflow summary**
1. The President opens a pending budget request; the System displays the requested amount and cost breakdown; the President reviews it, selects a decision and enters comments/reason.
2. A three-way decision node:
   - `[decision = reject]` → create REJECTED decision record → set status REJECTED.
   - `[decision = request changes]` → create CHANGES_REQUESTED decision record → set status CHANGES_REQUESTED.
   - `[decision = approve]` → check that the club has sufficient funds:
     - `[insufficient funds]` → show the error "insufficient funds – cannot approve" → final node (**request stays Pending**).
     - `[sufficient funds]` → create APPROVED decision record → allocate requested amount → recalculate remaining balance → set status APPROVED.
3. The three successful paths rejoin at a **merge** node.
4. **Fork** — the System notifies the Treasurer (who receives the budget decision notification in the Treasurer lane) while, in parallel, the President views the returned decision record.
5. **Join** → final node.

**Elements used:** 1 initial node, 2 final nodes, 17 actions, 2 decision nodes, 1 merge node, 1 fork, 1 join, 3 swimlanes, 5 guarded flows.

## AD_UC02 – View Dashboard

**Workflow summary**
1. A Club Member (any role) opens the dashboard; the System loads the member's events, budget and notifications.
2. **Fork** into three independent, parallel branches:
   - **Events loop:** `[more events]` → get event status → `[APPROVED or PENDING]` → add event to upcoming events / `[other status]` → skip; the two paths rejoin at a merge and loop back. `[no more events]` → join.
   - **Budget:** get remaining club budget balance → join.
   - **Notifications loop:** `[more notifications]` → check whether the notification is unread → `[unread]` → add to recent notifications / `[read]` → skip; merge and loop back. `[no more notifications]` → join.
3. **Join** → the System creates the `DashboardSummary` → the Club Member views the dashboard → final node.

**Elements used:** 1 initial node, 1 final node, 9 actions, 4 decision nodes, 4 merge nodes, 1 fork, 1 join, 2 swimlanes, 8 guarded flows.

---

## Review and Validation of the Activity Diagrams

The team reviewed all three diagrams against the twelve questions in the Lab 7 brief.

| # | Review question | Result |
|---|---|---|
| 1 | Does each Activity Diagram correspond to a use case selected in Lab 6? | **Yes.** AD_UC11, AD_UC07 and AD_UC02 correspond one-to-one to SD_UC11, SD_UC07 and SD_UC02. No new use cases were introduced. |
| 2 | Is the workflow consistent with the functional requirements and use case description? | **Yes.** UC11 follows FR11 + FR12 (form, validation, room clash check, warn-then-change-or-submit-anyway, route to room allocator, status Pending). UC07 follows FR7 + FR8 (approve / reject / return with comments; funds only allocated on approval; remaining balance updated). UC02 follows FR3 + FR4 (upcoming events, budget, recent notifications). |
| 3 | Clearly identifiable initial and final nodes? | **Yes.** Every diagram has exactly one initial node. UC11 and UC07 have two final nodes each (one for the unsuccessful outcome, labeled "proposal not submitted" / "request stays Pending", and one for the successful outcome); UC02 has one. |
| 4 | Actions named clearly and in a logical order? | **Yes.** All actions use verb–object names (e.g., "Submit proposal", "Allocate requested amount from budget") and follow the use-case step order. |
| 5 | Control-flow arrows used correctly? | **Yes.** Every action has exactly one incoming and one outgoing flow; branching only happens at decision/fork nodes and rejoining only at merge/join nodes. |
| 6 | Decision branches labeled with meaningful guards? | **Yes.** Every outgoing flow of every decision node has a guard (e.g., `[errors found]`/`[no errors]`, `[decision = approve]`, `[sufficient funds]`/`[insufficient funds]`, `[unread]`/`[read]`). Guards on each decision are mutually exclusive and complete. |
| 7 | Do alternative paths lead to appropriate outcomes? | **Yes.** Invalid input ends with the proposal not submitted; a clash either returns the user to the form or proceeds with a flagged clash; insufficient funds ends with the request still pending; every decision outcome in UC07 produces a decision record and a notification. |
| 8 | Merge nodes used appropriately? | **Yes.** Merges are used where alternative paths rejoin: the form-entry loop and the clash/no-clash rejoin in UC11, the three decision outcomes in UC07, and the loop-back and skip/add rejoin points in both UC02 loops. |
| 9 | Fork and join nodes used correctly? | **Yes.** Forks are used only for genuinely concurrent work: notifying the approver/treasurer while the initiating user sees the result (UC11, UC07), and the three independent dashboard aggregations (UC02). Every fork has a matching join. |
| 10 | Swimlanes used appropriately? | **Yes.** Each diagram separates the human actors from the System, and adds the receiving actor (Faculty Member room allocator in UC11, Treasurer in UC07) so it is clear who is responsible for each action. |
| 11 | Consistent with intended behavior and other models? | **Yes** — see the consistency section below. |
| 12 | Missing activities, unnecessary steps, contradictory paths, unreachable actions? | **None found.** Every action is reachable from the initial node and every path reaches a final node; there are no dead ends and no contradictory guards. |

---

## Consistency with Previous Labs

### Lab 3 – Requirements and Use Case Modeling
- Actor names in the swimlanes (President / Vice President, President, Treasurer, Faculty Member, Club Member) are the same as the actors in `Requirements/Use_Cases.md`.
- **UC11:** the form fields match FR11 exactly; the clash check against approved events, the warning showing the clashing event's name and time, and the choice to change details or submit anyway match FR12; routing to the building's room allocator and the status "Pending" match FR11 / UC11.
- **UC07:** the three outcomes (approve, reject, return with comments) match FR7 and UC7; allocation only on approval and the recalculated remaining balance match FR7 and FR8; the treasurer notification matches UC10 / FR4.
- **UC02:** the dashboard contents (upcoming events with approval status, remaining budget, recent notifications) match FR3 and UC2; access by every club role matches FR2/FR3.

### Lab 5 – Object Modeling and Class Diagrams
Each System action maps to an operation or attribute already in the consolidated class diagram — no new classes or operations were introduced:

| Activity Diagram action | Class Diagram operation / attribute |
|---|---|
| Create OnCampusEvent and validate details | `OnCampusEvent` constructor, `Event.validate(): List<String>` |
| Submit proposal | `Event.submit()` |
| Run clash check: retrieve approved events | `OnCampusEvent.runClashCheck()`, `Room.getApprovedEvents(date)` |
| Check overlap with next approved event | `Event.overlaps(other): boolean` |
| Determine clash result | `ClashCheck.run(): boolean`, `ClashCheck.hasClash()` |
| Show clash warning | `ClashCheck.getWarningMessage(): String` |
| Override check and flag clash | `ClashCheck.overrideAndSubmit()` |
| Get building's room allocator | `Building.getRoomAllocator(): FacultyMember` |
| Set status to PENDING | `Event.updateStatus(newStatus, expectedVersion)` |
| Create notification for room allocator | `Notification.createForEvent(recipient, event, message)` |
| Create REJECTED / CHANGES_REQUESTED / APPROVED decision record | `BudgetRequest.reject(...)`, `requestChanges(...)`, `approve(...)` → `BudgetDecisionRecord` |
| Check club has sufficient funds | `ClubBudget.hasSufficientFunds(amount): boolean` |
| Allocate requested amount / recalculate remaining balance | `ClubBudget.allocate(amount)`, derived `ClubBudget./remainingBalance` |
| Notify treasurer of budget decision | `Notification.createForBudget(recipient, request, message)` |
| Get event status | `Event.getStatus()` |
| Get remaining club budget balance | `ClubBudget.getRemainingBalance(): Decimal` |
| Check whether notification is unread | `Notification.isUnread(): boolean` |
| Load data / Create dashboard summary | `ClubMember.generateDashboard(events, budgets, notes): DashboardSummary` |

### Lab 6 – Sequence Diagram Modeling
- **Same use cases:** each Activity Diagram represents exactly the same use case as its Sequence Diagram (UC11 ↔ SD_UC11, UC07 ↔ SD_UC07, UC02 ↔ SD_UC02).
- **Same branching logic:** the `alt [errors.isEmpty()]`, `alt [!clashCheck.hasClash()]`, `alt [decision = …]` and `alt [sufficientFunds]` fragments appear as decision nodes with equivalent guards; the `loop [* for each approvedEvent]`, `loop [* for each event]` and `loop [* for each notification]` fragments appear as merge/decision loops; the `opt [status == APPROVED || status == PENDING]` and `opt [isUnread]` fragments appear as `[APPROVED or PENDING]` / `[unread]` branches.
- **Complementary, not contradictory:**
  - UC11 adds the `[change details]` path, which the sequence diagram did not draw but which FR12 requires ("the booker can change the details or submit anyway"). The sequence diagram models the submit-anyway path only; the activity diagram shows both, so the two do not conflict.
  - UC02 shows the three aggregations as a fork/join because they are independent; the sequence diagram shows them in one valid order (events → budget → notifications). Running them in that order is one legal execution of the fork, so the diagrams agree.
  - UC11 / UC07 show the notification and the response to the initiating user as parallel branches; in the sequence diagrams these are the `«create» Notification` message and the reply message, which have no dependency on each other.

### Confirmation
**The team confirms that all of the consistency checks listed above — against Lab 3 (requirements and use cases), Lab 5 (class diagram) and Lab 6 (sequence diagrams) — have been performed and verified, and no mismatches remain.** No earlier artifacts needed to be revised for Lab 7.

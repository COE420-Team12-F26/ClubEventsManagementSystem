# Individual Class Inventory — Budget Requests Component

**Student:** Abdelrahman Ahmed Elsayed
**Component:** Budget Requests (DFD processes 4.0 / 5.0, data stores D3 Budget Requests & History, D4 Club Budget)

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Contributor |
|---|---|---|---|---|
| C01 | BudgetRequest | Represents a single budget request submitted for an approved event: the requested amount, its cost breakdown, and its current status (Pending, Approved, Rejected, Changes Requested). | UC06, UC07, UC09 | Abdelrahman Ahmed Elsayed |
| C02 | CostBreakdownItem | Represents one line item within a budget request's cost breakdown (e.g., venue booking, snacks, equipment rental). | UC06 | Abdelrahman Ahmed Elsayed |
| C03 | ClubBudget | Tracks a club's total budget, the amount already allocated to approved requests, and the remaining balance; updates automatically when a request is approved. | UC08 | Abdelrahman Ahmed Elsayed |
| C04 | BudgetDecisionRecord | An unchangeable audit-log entry capturing one decision (approve/reject/request changes) made on a budget request — who decided, when, and any comments. | UC07, UC09, UC10 | Abdelrahman Ahmed Elsayed |
| C05 | Event *(stub)* | Minimal reference to the event a budget request is tied to. Full class owned by the Event Management component (Haziq) — included here only as the association target. | UC06 | Abdelrahman Ahmed Elsayed (stub; full class owned by Haziq Khalid) |
| C06 | ClubMember *(stub, superclass)* | Minimal reference to a club member who can hold a role. Full class owned by the Accounts & Roles component (Ahmed) — included here only so Treasurer/President can specialize it. | UC06, UC07 | Abdelrahman Ahmed Elsayed (stub; full class owned by Ahmed Abdelmohsen) |
| C07 | Treasurer *(stub subclass of ClubMember)* | The role that submits budget requests. | UC06 | Abdelrahman Ahmed Elsayed (stub; full class owned by Ahmed Abdelmohsen) |
| C08 | President *(stub subclass of ClubMember)* | The role that reviews and decides on budget requests. | UC07 | Abdelrahman Ahmed Elsayed (stub; full class owned by Ahmed Abdelmohsen) |

## Notes on stub classes

C05–C08 are **not** fully modeled here on purpose — they belong to teammates' components (Event Management: Haziq; Accounts & Roles: Ahmed) and only appear in this diagram as association endpoints, so the Budget Requests model is traceable and so Part B integration has something to merge against. They should be dropped/merged with the full versions of these classes during team integration (Exercise 3), not duplicated in the final consolidated diagram.

## Justification for relationships

- **Inheritance** (`ClubMember <|-- Treasurer`, `ClubMember <|-- President`): both roles genuinely *are a* ClubMember (specialization), and both are the exact roles used directly in this component's associations (Treasurer submits requests, President decides on them) — not added artificially.
- **Composition** (`BudgetRequest *-- CostBreakdownItem`): a cost breakdown item has no independent existence — it only exists as part of a specific budget request (FR6: "a breakdown of expected costs").
- **Associations with multiplicity**: reflect FR6–FR10 and the Scenarios/NFRs directly (e.g., one event can have multiple budget requests over time if resubmitted; one request accumulates a history of decision records per the Auditability NFR).

# Ahmed — Class Inventory (Exercise 1)

**System:** Club Events Management System — COE 420 Team 12  
**Contributor:** Ahmed Abdelmohsen  
**Scope:** FR1–FR5, NFR1–NFR5, UC1–UC5; P1/P6; D1 and D5.

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Contributor |
|---|---|---|---|---|
| C1 | UserAccount | Holds AUS account credentials (hashed), active status and assigned role; authenticates users and produces authorized personalized dashboard information. D1/P1/P6. | UC1, UC2, UC4, UC5 | Ahmed |
| C2 | ClubRole (abstract) | Common role permissions and policy; assigned to accounts in D1. | UC1, UC4, UC5 | Ahmed |
| C3 | PresidentRole | President-specific role permissions including role management. | UC1, UC2, UC4, UC5 | Ahmed |
| C4 | VicePresidentRole | Vice President permissions for event assistance and activity monitoring. | UC1, UC2, UC4 | Ahmed |
| C5 | ExecutiveRole | Executive permissions limited to assigned event responsibilities. | UC1, UC2, UC4 | Ahmed |
| C6 | TreasurerRole | Treasurer permissions for budget requests and budget information. | UC1, UC2, UC4 | Ahmed |
| C7 | Notification | Stores a recipient's event, budget, cancellation or action alerts and their read status; maps to D5/P6. | UC2, UC3 | Ahmed |

## Modeling notes

- The four concrete roles **inherit** from abstract `ClubRole`; `UserAccount` is associated with **exactly one** club role at a time and an individual role may be used by many accounts. Roles are not subclasses of users, because an account can change role without changing identity.
- The personalized dashboard is a **derived view** (`DashboardSummary` return type from `UserAccount.generateDashboard(...)`), built using the authorized account's events, budget requests and notifications. It is not a separate persistent class or DFD store.
- External `Event` and `BudgetRequest` are **stubs**, owned by Haziq and Abdelrahman respectively; these are excluded from Ahmed's seven-class inventory.
- Account (1) receives Notifications (0..*); each notification belongs to exactly one account. Each Notification may refer to 0..1 Event or 0..1 BudgetRequest, while each event or budget request can cause 0..* notifications.
- Passwords must be hashes, not plaintext; permission checks (including authorized role updates) are enforced server-side (FR1/FR2/FR5; NFR1). Dashboard, account and notification views should load within 3 seconds under normal conditions (NFR2). Reliability, usability and responsive browser compatibility remain design constraints (NFR3–NFR5).

Open `Ahmed_Class_Diagram.drawio` using https://app.diagrams.net/ for the editable UML class diagram.

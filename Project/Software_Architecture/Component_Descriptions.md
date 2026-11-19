# Lab 8 – Component Descriptions

[Design Goals](Architectural_Design_Goals.md) | [Style Selection](Architectural_Style_Selection.md) | [Class Distribution](Class_Distribution.md) | [Component Diagram](Component_Diagram/Software_Architecture_Component_Diagram.pdf)

The system is divided into five components. Two external systems are also used: the users' Web Browser and the Club Events Database (relational DBMS).

| ID | Component Name | Responsibilities and Supported Functionality | Layer / Owner |
|---|---|---|---|
| CO1 | Web Presentation | Role-based web pages and controllers; receives browser requests, checks input format and shows field errors (NFR14), calls the business components, and builds the role-based dashboard (FR3, UC2, P6). Supports the user side of UC1–UC15; NFR3, NFR5, NFR13. | Presentation / whole team |
| CO2 | Accounts & Roles | Authenticates users (FR1); stores accounts, clubs and club roles and answers role permission checks on the server (FR2, NFR1, NFR12); role changes by the President (FR5). UC1, UC4, UC5 · P1 · D1. | Core Services / Ahmed |
| CO3 | Event Proposal Management | Creates, validates, submits, edits, resubmits and cancels proposals (FR11, FR14, FR15); runs the room clash check (FR12); routes proposals to the room allocator or faculty advisor and records their decisions (FR13); refuses stale actions (NFR15). UC11–UC15 · P2, P3 · D2. | Domain / Haziq |
| CO4 | Budget Management | Budget request submission with cost breakdown (FR6); President's approve / reject / return workflow (FR7); fund check, allocation and remaining balance (FR8, NFR6); request history and budget summary (FR9, FR10). UC6–UC10 · P4, P5 · D3, D4. | Domain / Abdelrahman |
| CO5 | Notification & Audit Services | Creates, lists and marks notifications (FR4, UC3, UC10); defines the shared decision record rules and keeps the read-only audit trail of all event and budget decisions (FR9, NFR7). P6 · D5. | Core Services / Ahmed + team |

## Interfaces

Each component provides one interface and the database provides `IPersistence`, giving six interfaces in total. Their operations, parameter types and return types are defined on page 2 of the [component diagram](Component_Diagram/Software_Architecture_Component_Diagram.pdf).

| Interface | Provided by | Required by |
|---|---|---|
| `IWebUI` | CO1 | Web Browser |
| `IAccountService` | CO2 | CO1, CO3, CO4 |
| `IEventService` | CO3 | CO1, CO4 |
| `IBudgetService` | CO4 | CO1 |
| `INotificationAudit` | CO5 | CO2, CO3, CO4 |
| `IPersistence` | Database (external) | CO2, CO3, CO4, CO5 |

**Component dependencies:** Web Browser → CO1 (`IWebUI`); CO1 → CO2, CO3, CO4; CO3 → CO2, CO5; CO4 → CO3 (a budget request needs an APPROVED event), CO2, CO5; CO2 → CO5 (to read notifications); CO2, CO3, CO4, CO5 → Database (`IPersistence`). All dependencies point down the layers, or one way inside a layer, so there are no cycles.

# Lab 8 – Architectural Style Selection

[Design Goals](Architectural_Design_Goals.md) | [Component Descriptions](Component_Descriptions.md) | [Class Distribution](Class_Distribution.md) | [Component Diagram](Component_Diagram/Software_Architecture_Component_Diagram.pdf)

## 1. Selected Style

**Layered architecture** (three-tier: Presentation – Business Logic – Data), used as a web client–server system.

| Layer | Contents | Components |
|---|---|---|
| Presentation | Role-based web pages, request controllers and the dashboard view, reached from the browser over HTTPS | CO1 Web Presentation |
| Business Logic – Domain | The two main workflows: event proposals and budget requests | CO3 Event Proposal Management, CO4 Budget Management |
| Business Logic – Core Services | Services every workflow relies on: accounts, roles and access checks; notifications and the decision audit trail | CO2 Accounts & Roles, CO5 Notification & Audit Services |
| Data | Club events database holding data stores D1–D5 from the Level-0 DFD | External relational DBMS (`IPersistence`) |

## 2. Main Characteristics

- A layer only uses the layers below it, through their provided interfaces. Nothing calls upward.
- Relaxed layering: the presentation component may call a core service directly (for example `IAccountService` for login).
- Inside a layer, calls go one way only: Budget Management uses `IEventService` from Event Proposal Management to check that an event is approved (FR6), never the reverse. There are no cyclic dependencies.
- Each component provides one interface, and components only meet through its operations.
- Thin client: the browser only displays pages; every rule (permissions, clash check, budget checks) runs on the server.

## 3. Why It Is Appropriate

- The system is a web information system: users fill in forms, the server applies rules and the data is saved in a database. This is the problem the three-tier layered style is designed for.
- The Level-0 DFD already separates the processes (P1–P6) from the data stores (D1–D5). The business components carry out the processes and the data layer holds the stores.
- The team's three areas from Labs 3 and 5 (accounts/dashboard, events, budget) become separate business components, plus one shared services component, so each member can build and test their part behind its interfaces.
- For a semester project with a few hundred users, one layered web application is enough; separately deployed services would add work without benefit.

## 4. How It Supports the Design Goals

| Design Goal | How the layered style supports it |
|---|---|
| DG1 Security | Every request enters through CO1 and every business operation first calls `IAccountService.hasAccess()` in CO2; CO3 also checks that a reviewing faculty member is the event's approver (`Event.getApprover()`). Checks run on the server, so a request sent straight to the server is still checked (NFR12). The database is only reachable from the business layer. |
| DG2 Maintainability | Each area is a separate component and others only see its interface. A new budget approval step (NFR10) changes CO4 only, as long as `IBudgetService` stays the same. The user interface can change without touching business rules. |
| DG3 Reliability | All writes go through the business layer, which validates them (`Event.validate()`, `ClubBudget.hasSufficientFunds()`) before calling `IPersistence`. CO5 only appends decision records (`INotificationAudit.recordDecision()`), so they are never edited (NFR7). `expectedVersion` parameters stop one user from overwriting another's action (NFR15). |
| DG4 Performance | A request passes through at most three layers, a small cost for the 2–3 second targets. The dashboard is built from a few query calls and is never stored. |
| DG5 Scalability | Business components keep no per-user state between requests, so more copies can run behind a load balancer. Every club-wide list query takes a `clubId`, keeping each club's data separate. |

## 5. Advantages and Limitations

- **Advantage** – clear separation of user interface, business rules and stored data.
- **Advantage** – easier maintenance and testing: each component can be tested through its interfaces.
- **Advantage** – one place for role checks (CO2), used by every workflow.
- **Advantage** – matches the team's work split and the earlier DFD, class, sequence and activity models.
- **Limitation** – extra calls between layers can slow requests that touch many components (the dashboard calls three components). These are simple reads and fit the 3-second target.
- **Limitation** – a change to a core interface (for example `IAccountService`) affects every component above it, so core interfaces are kept stable and only extended.
- **Limitation** – one database and application server are a single point of failure (NFR8); handled with regular backups and the option of running more copies of the stateless business layer.
- **Limitation** – layers can be bypassed over time; the component diagram is the reference and every new dependency must point downward through a provided interface.

**Alternatives considered:** MVC on its own only organizes the user interface (it is used inside CO1); the Repository style would let every component write shared data directly, which weakens DG1 and DG3; Microservices are too heavy for a three-person semester project.

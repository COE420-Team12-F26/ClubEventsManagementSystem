# DFD Consistency and Balancing Check

Diagrams: [Context Diagram](Context_Diagram/Context_Diagram.png) | [Level-0 DFD](Level_0_DFD/Level_0_DFD.png) | [Core Processes Matrix](Core_Processes_Matrix.md)

## External Entities vs. Use Case Actors

| External Entity | Use Case Actors | Use Cases |
|---|---|---|
| Club Executive (President, Vice President, Executive, Treasurer) | President, Vice President, Executive, Treasurer | UC1-UC11, UC14, UC15 |
| Faculty Member (Event Approver) | Faculty Member | UC12, UC13 |

Both entities appear in the Context Diagram and in the Level-0 DFD.

## Balancing (Context Diagram -> Level-0 DFD)

| Context Diagram Flow | Direction | Level-0 Process |
|---|---|---|
| Login / Role Update | Club Executive -> System | 1.0 Authenticate Users & Manage Roles |
| Event Proposal / Update | Club Executive -> System | 2.0 Manage Event Proposals |
| Budget Request / Decision | Club Executive -> System | 4.0 Manage Budget Requests, 5.0 Review Budget Requests & Update Club Budget |
| Dashboard / Budget View Requests | Club Executive -> System | 6.0 Generate Dashboard & Notifications |
| Auth & Role Info | System -> Club Executive | 1.0 Authenticate Users & Manage Roles |
| Event Status / Clash / Feedback | System -> Club Executive | 2.0 Manage Event Proposals |
| Budget Status / Summary / History | System -> Club Executive | 4.0 Manage Budget Requests |
| Dashboard & Notifications | System -> Club Executive | 6.0 Generate Dashboard & Notifications |
| Login | Faculty Member -> System | 1.0 Authenticate Users & Manage Roles |
| Review Decision & Comments | Faculty Member -> System | 3.0 Review Event Proposals |
| Auth & Access | System -> Faculty Member | 1.0 Authenticate Users & Manage Roles |
| Event Details & Clashes | System -> Faculty Member | 3.0 Review Event Proposals |

## Processes vs. Requirements

| Level-0 Process | Functional Requirements | Use Cases |
|---|---|---|
| 1.0 Authenticate Users & Manage Roles | FR1, FR2, FR5 | UC1, UC4, UC5 |
| 2.0 Manage Event Proposals | FR11, FR12, FR15 | UC11, UC14, UC15 |
| 3.0 Review Event Proposals | FR13, FR14 | UC12, UC13 |
| 4.0 Manage Budget Requests | FR6, FR9 | UC6, UC9 |
| 5.0 Review Budget Requests & Update Club Budget | FR7, FR8, FR10 | UC7, UC8, UC10 |
| 6.0 Generate Dashboard & Notifications | FR3, FR4 | UC2, UC3 |

## Checklist

- [x] The Context Diagram shows the whole system as one process (0.0) with no internal processes or data stores.
- [x] External entities are consistent with the UML Use Case actors.
- [x] The Level-0 DFD uses the same external entities.
- [x] The Context Diagram and Level-0 DFD are balanced (see table above).
- [x] The Level-0 DFD has 6 processes (1.0-6.0).
- [x] The Level-0 DFD has 5 data stores: D1 User Accounts & Roles, D2 Event Proposals, D3 Budget Requests & History, D4 Club Budget, D5 Notifications.
- [x] The Core Processes Matrix contains the same processes (P1-P6 = 1.0-6.0).
- [x] DFD processes are consistent with the Functional Requirements and Use Cases.

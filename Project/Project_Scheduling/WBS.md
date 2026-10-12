# Work Breakdown Structure (WBS)

| ID | Task Name | Description | Predecessor(s) | Duration (Days) |
|---|---|---|---|---|
| T1 | Requirements Refinement & System Design | Finalize roles, statuses and rules. Design the database schema. Set up the repository. | - | 7 |
| T2 | Authentication & Role Management | Secure AUS email login, server-side role-based access checks, and assigning/changing club roles (FR1, FR2, FR5). | T1 | 5 |
| T3 | Event Management Module | Event proposals with validation and routing, room clash check, faculty approve/reject/request changes, resubmission and cancellation (FR11-FR15). | T2 | 10 |
| T4 | Budget Management Module | Budget requests for approved events, President approval with audit log, remaining-balance updates, budget summary and history (FR6-FR10). | T2 | 8 |
| T5 | Notifications & Role-Based Dashboard | Notifications for event and budget decisions/changes, and the role-based dashboard of events, statuses and budget (FR3, FR4). | T3, T4 | 5 |
| T6 | Integration, Testing & Deployment | Integrate all modules, run system and regression tests, fix defects, deploy the web app and write the user guide. | T5 | 6 |

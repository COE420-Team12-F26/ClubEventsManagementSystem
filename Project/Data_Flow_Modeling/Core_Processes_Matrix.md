# Core Processes Matrix

The processes below are the same six major processes shown in the [Level-0 DFD](Level_0_DFD/Level_0_DFD.png).

| Process No. and Name | Input Data | Output Data | Rules |
|---|---|---|---|
| P1 - Authenticate Users & Manage Roles | Login credentials, role updates, stored account information | Authentication results, role information, updated account records | Validate credentials against stored user accounts. Apply role changes only when permitted. |
| P2 - Manage Event Proposals | New event proposals, event updates, existing event information, room and building reference data (D2) | Stored event proposals, event status, clash information, event update notices | Check required event information. Save new or updated proposals for review. |
| P3 - Review Event Proposals | Pending proposals, clash data, faculty review decisions and comments | Event approval status, review feedback, event decision notifications | Faculty approver reviews proposals and clash information. Record decisions and update event status. |
| P4 - Manage Budget Requests | Budget request details, relevant event information | Pending budget requests, submission status, request history | Validate request details, register submissions, and maintain their pending status until a decision is made. |
| P5 - Review Budget Requests & Update Club Budget | Pending requests, request history, budget information, budget decisions | Budget decisions, updated allocations, audit history, budget summaries | Check remaining funds before approving allocations. Record decisions and update allocated and remaining budgets accordingly. |
| P6 - Generate Dashboard & Notifications | Dashboard requests, event updates, budget status, decisions, notification history | Dashboard information, event and budget notifications, stored notification records | Display relevant information according to user access. Generate notifications when event or budget statuses change. |

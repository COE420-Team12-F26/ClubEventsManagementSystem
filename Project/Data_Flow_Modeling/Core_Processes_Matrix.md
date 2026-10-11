# Exercise 3: Core Processes Matrix

**Project:** Club Events Management System  
**Course:** COE 420L – Software Engineering, Lab 4  
**Team:** Team 12

The following processes correspond to the six major processes numbered 1.0 through 6.0 in the team's Level-0 Data Flow Diagram. Process rules should be verified against the team's functional requirements and use cases before final submission.

| Process No. and Name | Input Data | Output Data | Rules |


1.0 Authenticate Users & Manage Roles** | Login credentials; role updates; existing user account and role information from D1 | Authentication and access results; role information; updated account and role records in D1 | Check supplied credentials against stored account data. Authorize access according to assigned roles. Apply role updates only when permitted and save valid changes. |

2.0 Manage Event Proposals** | Event proposals and updates from Club Executive; existing proposal information; approved event information | Event proposal records in D2; event status and clash information; event update notices | Validate required proposal details. Store new or revised event proposals for review. Provide the latest proposal status and relevant clash feedback. |

3.0 Review Event Proposals** | Pending proposals and clash data from D2; review decisions and comments from Faculty Member (Event Approver) | Event details and clashes for review; updated event decisions and statuses in D2; review feedback and decision notices | The faculty approver reviews the proposal and available clash information. Record the decision and comments. Update the proposal's approval status and communicate the outcome. 

4.0 Manage Budget Requests** | Budget request details from Club Executive; relevant approved-event information | Pending budget requests in D3; submission status; request records and notices | Verify that a budget request has the required information. Record it as pending until reviewed. Track submission status and request history. 

5.0 Review Budget Requests & Update Club Budget** | Pending requests and history from D3; available budget information from D4; budget decisions and comments | Decisions and audit history in D3; budget allocation updates in D4; budget decision and summary; decision notices | Review requests against available funds. Record approval or rejection and relevant comments. For approved allocations, update allocated and remaining budget without exceeding available funds. 

6.0 Generate Dashboard & Notifications** | Dashboard and budget-view requests; event updates, statuses and decisions; budget status and history; previously stored notifications from D5 | Dashboard and budget summaries; event and budget notifications for Club Executive; new notifications stored in D5 | Compile current event and budget information for authorized users. Generate appropriate notices when relevant statuses change. Store new notices and retrieve notification history when needed. 
# Project Scope

## Project Objective

University clubs currently manage events and budgets using a mix of WhatsApp groups, shared spreadsheets, and email, with advisor approvals often happening informally or getting lost across chats. This makes it hard to track which events are actually approved, what a club's remaining budget is, and who is responsible for what. Our project, the **Club Events Management System**, aims to fix this by giving clubs a single web-based platform where officers can propose events and request budgets, advisors can review and approve/reject them, and everyone involved can see the current status in one place instead of chasing people down.

## Target Users

- **Club Officers/Presidents** – create events, submit budget requests for those events, and manage their club's members.
- **Club Members** – view upcoming events and their status, and see general club announcements.
- **Faculty/Club Advisors** – review and approve or reject event proposals and budget requests submitted by their assigned clubs.
- **Student Council / University Administration** *(secondary user)* – may need an overview of club activity and total budget usage across clubs, depending on how far we get.

## In-Scope Features

- User accounts with role-based access (officer, member, advisor).
- Event proposal creation (title, description, date, location) by officers.
- Advisor approval workflow for events (approve, reject, or request changes).
- Budget request submission tied to a specific event.
- Advisor approval workflow for budget requests, with running totals of allocated vs. remaining club budget.
- Status tracking so members/officers can see whether an event or budget request is pending, approved, or rejected.
- Basic notifications (in-app, or email if time allows) when an approval decision is made.
- A simple dashboard showing upcoming approved events and current budget standing per club.

## Out-of-Scope Features

- Actual payment processing or integration with the university's real financial/banking systems — the system will track approved budget *amounts*, not move real money.
- A native mobile app (we're aiming for a responsive web app that works reasonably well on mobile browsers instead).
- Multi-university or multi-language support.
- Real-time chat/messaging between members (basic announcements only, not a full chat system).
- Advanced analytics/reporting beyond the basic dashboard described above.

These may be revisited later in the project if time and scope allow, but they are not part of the current planned deliverable.

## Major Deliverables

- A working web application implementing the in-scope features above (front end + back end + database).
- Source code hosted and version-controlled in this GitHub repository.
- Supporting documentation: this scope document, process model, risk register, stakeholder analysis, and feasibility study, plus requirements/design documents produced in later labs.
- A short user guide covering how officers, members, and advisors use the system.
- A final demo/presentation of the working system.

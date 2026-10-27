# Individual Class Inventory: Event Creation, Cancellation, and Approval

**Student:** Haziq Mohammad Khalid (@100601)
**Component:** Event Creation, Cancellation, and Approval
**Derived from:** FR11–FR15, NFR11–NFR15, UC11–UC15, Scenarios 1–2 (Haziq), Core Processes P2 (Manage Event Proposals) and P3 (Review Event Proposals), Level-0 DFD data store D2 (Event Proposals)

## Exercise 1: Candidate Classes

| Class ID | Class Name | Purpose/Responsibility | Related Use Case(s) | Contributor |
|---|---|---|---|---|
| C01 | Event *(abstract)* | The event proposal itself, and the main record held in D2 (Event Proposals). Stores the details common to every event: title, description, date, start and end time, expected attendees, status (Pending, Changes Requested, Approved, Rejected, Cancelled) and cancellation reason. Checks its own form input (no past date, end time after start time, title not blank, attendees > 0, as NFR14 requires). Handles submit, edit and resubmit, and cancel (only before the event starts, and a reason is required). Cancelled events are kept, not deleted. | UC11, UC12, UC13, UC14, UC15 | Haziq Khalid |
| C02 | OnCampusEvent | A kind of Event that is held in a university room. Holds the booked Room. When submitted, it runs a room clash check and is sent to the faculty member in charge of allocating rooms in that room's building. | UC11, UC12, UC14 | Haziq Khalid |
| C03 | OffCampusTrip | A kind of Event that takes place outside the university. Holds the destination. It is sent to the club's faculty advisor. When the trip is approved, it records that advisor as the faculty member going with the students (FR13). | UC11, UC12, UC14 | Haziq Khalid |
| C04 | Room | A bookable university room (for example EB2-130) chosen from a list when making an on-campus event. Holds the room code and capacity. Gives the clash check the approved events already booked in that room. | UC11 | Haziq Khalid |
| C05 | Building | A university building (for example EB1) that contains Rooms and has one faculty member in charge of allocating its rooms. Decides which faculty member gets an on-campus proposal. Supports the server-side check in NFR12 that only the building's room allocator may decide on that booking. | UC11, UC12 | Haziq Khalid |
| C06 | ClashCheck | Represents one run of the location clash check when an on-campus proposal is submitted. Lists the approved events in the same room at an overlapping time, with each event's name and time. Records whether the booker chose to "submit anyway". If they did, the warning stays attached to the proposal so the faculty reviewer sees it ("Pending Event & Clash Data" flow, D2 → P3). | UC11, UC12 | Haziq Khalid |
| C07 | EventReview | One review decision by a faculty member on a pending proposal: Approved, Rejected or Changes Requested. It records the reviewer, the time, and the comments or reason. A rejection cannot be saved without a reason. An event can collect several reviews across return and resubmit cycles, so earlier comments stay visible on the proposal (FR14). Each decision updates the event's status and triggers an event decision notice to P6. | UC12, UC13, UC14 | Haziq Khalid |

### Inheritance

`OnCampusEvent` and `OffCampusTrip` both specialize `Event`. This is a real generalization, not one added to meet the lab requirements. The two types share every common field and the whole status lifecycle. They differ in:
- **Location data:** a Room for an on-campus event, a destination for an off-campus trip.
- **Who reviews it:** the building's room allocator for an on-campus event, the club's faculty advisor for a trip.
- **Extra behaviour:** an on-campus event runs a clash check, and an approved trip records the accompanying faculty member.

## Referenced Classes Owned by Teammates (stubs only)

These classes are connected to my component, but they are modeled in my teammates' diagrams. In my class diagram they will appear as stubs.

| Class | Owner | Why my component references it |
|---|---|---|
| ClubMember (President / Vice President roles) | Ahmed Abdelmohsen | Only the President or Vice President of the event's club may create, edit, resubmit, or cancel an event (FR11, FR15, NFR12). |
| FacultyMember | Ahmed Abdelmohsen (user accounts, D1) | The reviewer on an EventReview. A faculty member can be a Building's room allocator, a Club's faculty advisor, or both. |
| Club | Ahmed Abdelmohsen | Owns its events and names its faculty advisor, who reviews the club's off-campus trips. |
| Notification | Ahmed Abdelmohsen (D5) | Created when an event is approved, rejected, returned, or cancelled (P3/P2 → P6). |
| BudgetRequest | Abdelrahman Ahmed Elsayed | A budget request can only be linked to an approved Event (FR6). |

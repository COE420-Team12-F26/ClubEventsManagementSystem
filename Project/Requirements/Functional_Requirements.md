Ahmed Abdelmohsen-Accounts, Roles, Notifications, and Dashboard

1\. User Login:  
The system shall allow registered club members to securely log in using their AUS email/username and password.

2\. Role-Based Access :  
The system shall assign users one of the club roles:  
President, Vice President, Executive, or Treasurer. The system shall provide different permissions depending on the user's role. For example:  
President: Can view and manage club event requests and monitor club activities.  
Vice President: Can assist with managing events and monitoring club activities.  
Executive: Can view and work on club events depending on their assigned responsibilities. Treasurer: Can manage and monitor budget requests and the club's available budget.

3\. Personalized Dashboard:  
The system shall display a dashboard based on the user's role. The dashboard may include: Upcoming events, Event approval status, Budget request status, remaining club budget, and  Recent notifications

4\. Notifications :  
The system shall notify club members when important actions occur, such as: An event request is approved or rejected; a budget request is approved or rejected; an event is updated or canceled; an action is required from a club member.

5\. Account and Role Management:  
The system shall maintain an account for each club member and store their assigned club role. Authorized users shall be able to update club member roles when positions change.



\## Abdelrahman Ahmed Elsayed — Budget Requests



6\. Submit Budget Request:

The system shall allow the Treasurer (or an authorized club officer) to submit a budget request for an approved event, specifying the requested amount and a breakdown of expected costs.



7\. Budget Approval Workflow:

The system shall allow the President to review a submitted budget request and either approve it, reject it, or return it with comments requesting changes, before any funds are marked as allocated.



8\. Running Budget Balance:

The system shall automatically update and display the club's remaining budget whenever a budget request is approved, so members can see up-to-date allocated vs. remaining funds at any time.



9\. Budget History Log:

The system shall keep a record of all submitted budget requests for a club, including their status (pending, approved, rejected) and the event they are linked to.



10\. Budget Summary View:

The system shall provide a summary view showing the club's total budget, amount already allocated to approved events, and remaining balance, accessible to the Treasurer and President.



## Haziq Khalid - Event Creation, Cancellation, and Approval



11\. Create Event Proposal:

The system shall allow the club President or Vice President to create an event proposal with a title, description, date, start and end time, location, and expected number of attendees. The proposal is marked as either an on-campus event, where the room is picked from a list of university rooms, or an off-campus trip. On-campus events are sent to the faculty member in charge of allocating rooms in that building and off-campus trips are sent to the club's faculty advisor. A submitted proposal gets the status "Pending".



12\. Location Clash Check:

When an on-campus event proposal is submitted, the system shall check it against all approved events and warn the booker if one is booked in the same room at an overlapping time, showing that event's name and time. The booker can change the details or submit anyway. If they submit anyway, the clash warning is shown to the faculty member in charge of allocating rooms on the proposal.



13\. Approve or Reject Event Proposal:

The system shall allow a faculty member to approve or reject the pending proposals sent to them, which are room bookings for the building they are in charge of, or trips from the clubs they advise. A rejection cannot be saved without a written reason, and that reason is shown to the club's members on the event page. When a faculty advisor approves a trip, the system lists them on the event as the faculty member going with the students.



14\. Request Changes to Event Proposal:

The system shall allow the faculty member to send a pending proposal back to the club with comments, which sets its status to "Changes Requested". The President or Vice President can then edit the proposal and resubmit it, which sets it back to "Pending". The faculty member's earlier comments stay visible on the proposal.



15\. Cancel Event:

The system shall allow the club President or Vice President to cancel a pending or approved event any time before it starts, after entering a cancellation reason. Cancelled events stay in the club's event list with the status "Cancelled" and are not deleted unless manually deleted.


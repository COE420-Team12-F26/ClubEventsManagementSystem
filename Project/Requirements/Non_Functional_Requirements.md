Ahmed Abdelmohsen Accounts, Roles, Notifications, and Dashboard

1\. Security:  
The system shall protect user accounts and club information through secure authentication and role-based access control. Users shall only be able to access features permitted for their assigned role.

2\. Performance:  
The dashboard, account information, and notifications shall load within 3 seconds under normal operating conditions.

3\. Usability:  
The system shall provide a simple and easy-to-understand interface so that the President, Vice President, Executives, and Treasurer can quickly access the information they need.

4\. Reliability:  
The system shall correctly save account information, club roles, notifications, and dashboard information without losing data during normal operation.

5\. Compatibility:  
The system shall work correctly on common web browsers and support different screen sizes, including laptops, tablets, and mobile devices.



\## Abdelrahman Ahmed Elsayed — Budget Requests



6\. Accuracy:

The system shall ensure all budget calculations (allocated amounts, remaining balance) are numerically correct and update immediately after a request is approved.



7\. Auditability:

The system shall keep an unchangeable record of every budget request decision (who approved/rejected it and when) so club spending can be reviewed later.



8\. Availability:

The budget request and approval features shall be available at least 99% of the time during the semester, since clubs may need to act close to event deadlines.



9\. Scalability:

The system shall be able to handle budget requests and history for multiple clubs and dozens of events per semester without a noticeable drop in performance.



10\. Maintainability:

The budget approval logic shall be implemented so future changes (e.g., a new approval step or budget category) don't require major changes to unrelated parts of the system.



## Haziq Khalid - Event Creation, Cancellation, and Approval



11\. Response Time:

Submitting an event proposal (including the location clash check) or saving an approval decision on it shall take no more than 2 seconds when up to 50 users are on the system at the same time.



12\. Access Control:

The system shall check on the server side that a user is the President or Vice President of the event's club before letting them create, edit, or cancel it, and that a faculty member is in charge of the building the event is in, or is the club's faculty advisor for an off-campus trip, before letting them approve, reject, or return it. Requests that fail this check shall be refused even if they are sent straight to the server without going through the website.



13\. Learnability:

A club member using the system for the first time shall be able to fill in and submit an event proposal in under 5 minutes without help. A faculty member shall be able to approve or reject a proposal within 3 clicks of opening the pending list, not counting typing a reason.



14\. Robustness:

The event form shall not accept a date in the past, an end time before the start time, a blank title or location, or an attendee count of zero or less. Each error shall be shown next to the field that caused it, and whatever the user already entered shall stay in the form.



15\. Data Integrity:

If two users act on the same event at nearly the same time (for example, the President cancels it while the faculty is approving it), the system shall apply only the first action and tell the second user that the event has changed. The second action shall not silently overwrite the first.


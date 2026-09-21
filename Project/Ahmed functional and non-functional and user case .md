## User Story

### Story 1:

### Ahmed is an Executive in a student club at AUS. He logs into the Student Club Event Approval and Budget Request System using his account. After logging in, the system recognizes his role and displays his dashboard.

From the dashboard, Ahmed can see the club's upcoming events, submitted event requests, approval statuses, and recent notifications. He notices a notification that one of the club's events has been approved. He opens the notification and views the updated event information.

The dashboard only shows Ahmed the features and information that he is allowed to access based on his role in the club.

## Functional Requirements

### 1.User Login

The system shall allow registered club members to securely log in using their AUS email/username and password.

### 2.Role-Based Access

The system shall assign users one of the club roles: President, Vice President, Executive, or Treasurer.

The system shall provide different permissions depending on the user's role.

For example:

* President: Can view and manage club event requests and monitor club activities.  
* Vice President: Can assist with managing events and monitoring club activities.  
* Executive: Can view and work on club events depending on their assigned responsibilities.  
* Treasurer: Can manage and monitor budget requests and the club's available budget.

### 3\. Personalized Dashboard

The system shall display a dashboard based on the user's role.

The dashboard may include:

* Upcoming events  
* Event approval status  
* Budget request status  
* Remaining club budget  
* Recent notifications

### 4.Notifications

The system shall notify club members when important actions occur, such as:

* An event request is approved or rejected.  
* A budget request is approved or rejected.  
* An event is updated or canceled.  
* An action is required from a club member.

### 5\. Account and Role Management

The system shall maintain an account for each club member and store their assigned club role.

Authorized users shall be able to update club member roles when positions change.

## Non-Functional Requirements:

### 1\. Security

The system shall protect user accounts and club information through secure authentication and role-based access control.

Users shall only be able to access features permitted for their assigned role.

### 2\. Performance

The dashboard, account information, and notifications shall load within 3 seconds under normal operating conditions.

### 3\. Usability

The system shall provide a simple and easy-to-understand interface so that the President, Vice President, Executives, and Treasurer can quickly access the information they need.

### 4\. Reliability

The system shall correctly save account information, club roles, notifications, and dashboard information without losing data during normal operation.

### 5\. Compatibility

The system shall work correctly on common web browsers and support different screen sizes, including laptops, tablets, and mobile devices.

## 4\. Use Cases

### Log In

Actor: President / Vice President / Executive / Treasurer

Description:  
The club member enters their login credentials. The system verifies the account, identifies the user's club role, and gives access to the appropriate features.

### View Dashboard

Actor: President / Vice President / Executive / Treasurer

Description:  
The club member opens their dashboard and views information relevant to their role, such as upcoming events, event approval statuses, budget information, and notifications.

### View Notifications

Actor: President / Vice President / Executive / Treasurer

Description:  
The club member views notifications about important updates, including event approvals, event rejections, budget decisions, cancellations, or required actions.

### View Role-Based Features

Actor: President / Vice President / Executive / Treasurer

Description:  
The user logs into the system, and the system provides access to features based on their assigned club role. For example, the Treasurer can access budget-related information while an Executive may mainly access event-related information.

### 

### Manage Club Member Roles

Actor: President / Authorized User

Description:  
The authorized user updates the role of a club member when there is a change in the club leadership, such as assigning a member as President, Vice President, Executive, or Treasurer.


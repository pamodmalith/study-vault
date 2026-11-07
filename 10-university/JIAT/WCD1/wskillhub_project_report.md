# COVER PAGE
---

# 1. Introduction
The SkillHub Training Academy is a rapidly expanding professional training organisation that currently relies on manual administration processes to manage student registrations, course enrolments, and trainer information. To improve operational efficiency and automate learner services, management requested the development of a web-based training management portal. 

This report outlines the proposed solution: a Java EE web application built using Servlets, JavaServer Pages (JSP), and Jakarta EE Web Container features. Due to project constraints, the application strictly utilizes in-memory data structures and Java EE attribute scopes (Request, Session, Application) to manage state and data persistence, bypassing the use of a traditional Database Management System (DBMS).

---

# 2. Requirements Analysis

### Users (Actors)
*   **Students/Learners:** Users who register, view available training programs, enroll in courses, and check their profiles.
*   **Trainers:** Instructors who log in to view their assigned courses and see enrolled students.
*   **Administrators:** Staff who add new courses, view student registrations, and manage trainer profiles.

### Functions (Features)
*   **Authentication:** Secure login and logout utilizing Java EE session management.
*   **Registration:** Account creation for new students.
*   **Course Browsing:** Displaying available professional development programs.
*   **Enrollment:** Allowing students to register for specific courses.
*   **Dashboard:** Centralized hub showing a user's active enrollments or system overviews for admins.

### Business Requirements
*   **Automation:** Transitioning from manual administration to an automated web portal.
*   **Efficiency:** Allowing students to self-serve their registrations and enrollments.
*   **Access:** Providing secure, 24/7 online access to training information.

### Assumptions
*   **Data Persistence:** Data loss upon server restart is acceptable for this prototype, as all data will be stored in Application-scoped memory (`ServletContext`).
*   **Initial Data Load:** The system will be pre-populated with dummy courses and a default admin user upon server startup using a `ServletContextListener`.
*   **User Base Size:** The concurrent user base is small enough that in-memory Java collections (e.g., `ArrayList`, `ConcurrentHashMap`) will handle the load without memory overflow issues.

### Constraints
*   **No DBMS:** Strict prohibition of any database management system (SQL or NoSQL).
*   **Technology Stack:** Must strictly use Java EE Web Components (Servlets, JSP) and GlassFish 7.
*   **State Management:** Application state and session tracking must be handled entirely via Java EE attribute scopes (Request, Session, Application).

---

# 3. Application Planning

### System Purpose
The primary purpose of this project is to develop a web-based training management portal for SkillHub Training Academy. This system replaces the existing manual administration processes for managing training workflows.

### Scope
The scope of this project is limited to building a pure Java EE web application utilizing Servlets and JSP pages deployed on a GlassFish 7 server. 
*   **Included:** Implementation of authentication, session management, and request dispatching utilizing exactly 5 Servlets, 6 JSP pages, and 1 ServletContextListener. Data state will be maintained strictly using Java EE attribute scopes.
*   **Excluded:** The use of any Database Management System (DBMS), payment gateways, and external API integrations are strictly out of scope.

### Expected Benefits
*   **Operational Efficiency:** Drastically reduces the administrative overhead associated with manual student registrations and attendance tracking.
*   **Secure Accessibility:** Provides 24/7 secure online access to training information for both staff and students.
*   **Scalable Foundation:** Establishes a structured Java EE architecture that can easily be connected to a persistent database in future iterations.

---

# 4. Application Architecture

The application follows a simplified Model-View-Controller (MVC) architectural pattern, adapted for an in-memory data approach:

*   **Model (Data & Logic):** Standard Java classes (POJOs) representing entities like `User`, `Course`, and `Enrollment`. A centralized `DataStore` class containing static thread-safe `ConcurrentHashMap` collections serves as the mock database.
*   **View (Presentation):** 6 JavaServer Pages (JSP) handling the user interface. These pages use Expression Language (EL) and JSTL to dynamically render data.
*   **Controller (Routing):** 5 Java Servlets acting as controllers. They intercept HTTP requests (GET/POST), interact with the in-memory Model, manage sessions, and forward requests to the Views using `RequestDispatcher`.
*   **Initialization:** A `ServletContextListener` acts as the entry point during server startup to populate the Model with dummy data and attach it to the Application scope.

---

# 5. Web Application Structure

The project utilizes a standard Maven Web Application directory layout managed via IntelliJ IDEA Ultimate. The deployment architecture is packaged as a standard WAR.

```text
SkillHub-Portal/
├── src/
│   ├── main/
│   │   ├── java/ (Contains all Java source code)
│   │   │   ├── com.skillhub.controller/
│   │   │   │   ├── LoginServlet.java
│   │   │   │   ├── LogoutServlet.java
│   │   │   │   ├── RegistrationServlet.java
│   │   │   │   ├── CourseServlet.java
│   │   │   │   └── ProfileServlet.java
│   │   │   ├── com.skillhub.model/
│   │   │   │   ├── User.java
│   │   │   │   └── Course.java
│   │   │   ├── com.skillhub.data/
│   │   │   │   └── DataStore.java (In-memory collections)
│   │   │   ├── com.skillhub.listener/
│   │   │   │   └── AppContextListener.java
│   │   │   └── com.skillhub.filter/
│   │   │       └── AuthFilter.java
│   │   └── webapp/ (Contains all web resources)
│   │       ├── css/
│   │       │   └── style.css
│   │       ├── jsp/
│   │       │   ├── login.jsp
│   │       │   ├── register.jsp
│   │       │   ├── dashboard.jsp
│   │       │   ├── courses.jsp
│   │       │   ├── profile.jsp
│   │       │   └── error.jsp
│   │       ├── index.jsp (Redirects to login or dashboard)
│   │       └── WEB-INF/
│   │           └── web.xml (Deployment Descriptor)
├── pom.xml (Maven dependencies)
```

### Deployment Descriptor (web.xml)
The deployment descriptor is configured to define `index.jsp` as the default landing page, enforce a default session timeout of 30 minutes, declare the `ServletContextListener`, and handle global error pages (404/500). 

---

# 6. Servlet Development

The application utilizes exactly five Servlets to act as the Controller layer. All servlets are mapped using Jakarta EE 10 `@WebServlet` annotations.

1.  **`LoginServlet` (`/login`)**: Intercepts `POST` requests from the login form. It authenticates credentials against the in-memory user list. Upon success, it generates an `HttpSession` to track the user's logged-in state and redirects to the dashboard.
2.  **`LogoutServlet` (`/logout`)**: Intercepts `GET` requests when a user clicks logout. It calls `session.invalidate()` to destroy the session and redirects the user back to the login page.
3.  **`RegistrationServlet` (`/register`)**: Processes `POST` requests from the student sign-up form. It creates a new `User` object and appends it to the Application-scoped `DataStore` collection, then redirects to the login page.
4.  **`CourseListServlet` (`/courses`)**: Handles `GET` requests by retrieving the static list of available training programs from the `DataStore` and attaching them to the `HttpServletRequest` attributes before forwarding to `courses.jsp` for rendering.
5.  **`EnrollmentServlet` (`/enroll`)**: Processes `POST` requests containing a course ID. It retrieves the current user from the `HttpSession`, adds the selected course to the user's active enrollments, and redirects back to the dashboard.

---

# 7. JSP Development

The presentation layer is built exclusively using JavaServer Pages (JSP), relying entirely on Jakarta Expression Language (EL) and the Jakarta Standard Tag Library (JSTL) to maintain a clean MVC architecture without raw Java scriptlets.

1.  **`login.jsp`**: The entry point for user authentication. Displays error messages using `${errorMessage}`.
2.  **`register.jsp`**: A sign-up form allowing new students to create an account.
3.  **`dashboard.jsp`**: The central hub that retrieves the logged-in user object from the session scope (`${sessionScope.user}`) to display a personalized greeting and active enrollments.
4.  **`courses.jsp`**: Uses the JSTL `<c:forEach>` tag to iterate over a collection of courses attached to the request scope by the `CourseListServlet`.
5.  **`profile.jsp`**: Displays the detailed information of the currently logged-in user.
6.  **`error.jsp`**: A global error-handling page configured in `web.xml` to catch 404 and 500 status codes, providing a user-friendly fallback.

---

# 8. Web Container Features

### Component Lifecycle Management
The GlassFish 7 web container manages the lifecycle of all web components. It reads the `web.xml` to initialize the `AppContextListener` on startup. Servlets are created as singletons, and the container manages their `init()`, `service()`, and `destroy()` phases.

### Request Interception (Filters)
To enforce security, a Jakarta EE Web Filter (`AuthFilter`) is utilized. The container intercepts incoming requests to protected URLs (`/jsp/dashboard.jsp`, `/jsp/profile.jsp`, `/enroll`). If an active session is not found, it blocks access and redirects to the login page.

### Request Dispatching and Routing
The application leverages the container's routing capabilities:
*   **Forwarding:** Servlets process logic and use `request.getRequestDispatcher("/path.jsp").forward(request, response)` to pass request/response objects to a JSP server-side.
*   **Redirecting:** Used after state-changing actions (login, register) via `response.sendRedirect()` to prevent form resubmission errors.

---

# 9. Session Management

Because HTTP is intrinsically stateless, the application maintains user continuity using the Jakarta EE `HttpSession` API, tracked via a JSESSIONID cookie. 

*   **Creation:** When a user authenticates via the `LoginServlet`, a new session is established (`request.getSession(true)`), and the `User` object is bound to it.
*   **Utilization:** Protected JSPs extract and display the logged-in user's data using EL (e.g., `${sessionScope.user.name}`).
*   **Termination:** When a user initiates a logout, `LogoutServlet` explicitly calls `session.invalidate()` to destroy the session object on the server.
*   **Timeout Configuration:** A global session timeout of 30 minutes is enforced via the `<session-timeout>` element in the `web.xml`.

---

# 10. Security Implementation

### Authentication Mechanism
Authentication is handled by the `LoginServlet`, which captures submitted credentials and cross-references them against the static, thread-safe `ConcurrentHashMap` in the `DataStore` utility class.

### Authorisation and Access Control
Authorisation is enforced via the `AuthFilter`. It inspects incoming requests for an active `HttpSession` containing a valid `user` attribute. If absent, the filter blocks access to the requested resource, effectively preventing unauthorized direct URL access (Directory Traversal prevention).

---

# 11. Deployment and Testing

### Build and Packaging Process
The application is packaged into a Web Application Archive (WAR) using IntelliJ IDEA Ultimate and Maven. Running the Maven `package` lifecycle compiles the Java classes, gathers the resources, and bundles them into `skillhub-1.0-SNAPSHOT.war`.

### GlassFish 7 Deployment
The WAR file is deployed to the GlassFish 7 Web Container via the Server Administration Console (Port 4848). The context root is configured to `/skillhub`, making the live application accessible at `http://localhost:8080/skillhub`.

### Testing Validation
*   **Functional Testing:** Verified that invalid credentials trigger error messages on the login page. Confirmed that clicking "Enroll" on the courses page successfully updates the user's active list on the dashboard.
*   **Security Testing (Direct Access):** Attempted to access `dashboard.jsp` directly via an incognito window. The `AuthFilter` successfully intercepted the request and redirected to the login page. Session invalidation was confirmed by hitting the browser's "Back" button post-logout, which forced a new login.

---

# 12. Operational Maintenance

### Scalability and Future Improvements
While the current application meets the immediate project constraints, the in-memory data store (`ConcurrentHashMap`) is highly volatile; all data is lost upon server restart. 
*   **Database Integration:** The most critical future improvement is replacing the mock `DataStore` with JDBC or JPA (Hibernate) to connect to a relational database (e.g., MySQL), ensuring permanent data persistence.
*   **Security Enhancements:** Future versions should implement password hashing (e.g., BCrypt) instead of storing plaintext passwords, and the application should be configured to run over HTTPS to secure session cookies in transit.
*   **Scalability:** Migrating away from purely in-memory session tracking to a distributed cache (like Redis) would allow the application to scale horizontally across multiple GlassFish instances.

---

# 13. Conclusion
The SkillHub Training Academy portal was successfully developed and deployed as a functioning Java EE Web Application. The project achieved all defined objectives, including the implementation of user authentication, session tracking, and request dispatching utilizing standard Servlets and JSP pages. By heavily leveraging Jakarta EE attribute scopes and Web Container features like Listeners and Filters, the application demonstrates robust MVC architecture and secure routing, even while adhering to the strict constraint of operating entirely without a Database Management System.

---

# Appendices

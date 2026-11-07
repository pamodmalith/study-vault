# Component A: Requirements Analysis
**Project:** SkillHub Training Academy Portal

### 1. Users (Actors)
*   **Students/Learners:** Users who register, view available training programs, enroll in courses, and check their profiles.
*   **Trainers:** Instructors who log in to view their assigned courses and see enrolled students.
*   **Administrators:** Staff who add new courses, view student registrations, and manage trainer profiles.

### 2. Functions (Features)
*   **Authentication:** Secure login and logout utilizing Java EE session management.
*   **Registration:** Account creation for new students.
*   **Course Browsing:** Displaying available professional development programs.
*   **Enrollment:** Allowing students to register for specific courses.
*   **Dashboard:** Centralized hub showing a user's active enrollments or system overviews for admins.

### 3. Business Requirements
*   **Automation:** Transitioning from manual administration to an automated web portal.s
*   **Efficiency:** Allowing students to self-serve their registrations and enrollments.
*   **Access:** Providing secure, 24/7 online access to training information.

### 4. Assumptions
*   **Data Persistence:** Data loss upon server restart is acceptable for this prototype, as all data will be stored in Application-scoped memory (`ServletContext`).
*   **Initial Data Load:** The system will be pre-populated with dummy courses and a default admin user upon server startup using a `ServletContextListener`.
*   **User Base Size:** The concurrent user base is small enough that in-memory Java collections (e.g., `ArrayList`) will handle the load without memory issues.

### 5. Constraints
*   **No DBMS:** Strict prohibition of any database management system (SQL or NoSQL).
*   **Technology Stack:** Must strictly use Java EE Web Components (Servlets, JSP) and GlassFish 7.
*   **State Management:** Application state and session tracking must be handled entirely via Java EE attribute scopes (Request, Session, Application).ne
---
# Component A: Application Planning
**Project:** SkillHub Training Academy Portal

### 1. System Purpose
The primary purpose of this project is to develop a web-based training management portal for SkillHub Training Academy. This system replaces the existing manual administration processes for managing student registrations, course enrolments, trainer information, and attendance records.

### 2. Scope
The scope of this project is limited to building a pure Java EE web application utilizing Servlets and JSP pages deployed on a GlassFish 7 server. 
*   **Included:** Implementation of authentication, session management, and request dispatching utilizing exactly 5 Servlets, 6 JSP pages, and 1 ServletContextListener. Data state will be maintained strictly using Java EE attribute scopes (Request, Session, Application) and in-memory collections.
*   **Excluded:** The use of any Database Management System (DBMS) is strictly out of scope. Payment gateways and external API integrations are also excluded.

### 3. Target Users
*   **Students:** End-users accessing the portal to self-manage course enrolments and view training programs.
*   **Trainers:** Teaching staff accessing the system to view assigned classes and student rosters.
*   **Administrators:** Management staff overseeing the entire operational workflow and user data.

### 4. Objectives
*   To successfully apply Servlet, JSP, Session Management, Security, and Web Container concepts to a real-world scenario.
*   To automate learner services by allowing users to register and enroll in technology and business development programs online.
*   To correctly configure deployment descriptors (`web.xml`) and implement session tracking and security mechanisms without relying on external databases.

### 5. Expected Benefits
*   **Operational Efficiency:** Drastically reduces the administrative overhead associated with manual student registrations and attendance tracking.
*   **Secure Accessibility:** Provides 24/7 secure online access to training information for both staff and students.
*   **Scalable Foundation:** Establishes a structured Java EE architecture that can easily be connected to a persistent database in future iterations.
---
# Component A: Application Architecture
**Project:** SkillHub Training Academy Portal

### 1. Architecture Overview
The application follows a simplified Model-View-Controller (MVC) architectural pattern, adapted for an in-memory data approach:

*   **Model (Data & Logic):** Standard Java classes (POJOs) representing entities like `User`, `Course`, and `Enrollment`. Since a DBMS is prohibited, a centralized `DataStore` class containing static `ConcurrentHashMap` or `ArrayList` collections will serve as the mock database.
*   **View (Presentation):** 6 JavaServer Pages (JSP) handling the user interface. These pages will use Expression Language (EL) and JSTL to dynamically render data passed from the Servlets.
*   **Controller (Routing):** 5 Java Servlets acting as controllers. They intercept HTTP requests (GET/POST), interact with the in-memory Model, manage sessions, and forward the request to the appropriate View (JSP) using `RequestDispatcher`.
*   **Initialization:** A `ServletContextListener` acts as the entry point during server startup to populate the Model with dummy data and attach it to the Application scope.
---
# Component A: Web Application Structure
**Project:** SkillHub Training Academy Portal
### 2. Directory Structure
The project utilizes a standard Maven Web Application directory layout managed via IntelliJ IDEA Ultimate.

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
├── pom.xml (Maven dependencies including jakartaee-web-api)
└── README.md
```

---
# Component A: Deployment Configuration
**Project:** SkillHub Training Academy Portal

### 1. Deployment Descriptor (web.xml)
Although modern Jakarta EE applications heavily utilize annotations (e.g., `@WebServlet`), the `web.xml` file remains crucial for configuring application-wide settings, defining the welcome file list, and configuring security constraints or session timeouts. 

For this project, the deployment descriptor is configured to:
*   Define `index.jsp` as the default landing page.
*   Enforce a default session timeout of 30 minutes.
*   Declare the `ServletContextListener` used for initializing our in-memory data.

**web.xml Configuration Code:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="[https://jakarta.ee/xml/ns/jakartaee](https://jakarta.ee/xml/ns/jakartaee)"
         xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
         xsi:schemaLocation="[https://jakarta.ee/xml/ns/jakartaee](https://jakarta.ee/xml/ns/jakartaee) [https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd](https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd)"
         version="6.0">
    <!-- Define the default landing page -->
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
    <!-- Configure session timeout (in minutes) -->
    <session-config>
        <session-timeout>30</session-timeout>
    </session-config>
    <!-- Register the Application Startup Listener -->
    <listener>
        <listener-class>com.skillhub.listener.AppContextListener</listener-class>
    </listener>
    <!-- Define Global Error Pages -->
    <error-page>
        <error-code>404</error-code>
        <location>/jsp/error.jsp</location>
    </error-page>
    <error-page>
        <error-code>500</error-code>
        <location>/jsp/error.jsp</location>
    </error-page>
</web-app>
```
### 2. Server Application Settings

The application is configured to deploy as a standard WAR (Web Application Archive) during the development phase on GlassFish 7.
- **Web Container:** GlassFish Server 7.x
- **Java Version:** Java Temurin 17 (JDK 17)
- **Packaging:** Standard `.war` archive generated via Maven.
- **Context Path:** `/skillhub` (The application will be accessible via `http://localhost:8080/SkillHub-1.0`)
---
# Component A: Servlet Development
**Project:** SkillHub Training Academy Portal

### 1. Servlet Architecture overview
The application utilizes exactly five Servlets to act as the Controller layer, processing incoming HTTP requests, interacting with the in-memory data store, and routing users to the appropriate JSP views using `RequestDispatcher`. All servlets are mapped using Jakarta EE 10 `@WebServlet` annotations rather than `web.xml` entries.

### 2. The 5 Required Servlets
1.  **`LoginServlet` (`/login`)**: Intercepts `POST` requests from the login form. It authenticates credentials against the in-memory user list. Upon success, it generates an `HttpSession` to track the user's logged-in state and redirects to the dashboard.
2.  **`LogoutServlet` (`/logout`)**: Intercepts `GET` requests when a user clicks logout. It calls `session.invalidate()` to destroy the session and redirects the user back to the login page.
3.  **`RegistrationServlet` (`/register`)**: Processes `POST` requests from the student sign-up form. It creates a new `User` object and appends it to the Application-scoped `DataStore` collection, then redirects to the login page.
4.  **`CourseListServlet` (`/courses`)**: Handles `GET` requests by retrieving the static list of available training programs from the `DataStore` and attaching them to the `HttpServletRequest` attributes before forwarding to `courses.jsp` for rendering.
5.  **`EnrollmentServlet` (`/enroll`)**: Processes `POST` requests containing a course ID. It retrieves the current user from the `HttpSession`, adds the selected course to the user's active enrollments in the `DataStore`, and redirects back to the dashboard.

### 3. Implementation Example: LoginServlet
This demonstrates the authentication mechanism and session creation using Jakarta EE.

```java
package com.skillhub.controller;

import com.skillhub.model.User;
import com.skillhub.data.DataStore;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;
import java.io.IOException;

@WebServlet(name = "LoginServlet", value = "/login")
public class LoginServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
            
        String email = request.getParameter("email");
        String password = request.getParameter("password");
        
        // Mock authentication against in-memory DataStore
        User authenticatedUser = DataStore.authenticate(email, password);
        
        if (authenticatedUser != null) {
            // Create a new session or return the current one
            HttpSession session = request.getSession(true);
            session.setAttribute("user", authenticatedUser);
            
            // Redirect to dashboard to prevent form resubmission
            response.sendRedirect(request.getContextPath() + "/jsp/dashboard.jsp");
        } else {
            // Attach error message and forward back to login page
            request.setAttribute("errorMessage", "Invalid email or password.");
            request.getRequestDispatcher("/jsp/login.jsp").forward(request, response);
        }
    }
}
```

---

# Component A: JSP Development

**Project:** SkillHub Training Academy Portal

### 1. View Layer Architecture

The presentation layer is built exclusively using JavaServer Pages (JSP). To maintain a clean Model-View-Controller (MVC) architecture, these pages contain zero Java scriptlets (`<% ... %>`). Instead, they rely entirely on Jakarta Expression Language (EL) and the Jakarta Standard Tag Library (JSTL) to dynamically render data passed down from the Servlet controllers via Request and Session scopes.

### 2. The 6 Required JSP Views

1. **`login.jsp`**: The entry point containing an HTML form for user authentication. It captures the email and password and POSTs them to the `LoginServlet`. It also uses EL to display error messages (`${errorMessage}`) if authentication fails.
2. **`register.jsp`**: A sign-up form allowing new students to create an account. It POSTs data to the `RegistrationServlet`.
3. **`dashboard.jsp`**: The central hub for authenticated users. It retrieves the logged-in user object from the session scope (`${sessionScope.user}`) to display a personalized greeting and lists their active course enrollments.
4. **`courses.jsp`**: A catalog page displaying all available training programs. It uses the JSTL `<c:forEach>` tag to iterate over a collection of courses attached to the request scope by the `CourseListServlet`.
5. **`profile.jsp`**: Displays the detailed information of the currently logged-in user, reading directly from the session attributes.
6. **`error.jsp`**: A global error-handling page configured in `web.xml` to catch 404 (Not Found) and 500 (Internal Server Error) status codes, providing a user-friendly fallback interface instead of a raw stack trace.
### 3. Implementation Example: courses.jsp

This snippet demonstrates how JSTL and EL are used to render dynamic data without embedding raw Java code in the HTML.

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="jakarta.tags.core" %>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SkillHub - Available Courses</title>
    <link rel="stylesheet" href="${pageContext.request.contextPath}/css/style.css">
</head>
<body>
    <header>
        <h1>Welcome, ${sessionScope.user.name}</h1>
        <nav>
            <a href="${pageContext.request.contextPath}/jsp/dashboard.jsp">Dashboard</a>
            <a href="${pageContext.request.contextPath}/logout">Logout</a>
        </nav>
    </header>

    <main>
        <h2>Available Training Programs</h2>
        
        <table border="1">
            <thead>
                <tr>
                    <th>Course ID</th>
                    <th>Course Name</th>
                    <th>Trainer</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <!-- Iterate over the 'courseList' attribute set by CourseListServlet -->
                <c:forEach var="course" items="${requestScope.courseList}">
                    <tr>
                        <td>${course.id}</td>
                        <td>${course.title}</td>
                        <td>${course.trainerName}</td>
                        <td>
                            <form action="${pageContext.request.contextPath}/enroll" method="post">
                                <input type="hidden" name="courseId" value="${course.id}">
                                <button type="submit">Enroll</button>
                            </form>
                        </td>
                    </tr>
                </c:forEach>
            </tbody>
        </table>
    </main>
</body>
</html>
```

---

# Component A: Application of Web Container Features
**Project:** SkillHub Training Academy Portal

### 1. Component Lifecycle Management
The GlassFish 7 web container takes full responsibility for managing the lifecycle of all web components. 
*   **Initialization:** Upon server startup, the container reads the `web.xml` and initializes the `AppContextListener`, triggering the instantiation of our in-memory `DataStore`. 
*   **Servlet Instantiation:** Servlets (like `LoginServlet` and `CourseListServlet`) are not manually instantiated via the `new` keyword. Instead, the web container creates singletons of these classes and manages their `init()`, `service()`, and `destroy()` lifecycle phases based on incoming client requests.

### 2. Session Tracking Management
Because HTTP is a stateless protocol, the web container provides the `HttpSession` API to maintain user state across multiple requests.
*   When a user successfully logs in via the `LoginServlet`, the container generates a unique Session ID (typically stored in a JSESSIONID cookie).
*   The application utilizes this container feature to bind the authenticated `User` object to the session scope, allowing the system to identify the user as they navigate between the dashboard, profile, and course catalog pages without needing to re-authenticate.

### 3. Request Interception (Filters)
To enforce security and prevent unauthorized access to protected resources (like the `dashboard.jsp` or `profile.jsp`), the application utilizes a Jakarta EE Web Filter. The web container intercepts incoming requests before they reach the target Servlet or JSP.

**Implementation of Authorization Filter:**
```java
package com.skillhub.filter;

import jakarta.servlet.*;
import jakarta.servlet.annotation.WebFilter;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;
import java.io.IOException;

// Intercepts requests to specific secure paths
@WebFilter(urlPatterns = {"/jsp/dashboard.jsp", "/jsp/profile.jsp", "/enroll"})
public class AuthFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
            throws IOException, ServletException {
            
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        HttpSession session = httpRequest.getSession(false);

        // Check if user is logged in
        if (session != null && session.getAttribute("user") != null) {
            // Authorized: allow the request to proceed
            chain.doFilter(request, response);
        } else {
            // Unauthorized: redirect to login page
            httpResponse.sendRedirect(httpRequest.getContextPath() + "/jsp/login.jsp");
        }
    }
}
```

### 4. Request Dispatching and Routing

The application leverages the container's routing capabilities to maintain a clean separation of concerns:
- **Forwarding:** Servlets process business logic and use `request.getRequestDispatcher("/path.jsp").forward(request, response)` to pass the request and response objects to a JSP for rendering. This happens entirely server-side.
- **Redirecting:** For operations that change state (like logging in, registering, or enrolling), the application uses `response.sendRedirect()`. This instructs the client's browser to make a new GET request, effectively preventing the "Confirm Form Resubmission" error if the user refreshes the page.
---

# Component A: 9. Session Management
**Project:** SkillHub Training Academy Portal

### 1. Session Tracking Strategy
Because HTTP is intrinsically stateless, the application must maintain user continuity across multiple requests. This is achieved using the Jakarta EE `HttpSession` API, which tracks users via a JSESSIONID cookie. 

### 2. Session Implementation
*   **Creation:** When a user successfully authenticates via the `LoginServlet`, a new session is established using `request.getSession(true)`. The authenticated `User` object is then bound to this session using `session.setAttribute("user", authenticatedUser)`.
*   **Utilization:** Subsequent requests to protected JSPs (like the dashboard and profile) utilize Expression Language (e.g., `${sessionScope.user.name}`) to extract and display the logged-in user's personalized data. Servlets also extract this session object to associate new course enrolments with the correct user.
*   **Termination:** When a user initiates a logout request via the `LogoutServlet`, the application explicitly calls `session.invalidate()`. This destroys the session object on the server and unbinds all associated data, ensuring the user is completely logged out.
*   **Timeout Configuration:** A global session timeout of 30 minutes is enforced via the `<session-timeout>` element within the `web.xml` deployment descriptor.

---

# Component A: 10. Security Implementation
**Project:** SkillHub Training Academy Portal

### 1. Authentication Mechanism
Authentication is the process of verifying a user's identity. In this application, authentication is handled by the `LoginServlet`.
*   The system captures the submitted email and password from the `login.jsp` form.
*   Because an external database is prohibited by the project constraints, the credentials are cross-referenced against the static, in-memory `ConcurrentHashMap` within the `DataStore` utility class.
*   If the credentials match, a session is granted; if they fail, the user is redirected back to the login page with an appropriate error message.

### 2. Authorisation and Access Control
Authorisation ensures that an authenticated user only has access to permitted resources. 
*   **Authorization Filter:** A Jakarta EE Web Filter (`AuthFilter`) is deployed to intercept all requests directed at protected endpoints, including `/jsp/dashboard.jsp`, `/jsp/profile.jsp`, and the `/enroll` servlet path.
*   **Enforcement:** The filter inspects the incoming `HttpServletRequest` for an active `HttpSession` containing a valid `user` attribute. If this attribute is absent, the filter blocks access to the requested resource and redirects the user to the login page, preventing unauthorized direct URL access.
*   **Role-Based Logic:** (Optional Implementation) The application’s `User` model includes a role attribute (e.g., "STUDENT" vs "ADMIN"). This can be leveraged within the JSPs using JSTL `<c:if test="${sessionScope.user.role == 'ADMIN'}">` to conditionally render admin-only features, such as adding new courses.
---
# Component A: 11. Deployment and Testing
**Project:** SkillHub Training Academy Portal

### 1. Build and Packaging Process
To deploy the application to a production or staging environment, it must be packaged into a Web Application Archive (WAR) file. Using IntelliJ IDEA Ultimate and Maven:
*   Open the Maven tool window (usually on the right sidebar).
*   Run the `clean` lifecycle phase to remove any previous builds.
*   Run the `package` lifecycle phase. Maven will compile the Java classes, gather the JSPs and `web.xml`, and bundle them into a single file (e.g., `skillhub-1.0-SNAPSHOT.war`) located in the `/target` directory of your project space.

### 2. GlassFish 7 Deployment
Once the WAR file is generated, it is deployed to the GlassFish 7 Web Container.
*   **Admin Console:** Access the GlassFish Server Administration Console (typically at `http://localhost:4848`).
*   Navigate to **Applications** in the left-hand menu.
*   Click **Deploy**, browse for the generated `skillhub-1.0-SNAPSHOT.war` file on your 1TB HDD, and set the Context Root to `/skillhub`.
*   Click **OK** to deploy. The application is now live and accessible at `http://localhost:8080/skillhub`.
### 3. Functional Testing
Manual testing was conducted to ensure all minimum requirements were met:
*   **Authentication:** Verified that providing invalid credentials triggers an error message on `login.jsp`, while valid credentials successfully generate an `HttpSession` and route the user to `dashboard.jsp`.
*   **Data Integrity:** Verified that new users registered via the `RegistrationServlet` are properly saved to the `DataStore` and can immediately log in.
*   **Course Enrollment:** Confirmed that clicking "Enroll" on `courses.jsp` successfully appends the course ID to the user's session and updates their active list on the dashboard.

### 4. Security & Session Testing
*   **Direct URL Access (Bypass Attempt):** Attempted to access `http://localhost:8080/skillhub/jsp/dashboard.jsp` directly via a private/incognito browsing window without logging in. The `AuthFilter` successfully intercepted the request and redirected back to the login page.
*   **Session Invalidation:** Clicked the "Logout" link, which triggered the `LogoutServlet`. Verified that hitting the browser's "Back" button and attempting to refresh a secure page forced a new login, confirming the session was completely destroyed.

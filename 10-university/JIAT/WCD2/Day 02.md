---
author:
  - Pamod Malith
type: lecture
course: WCD2
topic: page directives
date: 2026-11-14
tags:
  - jsp
  - java
  - wcd2
status: done
---
# JSP Directives
## Outline
- [[#Page Directives]]
- [[10-university/JIAT/WCD2/Day 03#Include Directive|Include Directives]]
- [[10-university/JIAT/WCD2/Day 03#Taglib Directive|Taglib Directives]]

## Page Directives
### Syntax
- The standard syntax uses `<%@ page ... %>` tags. You can define multiple attributes in a single tag or split them across several separate tags.
```java
<%@ page attribute="value" %>
```
### import option
- **Imports** Java packages, classes, or interfaces into the JSP file (the only attribute that can appear multiple times).
```java
<%@ page import="org.pamod.jsp.A" %>

//use name variable
<%= A.name%>
```

### contentType option
- The **contentType** option allows you specify the MIME type of the output page.
- The default is `text/html`. You can also use other values such as `text/plain` for outputting text format as follow:
```java
<%@ page contentType="text/plain" %>
<%@ page contentType="text/html" %>
```

> **MIME** - Multipurpose Internet Mail Extensions "`type/subtype`"

### isThreadSafe option
- **isThreadSafe** option allows you to indicate whether the page is processed as thread-safe. The default value of the isThreadSafe is true therefore all JSP pages are onsidered thread-safe. If isThreadSafe is set to false, JSP engine ensures that only one thread at a time is executing the current JSP page.
```java
<%@ page isThreadSafe="true|false" %>
```

### session option
- Specifies whether or not the JSP page participates in HTTP sessions.
- Determines whether the page participates in an HTTP session (enables the implicit `session` object).
```java
<%@ page session="true|false" %>
```

### buffer option
- buffer options let you control the JSP buffering.
- You can turn buffer on or off, or even specify the buffer size using buffer option as follow:
```java
<%@ page buffer="none" %>
<%@ page buffer="64kb" %>
```

### autoFlush option
- **autoFlush** options let you control the JSP buffering.
- **autoFlush** option controls whether the buffer is flushed automatically when it is full. The default value of **autoFlush** is `true` means JSP automatically flush the buffer when it is full.
```java
<%@ page autoFlush="true|false" %>
```
### info option
- Info option allows you to define the description of the servlet. Then you can access this value by calling the method:
- Sets a descriptive text string for the JSP that can be retrieved via `getServletInfo()`.
```java
<%@ page info="User Profile Page" %>

//get info
<%= getServletInfo() %>
```
### extends option
- In some cases, you need to create your own super class of a JSP page and using this supper class in different JSP pages. In order to do so, you need to create a super class first. This supper class typically inherits from the HttpServlet class. And in the JSP pages, you use the extends option to use the supper class.

---
##
Next: [[10-university/JIAT/WCD2/Day 03|Day 03]]

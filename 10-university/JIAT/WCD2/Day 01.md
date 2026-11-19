---
author:
  - Pamod Malith
type: lecture
course: WCD2
topic:
date: 2026-11-12
tags:
  - jsp
  - java
  - wcd2
status: done
---
# Day 01

## Key idea (one sentence)
In this lecture main focus is **JSP Scopes**.

## Main Scopes
- Page Scope
- Request Scope
- Session Scope
- Application Scope

### Page Context
We can use page context reference to get attributes from any scope, including the page scope for attributes bound to the PageContext.

```java
getAttribute(String name)
getAttribute(String name, int scope)
findAttribute(String name)
getAttributeNamesInScope(int scope)
//more
```

### Using pageContext
```java
<%
    pageContext.setAttribute("a", new src.A());
%>
<%
    src.A t=(src.A) pageContext.getAttribute("a");
    out.print(t.x);
%>
```

### Using pageCotext with scopes
```java
<%
    pageContext.setAttribute("a", new src.A(),pageContext.SESSION_SCOPE);
%>
<%
    src.A m=(src.A) pageContext.getAttribute("a", PageContext.SESSION_SCOPE);
    out.print(m.x);
    
    //same `session` is implicit object of getSession
    src.A t=(src.A) session.getAttribute("a");
    out.print(t.x);
%>
```

To find attribute name in all scopes can use `findAttribute("name"):
```java
<%= pageContext.findAttribute("a"); %>
```
## Related
- [[ ]]

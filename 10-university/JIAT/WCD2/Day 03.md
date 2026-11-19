---
author:
  - Pamod Malith
type: lecture
course: WCD2
topic:
date: 2026-11-15
tags:
  - jsp
  - java
  - wcd2
status: done
---
### isElIgnored option
- isELIgnored option is used to disable the evaluation of the expression language (EL).
- The isELIgnored attribute gives you the ability to disable the evaluation of Expression Language (EL) expressions which has been introduced in JSP 2.0.
```java
<%@ page isElIgnored="" %>
```

### errorPage option
- errorPage option allows you to indicate the error page when an error occurs in the current executing JSP page.
```java
<%@ page errorPage="error.jsp" %>
```

### isErrorPage option
- isErrorPage option indicates that the current JSP page can be used as an error page for other JSP pages. When you set isErrorPage equal true, JSP engine creates an implicit exception object which contains the Throwable object that triggers the call to the error page.
### isScriptingEnabled
- Determines if the scripting elements are allowed for use.
- The isScriptingEnabled attribute determines if the scripting elements are allowed for use.
- The default value (true) enables scriptlets, expressions, and declarations. If the attribute's value is set to false, a translation-time error will be raised if the JSP uses any scriptlets, expressions (non-EL), or declarations.
- As a example we can use web.xml also to get this option.
```xml
<jsp-config>
	<jsp-property-group>
		<url-pattern>* jsp</url-pattern>
		<scripting-invalid>true</scripting-invalid>
	</isp-property-group>
</isp-config>
```
## Include Directive
- Include directive lets you to include a file (JSP or HTML) when JSP engine translates the JSP page into a serviet.
- ```java
	<%@ include file="relative url" %>
	```
- The filename you specify in the include directive is a relative URL. If you provide a file name without associated path, the JSP compiler always assumes that the file is in the same directory as the current JSP page.
- Normally the include directive is used to include common sections of a JSP page in a web application or website such as header, navigation bar, footer... to make those sections reusable in every JSP page.
## Taglib Directive
- JSP has a set of standard tags available. JSP also allows you to create new custom tags that look like HTML or XML tags. In this case, you can use taglib directive to use your custom tag in your JSP page.
```java
<%@ taglib uri=http://localhost/mytag/taglib/mytaglib prefix="jsptag" %>
```

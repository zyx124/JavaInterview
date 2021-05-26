# Java Web and RESTful API



## Servlet

Servlet is a web component that is deployed on the server to create dynamic web page.

![](/home/zyx/repo/JavaInterview/Servlet.jpg)

**Life Cycle**; 

- init(): called once 
- service(): doGet(), doPost(), doPut(), doDelete()
- destroy(): called once

**Configuration**

`@Webservlet()` with `name, urlPattern, loadOnStartUp, displayName, initParams` as parameters.

**Redirect and forward**:

- `RequestDispachers` include() and forward()
- `HttpServletResponse` sendRedirect() 
- `ServletContext` setAttribute()
- Java system-wide Properties list
- singleton class 

![redirectVsForward](/home/zyx/repo/JavaInterview/redirectVsForward.png)

###### Attributes

key-value pairs

**methods**: 

- `setAttribute`()
- `getAttribute`()
- `getAttributeNames`()
- `removeAttribute`()

**scope**: 

- request (request.setAttribute())
- session: `httpSession`, login info available in session, once session expires, `IllegalStateException`
- application: accessible in the application. e.g. dburl, username, password should be accessible multiple servlets in the same application.

**HTTP**

**Stateless**: don't know if two request from same user or not.

**Cookies**: credential stored on client side

**Session:** credential stored on server side, give it an ID, and let the client know that id, passed back at ever http request.

**Session API**:

`getSession()` creates a new `HttpSession` object and add a cookie to the response object with name `JSESSIONID` 

###### Filter

invoked at pre-processing or post-processing of a request,filtering tasks like conversion, logging, compression encryption, validation etc.

**filter life cycle:** init(config) -> doFilter() -> destroy()

**filter vs interceptor**
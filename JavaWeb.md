### JavaEE

即 J2EE,JEE. Java Platform,Enterprise Edition,是对 JavaSE(Java Platform,Standard Edition)的扩展,加入面向企业开发支持.包括 Servlet, WebSocket,EL,EJB.

JaveEE平台提供了API标准,但不一定提供实现.

### Servlet

一套用于处理HTTP请求的API标准.可以基于Servlet实现HTTP请求.

要使用Servlet需要Servlet Container.

Apache Tomcat , Glassfish, JBoss, Jetty实现了对于Servlet Container的支持.

Servlet的生命周期

- init()           初始化
- service()    处理客户端请求
- destroy()   终止
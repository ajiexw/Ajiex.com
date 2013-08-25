Date:2013-08-15
Tags: Android

##Servlet

Servlet是一种服务器端的java应用程序，可以生成动态的web页面。担当客户请求与服务器响应的中间层。是java web技术的核心基础。

同传统的cgi（Common Gateway Interface公共网关接口)相比，Servlet效率更高，更容易使用，具有更好的移植性。

同jsp(JavaServer Pages)相比，Servlet输出html语句还是采用老的cgi方式，一句一句输出，编写和修改html不方便。

Servlet是一个早期不完善的产品，写业务层很好，写表面层很臭，并且两层混杂。

而单纯的jsp语言执行效率非常低，无法负担大量用户访问，同时将表现层和业务层混在一起，修改不方便，并且代码不能重复利用，修改一个地方经常牵涉到十几页code。

采用组件技术就只改组件就可以了。SUN推出JSP+BEAN，用jsp写表现层，用BEAN写业务层。

学习路线：Servlet入门--再上JSP--再上JSP+BEAN

Web 开发是离不开 HTTP 协议的，而 Servlet 规范其实就是对 HTTP 协议做面向对象的封装，HTTP协议中的请求和响应就是对应了 HttpServletRequest 和HttpServletResponse 这两个接口。可以通过 HttpServletRequest 来获取所有请求相关的信息，包括 URI、Cookie、Header、请求参数等等，别无它路。因此当你使用某个框架时，你想获取HTTP请求的相关信息，只要拿到 HttpServletRequest 实例即可。而HttpServletResponse接口是用来生产 HTTP 回应，包含 Cookie、Header 以及回应的内容等等。

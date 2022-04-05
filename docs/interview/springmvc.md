* 说一下mvc执行流程，SpringMVC的请求过程？[参考1](https://www.cnblogs.com/hamawep789/p/10840774.html) [参考2](https://www.jianshu.com/p/8a20c547e245)

  * 用户发起请求到前端控制器（DispatchServlet）
  * 前端控制器接到请求后会调用HandlerMapping处理映射器
  * HandlerMapping根据url，查找该url对应方法的反射对象（具体的处理器），生成处理器执行链HandlerExecutionChain(包括处理器对象和处理器拦截器)一并返回给DispatcherServlet。
  * DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter
  * 适配器执行handler,通过反射调用controller中的业务方法(处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作)
  * handler执行完成后返回一个ModelandView对象给前端控制器
  * 前端控制器将modelanview传给viewReslover视图解析器
  * 视图解析器解析后返回具体的view
  * 前端控制器根据view进行视图的渲染（将模型数据填充到视图中）
  * DispatcherServlet响应用户。

* 一个request 要经过哪些类（直接说springMVC就行了）（同上）
* 怎么使用springmvc实现权限控制 原理是什么
* mvc流程，mvc与strus2区别？
* mvc请求时怎么看是重定向还是转发？作为开发者怎么去定义是重定向还是转发？
* **mvc的拦截器如何使用？**[参考](https://www.cnblogs.com/black-spike/p/7813238.html)
* 说说SpringMVC的controller层？
* 说一说ssm常用的注解？mybatis的xml中   常用的标签，spring都有哪些模块？
* 说一说一个客户请求，从前端到后台的整个流程？
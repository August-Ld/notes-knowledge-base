## SpringMVC的执行流程

### 视图阶段（老旧 JSP 等）

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401140411304.png" alt="image-20240401140411304" style="zoom:50%;" />

1. 用户发送请求到前端控制器 DispatcherServlet
2. DispatcherServlet 收到请求调用 HandlerMapper（处理器映射器）
3. HandlerMappering 找到具体的处理器，生成处理对象及处理器拦截器（如果有），在一起返回给 DispatcherServlet。
4. DispatcherServlet 调用 HandlerAdapter（处理器适配器）
5. HandlerAdapter经过适配调用具体的处理器（Handler/Controller）
6. Cotroller 执行完成返回ModelAndView 对象
7. HandlerAdapter 将 Controller 执行结果 ModelAndView 返回给 DispatcherServlet
8. DispatcherServlet 将 ModelAndView 传给 ViewResolver（视图解析器）
9. ViewResolver 解析后返回具体的 View（视图）
10. DispatcherServlet 根据 View 进行渲染视图（即将模型数据填充至视图中）
11. DispatcherServlet 响应用户。



### 前后端分离阶段（接口开发，异步）

<img src="/Users/ding/Library/Application Support/typora-user-images/image-20240401140742322.png" alt="image-20240401140742322" style="zoom:50%;" />

1. 用户发送请求到前端控制器 DispatcherServlet
2. DispatcherServlet 收到请求调用 HandlerMapper（处理器映射器）
3. HandlerMappering 找到具体的处理器，生成处理对象及处理器拦截器（如果有），在一起返回给 DispatcherServlet。
4. DispatcherServlet 调用 HandlerAdapter（处理器适配器）
5. HandlerAdapter经过适配调用具体的处理器（Handler/Controller）
6. 方法上添加了@ResponseBody
7. 通过 HttpMessageConverter 来返回结果转换为 JSON 并响应



### SpringMVC 常见的注解有哪些？

![image-20240401152307164](/Users/ding/Library/Application Support/typora-user-images/image-20240401152307164.png)
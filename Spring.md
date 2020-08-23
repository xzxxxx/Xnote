 # Spring

## beanFactory 和 FactoryBean

beanFactory :生产bean的工厂

FactoryBean：本身也是一个bean ，生产指定的bean

![image-20200727143546276](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200727143546276.png)





## 三级缓存

解决了循环依赖是，aop对象的问题

![image-20200804151708368](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200804151708368.png)

## Spring boot 自动配置

resource /  META-INF/spring.factories 









# SpirngMVC

## 工作流程

1. 用户请求先经过DispatcherServlet
2. DispatcherServlet收到请求后调用HandlerMapping，请求获取Handler
3. HandlerMapping根据请求url找到具体的Handler，==生成Handler对象==和HandlerExecuteChain拦截器处理链对象，将这些对象返回给DispatcherServlet
4. DispatcherServlet调用HandlerAdapter
5. HandlerAdapter==调用具体的Handler==，执行Handle方法
6. Handler执行完返回ModelAndView对象
7. HandlerAdapter将ModelAndView对象返回给DispatcherServlet
8. 


​		 [第二章 微服务网关基础组件 - zuul入门](https://www.cnblogs.com/java-zhao/p/6656518.html) 	

## **一、zuul简介**

### **1、作用**

zuul使用一系列的filter实现以下功能

- 认证和安全 - 对每一个resource进行身份认证
- 追踪和监控 - 实时观察后端微服务的TPS、响应时间，失败数量等准确的信息
- 日志 - 记录所有请求的访问日志数据，可以为日志分析和查询提供统一支持
- 动态路由 - 动态的将request路由到后端的服务上去
- 压力测试 - 逐渐的增加访问集群的压力，来测试集群的性能
- 限流 - allocating capacity for each type of request and dropping requests that go over the limit
- 静态响应 - 直接在网关返回一些响应，而不是通过内部的服务返回响应

### **2、组件：**

- zuul-core：library which contains the core functionality of compiling and executing [Filters](https://github.com/Netflix/zuul/wiki/Filters)
- zuul-netflix：library which adds other NetflixOSS components to Zuul - using Ribbon for routing requests, for example.

### **3、例子：**

- zuul-simple-webapp：webapp which shows a simple example of how to build an application with zuul-core
- zuul-netflix-webapp：webapp which packages zuul-core and zuul-netflix together into an easy to use package

github地址：https://github.com/Netflix/zuul/

 

## **二、zuul filter**

### **1、关键元素**

- Type：most often defines the stage during the routing flow when the Filter will be applied (although it can be any custom string)
  - 值可以是：pre、route、post、error、custom
- Execution Order: filter执行的顺序（applied within the Type, defines the order of execution across multiple Filters）
- Criteria：filter执行的条件（the conditions required in order for the Filter to be executed）
- Action: filter执行的动作（the action to be executed if the Criteria is met）

**注意点：**

- filters之间不会直接进行通讯交流，他们通过一个RequestContext共享一个state
  - 该RequestContext对于每一个request都是唯一的
- filter当前使用groovy来写的，也可以使用java
- The source code for each Filter is written to a specified set of  directories on the Zuul server that are periodically polled for changes
- zuul可以动态的read, compile, and run these Filters
  - 被更新后的filter会被从disk读取到内存，并动态编译到正在运行的server中，之后可以用于其后的每一个请求（Updated  filters are read from disk, dynamically compiled into the running  server, and are invoked by Zuul for each subsequent request）

### **2、filter type**（与一个典型的request的生命周期相关的filter type）

- PRE Filters

  - 执行时机： before routing to the origin.
  - 这类filter可能做的事
    - request authentication
    - choosing origin servers（选机器）
    - logging debug info.
    - 限流

- **ROUTING Filters**

- - 这类filter可能做的事：真正的向service的一台server（这台server是pre  filter选出来的）发请求，handle routing the request to an origin，This is where the origin HTTP request is built and sent using Apache HttpClient or  Netflix Ribbon.

- POST Filters

  - 执行时机：after the request has been routed to the origin
  - 这类filter可能做的事
    - adding standard HTTP headers to the response
    - gathering statistics and metrics
    - streaming the response from the origin to the client

- ERROR Filters

  - 执行时机：其他三个阶段任一阶段发生错误时执行（when an error occurs during one of the other phases）

- CUSTOM Filters

  - 沿着默认的filter流，zuul允许我们创建一些自定义的Filter type，并且准确的执行他们。
  - 例如：我们自定义一个STATIC type的filter，用于从zuul直接产生响应，而不是从后边的services（we have a custom STATIC type that generates a response within Zuul instead of  forwarding the request to an origin）

 

## **三、zuul request lifecycle（filter流）**

![img](%E5%BE%AE%E6%9C%8D%E5%8A%A1%E7%BD%91%E5%85%B3%E5%9F%BA%E7%A1%80%E7%BB%84%E4%BB%B6%20-%20zuul%E5%85%A5%E9%97%A8.assets/866881-20170106171654316-977255062.png)

说明：对应（二）的filter type来看

 

## **四、zuul核心架构**

![img](%E5%BE%AE%E6%9C%8D%E5%8A%A1%E7%BD%91%E5%85%B3%E5%9F%BA%E7%A1%80%E7%BB%84%E4%BB%B6%20-%20zuul%E5%85%A5%E9%97%A8.assets/866881-20170106191748566-1274562936.png)

zuul的核心就是：filter、filter流与核心架构。这些在下一章会以代码的形式做展示。

 

​    分类:             [微服务网关](https://www.cnblogs.com/java-zhao/category/977216.html)

​    标签:             [zuul](https://www.cnblogs.com/java-zhao/tag/zuul/),             [网关](https://www.cnblogs.com/java-zhao/tag/网关/)
# 使用Zuul构建API Gateway

https://blog.csdn.net/zhanglh046/article/details/78651993

# 一 微服务网关背景及简介

不同的微服务一般有不同的网络地址，而外部的客户端可能需要调用多个服务的接口才能完成一个业务需求。比如一个电影购票的收集APP,可能回调用电影分类微服务，用户微服务，支付微服务等。如果客户端直接和微服务进行通信，会存在一下问题：

\# 客户端会多次**请求不同微服务**，增加客户端的复杂性

\# 存在跨域请求，在一定场景下处理相对复杂

\# 认证复杂，每一个服务都需要**独立认证**

\# 难以重构，随着项目的迭代，可能需要**重新划分微服务**，如果客户端直接和微服务**通信**，那么重构会难以实施

\# 某些微服务可能使用了其他协议，直接访问有一定困难

上述问题，都可以借助微服务网管解决。微服务网管是介于客户端和服务器端之间的中间层，所有的外部请求都会先经过微服务网关，架构演变成：

![img](%E4%BD%BF%E7%94%A8Zuul%E6%9E%84%E5%BB%BAAPI%20Gateway.assets/20171128095912633.png)



这样客户端只需要和网关交互，而无需直接调用特定微服务的接口，而且方便监控，易于认证，减少客户端和各个微服务之间的交互次数

 

# 二 Zuul 简介

Zuul是Netflix开源的微服务网关，他可以和Eureka,Ribbon,Hystrix等组件配合使用。Zuul组件的核心是一系列的过滤器，这些过滤器可以完成以下功能：

\# 身份认证和安全: 识别每一个资源的验证要求，并拒绝那些不符的请求

\# 审查与监控：

\# 动态路由：动态将请求路由到不同后端集群

\# 压力测试：逐渐增加指向集群的流量，以了解性能

\# 负载分配：为每一种负载类型分配对应容量，并弃用超出限定值的请求

\# 静态响应处理：边缘位置进行响应，避免转发到内部集群

\# 多区域弹性：跨域AWS Region进行请求路由，旨在实现ELB(ElasticLoad Balancing)使用多样化

Spring Cloud对Zuul进行了整合和增强。目前，Zuul使用的默认是Apache的HTTP Client，也可以使用Rest Client，可以设置ribbon.restclient.enabled=true.

 

 

# 三 编写Zuul微服务网关

\# 添加依赖

Zuul的依赖肯定是要加的，如何和Eureka配合使用， Zuul需要注册到Eureka上，但是Zuul的依赖不包含Eureka Discovery客户端，所以还需要添加Eureka的客户端依赖

<dependency>

   <groupId>org.springframework.cloud</groupId>

   <artifactId>spring-cloud-starter-eureka</artifactId>

</dependency>

<dependency>

   <groupId>org.springframework.cloud</groupId>

   <artifactId>spring-cloud-starter-zuul</artifactId>

</dependency>

 

\# 启动类加上注解@EnableZuulProxy

它默认加上了@EnableCircuitBreaker和@EnableDiscoveryClient

@SpringBootApplication

@EnableZuulProxy

public class ZuulApplication {

​    public static  void main(String[] args)  throws Exception {

​      SpringApplication.*run*(ZuulApplication.class, args);

   }

}

\# 配置application.yml

server:

  port: 8040

spring:

  application:

   name: microservice-gateway-zuul

eureka:

  client:

   register-with-eureka: true

   fetch-registry: true

   service-url:

​    defaultZone:http://nicky:123abcABC@localhost:8761/eureka

  instance:

   ip-address: true

zuul:

  ignoredServices: '*'

  routes:

   microservice-provider-user: /ecom/**

zuul:

 ignoredServices: '*' // 忽略所有请求

 routes:

  服务名: /ecom/** //允许将服务名映射到ecom

 

\# 启动Eureka，Zuul和 其他应用

访问http://localhost:8040/ecom/user/1

或者

http://localhost:8040/microservice-provider-user/user/1

都可以

 

# 四 微服务网关相关的配置

## 4.1 路由路径的配置

zuul:

 ignoredServices: '*' // 忽略所有请求

 routes:

服务名: /ecom/** //允许将服务名映射到ecom

 

 

为了更加细粒度控制路由路径：

// 表示只要HTTP请求是 /ecom开始的，就会forward到服务id为users_service的服务上面

zuul:

 routes:

  users: 

   path:/ecom/** // 路由路径

   serviceId: users_service // 服务id

 

zuul:

 routes:

  users: // 路由名称，随意，唯一即可

   path: /ecom/** // 路由路径

   url:http://localhost:9000

 

## 4.2 不破坏Zuul的Hystrix和Ribbon特性

上述简单的url路由配置，不会作为Hystrix Command执行，也不会进行ribbon的负载均衡。为了同时指定path 和 url，但是不破坏Zuul的Hystrix和Ribbon特性：

zuul:

 routes:

  users:

   path:/ecom/**

   serviceId:  *microservice-provider-user*

 

ribbon:

 eureka:

  enabled:false // ribbon禁掉Eureka

 

*microservice-provider-user*:

 ribbon:

listOfServers: localhost:9000,localhost:9001

 

## 4.3 使用正则表达式指定Zuul的路由匹配规则

借助PatternServiceRoute  Mapper实现从微服务到映射路由的正则配置，例如：

​    @Bean

​    publicPatternServiceRouteMapper serviceRouteMapper(){

​      // servicePattern: 指的是微服务的pattern

​      // routePattern: 指的是路由的pattern

​      // 当你访问/microservice-provider-user/v1 其实就是

​      // localhost:8040/v1/microservice-provider-user/user/1

​      return newPatternServiceRouteMapper(

​         "(?<name>^.+)-(?<version>v.+$)","${version}/${name}"

​      );

   }

## 4.4 路由的前缀

zuul.prefix: 我们可以指定一个全局的前缀

strip-prefix: 是否将这个代理前缀去掉

zuul:

 prefix: /ecom

 strip-prefix: false

 routes:

  microservice-provider-user: /provider/**

 

比如你访问http://localhost:8040/ecom/microservice-provider-user/user/1，其实真实访问路径是/ecom/user/1

 

zuul:

 prefix: /ecom

 strip-prefix: true

 routes:

  microservice-provider-user: /provider/**

比如你访问http://localhost:8040/ecom/microservice-provider-user/user/1，其实真实访问路径是/user/1,因为我们可以将前缀去掉

 

如果strip-prefix只是放在路由下面，那么就是局部的，不会影响全局

zuul:

 prefix: /ecom

 routes:

  abc:

   path: /provider/**

   service-id: microservice-provider-user

   strip-prefix: true

 

比如你访问http://localhost:8040/ecom/microservice-provider-user/user/1

其实真实访问路径是/user/1,因为我们可以将前缀去掉

 

zuul:

 prefix: /ecom

 routes:

  abc:

   path: /provider/**

   service-id: microservice-provider-user

   strip-prefix: false

比如你访问http://localhost:8040/ecom/provider/user/1

其实真实访问路径是/provider/user/1,因为我们可以将前缀去掉

 

## 4.5 忽略某些路径

zuul:

 ignoredPatterns: /**/admin/**

 routes:

users: /myusers/**

过滤掉path包含admin的请求

 

# 五 Zuul的安全和Header

## 5.1 敏感的Header设置

在同一个系统中微服务之间共享Header,但是某些时候尽量防止让一些敏感的Header外泄。因此很多场景下，需要通过为路由指定一系列敏感的Header列表。例如：

zuul:

  routes:

   abc: 

​    path: /provider/**

​    service-id: microservice-provider-user

​    sensitiveHeaders:Cookie,Set-Cookie,Authorization

​    url: https://downstream

## 5.2 忽略Header

被忽略的Header不会被传播到其他的微服务去。其实敏感的Heade最终也是走的这儿

zuul:

 ignored-headers: Header1,Header2

默认情况下，ignored-headers是空的

 

 

# 六strangle模式

在迁移现有应用程序或API时，一种常见的模式是“扼杀”旧的端点，用不同的实现慢慢替换它们。Zuul代理是一个有用的工具，因为您可以使用它来处理来自旧端点客户端的所有流量，但是将一些请求重定向到新端点。

zuul:

 routes:

  first:

   path: /first/**

   url: http://first.example.com

  second:

   path: /second/**

   url: forward:/second

  third:

   path: /third/**

   url: forward:/3rd

  legacy:

   path: /**

   url: http://legacy.example.com // 遗留的或者剩余的都走这个路径

 

 

# 七 使用Zuul上传文件

## 7.1 如果我们不使用Zuul上传文件，即通过web框架，也可以实现文件上传，即原始的文件上传功能：

\# 添加对应的依赖

<dependency>

 <groupId>org.springframework.cloud</groupId>

 <artifactId>spring-cloud-starter-eureka</artifactId>

</dependency>

<dependency>

 <groupId>org.springframework.boot</groupId>

 <artifactId>spring-boot-starter-web</artifactId>

</dependency>

<dependency>

 <groupId>org.springframework.boot</groupId>

 <artifactId>spring-boot-starter-actuator</artifactId>

</dependency>

 

\# 创建上传文件的controller

@Controller

public classFileUploadController {

 

​    @RequestMapping(value="/upload",method=RequestMethod.*POST*)

​    public @ResponseBody StringhandleFileUpload(

​         @RequestParam(value="file",required=true)MultipartFile file){

​      try {

​         byte[] in = file.getBytes();

​         File out = new File(file.getOriginalFilename());

​         FileCopyUtils.*copy*(in, out);

​         return out.getAbsolutePath();

​      } catch (IOException  e) {

​         // TODO Auto-generatedcatch block

​         e.printStackTrace();

​      }

​      return null;

   }

}

 

\# 编写配置文件application.yml

![img](%E4%BD%BF%E7%94%A8Zuul%E6%9E%84%E5%BB%BAAPI%20Gateway.assets/20171128095916797.png)



\# 使用工具CURL测试

curl -F"file=@微服务简介.docx"localhost:8086/upload

 

## 7.2 通过Zuul上传文件

\# 不添加/zuul，上传小文件没有问题

curl -F"file=@Hystrix.docx" http://localhost:8050/microservice-consumer/upload

 

\# 不添加/zuul前缀上传大文件

curl -F"file=@ATGPUB.DMP" http://localhost:8050/microservice-consumer/upload

如果不加/zuul前缀就会报错

{"timestamp":1507123311527,"status":500,"error":"InternalServerError","exception":"org.springframework.web.multipart.MultipartException","message":"Couldnot parse multipart servlet request; nested exception isjava.lang.IllegalStateException:  org.apache.tomcat.util.http.fileupload.FileUploadBase$SizeLimitExceededException:the request was rejected because its size (132780234) exceeds the  configuredmaximum(10485760)","path":"/microservice-consumer/upload"}

 

\# 添加/zuul前缀，上传大文件

curl -F"file=@ATGPUB.DMP"http://localhost:8050/zuul/microservice-consumer/upload

 

{"timestamp":1507123418018,"status":500,"error":"InternalServerError","exception":"com.netflix.zuul.exception.ZuulException","message":"TIMEOUT"}

此时已经不是文件大小的错误了，我们可以将上传大文件的超时时间设置长一些

hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds:60000

ribbon:

 ConnectTimeout: 3000

 ReadTimeout: 60000

 

# 八 Zuul的容错与回退

在Spring Cloud, Zuul默认已经整合了Hystrix, 而且如果启动了Dashborad，也可以知道Zuul对Hystrix监控的粒度是微服务，而不是某一个API; 同时也说明所有经过Zuul的请求都会被Hystrix保护起来。

8.1 为Zuul添加回退

想要为Zuul添加回退，需要实现ZuulFallbackProvider接口。在实现类中，指定为哪一个微服务提供回退，并且提供一个ClientHttpResponse作为回退响应。

 

编写Zuul回退类：

@Component

public classUserFallbackProvider  implementsZuulFallbackProvider{

 

​    @Override

​    public String getRoute(){

​      // 指定为哪一个微服务提供回退

​      return "microservice-provider-user";

   }

 

​    @Override

​    publicClientHttpResponse fallbackResponse() {

​      return newClientHttpResponse() {

​         @Override

​         public HttpHeadersgetHeaders() {

​           // 设置header

​           HttpHeaders headers = new HttpHeaders();

​           MediaType mediaType = new MediaType("application",

​                 "json",Charset.*forName*("UTF-8"));

​           headers.setContentType(mediaType);

​           return headers;

​         }

 

​         @Override

​         public InputStreamgetBody()  throws IOException {

​           // 响应体

​           return newByteArrayInputStream(

​              "Usermicro-service is unavailable, please try it again later!".getBytes());

​         }

 

​         @Override

​         public StringgetStatusText()  throws IOException {

​           // 返回状态文本

​           return this.getStatusCode().getReasonPhrase();

​         }

 

​         @Override

​         public HttpStatusgetStatusCode()  throws IOException {

 

​           return HttpStatus.*OK*;

​         }

 

​         @Override

​         public intgetRawStatusCode() throws IOException {

​           // 返回数字类型的状态码

​           return this.getStatusCode().value();

​         }

 

​         @Override

​         public void close() {

 

​         }

​      };

   }

}

 

重新启动Eureka Server，microservice-gateway-zuul-fallback以及microservice-provider-user

我们正常通过zuul访问microservice-provider-user微服务

http://localhost:8040/microservice-provider-user/user/1 没有问题

然后我们关掉microservice-provider-user微服务

```
在访问http://localhost:8040/microservice-provider-user/user/1，则会出现User micro-service is unavailable, please try it again later!
```

而不是以前不友好的那个页面了

 

九 Zuul的高可用
 Zuul的高可用非常关键，因为外部请求到后端的微服务的流量都会经过Zuul。故而在生产环境中一般都需要部署高可用的Zuul以避免单点故障

9.1 Zuul客户端注册到了Eureka Server上

此种情况，Zuul的高可用实现比较简单，只需将多个Zuul节点注册到Eureka Server,就可以实现Zuul高可用。此时，Zuul与其他的微服务高可用没啥区别。Zuul客户端会自动从Eureka  Server中查询Zuul Server的列表，并使用Ribbon负载均衡的请求Zuul集群

![img](%E4%BD%BF%E7%94%A8Zuul%E6%9E%84%E5%BB%BAAPI%20Gateway.assets/20171128095919917.png)



9.2 Zuul客户端未注册到Eureka Server上

如果Zuul客户端未注册到Eureka上，因为微服务可能被其他微服务调用，也可能直接被终端调用，比如手机App。此种情况下，我们需要借助额外的负载均衡器来实现Zuul的高可用，比如Nginx，比如HAProxy或者F5等

![img](%E4%BD%BF%E7%94%A8Zuul%E6%9E%84%E5%BB%BAAPI%20Gateway.assets/20171128095923392.png)



# 九 Sidecar



我们可以使用Sidecar整合非JVM微服务，比如C++、Python、PHP等语言写的。其他非JVM微服务可操作Eureka的REST端点，从而实现注册与发现。事实上，也可以使用sidecar更加方便整合非JVM微服务


 \# 首先添加依赖

<dependency>

 <groupId>org.springframework.cloud</groupId>

 <artifactId>spring-cloud-starter-eureka</artifactId>

</dependency>

<dependency>

 <groupId>org.springframework.cloud</groupId>

 <artifactId>spring-cloud-starter-zuul</artifactId>

</dependency>

<dependency>

 <groupId>org.springframework.cloud</groupId>

 <artifactId>spring-cloud-netflix-sidecar</artifactId>

</dependency>

 

\# 添加启动类，在启动类上加上@EnableSidecar注解，声明这是一个Sidecar

这个注解整合了三个注解即：

@EnableCircuitBreaker

@EnableDiscoveryClient

@EnableZuulProxy

@SpringBootApplication

@EnableSidecar

public classZuulSidecarApplication {

​    public static  void main(String[] args)  throws Exception {

​      SpringApplication.*run*(ZuulSidecarApplication.class, args);

   }

}

\# 编写application.yml

Sidecar.port指的就是其他语言微服务的端口

![img](%E4%BD%BF%E7%94%A8Zuul%E6%9E%84%E5%BB%BAAPI%20Gateway.assets/20171128095928250.png)



Sidecar与其他语言的微服务分离部署

上面我们是将其他语言微服务和sidecar放在同一个机器上，现实中，常常会将Sidecar与IVM微服务分离部署，部署在不同的主机上面或者容器中，这时候应该如何配置呢？

方法一：

eureka:

 instance:

hostname: # 非JVM微服务所在的hostname

方法二：

sidecar:

 hostname: # 非JVM微服务所在的hostname

 ip-address: # 非JVM微服务所在的IP 地址

*注意：如果这种微服务太多，而且还涉及到集群的话使用sidecar我们应该权衡一下。因为一个sidecar只能对应一个其他语言写的微服务，如果很多，那表示就多个sidecar了。*

 

# 十 Zuul的过滤器

过滤器是Zuul的核心组件，Zuul大部分功能都是通过过滤器来实现的。

## 10.1 过滤器类型和请求生命周期

Zuul中定义了四种标准的过滤器类型，这些过滤器类型对应于典型的生命周期。

PRE: 这种过滤器在请求被路由之前调用。可利用其实现身份验证等

ROUTING: 这种过滤器将请求路由到微服务，用于构建发送给微服务的请求，并使用Apache Http Client或者Netflix Ribbon请求微服务

POST: 这种过滤器在路由到微服务以后执行，比如为响应添加标准的HTTP Header，收集统计信息和指标，将响应从微服务发送到客户端等

ERROR: 在其他阶段发生错误时执行该过滤器

除了默认的过滤器类型，Zuul还允许创建自定义的过滤器类型。

![img](%E4%BD%BF%E7%94%A8Zuul%E6%9E%84%E5%BB%BAAPI%20Gateway.assets/20171128095931559.png)



## 10.2 编写Zuul过滤器



我们只需要继承抽象类ZuulFilter过滤器即可，让该过滤器打印请求日志

public classPreRequestLogFilter  extends ZuulFilter {

 

​    private static  final Logger *logger* = LoggerFactory.*getLogger*(

​         PreRequestLogFilter.class);

​    @Override

​    public Object run() {

​      RequestContext context = RequestContext.*getCurrentContext*();

​      HttpServletRequest reqeust = context.getRequest();

​      PreRequestLogFilter.*logger*.info(

​           String.*format*("send %srequest to %s",

​           reqeust.getMethod(),

​           reqeust.getRequestURL().toString()));

​      return null;

   }

 

​    @Override

​    public boolean shouldFilter() {

​      // 判断是否需要过滤

​      return true;

   }

 

​    @Override

​    public int filterOrder() {

​      // 过滤器的优先级，越大越靠后执行

​      return 1;

   }

 

​    @Override

​    public StringfilterType() {

​      // 过滤器类型

​      return "pre";

   }

}

 

修改启动类，为启动类添加：

@SpringBootApplication

@EnableZuulProxy

public classZuulFilterApplication {

​    @Bean

​    publicPreRequestLogFilter preRequestLogFilter(){

​      return newPreRequestLogFilter();

   }

 

​    public static  void main(String[] args)  throws Exception {

​      SpringApplication.*run*(ZuulFilterApplication.class, args);

   }

}

## 10.3 禁用Zuul过滤器

Spring  Cloud默认为Zuul编写并启用了一些过滤器，例如DebugFilter,FromBodyWrapperFilter，PreDecorationFilter等，这些过滤器都存放在spring-cloud-netflix-core这个jar里的

在某些场景下，希望禁掉一些过滤器，该怎办呢？

只需设置zuul.<SimpleClassName>.<filterType>.disable=true即可，比如

zuul.PreRequestLogFilter.pre.disable=true

 

# 十一 Zuul聚合微服务

许多场景下，一个外部请求，可能需要查询Zuul后端多个微服务。比如说一个电影售票系统，在购票订单页上，需要查询电影微服务，还需要查询用户微服务获得当前用户信息。如果让系统直接请求各个微服务，就算Zuul转发，网络开销，流量耗费，时长都不是很好的。这时候我们就可以使用Zuul聚合微服务请求，即应用系统只发送一个请求给Zuul,由Zuul请求用户微服务和电影微服务，并把数据返给应用系统。












## 业务业务





## 服务保障

不相信前端，不相信上游服务

业务失败/请求重试做好后续机制和监控警告



## IOC

之前使用Autofac的默认的生命周期；默认的不是单例的哦

默认自定义的业务seriver/Manager 每次都会返回一个新的实例；

但Logger，自定义校验器类是使用的单例

Lifetime Scope没有太多关注，再加上同一请求中链路会往复交织 在跨服务链路节点下往往有多次出场 ，再加上异步Task甚至有requestId或者user识别身份标识的丢失或者错乱；但应该这种周期模式有其用武之地的；

三种生命周期：

> ```
> Per Dependency
> Single Instance
> Per Lifetime Scope
> ```

## 配置中心



## ApiGateway

之前是使用的



## DB

索引之外，

Linq% Lambda

三种类型



## 其他服务组件



服务注册发现	原先使用con	Eureka+Ribbon

认证授权			Id4	Spring Security OAuth2



## MQ










【文集	伟大的架构】

https://www.jianshu.com/nb/6441016

# 四种伟大的程序架构1/4	干净

https://www.jianshu.com/p/0be992b7d0e6

--Clean Architecture

--Uncle Bob在2012年对web应用提出的建议



[葡萄喃喃呓语](https://www.jianshu.com/u/2c67926c48ce)

2016.09.24 19:51:47字数 1,937阅读 1,790

------

## The Clean Architecture | 8th Light 

 [https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html](https://link.jianshu.com?t=https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)

### 干净的架构The Clean Architecture

这是著名软件大师Bob大叔提出的一种架构，也是当前各种语言开发架构。干净架构提出了一种单向依赖关系，从而从逻辑上形成一种向上的抽象系统。
 我们经常听说过如下各种架构：
 [六边形架构Hexagonal Architecture](https://link.jianshu.com?t=http://alistair.cockburn.us/Hexagonal+architecture) (也称为 端口和[适配器](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%CA%CA%C5%E4%C6%F7&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=4&seller_id=1&di=128)) 这是由Alistair Cockburn 提出，被Steve Freeman和 Nat Pryce在他们的书籍[Growing Object Oriented Software](https://link.jianshu.com?t=http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627)中采取的。
 [Onion Architecture](https://link.jianshu.com?t=http://jeffreypalermo.com/blog/the-onion-architecture-part-1/) 作者Jeffrey Palermo
 [Screaming Architecture](https://link.jianshu.com?t=http://blog.8thlight.com/uncle-bob/2011/09/30/Screaming-Architecture.html) Bob大叔
 [DCI](https://link.jianshu.com?t=http://www.amazon.com/Lean-Architecture-Agile-Software-Development/dp/0470684208/) 由James Coplien和Trygve Reenskaug推动
 [BCE](https://link.jianshu.com?t=http://www.amazon.com/Object-Oriented-Software-Engineering-Approach/dp/0201544350)  Ivar Jacobson在他的书籍*Object Oriented Software Engineering: A Use-Case Driven Approach*提出

虽然这些架构在细节上都略有不同，但他们都非常相似。它们都具有相同的目标，那就是**分离关注**。他们都通过软件**分层**来实现这种分离。至少有一个层代表业务规则，而另一个层用于接口。

![img](DCI%E4%B8%8EDDD.assets/2569324-db9b81e9e57d1cb8.png)

Clean Architecture

### **依赖规则Dependency Rule**

 上图中同心圆代表各种不同领域的软件。一般来说，越深入代表你的软件层次越高。**外圆是战术实现机制**，**内圆的是战略核心策略**。
 使此体系架构能够工作的关键是**依赖规则**。这条规则规定[源代码](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%D4%B4%B4%FA%C2%EB&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=3&seller_id=1&di=128)只能**向内依赖**，在最里面的部分对外面一点都**不知道**，也就是内部不依赖外部，而外部依赖内部。这种依赖包含代码名称，或类的函数，变量或任何其他命名软件实体。
 同样，在外面圈中使用的数据格式不应被内圈中使用，特别是如果这些数据格式是由外面一圈的框架生成的。我们**不希望任何外圆的东西会影响内圈层**。

### **实体Entities**

 实体**封装的是企业业务规则**，一个实体能是一个带有方法的对象，或者是一系列数据结构和函数，只要这个实体能够被不同的应用程序使用即可。
 如果你没有编写企业软件，只是编写简单的应用程序，这些实体就是应用的业务对象，它们封装着最普通的高级别业务规则，你不能希望这些实体对象被一个页面的分页导航功能改变，也不能被安全机制改变，**操作实现层面的任何改变不能影响实体层，只有业务需求改变了才可以改变实体**。

### **用例Use Cases**

 在这个层的软件包含应用指定的业务规则，它**封装和实现系统的所有用例**，这些用例会混合各种来自实体的各种数据流程，并且指导这些实体使用企业规则来完成用例的功能目标。
 我们并不期望改变这层会影响实体层. 我们也不期望这层被更外部如[数据库](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%CA%FD%BE%DD%BF%E2&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=1&seller_id=1&di=128) UI或普通框架影响，这层也是因为关注而外部分离的。
 我们期望应用层面的技术操作都不能影响用例层，如果**需求中用例发生改变，这个层的代码才会发生改变**。

### **接口适配器Interface Adapters**

 这一层的软件基本都是一些[适配器](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%CA%CA%C5%E4%C6%F7&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=4&seller_id=1&di=128)，主要用于将用例和实体中的数据转换为外部系统如数据库或Web使用的数据，在这个层次，可以包含一些GUI的MVC架构，表现视图 [控制器](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%BF%D8%D6%C6%C6%F7&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=2&seller_id=1&di=128)都属于这个层，模型Model是从控制器传递到用例或从用例传递到视图的数据结构。
 通常在这个层数据被转换，从用例和实体使用的数据格式转换到持久层框架使用的数据，主要是为了存储到数据库中，这个圈层的代码是一点和数据库没有任何关系，如果数据库是一个SQL数据库， 这个层限制使用SQL语句以及任何和数据库打交道的事情。.

### 框架和驱动

 最外面一圈通常是由一些框架和工具组成，如[数据库](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%CA%FD%BE%DD%BF%E2&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=1&seller_id=1&di=128)Database, Web框架等. 通常你不必在这个层不必写太多代码，而是写些**胶水性质的代码与内层进行粘结通讯**。这个层是细节所在，**Web技术是细节，数据库是细节，我们将这些实现细节放在外面以免它们对我们的业务规则造成影响伤害**。

### 只有四个圈层吗？

 这个圆圈图是示意性的。您可能会发现您需要的不仅仅是这四个。也没有规定说你必须始终只有这四个。然而，依赖规则始终适用。源代码的**依赖关系总是由外向内。当你越向内时，抽象水平越高。而最外面的一圈是低层次的具体细节。当你越向内时软件变得越为抽象，封装了更高层次的策略**。

### 跨边界流程

 在图的右下方是我们如何越过圆边界的例子。它显示[控制器](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%BF%D8%D6%C6%C6%F7&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=2&seller_id=1&di=128)和界面之间是如何和用例进行通信的。注意控制流程。它开始于控制器，通过用例，然后在界面处执行。还要注意[源代码](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%D4%B4%B4%FA%C2%EB&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=3&seller_id=1&di=128)的依赖关系。他们中的每一个点都是指向内部用例。我们通常使用依赖注入来实现这种依赖。
 那么数据如何跨层流动呢？

##### DTO

 通常跨层的数据是简单的数据结构。如果你喜欢你可以使用基本结构或简单的数据传输对象DTO。或可以函数可以调用数据参数。或者你可以打包到哈希表中，或为它建构一个对象。最重要是跨层传递是孤立的、 简单的数据结构。
 我们不想让这个数据结构是一个实体或[数据库](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%CA%FD%BE%DD%BF%E2&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=1&seller_id=1&di=128)记录，因为我们不希望它们有任何的依赖关系，这会违反了依赖规则。例如，许多数据库框架在查询响应中返回一个方便的数据格式。我们可能会要求这对这个记录重构，因为我们不想要跨层向内传递数据库记录。这就违反了依赖规则，它会迫使内圈要知道关于外圈的东西。所以当我们跨层传递数据，它总是以对内圈最方便的形式。

###  总结

 符合这些简单的规则将会节省您大量的头痛开发。通过将软件**分离到各种层**，并符合**依赖规则**，这样您创建一个系统**本质上是可测试**，这意味着很多好处。

------

Facebook移动架构：Android Flux架构详解 - 51CTO.COM
 [http://developer.51cto.com/art/201508/489423.htm](https://link.jianshu.com?t=http://developer.51cto.com/art/201508/489423.htm)

**今天: Clean Architecture**
 目前的趋势是采用**Uncle Bob**在2012年对web应用提出的建议： [Clean Architecture](https://link.jianshu.com?t=https://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html)。
 但是我发现Clean Architecture对于绝大多数安卓应用来说都有点过度设计了。









# 四种伟大的程序架构2/4--DCI架构

## DCI架构

链接：https://www.jianshu.com/p/dfbf66272b7c

DCI架构 Data, context and interaction - 解道Jdon  [http://www.jdon.com/dci.html](https://link.jianshu.com?t=http://www.jdon.com/dci.html)

DCI是对象的Data数据, 对象使用的Context场景, 对象的Interaction交互行为三者简称，**DCI是一种特别关注行为的模式**(可以对应GoF行为模式)，而**MVC模式是一种结构性模式**，DCI可以使用演员场景表演来解释，某个实体在某个场景中扮演包公，实施包公升堂行为；典型事例是银行帐户转帐，转帐这个行为按照[DDD](https://link.jianshu.com?t=http://www.jdon.com/ddd.html)很难划分到帐号对象中，它是跨两个帐号实例之间的行为，我们可以看成是帐号这个实体(PPT，见[四色原型](https://link.jianshu.com?t=http://www.jdon.com/colorUML.html))在转帐这个场景，实施了钞票划转行为，这种新的角度更加贴近需求和自然，结合[四色原型](https://link.jianshu.com?t=http://www.jdon.com/colorUML.html) [DDD](https://link.jianshu.com?t=http://www.jdon.com/ddd.html)和DCI可以一步到位将需求更快地分解落实为可运行的代码，是国际上软件领域的一场革命。

 

　　 

 DCI是对象的Data数据, 对象使用的Context场景,  对象的Interaction交互行为三者简称，   DCI是一种特别关注行为的模式(可以对应GoF行为模式)，而MVC模式是一种结构性模式，DCI可以使用演员场景表演来解释，某个实体在某个场景中扮演包公，实施包公升堂行为；典型事例是银行帐户转帐，转帐这个行为按照[DDD](https://www.jdon.com/ddd.html)很难划分到帐号对象中，它是跨两个帐号实例之间的行为，我们可以看成是帐号这个实体(PPT，见[四色原型](https://www.jdon.com/colorUML.html))在转帐这个场景，实施了钞票划转行为，这种新的角度更加贴近需求和自然，结合[四色原型](https://www.jdon.com/colorUML.html)[DDD](https://www.jdon.com/ddd.html)和DCI可以一步到位将需求更快地分解落实为可运行的代码，是国际上软件领域的一场革命。



### [DDD DCI和领域事件 ](http://www.jdon.com/jdonframework/dci.html)              

 [**DCI架构是什么？**](http://www.jdon.com/37976) 

 [DCI架构本质](http://www.jdon.com/38266#23127442) 

 [**MVC模式已死?**](http://www.jdon.com/38448) 

[面向函数式编程(Functional programming)专题](https://www.jdon.com/functional.html)

[并行并发编程模型](http://www.jdon.com/tags/4973)

[EDA事件驱动或CQRS Event Sourcing架构](https://www.jdon.com/eda.html)

[异步编程架构](http://www.jdon.com/tags/544)

### [更多DCI相关讨论](http://www.jdon.com/tags/10443)

作者：葡萄喃喃呓语
链接：https://www.jianshu.com/p/dfbf66272b7c
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



## DCI代表数据，上下文，交互【[URL](https://www.jdon.com/51050)】

 来自Kamil Toszek一篇DCI与DDD结合的文章：我正在实践领域驱动设计方法，它有一些很好的部分比如有界上下文（模块分离很好 - 每个模块代表上下文边界），还有一些 - 对我来说 - 不是那么好的部分：领域富血模型。



，这个想法是应用程序可以在数据（软件是什么）和功能（软件做什么）之间分开，DCI表示该模型应该是贫血的，但您可以将角色应用到您的模型，从而为你的模型（可切换）这个角色具有的功能。如果一个Person类只有基本属性，将TaxPayerRole这个角色作为构造函数的入参来接受，并为在某些上下文使用场景中提供TaxPayerRole的功能（用例）；也可以将DadRole作为入参接受。

正如Robert Martin写道：“您的架构会大声喊出它的功能，而不是依赖它正在使用的框架”。







# 四种伟大的程序架构3/4--DDD/CQRS

https://www.jdon.com/cqrs.html

## CQRS架构

　　命令查询的责任分离Command Query Responsibility Segregation  (简称CQRS)模式是一种架构体系模式，能够使改变模型的状态的命令和模型状态的查询实现分离。这属于DDD应用领域的一个模式，主要解决DDD在数据库报表输出上处理方式。                                                    

　　Greg Young在infoQ的采访中“[State Transitions in Domain-Driven Design](http://www.infoq.com/interviews/greg-young-ddd)”谈到了CQRS，Greg 解释了把领域模型分为两种：状态校验，以及状态转换，维持当前状态的一个视图。

![img](https://www.jdon.com/simgs/tools/cqrs.png)

　　在客户端就将数据的CRUD的新增修改删除CUD等操作和查询R进行分离，前者称为Command，走Command bus进入Domain对模型进行操作，而查询则从另外一条路径直接使用SQL对数据进行操作，比如报表输出等，发挥SQL的特点。

　　当一个Command进来时，从仓储Repository加载一个聚合aggregate对象群，然后执行其方法和行为。这样，会激发聚合对象群产生一个事件，这个事件可以分发给仓储Repository，或者分发给Event  Bus事件总线，比如JavaEE的消息总线等等。事件总线将再次激活所有监听本事件的处理者。当然一些处理者会执行其他聚合对象群的操作，包括数据库的更新。

下图是[开源JiveJdon](https://github.com/banq/jivejdon)的CQRS图：

![img](https://www.jdon.com/simgs/jivejdon/cqrs.png)

　　在JiveJdon中，查询数据库是使用缓存，而写入数据库使用普通MySQL，两者之间数据同步通过领域事件实现最终一致性。

　　查询与写入数据库的分离，可以实现专门为各自查询 读取而设计特别的数据表结构，专门为查询进行优化。

　　如果采取事件溯源EventSourcing，保存记录的不是聚合当前状态，而是导致状态变化的事件日志 ，那么可以回放，从而找到重要状态改变的轨迹与原因，这是从事件日志追溯来源。

　　虽然这种架构有些复杂，但是好处却很多，主要的是实现透明的分布式处理Transparent  distributed  processing，当使用事件作为状态改变的引擎时，你可以通过实现多任务并发处理，比如通过JVM并行计算或事件消息总线机制，事件能够很容易序列化，并在多个服务器之间传送。而查询操作则专门优化。                  

> ### [**事件是一等公民**](http://www.jdon.com/49246)





# 四种伟大的程序架构4/4--六边形架构



https://www.jdon.com/eda.html

##  EDA事件驱动架构

　　事件代表过去发生的事件，事件既是技术架构概念，也是业务概念。以事件为驱动的编程模型称为事件驱动架构EDA。 

　　EDA是一种以事件为媒介，实现组件或服务之间最大松耦合的方式。传统面向接口编程是以接口为媒介，实现调用接口者和接口实现者之间的解耦，但是这种解耦程度不是很高，如果接口发生变化，双方代码都需要变动，而事件驱动则是调用者和被调用者互相不知道对方，两者只和中间消息队列耦合。

![eda](https://www.jdon.com/simgs/eda.png)

## 　　事件驱动有以下特征：

1. 生产者producer发生实时事件
2. 推送通知
3. 生产者发射即完成fire-and -orget
4. 消费者consumer立即响应
5. 事件与命令是有区别的

　　借助消息系统异步模型的特点，事件驱动也有异步特征，传统方法调用比如调用b.xxmethod()是一种同步模型，这时必须等待b的方法执行完才能继续执行其他代码，RPC远程方法调用也是一种同步模型，而对于异步模型来说，事件生产者发出事件后，不必等待回应，可以继续执行下面的代码。

　　但是不代表使用了消息系统的架构都是EDA，SOA面向服务驱动的架构中也使用消息系统作为ESB，两者使用方式不同，三种不同交互方式：

1. 时间驱动：比如cron定时计划执行
2. 请求驱动：客户端和服务器端之间，常见SOA
3. .事件驱动：以事件为特征。实时。

　　请求驱动+消息系统和事件驱动+消息系统有本质区别，前者是由请求者作为消息生产者，主要目的是为了得到响应，因此是一种请求响应模型；而后者重点是在消息消费者，不是在消息生产者，业务逻辑站在消费者角度完成，业务逻辑的完成靠事件驱动来执行，而前者业务逻辑是在消息生产者完成，当业务逻辑中需要什么依赖或资源，依靠发送消息来拉取完成。这两种区别本质是拉Poll和推Push的区别。

　　正是因为EDA这种和传统SOA的本质区别，现在诞生一种领域EDA，其中包括[CQRS](https://www.jdon.com/cqrs.html) [EventSourcing](http://www.jdon.com/tags/17268) [领域事件](http://www.jdon.com/tags/20395)等等。同时，传统的SOA将业务领域逻辑切分成不同系统，对外表现为服务，这种方式导致业务逻辑跨越多个系统，导致业务逻辑散落各处，寻找维护不方便，造成业务逻辑的污染和膨胀。

　　使用EDA改造传统SOA，比如，如果一个报表系统想知道交易系统的状态，它不是发送一个消息给交易系统，拉取它当前的状态，而是向事件总线订阅，这样当交易系统有状态报告时，将发出事件通知报表系统。

　　EDA的可扩展性和吞吐量上要强于传统SOA，EDA类似组装生产线，下图对于一个顺序线性的处理过程，6个步骤分别是接受 确认 保存 产生PDF 发送Email 输出展现，花去365ms：

![img](https://www.jdon.com/simgs/soa/edascale.png)

而组装线的EDA方式，总是询问着6步中是否可以让别人协同帮助完成？其中第4步和第5步是可以的，因此整个处理时间提升到115ms，提升了70%的响应时间：

![img](https://www.jdon.com/simgs/soa/edascale2.png)

详细的组装线如下，这实际也是一种SEDA，Staged EDA:

![img](https://www.jdon.com/simgs/soa/seda2.png)

　　最终我们可以完成一个新的基于领域事件的D-EDA+SOA架构如下：

![eda soa](https://www.jdon.com/simgs/soa/edasoa.png)

相关文章

---









### [你应该知道的四种优秀架构](http://designzum.com/2014/01/28/4-great-programming-architectures-you-should-know/)

[回到目录](https://www.cnblogs.com/Leo_wl/p/3981061.html#_labelTop)

#除非你是非常熟悉基础编程的整个世界，否则你很难了解编程架构到底是什么。所以我们假设你并不太了解编程技术，那么我会说，编程是一种定义逻辑的途径或方法，这种逻辑以代码方式设计，让指定的编译器能够理解它，让编译器能够知道如何指挥计算机执行相应的功能。对于一个新手来说，这可能是编程的最简单的定义(banq注：对于缺乏逻辑的新手，这可能是最难懂的定义，因此，对于可以不编程的架构师来说最基本的能力是逻辑能力)。  基本上有三种类型的编程：低层次的编程，中间级编程和高级编程。所有这三种编程类型中通用的事情是：都可以执行相同的功能。只是对于不同编程类型具体执行的方式是不同的。  当一个程序很容易地运行,有可能在同样设备上也有其它程序运行。但是，如果所有正在运行的程序需要内部通讯怎么办？这是由该程序的架构来解决这个问题。一个程序架构是一种设计的结构,在设计时就要考虑相互通讯方案，两个程序通讯也许中间需要经历许多相互等待的阶段，因此，当你使用你喜欢的任何语言编写这种程序时，必须要记住，程序必须遵循的架构，如线性编程(顺序编程)，也就是说所有的步骤要遵循一个接一个的顺序，这当然会付出执行时间的代价。另一方面，当所有的步骤都是并行执行，并在最后一步全部完成时，所需的累积(cumulative )时间是相当少。因此，最好的架构是至少有一个累积等待阶段。  但是，这并不意味着线性编程就没有用而总是使用并行架构。重要的是要知道最好的几种可能的架构，这样您可以轻松地创建一个适合所有要求的优化方案。  下面是四种伟大的程序架构：  1. [Clean架构](http://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html) ![img](http://blog.8thlight.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)  外圈的层次可以依赖内层，反之不可以，内圈核心的实体代表业务，不可以依赖其所处的技术环境。  2.[DCI架构](http://www.artima.com/articles/dci_vision.html)，[本站中文](http://www.jdon.com/dci.html)[**DCI**](http://www.jdon.com/dci.html)架构专题，DCI代表Data, Context, Interaction。  3.[DDD/CQRS](http://cqrsguide.com/ddd) 领域驱动设计，[本站](http://www.jdon.com/ddd.html)[**DDD**](http://www.jdon.com/ddd.html)专题  领域驱动设计对于成功交付和维护[**CQRS**](http://www.jdon.com/cqrs.html)的系统非常重要。 [**DDD**](http://www.jdon.com/ddd.html)作为一项战略方针，允许将复杂的问题域划分为单独的块（称为有界上下文），虽然有很多方式如：不同的心智Mental模式，组织政治，域语言学等也是这样做，但是[**DDD**](http://www.jdon.com/ddd.html)建立了一个有界的心智mental模式，这样商务人士也可以理解，程序员也可以很容易地在代码中实现。   CQRS，作为一种战术办法，是实现[**DDD**](http://www.jdon.com/ddd.html)建模领域的最佳途径之一。事实上，它就是因为这个目标而诞生在这个世界上。  相关资源：[DDD – ](http://prezi.com/akrfq7jyau8w/ddd-cqrs-leaven-v20/)[**CQRS**](http://www.jdon.com/cqrs.html) Leaven V20  4.[六边形架构](http://alistair.cockburn.us/Hexagonal+architecture) ![img](http://alistair.cockburn.us/get/2301)  允许应用程序都是由用户，程序，自动化测试或批处理脚本驱动的，在[**事件驱动**](http://www.jdon.com/eda.html)和数据库环境下被开发和隔离测试。一个事件从外面世界到达一个端口，特定技术的适配器将其转换成可用的程序调用或消息，并将其传递给应用程序。该应用程序是可以无需了解输入设备的性质(调用者是哪个)。当应用程序有结果需要发出时，它会通过一个端口适配器发送它，这个适配器会创建接收技术（人类或自动）所需的相应信号。该应用程序与在它各方面的适配器形成语义良性互动，但是实际上不知道适配器的另一端的谁在处理任务。

​    分类:             [[12\]Architecture](https://www.cnblogs.com/Leo_wl/category/225687.html)

​        [好文要顶](javascript:void(0);)        [关注我](javascript:void(0);)    [收藏该文](javascript:void(0);)    [![img](DCI%E4%B8%8EDDD.assets/icon_weibo_24.png)](javascript:void(0);)    [![img](DCI%E4%B8%8EDDD.assets/wechat.png)](javascript:void(0);)

[![img](DCI%E4%B8%8EDDD.assets/u104109.gif)](https://home.cnblogs.com/u/Leo_wl/)

​            [HackerVirus](https://home.cnblogs.com/u/Leo_wl/)
​            [关注 - 247](https://home.cnblogs.com/u/Leo_wl/followees/)
​            [粉丝 - 3625](https://home.cnblogs.com/u/Leo_wl/followers/)        

​                [+加关注](javascript:void(0);)    

​        0    

​        0    

​    

[关注](javascript:void(0);) | [顶部](https://www.cnblogs.com/Leo_wl/p/3981061.html#top) | [评论](javascript:void(0);)

​    [« ](https://www.cnblogs.com/Leo_wl/p/3980783.html) 上一篇：    [干净的架构The Clean Architecture](https://www.cnblogs.com/Leo_wl/p/3980783.html)    
​    [» ](https://www.cnblogs.com/Leo_wl/p/3981121.html) 下一篇：    [Scala是一门现代的多范式编程语言](https://www.cnblogs.com/Leo_wl/p/3981121.html)

​		posted on  2014-09-19 11:27 [HackerVirus](https://www.cnblogs.com/Leo_wl/) 阅读(1918) 评论(0) [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=3981061) [收藏](javascript:void(0)) 





[刷新评论](javascript:void(0);)[刷新页面](https://www.cnblogs.com/Leo_wl/p/3981061.html#)[返回顶部](https://www.cnblogs.com/Leo_wl/p/3981061.html#top)

​    注册用户登录后才能发表评论，请     [登录](javascript:void(0);)     或     [注册](javascript:void(0);)，    [访问](https://www.cnblogs.com/) 网站首页。

[【推荐】了解你才能更懂你，博客园首发问卷调查，助力社区新升级](https://www.wenjuan.com/s/3uIjeq0/)
[【推荐】超50万行VC++源码: 大型组态工控、电力仿真CAD与GIS源码库](http://www.ucancode.com/index.htm)
[【推荐】首次公开！三代技术人深度对话，《云上朗读者》开放下载](https://developer.aliyun.com/article/761673?utm_content=g_1000133217)

<iframe style="border: 0px none; vertical-align: bottom; --darkreader-inline-border-top:currentcolor; --darkreader-inline-border-right:currentcolor; --darkreader-inline-border-bottom:currentcolor; --darkreader-inline-border-left:currentcolor;" src="https://3d4ed21b595eb4eaa92850b539308b03.safeframe.googlesyndication.com/safeframe/1-0-37/html/container.html" id="google_ads_iframe_/1090369/C1_0" title="3rd party ad content" name="" scrolling="no" marginwidth="0" marginheight="0" data-is-safeframe="true" data-google-container-id="1" data-load-complete="true" data-darkreader-inline-border-top="" data-darkreader-inline-border-right="" data-darkreader-inline-border-bottom="" data-darkreader-inline-border-left="" width="300" height="250" frameborder="0"></iframe>

**相关博文：**
·  [四种JavaEE架构简介](https://www.cnblogs.com/shaohsiung/p/9552990.html)
·  [优秀架构师必须掌握的架构思维](https://www.cnblogs.com/kaleidoscope/p/9722599.html)
·  [优秀设计团队必需的四种成员](https://www.cnblogs.com/liangle/archive/2011/07/26/2512648.html)
·  [优秀设计团队必需的四种成员](https://www.cnblogs.com/rmbteam/archive/2011/07/26/2117772.html)
·  [直击架构本质：优秀架构师必须掌握的几种架构思维](https://www.cnblogs.com/lfs2640666960/p/9439857.html)
»  [更多推荐...](https://recomm.cnblogs.com/blogpost/3981061)

​    **最新 IT 新闻**:    
 ·              [对话华住创始人季琦：酒店业在三四线城市还有盈利机会](https://news.cnblogs.com/n/663957/)            
 ·              [王健林电商帝国梦碎：曾拉拢腾讯百度组局，一年换一个负责人](https://news.cnblogs.com/n/663956/)            
 ·              [马化腾每天刷Leetcode？代码你打算写到几岁？](https://news.cnblogs.com/n/663951/)            
 ·              [【书评】《科技向善》：在“基因编辑”这件事中，技术无辜吗？](https://news.cnblogs.com/n/663955/)            
 ·              [金嗓子创始人江佩珍限制出境：为何欠5100万广告费不还？](https://news.cnblogs.com/n/663954/)            
​    » [更多新闻...](https://news.cnblogs.com/)

​	Powered by:  
​    [博客园](https://www.cnblogs.com/) 
 Copyright © 2020 HackerVirus




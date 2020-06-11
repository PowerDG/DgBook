

# 四种伟大的程序架构1/4	干净

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
 通常跨层的数据是简单的数据结构。如果你喜欢你可以使用基本结构或简单的数据传输对象DTO。或可以函数可以调用数据参数。或者你可以打包到哈希表中，或为它建构一个对象。最重要是跨层传递是孤立的、 简单的数据结构。
 我们不想让这个数据结构是一个实体或[数据库](https://link.jianshu.com?t=http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fwww%2Ejdon%2Ecom%2Fartichect%2Fthe%2Dclean%2Darchitecture%2Ehtml&p=baidu&c=news&n=10&t=tpclicked3_hc&q=banq_cpr&k=%CA%FD%BE%DD%BF%E2&k0=%CA%FD%BE%DD%BF%E2&kdi0=8&k1=%BF%D8%D6%C6%C6%F7&kdi1=8&k2=%D4%B4%B4%FA%C2%EB&kdi2=1&k3=%CA%CA%C5%E4%C6%F7&kdi3=1&k4=sql&kdi4=8&sid=1e7c4ea576ec9f0b&ch=0&tu=u1683405&jk=bd19187c218e7435&cf=29&fv=14&stid=9&urlid=0&luki=1&seller_id=1&di=128)记录，因为我们不希望它们有任何的依赖关系，这会违反了依赖规则。例如，许多数据库框架在查询响应中返回一个方便的数据格式。我们可能会要求这对这个记录重构，因为我们不想要跨层向内传递数据库记录。这就违反了依赖规则，它会迫使内圈要知道关于外圈的东西。所以当我们跨层传递数据，它总是以对内圈最方便的形式。
 总结
 符合这些简单的规则将会节省您大量的头痛开发。通过将软件**分离到各种层**，并符合**依赖规则**，这样您创建一个系统**本质上是可测试**，这意味着很多好处。

------

Facebook移动架构：Android Flux架构详解 - 51CTO.COM
 [http://developer.51cto.com/art/201508/489423.htm](https://link.jianshu.com?t=http://developer.51cto.com/art/201508/489423.htm)

**今天: Clean Architecture**
 目前的趋势是采用**Uncle Bob**在2012年对web应用提出的建议： [Clean Architecture](https://link.jianshu.com?t=https://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html)。
 但是我发现Clean Architecture对于绝大多数安卓应用来说都有点过度设计了。









# 四种伟大的程序架构2/4--DCI架构

## DCI架构

DCI架构 Data, context and interaction - 解道Jdon  [http://www.jdon.com/dci.html](https://link.jianshu.com?t=http://www.jdon.com/dci.html)

DCI是对象的Data数据, 对象使用的Context场景, 对象的Interaction交互行为三者简称，**DCI是一种特别关注行为的模式**(可以对应GoF行为模式)，而**MVC模式是一种结构性模式**，DCI可以使用演员场景表演来解释，某个实体在某个场景中扮演包公，实施包公升堂行为；典型事例是银行帐户转帐，转帐这个行为按照[DDD](https://link.jianshu.com?t=http://www.jdon.com/ddd.html)很难划分到帐号对象中，它是跨两个帐号实例之间的行为，我们可以看成是帐号这个实体(PPT，见[四色原型](https://link.jianshu.com?t=http://www.jdon.com/colorUML.html))在转帐这个场景，实施了钞票划转行为，这种新的角度更加贴近需求和自然，结合[四色原型](https://link.jianshu.com?t=http://www.jdon.com/colorUML.html) [DDD](https://link.jianshu.com?t=http://www.jdon.com/ddd.html)和DCI可以一步到位将需求更快地分解落实为可运行的代码，是国际上软件领域的一场革命。



作者：葡萄喃喃呓语
链接：https://www.jianshu.com/p/dfbf66272b7c
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



## DCI代表数据，上下文，交互【[URL](https://www.jdon.com/51050)】

 来自Kamil Toszek一篇DCI与DDD结合的文章：我正在实践领域驱动设计方法，它有一些很好的部分比如有界上下文（模块分离很好 - 每个模块代表上下文边界），还有一些 - 对我来说 - 不是那么好的部分：领域富血模型。



，这个想法是应用程序可以在数据（软件是什么）和功能（软件做什么）之间分开，DCI表示该模型应该是贫血的，但您可以将角色应用到您的模型，从而为你的模型（可切换）这个角色具有的功能。如果一个Person类只有基本属性，将TaxPayerRole这个角色作为构造函数的入参来接受，并为在某些上下文使用场景中提供TaxPayerRole的功能（用例）；也可以将DadRole作为入参接受。

正如Robert Martin写道：“您的架构会大声喊出它的功能，而不是依赖它正在使用的框架”。
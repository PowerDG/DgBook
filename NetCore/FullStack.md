



## C#部分：

### 1，try后的finallyⅡ

1，try里有一个return语，那点跟在这个try后的finallyⅡ里的代码会不会?
被执行，什么时候被执行



finally可以没有，也可以只有一个。无论有没有发生异常，它总会在这个异常处理结构的最后运行。即使你在try块内用return返回了，在返回前，finally总是要执行，这以便让你有机会能够在异常处理最后做一些清理工作。如关闭数据库连接等等。 



### 2、开启一个新的线程的方法

https://blog.csdn.net/u010317783/article/details/79215833

https://blog.csdn.net/hyy_sui_yuan/article/details/81263281



1.异步委托开启线程 Action

3.通过线程池开启线程

```csharp
Thread thread = new Thread(new ParameterizedThreadStart(RunRefreshProjectMoneyAndOtherMoney));

object[] paramObj = { Token, source.KeyID };

thread.Start(paramObj);
```



 

```csharp
public class Program
    {
        public static void Main(string[] args)
        {
            Task task = new Task(DownLoadFile_My);
            task.Start();
            Console.ReadKey();
        }
        static void DownLoadFile_My()
        {
            Console.WriteLine("开始下载...线程ID:"+Thread.CurrentThread.ManagedThreadId);
            Thread.Sleep(500);
            Console.WriteLine("下载完成!");
        }
    }
```







### 3、如何解决.net中的内存泄漏问题?

https://blog.csdn.net/weixin_34050427/article/details/86054382

https://www.cnblogs.com/darrenji/p/4650822.html

https://www.cnblogs.com/Jessy/p/3605404.html

在.NET中，虽然CLR的GC垃圾回收器帮我们自动回收托管堆对象，释放内存，最大程度避免了"内存泄漏"(应用程序所占用的内存没有得到及时释放)，但.NET应用程序"内存泄漏"的问题还是会存在，如果不加以注意，"内存泄漏"时有发生。

 

 *有关流以及Reader或Writer引起的内存泄漏*

● FileStream
● MemoryStream
● StreamReader
● TextWriter

​			其它与流有关的类，我们也需要注意在用完后及时关闭：



 *静态引用引起的内存泄漏*

​			除非应用程序关闭，对应的内存一直得不到释放。

​			比如有如下遵循"Siingleton"模式的类(没考虑线程安全)。

​			避免让静态实例引用其它实例对象，避免出现静态实例的链式引用。

*委托引起的内存泄漏

​			在调用objectTwo的Dispose方法之前，先解开两者的依赖关系。修

 *非托管资源引起的内存泄漏**

​			对于创建的非托管类型的实例ponter，需要显式释放。	

*实现IDisposable接口的类引起的内存泄漏*

 

所有实现IDisposable接口的类都有一个Dispose方法，如果忘记调用，就造成"内存泄漏"。

### 4.WCF的工作原理以及三种通讯方式

https://zhidao.baidu.com/question/448681621.html



WCF的全称是Windows Communication Foundation，Windows通信基础的意思，是baiMicrosoft为构建面向服务的应用程序而提供的du统一编程模型，zhi它整合.NET平台下所有和分布式系统有关的技术。

主机进程就是服务端，在其中需要创建服务，然后将服务通过EndPoint（终结点）与客户端进行通信。客户端在使用服务时，需要首先创建一个代理服务，然后调用这个代理服务



其次：

WCF本质是面向服务的。主要分为服务端和客户端两部分。服务端和客户端进行通信

　WCF在通信过程中有三种模式：请求与答复、单向、双工通信。以下我们一一介绍。

https://blog.csdn.net/weixin_30648587/article/details/97636861

https://blog.csdn.net/weixin_33860722/article/details/85550270





## 前端部分：

1. documen.write和 innerHTML的区别2、行内元素有哪些?块级元素有哪些?空(void)元素有那些?
  3、页面导入样式时，使用link和@import有什么区别?
  4、CSS选择符有哪些?

  

  

  

  ### 5、JQuery和Vue.js的区别

  6、JavaScript中，如何将以下obj1和obj2合并成last对象 var obj1 = ( name：’Jack'， age：234 )
  var obj2 = ( job：’Worker'， location：’Shanghai' )
  var last = ( name：'Jack'， age：234， job：’Worker'， location：’Shanghai' )

数据库部分：
1. 查询语句不同元素 (where， jion， limit. group by. having 等等) 执行先后顺序?
2、哪些列适合建立索引、哪些不适合建索引?
3、数据库三范式是什么?
4、什么是内连接、外连接、交义连接、笛卡尔积等?
5. drop. truncate. delete 区别1
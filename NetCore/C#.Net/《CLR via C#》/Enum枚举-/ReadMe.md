



每个枚举成员均具有相关联的常数值。此值的类型就是枚举的底层数据类型。每个枚举成员的常数值必须在该枚举的底层数据类型的范围之内。如果没有明确指定底层数据类型则默认的数据类型是int类型。

枚举成员不能相同，但枚举的值可以相同。



```csharp
    enum Man:int //四大名著枚举
        {
            西游记 = 1,
            红楼梦 = 2,
            三国演义 = 3,
            水浒传 = 0
        }
        
        
                //如果要显示指定枚举的底层数据类型很简单，只需在声明枚举的时候加个冒号，后面紧跟要指定的数据类型（可指定类型有：byte、sbyte、short、ushort、int、uint、long、ulong）。 	显式设置枚举的成员常量值，默认是从0开始，逐个递增的。但是以下例子却设置成了1,2,3,0。而且成员值可以一样的。    如下示例：由枚举值获取枚举名称与由枚举名称获取枚举值
```

# [C#枚举的简单使用](https://www.cnblogs.com/xiongze520/p/10271350.html)
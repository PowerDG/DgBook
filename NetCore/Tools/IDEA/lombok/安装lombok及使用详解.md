# [lombok在IntelliJ IDEA下的使用](https://www.cnblogs.com/yjmyzz/p/lombok-with-intellij-idea.html)



 

# Intellij IDEA 安装lombok及使用详解

项目中经常使用bean，entity等类，绝大部分数据类类中都需要get、set、toString、equals和hashCode方法，虽然eclipse和idea开发环境下都有自动生成的快捷方式，但自动生成这些代码后，如果bean中的属性一旦有修改、删除或增加时，需要重新生成或删除get/set等方法，给代码维护增加负担。而使用了lombok则不一样，使用了lombok的注解(@Setter,@Getter,@ToString,@@RequiredArgsConstructor,@EqualsAndHashCode或@Data)之后，就不需要编写或生成get/set等方法，很大程度上减少了代码量，而且减少了代码维护的负担。故强烈建议项目中使用lombok，去掉bean中get、set、toString、equals和hashCode等方法的代码。
————————————————
版权声明：本文为CSDN博主「zhglance」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zhglance/article/details/54931430
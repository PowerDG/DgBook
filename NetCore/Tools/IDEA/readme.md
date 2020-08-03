## IDEA直接跳转到方法的实现类

chy_628 2018-12-13 15:34:31 22280 收藏 2
版权

**1、Ctrl + Alt + 鼠标左键**

**2、Ctrl + Alt + B**

———————————————
版权声明：本文为CSDN博主「chy_628」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/chy_628/article/details/84989533





## IntelliJ IDEA使用技巧--跳转

Torey_Li 2018-09-15 16:30:03 6819 收藏 4
分类专栏： IntelliJ IDEA使用技巧
版权

注：以下命令都是在windos下测试，其他系统环境下，有可能不同

1 Ctrl+Shift+F12 全屏

2 Alt+2:会跳到Favorites,Favorites作用是方便看到代码中加的标签，收藏，代码中加的断点，也会在这里显示

在全屏的情况下按：Alt+1 ,光标会调到左边Project，再次按Esc,光标会再次跳到编辑界面

3 Ctrl+Shift+A 搜索快捷键位置：Help-->Find Action...-->Ctrl+Shift+A

4 快速打开菜单栏Alt+菜单栏名称首字母，例如：File--->Alt+File;Edit--->Alt+E;View--->Alt+V;

==========高效定位代码==========

无处不在的跳转

1 Ctrl+alt+]/[ 项目之间的跳转,保证有两个项目，快捷键位置 Window--->Next Project Windowm-->Ctrl+alt+]/[

2 Ctrl+E 今天编辑的文件之间的跳转，快捷键的位置：Ctrl+Shift+A---->搜索框输入Recent Files--->Ctrl+E

3 Ctrl+Shift+E 当前编辑的文件不想在搜索框搜索， 快捷键位置：Ctrl+Shift+A--->Recently Changed Files-->Ctrl+Shift+E

4 Ctrl+Shift+Backspace 编辑位置的跳转(上次编辑的地方跳过去)：快捷键位置：Alt+N || Navigate--->Last Edit location--->Ctrl+Shift+Backspace

5 Ctrl+Alt+左箭头/右箭头 浏览位置的跳转（上次浏览过的地方跳过去）：快捷键位置：按Alt+N || Navigate-->Back/Forward-->Ctrl+Alt+左箭头/右箭头

6 利用书签进行跳转：

6.1 Ctrl+F11 新建带有标记书签：快捷键位置：Ctrl+Shift+A--->搜索输入Toggle Bookmark with Mnemonic-->Ctrl+F11

6.2 如何根据标记书签查找：Ctrl+标记（例如：标记1，Ctrl+1;标记2：Ctrl+2)

7 Alt+Shift+F 收藏位置和文件：Alt+2--->调出Favorites--->Ctrl+Shift+A--->搜索框输入：Add to Favorites--> Alt+Shift+F

7.1 收藏函数是：将光标定位的该函数内，按Alt+Shift+F,则会收藏函数，可以新建函数-->Add to New Favorites List

7.2 收藏类是：将光标定位在类级别（函数外），按Alt+Shift+F,则会收藏函数，可以新建函数-->Add to New Favorites List

8 字符跳转插件emacsIdea跳转

8.1 emacsIdea插件安装方法：(Alt+h||help--->Find Action...)||Ctrl+Shift+A--->搜索框输入plugins-->按Enter-->搜索框输入：emacsIDEAs

8.2 Ctrl+J

9 编辑区跳转到文件区： Alt+1；文件区跳转到编辑区：Esc

10 利用vim进行多编辑区跳转

11 可以利用vim进行多窗口编辑

该教材是在 慕课网 平台 闪电侠 老师的 《IntelliJ IDEA神器使用教程》课程 整理的笔记，该教程链接：https://www.imooc.com/learn/924
————————————————
版权声明：本文为CSDN博主「Torey_Li」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Torey_Li/article/details/82715213
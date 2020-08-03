# VMware安装macOS 10.14完整教程（详细图文版）



###### **10.不要着急启动！！！，上述搞定后需要更改这个虚拟机的配置文件**

```bash
在如图19行的位置添加 smc.version = "0"
```

参考资料：

系统及前期准备
https://blog.csdn.net/qq_41101213/article/details/85179957
https://blog.csdn.net/galaxy_yyg/article/details/82891044
https://blog.csdn.net/u011415782/article/details/78505422
https://blog.csdn.net/iamlvshijie/article/details/71056109
VMware tools安装
https://blog.csdn.net/weixin_43822878/article/details/88018770
https://jingyan.baidu.com/article/6525d4b15dd175ac7c2e9443.html
————————————————
版权声明：本文为CSDN博主「CodeAllen的博客」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/super828/java/article/details/105473062



# VMware虚拟机安装黑苹果MacOS

https://blog.csdn.net/weixin_30652491/article/details/98507424





1、关于VMware 15虚拟机的安装，这里就不赘述了，大家自行下载安装即可。

**2、**默认的 VMware 是不支持识别苹果系统镜像的，需要先关闭虚拟机，解压缩 Unlocker_v3.0.zip ，找到里面 win-install.cmd ，然后右键点击，选择以“**管理员身份运行**”进行**解锁**，如下图所示：

注意：Unlocker 不能放在含有中文的目录路径里，不然会出现“Can`t load frozen modules”的错误。

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第2张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554283177911540.png)

然后会弹出如下窗口，然后等待该窗口自动运行完毕即可。

注意：为了防止安装失败，解锁前请先关闭杀毒软件。

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第3张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554283297109484.png)

**3、**解锁完打开 VMware15 虚拟机，**新建一个虚拟机**，建立过程也很简单，一般都默认就可以。

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第4张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554283717399769.png)

选择镜像的时候需要注意，默认只会显示 .iso 格式的文件，点击右下角选择**“所有文件”**，就可以显示我们刚刚下载的镜像了。

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第5张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554283688120959.png)

现在 VMware 虚拟机就可以识别并安装苹果系统镜像了，在建立虚拟机的时候就会显示 Apple Mac OS X 的选项了。

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第6张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554283810129918.png)

这里说明下，系统镜像不管是 iso 还是 cdr 格式，都是一样的。（我给大家提供的镜像为crd的）

**4、**新建虚拟机完毕后**不要立即启动**，先找到保存虚拟机文件的目录，找到后缀为 .vmx 的文件，比如小编的是 macOS 10.vmx 。

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第7张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554283965812646.png)

然后用记事本打开，然后在**最后\**添加\**一行** smc.version = 0 ，保存退出。





### smc.version = 0

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第8张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554284027197787.png)

**5、**接下来我们就可以**启动虚拟机**开始苹果MacOS系统了。





安装过程有点慢，大约12分钟，视电脑配置而定，大家耐心等待就是了，安装完成会有语音提示。

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第16张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554284102207008.png)

安装完成后，弹出欢迎使用和设置界面，接下来就是一些简单的设置了，一看就会，设置完即可进入苹果系统啦！

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第17张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554285747936771.png)

对了，一开始不能联网，在 VMware 的 MacOS 10 上面右击，选择“设置”，进行虚拟机设置。

### 网络连接选择 **桥接模式** 就能联网了，如果还是不行，可以这几个切换多试一下。

![VMware虚拟机安装黑苹果MacOS Mojave系统详细教程 经验总结 第18张](VMware%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85%E9%BB%91%E8%8B%B9%E6%9E%9CMacOS.assets/201904031554287353831193.png)

声明：本文由[w3h5](https://www.w3h5.com/)原创，转载请注明出处：[《VMware虚拟机安装黑苹果MacOS Mojave系统详细教程》](https://www.w3h5.com/post/236.html)

转载于:https://www.cnblogs.com/deshun/p/10652385.html


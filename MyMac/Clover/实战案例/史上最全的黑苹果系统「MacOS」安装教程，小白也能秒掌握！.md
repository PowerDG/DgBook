公众号关注 「奇妙的 Linux 世界」

设为「星标」，每天带你提升技术视野！

![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy84WkZ6clJqcWF0b3BJcDNUUGYxaDE3R0ozM1NlWVh4YTNSTWljOXlsb09rTnJJSFFLcEdtM0htZnJoQktoVkRYMmlibmlieUhIblRPVm1OUk5saFUyaWM3UHcvNjQw)



**关于黑苹果**

折腾过的人应该不陌生，自从苹果采用 Intel 的处理器，被解锁后可以安装在 Intel CPU 与部分 AMD CPU 的机器上。从而出现了一大批非苹果设备而使用苹果操作系统的机器，被称为黑苹果(Hackintosh)。

在 Mac 苹果机上面安装原版 Mac 系统的被称为白苹果（ Macintosh ）与黑苹果相对。(摘抄自百度)，很多人就是因为没有正确的安装黑苹果，导致安装失败，最终放弃。

我给大家分享一下我安装黑苹果的经历，我的方法也许不是最简单的，但是希望你能喜欢。(本文部分经验来源于百度)

**第一部分：安装前科普**

**1.要安装黑苹果，首先要确定你安装黑苹果的引导，现在安装黑苹果的引导有四种**

(1).Clover (四叶草，现在最主流的一种)



![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xVmlhczlGb01zdnFramZSbFE5QlduQUtUMHdnS1FMckNhelJYak1JZ3NCaGlhajVkaWJCRHVHOVRBLzY0MA)
Clover 引导界面


(2).OpenCore (现在新出的一种引导方式，但是配置难度极高，没有图形化的配置界面，对新手不友好，虽然有国外大佬做了图形化的配置界面，但是还是不太稳定)



![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xUlVVZm1YQkVrc3JpY0s5VmhVZWlhSFdvMTAzTmNQeEJ3NVNxTEFCMDU1RmlhSFlnN2JhN2NHdFlRLzY0MA)

Opencore

(3).Ozmosis (一种最先进的引导方式，通过修改主板的 Bios，模拟白果的启动方式，一种最接近白果的引导，但是，也是最难做的一种引导)



![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xM1k4WTdBTjVlWXlZVW0xeTJQcFFFU1Vnb3R2ODB4aWN5ZHlkZDhUUEZIOFdrenNYUG1yaWI3ckEvNjQw)

Ozmosis


(4).Chameleon (变色龙，旧时代的产物，很早以前的黑苹果大多采用这种引导，不过随着时代的发展，现在几乎没人用了)



![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xdHRFTVhFaHJWM2lhektnc3hsaDRpYU1aZHd1TzhWazdBQmliUTJCSVhzYnlJUXNlT05xeU52TWNBLzY0MA)

变色龙引导界面


本文只涉及 Clover 的安装方法，其他方法不涉及(实际上我只会一点点 OC，其他我也不会)。

### **2.下面介绍下列比较主流的引导的原理

**
(1).白果：首先经历电脑加电 → 启动 Mac 的 UEFI Bios → 加载 NVRAM → 进入 macOS

(2).Clover：首先经历电脑加电 → 加载四叶草的 EFI 微系统 → 模拟白果的 EFI 和加载参数 → 进入 macOS

(3).Ozmosis：首先经历电脑加电 → 启动 Ozmosis 模拟的 Mac 的 Uefi Bios → 加载 NVRAM → 进入 macOS

可以看到，Ozmosis 的引导是最接近白果的引导，这就是它为什么优秀，然而它的高难度让很多人劝退。



### **3.下面说到黑苹果的镜像，镜像有分为两大类，一类为原版(也就是最好的镜像)，一类为懒人版。**

(1).原版：基于官方镜像制作的镜像(后缀名.dmg)，集成了 Clover 或者 Opencore 的引导，可以直接通过 TransMac 软件写入 U  盘，这类镜像比较纯净，安装后体验更好，这里推荐使用黑果小兵大大的镜像，集成了很多驱动和配置文件，强烈推荐使用。



**博客链接:** https://blog.daliansky.net/ (我没有打广告)


(2).懒人版：解除了苹果系统安装 gpt 磁盘的要求，让其能安装在 mbr 的磁盘上，这样稳定性差。苹果系统原生是安装在 gpt 格式的硬盘上。(后缀名 .cdr)

(3).两者区别:原版需要安装在 guid（gpt）磁盘上且 efi 要大于 200 M，而懒人没有磁盘格式限制 mbr，gpt 都可以安装( AMD：CPU  只能是懒人版这个是没的跑了)(英特尔：对于支持 uefi 启动都可以选择 clover 引导安装原版，当然 clover 也是支持传统 bios 引导的)。


另外，原版需要U盘安装，而懒人版则可以硬盘安装。由于懒人是对解除了安装mbr的要求所以稳定来说没有原版好。建议能安装原版就装原版。

### **4.一些不应该 cai 的坑**


有一些实在装不上黑苹果的小伙伴，就去某宝搜索黑苹果安装，某宝上安装黑苹果的服务琳琅满目，但是，大家最好不要去 cai 这些坑，当然你执意要 cai 我也拦不住你。



这些店铺，大多采用恢复镜像的方式安装黑苹果，用店家自己做的镜像，恢复完成之后直接就进入了欢迎界面，恕我直言:这种镜像就是 emmm，甚至比懒人版还 emmm，然后店家再随意给你配置下引导，随便放几个基础的驱动草草了事，然后映入眼帘的满屏幕的小白的好评。

如果你实在不想动手，只想体验一下黑苹果系统的话，那根据你自己的想法酌情考虑，如果你要满足日常使用的话，那我还是不推荐。这种安装黑苹果的方式完美度一般都很低，再加上 emmm 的店家态度和服务，emmmmmmmm (我可是亲身体会过)



### **5.关于个人对于黑苹果完美程度的定义

**
(1).驱动三卡(声卡、网卡、显卡) 百分之三十(淘宝店家大概就这样)


(2).在三卡驱动的基础上，显示正常，无卡顿，CPU 变频正常 百分之五十


(3).在百分之五十的基础上，睡眠正常，外加能无线上网，隔空投送正常使用，蓝牙正常使用(一般这么做需要买一块黑苹果无线网卡)，硬件解码正常 百分之八十


(4).在百分之八十的基础上，facetime 和短信能够正常使用的，能够正常的进行小版本的更新 百分之九十九(别问我为什么会有那百分之一，我不知道)



### **6.台式机和笔记本安装黑苹果的差别

**

(1).台式机：一般安装黑苹果更加的简单，能更好的接近完美状态，扩展性强



(2).笔记本：相对于台式机，会比较难，多了电池驱动和屏幕亮度调节，外放，前置摄像头，触摸板，键盘等。

一般带有独显的笔记本在macOS下都是不能驱动的，除非出厂屏蔽了 CPU 的核显或者能够在 Bios 设置中屏蔽核显，不然在 macOS 下只能驱动核显

(**关于核显的驱动:** https://blog.daliansky.net/CoffeeLake-UHD-630-black-screen-direct-bright-screen-and-correct-adjustment-of-brightness-adjustment.html )。

还有，大部分笔记本的 wifi 在 macOS 下也是不能驱动的，除非是博通的网卡，不然就要更换黑苹果的网卡或者外接 USB 免驱网卡使用。

### **7.关于 CPU 对黑苹果安装的影响**

(1).英特尔:因为 mac 使用的就是英特尔的 CPU，所以不用进行额外的操作。

(2).AMD:因为架构的不同，macOS 原生的内核无法支持，所以要下载别人改好的镜像，比较麻烦，然后在安装时也要替换内核文件。太过复杂，我就不再赘述了。



### **8.显卡对黑苹果的影响

**
(1).AMD:这边的显卡一般都是免驱的，因为 mac 用的就是 a 卡(苹果和 AMD 关系可好了呢)，所以插上就能用。

(2).老黄卡:要知道老黄和苹果的关系决裂了，苹果不再让老黄适配新系统的驱动，最多支持到 10.13.6，不过还是有一些免驱的N卡还能在 macOS 下免驱，开普勒 GK104/107/110在macOS  下还有原生驱动支持(当然新系统也是免驱的)，其他的要通过 webDriver 驱动安装，与新系统无 缘(不过我还是在网上看到有人在 10.14  系统驱动了非免驱的 N 卡)。



### **9.驱动分类

**
相信很多人对驱动的分类都很头痛，我大概介绍下几种常见的。

**1. ACPIBatteryManager.kext 电源管理驱动**

- RehabMan-Battery 驱动下载

  

驱动一般为 zip 文件 RehabMan-Battery-2018-0915.zip，解压后在 Release目录下会有 ACPIBatteryManager.kext。拷贝到目录 EFI/CLOVER/kexts/Other/ 目录下，重启黑苹果。

**
\2. 键盘鼠标触摸板驱动**

- VoodooPS2Controller.kext


**3. 屏幕亮度调节

**

- IntelBacklight.kext

**
\4. USB 驱动

**

- USBInjectAll.kext

**
\5. 有线网卡 驱动
**

- RealtekRTL8111.kext

**
\6. FakeSMC 驱动**

- FakeSMC.kext 用来欺骗 mac 系统要安装的 PC 是 mac 硬件

**
\7. FakePCIID 驱动**

- FakePCIID.kext 用来冒 仿PCI device

**
\8. CodecCommander 驱动**

- CodecCommander.kext

  

- 耳机有杂音、睡眠唤醒无法自动切换或无声

**
\9. WIFI 蓝牙驱动**

- BrcmPatchRAM2.kext
- BrcmFirmwareData.kext

**
\10. Lilu**

- Lilu.kext 内扩展驱动

**11. CoreDisplayFixup 

**

- 4K分辨率驱动

**
\12. IntelGraphicsFixup 驱动**

- 修复黑苹果系统后出现的花屏，卡顿，闪屏

像这种驱动一般都要放在引导文件夹的驱动文件夹中。**摘自:** 


https://blog.csdn.net/u010953692/article/details/82874109 

**13. Apple ALC.kext**

- 用于仿冒苹果官方声卡的驱动，需要搭配Lilu.kext驱动使用

**
\14. voodooHDA.kext**

- 万能声卡驱动，但是会有杂音，不推荐用

**15. webDriver**

- N卡驱动

**16. WhateverGreen.kext**

- 显卡驱动补丁，解决各种如黑屏、花屏、睡 眠黑屏等小问题

**17. IntelMausiEthernet.kext**

- 英特尔有线网卡驱动

**18. USBInjectAll.kext**

- USB 注入驱动

  

**第二部分：安装**

安装前准备：一台电脑，一个 8g 及以上的 U 盘，一个 30g 以上的分区，一双手，一个大脑。

到这里可能有很多人就会问了:安装黑苹果究竟有什么用呢？比如，有的人需要使用 macOS 下的独占软件，如 xcode 和 fcpx 等，而又买不起 mac 的人，就是黑苹果的忠实用户，以低价获得了高配 mac 的性能。

还有一些人需要用到 mac 环境做开发，也会选择安装黑苹果。有一小部分人有着折 腾精神，不装黑苹果就不开心，就是要装黑苹果。

这是我的配置，已经开启核显硬解 ，另外我也插了个黑苹果的网卡，隔空投送，蓝牙，wifi 一起解决。



**工具链接:** 

https://pan.baidu.com/s/105Fp3rOdZlKJPJyhogl47w 提取码: 98u7



**EFI链接:**

https://pan.baidu.com/s/1ayKZTnqrYgC9c1AYjxWOog 提取码: vh1h

这里也有一些机型的 EFI 文件，包括台式机和笔记本，来源于黑果小兵。

**链接:** 

https://blog.daliansky.net/Hackintosh-long-term-maintenance-model-checklist.html



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xUERrZkhJd2lhS1prUXFsMlRQYjZ1NXJ2NXpVU09LaWNzR3JCOUVpYVd1T3pHVDl3UDFhQU9VVmNRLzY0MA?x-oss-process=image/format,png)



### 步骤



**下载链接:** 


https://pan.baidu.com/s/1aTpLhOHMnJjXiIQVAYOfuQ 提取码:60d8



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xY25aSko3UEpMc2lheXhFOFNpY2ljbGtuUGJFaWNHZkRaM2lhMUlEbzI3ZUxWdTlUMm9SUFY1OWJBRFEvNjQw?x-oss-process=image/format,png)

插入U盘打开软件


\2. 在软件里找到自己的 U 盘 



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xc2lhY0FFM1RMT2hnbDYyUnoxdHBiWWgydmNIR2ljSDdNMTBwQzZualJMUUZHc0dZblhWQ1dVU2cvNjQw?x-oss-process=image/format,png)


\3. 右键 U 盘，把 U 盘格式化为 mac 的格式，之前请备份好数据，然后再选择下载好镜像的路径写入U盘。(需要原版镜像，推荐黑果小兵的，后缀为 .dmg 的文件)





![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xazN5ZWp5NnZtZzZKUXh4M0NIZzYyMHhaamN4WGJiSDBYRzBNMVpwN1h3ZVVsV1U1eUpRQUF3LzY0MA?x-oss-process=image/format,png)





![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xaHd5SmliVmljcmhXTGxTNnRIQ0duTnU1aWE1NGJpYTBkS3RCMU4wQ2xWVmhTUlVuU05ISVVSSUlnQS82NDA?x-oss-process=image/format,png)




写入速度取决于你的 U 盘速度和 USB 端口的协议，我用的是 3.0 的盘，所以会快些。

\4. 写入完成后，重启电脑，进入 Bios 设置，打开 Ahci 选项，关于不同主板如何打开请自行百度。

\5. 修改完成后，保存设置，重启电脑，选择 U 盘引导(如果你连这个都不会那就。。。。。。emmmm),进入 Clover 引导界面，然后通过方向键选择 Boot macOS from Install macOS ,回车进入。



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xNlV3R2dDTzZpYXQxRmtlbEdTU09KMEJxOFkwYVNIR1ZSbWRuY1Rsam5FdXNmMWhKQWUxNHpFUS82NDA?x-oss-process=image/format,png)

\6. 回车后，屏幕上会跑代码，如果你的配置文件及驱动没有问题的话，然后过一段时间后就会进入安装界面。

如果进入过程中卡住报错，就上百度搜索一下最后一行卡住的代码，会有解决方案的。不过用黑果小兵的镜像报错几率应该不大。

关于各种常见的报错及解决方案，**请参考：**http://bbs.pcbeta.com/viewthread-1280262-1-1.html?source=1

另外我也自己整理了一份台式机和笔记本通用的 EFI 文件，大家根据自己的配置删除多余的驱动。

**万能 EFI 链接基于 Clover 5093 制作：**https://pan.baidu.com/s/19vOQVOvnL4T9hINp3DHjyw  提取码:oca0

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xNFl5RDVJR3dzMnVibTJ6MzZpYk5RZ0lENXZWSFdzTXJkN2ljd3BHMlRJcFd4QzBSU3V2V2UzYlEvNjQw?x-oss-process=image/format,png)

进入过程，等待时间取决于U盘的速度


\7. 如果不出意外，应该进入了 macOS 实用工具，选择磁盘工具，然后提前先准备一个 30 G以上的分区。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xWlB0OWY3NHNmRE80TFdwY0lpYXR0NWdMd0xHaG5IaWFzaFVRYTFxMVRTcTBlMG9lVVhnRWhQdGcvNjQw?x-oss-process=image/format,png)



\8. 选择要安装的盘，然后抹掉，格式为 APFS，名字不要有中文。



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xeUNkZFlkNmU5cENBUUl4ckg2NHhlMVU3aFhPU01pYjJZdXVKT09pYTBhTDNOckVRd1lORjNoVVEvNjQw?x-oss-process=image/format,png)



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xOEl2d1JFZFJzaWNhZXFvbGdmRVVQWGhjbTFjR3hWaWNoRmd0aWNWanVuN2dpYzRST2cxYjVXcXlLZy82NDA?x-oss-process=image/format,png)



\9. 如果抹掉失败的话，就是你的 EFI 分区小于 200mb，请自行扩容，参考百度，如果你是单独一块硬盘，就不会失败，系统会自动帮你建立一个 EFI 分区。然后抹掉完成后退出，然后再选择安装 macOS。





![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xZ2lhcHNBODQySXoxcHdEbmlhUVlRYWtDQUM2ZGdwV1dxMnYyYnFRWEdXSWVldmE0TjFzMThpY3RRLzY0MA?x-oss-process=image/format,png)





### \10. 一路继续，

然后选择要安装的硬盘(注:直到进入系统前，先把网线拔掉，不然会安装失败，进入系统后再插上网线)，然后确定开始安装。

速度仍然取决于你的 U 盘速度。这一阶段安装完成后重启，然后 Clover 界面选择你的硬盘，这个时候应该能识别出来，后面会有硬盘名。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xcUNqMDBKYkxnYWNiOFpqZWNZMFBKakNQQzV0SDNCaHV3T0RGNEIwVjhpY2ljelpIaEtXTmNRTWcvNjQw?x-oss-process=image/format,png)



![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xR2lhOGljUmliMEpGbllycjBIdU1Kd1NCamlhdG9wV0hiOW42OUttdGd3a1hPVHNmTXNoRWJEZ1hSQS82NDA)



![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xNHByd2cxdmRWSE1tQzJ2MGlhMVVMbHBPSVdiczdoTjhLM1VocmFzQkxGZUpDdm9MaWF4dlpOekEvNjQw)



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xR1JXdDlwZzdFMjA1YjVraWNmSGliWVM3ekFMMmlhRnU2ejYzZG11cTRFa3B6QTBCdU1hZzU0Skx3LzY0MA?x-oss-process=image/format,png)

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xR1JXdDlwZzdFMjA1YjVraWNmSGliWVM3ekFMMmlhRnU2ejYzZG11cTRFa3B6QTBCdU1hZzU0Skx3LzY0MA?x-oss-process=image/format,png)



### \11. 重启几次后，进入系统。



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xTnQzcXEzQXpRQnJLZGoyRWlja3FxNkFDM2dUb2liaWJpYldoQWlhV0dkbGxQY0VyZG5mV25tbzROaWJRLzY0MA?x-oss-process=image/format,png)


\12. 进入系统后，修改的你的 EFI 文件，通过 clover configurator 挂载你的 U 盘的 efi 分区，然后打开你的  config 文件，去掉 -v 启动参数，启动时就不会跑代码，直接就是 macOS 的开机动画，然后勾选上一些你需要的选项。

**参考链接: 
**
www.cnblogs.com/SemiconductorKING/p/6534821.html

然后把你的 EFI 文件放入你的硬盘的 EFI 分区中(这是单独一块硬盘安装 macOS 的操作)。若你是单硬盘双系统，将 EFI 文件夹中的  Clover 文件夹放入硬盘 EFI 分区的 EFI 文件夹中，然后进入 windows 中，下载 EasyUEFI 软件，添加 Clover  启动项(详细方法百度)。



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xa0ZjVkNzMm5sbVpkaGZLYkpPWlU3NTRUaWNJVHBJeU0yVmRrVXRJYWN1M0JUa0tOTUJVYU92Zy82NDA?x-oss-process=image/format,png)



![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL3N6X21tYml6X2pwZy9TUDM1UDlpYkFnNUdsVlE3SzV2YVNHaWFoQVRST3c3N25xaWF6eGFPWTZKU1VnOWE2VGY4Zk9SN1g3SkMwQkZtZkEzTWZzNjFFYTE5aEhjVGxaeWFzMjNDQS82NDA?x-oss-process=image/format,png)



13.拔掉 U 盘，使用你的硬盘引导，不出意外，你的硬盘应该能引导进入黑苹果系统了。

14.最后，测试你的黑苹果功能，声音，显示，上网，CPU 变频，硬件解码，隔空投送，wifi，蓝牙，facetime，睡眠等功能。

15.测试完成无误后，安装完成，享受你的黑苹果之旅。

## **扩展阅读**@

- **本文 bilibili 测试视频链接：**https://b23.tv/BV1M741167PW
- **保姆级教程：**https://www.bilibili.com/video/BV1W44112792
- **疑难杂症解决：**https://www.xiaoyi.vc/black-macos-tips

> 本文转载自：「酷安」，原文：https://tinyurl.com/vceth3k，版权归原作者所有。欢迎投稿，投稿邮箱: `editor@hi-linux.com` 。

![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2dpZi9WM2xsN0ZNeUd5TXFRQzdKUk5nVlpsc2lhSmliU3lwMjdVU2xSaWExOTRLNk5xZnZ6OFdibGc3SERjZU9uNFkzTWVrcHBTMTRsYXpSWlRLTGR0MkJIdVlHQS82NDA)

![img](%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%E7%9A%84%E9%BB%91%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F%E3%80%8CMacOS%E3%80%8D%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%EF%BC%8C%E5%B0%8F%E7%99%BD%E4%B9%9F%E8%83%BD%E7%A7%92%E6%8E%8C%E6%8F%A1%EF%BC%81.assets/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9WM2xsN0ZNeUd5TVNCUWxocHdLMGozak9TNWljS21GbmNHSmRZOGtMNWtZNkFzd0RwT0JzejA2R0Zvd29EaWNMMmRDMmZyOWhhaWJucndOZFo3ckpMWHFVQS82NDA)

**你可能还喜欢**

点击下方图片即可阅读



[![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy84WkZ6clJqcWF0cEVhdm9NWFdtNVRMV2pVV0VVa0VKOENpYjZDNjg0VXdnTGpCaGNIM1JpY2xNSHVBVjR1MmdVVzAwMXo2dkhhaWNpYXhXYzFpYnNqNllDNUFBLzY0MA?x-oss-process=image/format,png)](http://mp.weixin.qq.com/s?__biz=MzI3MTI2NzkxMA%3D%3D&chksm=eac53a43ddb2b35542b485a5f651af4650f78d5cb667820de096b3f4046305a9a94179cd3302&idx=1&mid=2247490410&scene=21&sn=ff83793d21f6c7f80816b2876783c4f0#wechat_redirect)

可能是目前为止全网最好的介绍分布式系统原理的中文文档！
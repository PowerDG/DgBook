# 黑苹果macOS Catalina 10.15.4 安装过程详细记录

https://blog.csdn.net/qq_28735663/article/details/105250240



最新黑苹果10.15.4安装教程，附带各电脑EFI驱动合集、原版引导镜像，图文并茂简单易懂…

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMTc5NzU5My1lODllMWQxNjg3YWQ0ZDI5LmpwZw)

### 一、准备工作

1.一个8G以上的U盘（安装 10.15 Catalina 必须要16G及以上的U盘 ）；

2.Mac OS镜像、TransMac（刻录工具）、DiskGenius（分区工具）、EasyUEFI(引导工区)、EFI驱动文件。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS03OTczYWIyN2FlYWM5NTc1LnBuZw)

**安装工具**获取链接：https://pan.baidu.com/s/1pwUVVo1Ud4yxO29k_ckTBw提取码：qs05

**安装镜像**获取链接：https://pan.baidu.com/s/1nCNgYTp9fuRPCCwQjM0KFw提取码：ctro

### 二、制作启动U盘

\1. 打开balenaEtcher软件，点击“Select image”选择你刚才下载好的安装镜像；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMTc5NzU5My0zMTdiOGI5MTE3N2VjNDdlLnBuZw)

打开balenaEtcher软件

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMTc5NzU5My0zZDExNTJkY2ZiYmNiNTZiLnBuZw)

选择下载好的安装镜像

2.然后点击“Select drive”选择你的U盘，如果你直插有一个U盘，软件会自动选择；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMTc5NzU5My05MzczOTBkYzYzNjQ0YmRmLnBuZw)

\3. 接着点击“Flsh！”开始刻录黑苹果安装镜像到U盘；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMTc5NzU5My1jYzczYjdmYTFhNzYxNjRjLnBuZw)

刻录中。。。

4.刻录完成后会进行一次完整性检测；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMTc5NzU5My1hNjAxYmY0OTg0MThhZjRmLnBuZw)

检测中。。。

5.等到提示“Flash Complete！”Successful device 就完成安装镜像的制作了，把U盘弹出并拔出。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMTc5NzU5My0yYjU4YTM1ODgxZGNmZWUzLnBuZw)

6.至此，黑苹果镜像就刻录完成！

### 三、配置四叶草引导驱动

1.打开分区工具DiskGenius，找到U盘上的ESP（有的叫EFI）分区，删除自带的EFI文件。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0wZjNmMjZmYTg5MGE4ZTIwLnBuZw)

2.把适合自己电脑EFI文件夹复制进去。这里只能用快捷键粘贴，Ctri+C复制，Ctrl+V粘贴。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0zOWQ2NmQ3MmU1MzkzZTFlLnBuZw)

### EFI下载

这是我远程安装收集的一些EFI资源（六年老店，辛苦远程，原创合集），集合多种电脑配好的EFI驱动文件，除三代处理器以下或更低配置的电脑，均已更新**支持10.15.4**，基本主流配置的电脑都有收集，可直接替换使用。EFI合集每周更新并新增文件，逢15.0/15.2/15.4/15.6更新驱动。注：EFI只有实机安装才能收集，凭空无法配置。网上有流出的旧版资源（淘宝、闲鱼、GitHub），文件不全且年久失修，不保证适合新系统，旧电脑装旧系统勉强可用，新电脑新系统，请**务必**获取最新合集。

**[点击这里跳转至官方教程下载](https://links.jianshu.com/go?to=https%3A%2F%2Fmacx.top%2F2425.html)**

### 四、制作Mac系统盘

制作Mac系统盘分为分区安装与整个磁盘安装两种情况，其实大同小异，但问的人很多，这里就都讲一下。分区安装的意思就是我一块磁盘几个分区中的一个用来装Mac系统，跟Windows系统的C盘D盘一样，数据互不影响。整个磁盘安装就是我一整块磁盘都用来装Mac系统，我个人建议，有条件的还是选整个磁盘安装。

1.分区安装

a.计算机右键>管理>磁盘管理

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1jYjkyN2Q4NGMwNGFlMTBlLnBuZw)

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS04OGRkMTQyOGUzMTFmN2M5LnBuZw)

b.首先检查磁盘是否符合要求，磁盘格式必须为GPT格式(GUID)，即右键这个磁盘，“转换为MRB磁盘”为灰色就是GPT格式。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0xYzY3NGQ4Yjc0MGMzNjMxLnBuZw)

c.另外磁盘不能有小于200M的分区且必须有EFI分区（EFI分区也必须大于200M），满足这两个基本条件后，右键压缩卷（用分区工具查看）。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1lODVhMzBhNzEyMDlkYjY1LnBuZw)

d.输入压缩大小，根据自己情况及要求输入，最小25G。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1kNWVjMTM5Y2E1YWRmMzYzLnBuZw)

e.接下来选中被压缩的分区，右键新建简单卷。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1hYmE2MmMzODlmOTA5YzU4LnBuZw)

f.然后就一路默认下一步，但注意有一项要选择“**不要格式化这个卷**”；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS03ZmRiMGRjZmQxNTQxMWQwLnBuZw)

g.一路默认下一步即可；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS00ZWJkOWZiNjdlOWM1MGM0LnBuZw)

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS05NDRiODlmOWUwNzMzZjY2LnBuZw)

**注意**：这一步必须要选“不要格式化这个卷”；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS00NzBjMjVjYWI5NWViNTc0LnBuZw)

h.继续默认下一步直至完成；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS03NGU0NDJiZTkzMTU4NWNiLnBuZw)

I.最终结果是这样的：





：

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1jYjc2ZmI5ZjU4YTllNDRiLnBuZw)

2.整个硬盘安装

a.整个磁盘只装mac系统，那就删除磁盘所有分区，变成一个全新的磁盘即可。如下图：

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1iMDJlMjdmOTQwNGQ5M2EwLnBuZw)

b.在系统磁盘管理中会显示未分配：

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1lMmJmODI1YWJlZTY0ODdhLnBuZw)

至此安装盘搞定，黑苹果完成了85%。

### 五、安装Mac系统

1.设置BIOS，重启按F2用U盘进BIOS（电脑不一样，按键也不一样，就看自己电脑型号）;

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS03NjU5OTBhZTdiYTVmODQ2LnBuZw)

2.首先设置硬盘模式为AHCI，否则会出现磁盘工具读不到内置硬盘的情况（即找不到自己做的苹果安装盘）；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0xYmNmYzgyODhjZmI2YWMwLnBuZw)

3.然后关闭安全启动Secure Boot或者选择其他操作系统（这是微软为了防止安装Windows操作系统的电脑改装linux而设置的，不关闭无法启动到四叶草）；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS01Yjk2OGFkZDk3NDQ1ZDA0LnBuZw)

4.打开UEFI引导模式；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0yYjk0MDM0ZjFjMTQ4MmUzLnBuZw)

5.第一启动项选择自己刻录好的U盘，EUFI+U盘名称；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1iZDlmMTBiOTgxNzIxNzM3LnBuZw)

6.保存重启，进入四叶草引导界面；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS03MDhjN2E3NDQ3ZGMzZmI2LnBuZw)

7.进入clover到四叶草界面后，选择Boot MacOS install from install macOS Catalina，并回车进入；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS00NjU5YWQ5MDFmMDczNmJlLnBuZw)

8.过一会儿会进入如下界面;

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS04N2RmMzNmNTc3OGIwZWRmLnBuZw)

9.跑完代码或进度条之后进入安装界面，先选择磁盘工具；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1kYmUwN2E4NDRiNDFmZjViLnBuZw)

10.选中自己做的Mac系统盘，一般名字颜色比较浅，灰色的那个（看清楚千万不要选错）；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1iYmVlMzM1NmFmNDQyZjE4LnBuZw)

11.然后抹掉磁盘，名字随便输，格式按固态硬盘选APFS格式，机械硬盘选扩展日志式；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1kZGRhNzBmYTRjYTRiZjFiLnBuZw)

摸盘时格式推荐（可自行选择）

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS02OGQ5N2I3YjkzNTc1OGM3LnBuZw)

**提示：**如果摸盘失败，提示空间不足，就是你的磁盘格式没处理好，重新进win修改格式；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0zZTc0NzY4ZTViOTI1NjIyLnBuZw)

12.抹掉磁盘之后，关闭“磁盘工具”，选择“安装MAC OS ”;

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1iNjE4MmE2OTA0Yzg2NGNhLnBuZw)

14.然后找到自己刚刚摸掉的那个盘，选中后安装！

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0zNzlhMzdjMGM4Zjc3ZTE1LnBuZw)

一路同意默认下一步即可；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1lOGQ4NjgxOTQ4ODVlMGU0LnBuZw)

第一次安装界面：

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS05ODJmYzlhMzQwMWQ3OTVkLnBuZw)

14.第一次安装完后会重启，重启还是选U盘启动，进入四叶草选择Boot MacOS from X，（X你刚刚抹掉的那个安装盘的名字）过一会儿会进入正式安装界。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0wZjgwNDdjMzkxNTYyY2FjLnBuZw)

二次安装界面：

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0wM2QyZGUwNDNmMGNlZTU1LmpwZw)

16.二次安装完，黑苹果就安装到你的硬盘了，重启后还是选U盘启动，进入四叶草选择 X（你抹掉磁盘的名字），进入苹果系统；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1hMjI3MmU2NDRkMjMxOTQ1LnBuZw)

17.设置好账户密码等，就可以进入桌面了；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS03MzNiN2M5MDQ4Nzg4NzY1LnBuZw)

18.Mac系统桌面以及启动台展示；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1hZTUxZDRjZDBiOGRkOTMwLnBuZw)

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1jYTQ0ZjU4MWM0MjQ3ZTcyLnBuZw)

### 六、更改硬盘启动

1.打开分区工具，把刚刚的EFI文件夹里的clover文件夹拷贝到硬盘的ESP分区（有的显示“EFI”，操作跟之前一样，用快捷键拷贝）；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1lYzA1OGJhZjIxYWRiZTBhLnBuZw)

2.安装EasyUEFI引导工具(win要GPT引导模式，否则会报错或者无引导项)；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0yZGNmMjJmYmZmYTgyOTQ0LnBuZw)

3.装完之后，打开主界面，管理EFI启动项；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0wMjcyYTNhMDdlMjQwOGYyLmpwZWc)

4.进入管理界面后，点击 “**+**”，新建引导项；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS0xZmI4Y2M5YzBiYWQ1OGI2LmpwZw)

5.类型选择Linux或其他操作系统；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS04Nzc3MWI5MmM3ZWVmOTFmLnBuZw)

6.描述，即新建引导项的名字，自己随便输，我这里输入的“CLOVER”（注意英文输入法输入）；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1jYjk2ZGY2NGJmODhjMWZlLnBuZw)

7.选择目标分区，选中自己硬盘的ESP分区,即刚才拷贝EFI文件的分区，然后点浏览，进行下一步；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS04YmUxMjkyY2Q4ZWRkMmVkLnBuZw)

8.找到EFI/CLOVER/文件夹下的 CLOVERX64.efi 启动文件；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1hYzAyMjg2MTQ5NmQ4NDU2LnBuZw)

9.选中后确定，完成新建引导项；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS04N2ZhMTczOTIxN2QyNzIzLnBuZw)

10.然后调节新建的引导项为第一启动项；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS04MzRiMmU0YzhiYTQ1ZjI1LnBuZw)

11.最终，硬盘引导修改完成，以后可以拔掉U盘启动mac系统了；

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1lMzhiZjZlNjE3NzdmZjY4LnBuZw)

至此黑苹果安装教程写完，祝各位尽早吃上黑苹果！

**悦享老店原创 EFI 合集**

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS1jNzQzYzJkODRmNjg3MGMxLnBuZw)

这里有我远程安装收集的一些EFI资源（五年老店，辛苦远程，原创合集），集合多种电脑配好的EFI驱动文件，除三代处理器以下或更低配置的电脑，均已更新支持10.15.4，基本主流配置的电脑都有收集，可直接替换使用。EFI合集每周更新并新增文件，逢15.0/15.2/15.4/15.6更新驱动。注：EFI只有实机安装才能收集，凭空无法配置。网上有流出的旧版资源（淘宝、闲鱼、GitHub），文件不全且年久失修，不保证适合新系统，旧电脑装旧系统勉强可用，新电脑新系统，请**务必**获取最新合集，**[官方本地下载](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.macx.top%2F105.html)**或关注微信公众号**悦享软件**回复**黑苹果**获取合集且可以**永久更新** 。

![img](Catalina%2010.15.4%20%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B%E8%AF%A6%E7%BB%86%E8%AE%B0%E5%BD%95.assets/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yMDU1NDczMS05ZmNlOWFlZGZmMDYzMzg0LmpwZw)



- ​                        
# 黑苹果小兵clover目录及驱动介绍

https://blog.csdn.net/weixin_44520133/article/details/104233350

# CLOVER目录结构

ACPI\ORIGIN： 保存提取到的原始DSDT文件。
ACPI\PATCHED： 存放修改好的用户DSDT.aml和SSDT.aml 。
CLOVERX64.efi： 64位CLOVER的主启动文件 。
config.plist： CLOVER配置文件。
DOC: CLOVER的帮助文档 。
DRIVERS64UEFI： 使用UEFI模式加载64位CLOVER所需要的驱动文件。
KEXTS： 使用驱动注入时，CLOVER加载的驱动文件的存放位置。
MISC： 存放CLOVER环境下的截图文件。
OEM: 分文件夹存放ACPI、config.plist等文件。用以加载，实现单个U盘引导多个黑苹果系统。
ROM: 保存提取到的显卡ROM文件。
THEMES： CLOVER主题存放位置。
TOOLS： EFI Shell存放位置，放置用于进入shell环境的.efi文件，不可用于引导OSX，但可运行一些.efi程序。

我们与黑果大神的EFI文件做对比后则发现一些不同之处：
1、没有 DOC、MISC、OEM、ROM这4个文件夹。
由此可知，这几个文件夹并不是CLOVER必备的文件目录。

2、多出Common-patches-for-hackintosh和drivers-Off这2个目录。
由名称可以看出，分别是黑苹果常用补丁和不生效的驱动文件。

3、有多个config.plist配置文件，根据其命名可得知是与核显名称相对应。

在这里插入图片描述

我们打开ACPI文件夹，发现其下只有PATCHED文件夹，继续打开，有2个文件：
1、SSDT-Disable-DGPU，作用是屏蔽独立显卡。
本人电脑只有核显，并没有独立显卡，所以可以删除，对于双显卡的朋友，则建议保留。
2、SSDT-UIAC-ALL，解决USB端口不识别或将USB3.0识别为USB2.0的问题，所以保留。

Common-patches-for-hackintosh文件夹内config_patches补丁，虽然也无效，但对将来配置其他config.plist文件有用，所以也予以保留。
若是您想删除那就删除吧，没什么影响。

DRIVERS64UEFI和TOOLS目录请保持不变，我们主要对THEMES、KEXTS目录以及
config.plist文件做修改。

1、THEMES：
这里是存放CLOVER主题的目录，如果你不想修改主题的话，也可不做处理。

打开目录，有3个文件夹，与之对应的是3套主题：
Clovy、Hackintosh_ID、SimpleThemeDark。
我都不喜欢，所以全部删除，并将我喜欢的主题MAC文件夹拷贝至此处。

2、KEXTS：
这里是存放驱动文件以驱动你的硬件，一般将驱动文件存放于KEXTS目录下10.14目录或OTHER目录下。

我们打开后发现只有OTHER目录，继续双击打开。可以看到如下图所示：

在这里插入图片描述

先将其中的Backup文件夹移至他出存放，过会还会使用。
再根据自己电脑的硬件选择有用的驱动文件，并将无用驱动删除。

现将下图中驱动文件略作解释：

在这里插入图片描述

文件说明.MD：
非驱动文件，阅后删除。

文件说明.PDF：
同上，阅后删除。

AHCI_3rdParty_eSATA.kext：
ASMedia ASM1061, JMicron 36x (又称GSATA)和Marvell 88SE912 SATA控制器驱动。与我电脑硬件设备不符，所以删除。

AHCI_3rdParty_SATA.kext：
同上，所以删除。

AHCI_Intel_Generic_SATA.kext：
英特SATA驱动，具体对应芯片组不明，删除。

AppleIGB.kext：
Intel 82575, 82576, 82580, dh89xxcc, i350, i210和 i211网卡驱动。
与我设备不符，删除。

AppleIntelE1000e.kext：
Intel系列 82540, 82541, 82542, 82543, 82544, 82545, 82546, 82547, 82578 (P55/H55)， 82579 (P67/H67)， 82574L ，82571 ，82572 ，82573 ，82574， 82583， I217V网卡驱动，与我设备不符，删除。

AtherosE2200Ethernet.kext：
高通AR816x, AR817x 和 Killer E220x的网卡驱动。与我设备不符，删除。

FakeSMC_v1800.kext：
模拟苹果SMC芯片及加密通讯，欺骗MAC系统以为是白苹果设备，为黑苹果必备驱动，绝不可删除。

GenericUSBXHCI.kext：
USB3.0驱动，保留。

IntelMausiEthernet.kext：
英特系列82578LM、82578LC、82578DM、82578DC、 82579LM、82579V、I217LM、I217V、I218LM、 I218V、I218LM2、I218V2、I218LM3、I219V、 I219LM、I219V2、I219LM2、I219LM2网卡驱动，与我设备不符，删除。

Lilu_v1.3.2.kext：
黑苹果内核扩展补丁驱动，不可删除。

RealtekRTL8100.kext：
RTL8101E、RTL8102E、RTL8103E、RTL8401E、RTL8105E、RTL8402、RTL8106E、RTL8106EUS、RTL8107E、RTL8139网卡驱动。
本机网卡RTL8105E，所以保留。

RealtekRTL8111.kext：
RTL8168X/8111X（X=无/B/C/D/E/F/G）网卡驱动，与我设备不符，删除。

SATA-100-series-unsupported.kext：
英特100系列主板SATA磁盘识别驱动，本人电脑H110主板，所以保留。

SATA-200-series-unsupported.kext：
英特200系列主板SATA磁盘识别驱动，与我设备不符，删除。

SATA-RAID-unsupported.kext：
SATA磁盘阵列驱动，与我设备不符，删除。

USBInjectAll_v0.7.1.kext：
USB端口驱动，必备驱动，保留。

VoodooPS2Controller_v1.9.2.kext：
PS2接口驱动，使用USB接口键鼠可以删除。
但本人电脑触控板模拟PS2鼠标，所以保留，不能删除。

WhateverGreen_v1.2.7.kext：
显卡驱动补丁集，保留，不能删除。

XHCI-200-series-injector.kext：
英特200系列主板驱动，与我设备不符，删除。

XHCI-300-series-injector.kext：
英特300系列主板驱动，与我设备不符，删除。

XHCI-unsupported.kext：
英特X99系列主板驱动，与我设备不符，删除。

至此，OTHER目录所剩驱动文件如下：

在这里插入图片描述

我们还需一些驱动文件来驱动我的电脑硬件设备。
黑果小兵大神还为我们准备了一些其他硬件驱动，存放在我们刚才移至他处的BACK文件夹下，我们再将所需驱动拷贝至OTHER目录下。

所需拷贝文件列表如下：
AppleALC_v1.3.5.kext：仿冒声卡驱动。
AppleBacklightInjector.kext：笔记本显示屏亮度调节驱动。
BrcmFirmwareData.kext：蓝牙驱动。
BrcmPatchRAM2.kext：蓝牙驱动。
FakePCIID.kext：黑苹果必备的一个驱动文件，由于macOS系统会对PCI device-id进行验证，导致黑苹果的硬件不能通过这一验证，所以需要就需要这个PCIID文件屏蔽验证。
FakePCIID_Broadcom_WiFi.kext：博通WIFI驱动，注：本机无线网卡BCM94352z。
HibernationFixup.kext：睡眠唤醒补丁。
CodecCommander.kext：解决睡眠唤醒后声卡无音补丁。
CPUFriend.kext：动态注入CPU电源管理数据，以实现变频。

ACPIBatteryManager.kext：笔记本电池驱动。
特别说明：如无此驱动，偏好设置下节能选项内无电池项，此驱动需DSDT配合，否则读取电池电量为零，但不知为何，添加此新版驱动后，我的AIRBOOK笔记本电脑在输入登录密码后会自动重启，所以我没有添加该驱动。

在BACK文件下还有一个名为FakeSMC_v1800_with_Sensors.kext的驱动文件，作用于FakeSMC_v1800.kext相同，区别是它包含一些传感器驱动，添加传感器驱动后可检测到诸如CPU温度等信息。

在这里插入图片描述

可右击点击此文件，在弹出菜单中左键点击“显示包内容”，再依次打开Contents目录及其下的Plugins目录，可以看到如下图所示传感器驱动：

在这里插入图片描述

可将其全部复制到OTHER目录下后可看到修改完成后的驱动文件：

在这里插入图片描述

============================================================
某些强迫症患者，比如我，还可将驱动名称中所包含的版本信息去除，经重命名后效果如下图所示：

在这里插入图片描述
当然这并没有什么不同，只是自己望着比较顺眼，哈哈！

3、config.plist：
CLOVER配置文件详细说明见我以前写的一篇教程。

MAC 10.14 安装教程4-制作安装EFI文件
https://www.jianshu.com/p/2ad57fca5969

这里，在MAC系统下将其使用Clover Configurator工具的配置过程加以说明

打开config.plist文件：

10.png

勾选“ADDPNLF”选项，其作用显示亮度调节滑块，勾选后如下图：

在这里插入图片描述

点击左侧“Boot”选项页。
通过点击下图中红圈勾选处的减号按键取消篮圈勾选处的启动参数：

在这里插入图片描述

上图中蓝色圈勾选处参数为开机跑代码，以方便排查错误。
取消后开机由跑码改为读取进度条。

本人已试验过，可轻松安装系统，所以为了美观，特意取消此处启动参数。
与我电脑同款的朋友，可以取消启动参数，其他朋友建议暂时不要取消，等安装成功后无误再取消此处参数。

更改后如下图所示：

在这里插入图片描述
点击左侧“Devices”选项页。

在这里插入图片描述
在红色圈勾选处填入数字“2”，其作用为仿冒声卡驱动。

更改后如下图所示：

在这里插入图片描述

点击左侧“Gui”选项页。

在这里插入图片描述

如上图红色圈勾选处所示，更改3项：
1、取消勾选“Linux”，作用CLOVER不引导Linux系统。
2、将分辨率改为2560X1440，AIRBOOK屏幕为夏普2.5K屏，分辨率为2560X1440。
其它朋友请根据自己电脑屏幕的实际分辨率进行修改，只要一致即可。
3、将CLOVER主题设为MAC，与之前EFI\CLOVER\THEMES\MAC保持一致。
不想更改主题的朋友请忽略此处修改。

更改后如下图所示：

在这里插入图片描述
点击左侧“SMBIOS”选项页。

在这里插入图片描述

点击红色圈所勾选出按钮，在弹出菜单中选择机型。
我电脑的CPU为I7-7600U，故选择最为接近的机型：MacBookPro14,3。
其他朋友请根据自己CPU型号选择，注意，机型选择很重要，务必请认真。

选好机型后如下图所示：

在这里插入图片描述

如上图中所示：
请随意用鼠标点击几下红色圈所勾选按钮。
作用是随机生成新的电脑序列号和SMUUID。

点击左侧“System Parameters”选项页。

在这里插入图片描述

如上图红色圈勾选处所示，更改2项：
1、点击几下红色圈所勾选1处按钮，随机生成新的UUID。
新的UUID在上图蓝色圈勾选处显示。
2、在红色圈所勾选2处，填入数字“0501”，此处为屏幕默认亮度值。

更改后如下图所示：

在这里插入图片描述
点击左侧“Rt Variables”选项页。

在这里插入图片描述

点击几下红色圈所勾选处按钮，随机生成新的ROM序列号。
新的信息在蓝色圈勾选处显示。

更改后如下图显示：

在这里插入图片描述
至此，config.plist文件已全部修改完毕。

保存退出后，将EFI拷贝至硬盘ESP分区根目录下即可。

接下来您就可以体验到与白苹果一般丝滑流畅的安装过程了。

https://www.jianshu.com/p/81e329c50120
------------------------------------------------
版权声明：本文为CSDN博主「wukuy」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_44520133/article/details/104233350  
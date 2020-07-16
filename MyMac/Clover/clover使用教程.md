[ 黑果小兵的部落阁 ](https://blog.daliansky.net/)

Hackintosh安装镜像、教程及经验分享

- [
  首页](https://blog.daliansky.net/)
- [
  归档](https://blog.daliansky.net/archives/)
- [
  分类](https://blog.daliansky.net/categories/)
- [
  标签](https://blog.daliansky.net/tags/)
- [
  关于](https://blog.daliansky.net/about/)
- [
  搜索](javascript:;)



# clover使用教程

 2017-10-20 |  2020-05-13 |  [教程](https://blog.daliansky.net/categories/教程/) | 545384

 7.8k |  14 分钟

[![img](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/Clover_Logo.png)](http://7.daliansky.net/Clover_Logo.png)

#  Clover的前世今生

> 前言：*先将最最基本的操作发出来，然后再写完整的教程*

##  如何打开啰嗦模式进行排错

开机进入clover引导界面，
[![1-main](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/1-main.png)](http://7.daliansky.net/1-main.png)
在要引导的分区卷标上按 `空格` 即可进入
[![space2](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/space2.png)](http://7.daliansky.net/space2.png)
勾选以下选项：
[![space-selected](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/space-selected.png)](http://7.daliansky.net/space-selected.png)
选择 `Boot macOS with selected options` 启动
出现错误画面拍照发群里寻求帮助。

##  Clover是什么

什么是Clover（三叶草）呢？显然它不是指的草地里用来喂牛的草啦。Clover是一个软件，是一个新型的启动器，它能够让普通的PC上用上Mac OS X系统。

苹果公司（Apple）限制Mac OS X系统只能在Apple设备上使用，并且苹果不保证Mac OS  X在其它设备上能够正常工作。所以，用户需要承担一定的风险。当然，为了避免其它的法律纠纷，你不应该用作商业用途。装上了Mac OS  X的非苹果电脑，就叫做黑苹果(Hackintosh)。

##  名字的来源

启动器的名字 `Clover` 由一位创建者kabyl命名。他发现了四叶草和Mac键盘上Commmand键的相似之处，由此起了Clover这个名字。

> 维基百科：*四叶草是三叶草的稀有变种。根据西方传统，发现者四叶草意味的是好运，尤其是偶然发现的，更是祥瑞之兆。另外，第一片叶子代表信仰，第二片叶子代表希望，第三片叶子代表爱情，第四片叶子代表运气。*

##  Clover能干什么

Clover是一个操作系统启动加载器(boot loader)，能够同时运行于支持EFI方式启动的新式电脑和不支持它的老式电脑上。一些操作系统可以支持以EFI方式启动，比如OS X,  Windows 7/8/10 64-bit, Linux；也有不支持的，比如Windows  XP，它只能通过传统的BIOS方式来启动，也就是通过启动扇区来启动。

EFI不仅存在于操作系统的启动过程中，它还会创建操作系统可访问的表和服务(tables and services)，操作系统的运行依赖于EFI正确的提供功能。从内建的UEFI来启动OS X是不可能的，用原始的DUET来启动OS  X也不可能。CloverEFI和CloverGUI做了大量的工作来修正内部表，让运行OS X成为可能。

> 译注：*DUET(Developer’s UEFI Emulation)，开发者的UEFI模拟*

##  Clover的两种启动方式

启动方式A: 基于BIOS的电脑（老式主板）
`BIOS->MBR->PBR->boot->CLOVERX64.efi->OSLoader`

启动方式B: 基于UEFI的电脑（新式主板）
`UEFI->CLOVERX64.efi->OSLoader`

##  Clover兼EFI的目录结构

[![EFI](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/EFI.png)](http://7.daliansky.net/EFI.png)

##  Clover驱动程序详解

BIOS启动过程中（启动方式A）要用到drivers32或drivers64目录，UEFI启动过程中（启动方式B）则使用 `drivers64UEFI` 目录。它们的内容会根据配置和BIOS版本而有所不同。
**必须要提的一点是这些驱动程序只在bootloader运行时有效，不会影响最终启动的操作系统。**
至于到底要使用哪些驱动程序由用户来决定。

###  Drivers64UEFI目录几必备的驱动程序：

| 驱动程序                | 详解                                                         |
| ----------------------- | ------------------------------------------------------------ |
| ApfsDriverLoader-64.efi | 苹果新推出的`apfs`文件系统，macOS 10.13/10.14必备            |
| FSInject.efi            | 控制文件系统注入kext到系统的可能性。详细解释请参照[WithKexts](https://clover-wiki.zetam.org/Configuration#boot-args) |
| HFSPlus.efi             | HFS+文件系统驱动程序。这个驱动对于通过启动方式B来启动Mac OS X是必须的。启动方式A中用到的启动程序(CloverEFI)已经包含了这个驱动 |
| AptioMemoryFix-64.efi   | 修复AMI Aptio EFI内存映射。如果没有就不能启动OS X            |
| OsxFatBinaryDrv-64.efi  | 允许加载FAT模块比如boot.efi                                  |
| CsmVideoDxe.efi         | 比UEFI里提供更多分辨率的显卡驱动(可选)                       |

#  Clover Bootloader详解

本节会详细介绍Clover各项功能的用法

##  Clover主界面

使用Clover开机引导后，默认的系统界面如下：
[![1-main](http://7.daliansky.net/1-main.png)](http://7.daliansky.net/1-main.png)

本例中:

- 蓝色图标为 `Windows 10` 的引导
- 绿色图标为 `Ubuntu Linux` 的引导
- 橙色图标为 `macOS High Sierra` 的引导
- 红色图标为 `macOS Sierra`的引导

当你想引导到不同的操作系统，只需要移动键盘上的左右键到各自的图标后回车即可。

##  帮助菜单 F1

按 `F1` 键会呼出 `Clover` 的帮助信息
[![Help_F11](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/Help_F11.png)](http://7.daliansky.net/Help_F11.png)

- ESC 退出子菜单，恢复到主菜单
- F1 帮助
- F2 保存 `preboot.log` 到 `EFI/CLOVER/misc/` 目录下，以便于您排错
- F3 显示 `被隐藏` 的入口
  - 比如你在 `config.plist` 中隐藏了 `Recovery HD`
    [![HideVolume](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/HideVolume.png)](http://7.daliansky.net/HideVolume.png)
  - 当你想进入恢复模式的时候，可以不需要修改 `config.plist` 而直接按 `F3` 显示出那些被你隐藏的引导项。如下图：
    [![f3](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/f3.png)](http://7.daliansky.net/f3.png)
- F4 提取 `DSDT` 保存到 `EFI/CLOVER/ACPI/origin/`
  - 此选项会经常用到。比如你的机器出现问题，需要别人帮助你解决问题，人家会跟你要 `DSDT` ，这个时候你只需要在 `Clover` 引导界面按下快捷键 `F4` 即可提取
- F5 提取修正过的 `DSDT` 保存到 `EFI/CLOVER/ACPI/origin/`
- F6 提取显卡ROM `VideoBios` 保存到 `EFI/CLOVER/misc/`
- F10 截屏，截取当前界面，保存到 `EFI/CLOVER/misc/`
- F11 重置NVRAM,r4299版本新增
- 空格 选定菜单项的详细信息
- 数字 1-9 菜单项的快捷键
- A 关于 `Clover`
  [![aboutclover](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/aboutclover.png)](http://7.daliansky.net/aboutclover.png)
- O `Clover` 选项
  - 此选项是 `Clover` 的核心，所有的选项都在这个菜单里，当你无法引导进入 `macOS` 系统使用 `Clover Configurator` 进行选项调整时，可以通过该选项进行修改进入系统。**后面会详细介绍**
    [![options](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/options.png)](http://7.daliansky.net/options.png)
- R 软复位
- U 退出

##  如何打开啰嗦模式进行排错【重复内容，目的是加深印象】

开机进入clover引导界面，
[![1-main](http://7.daliansky.net/1-main.png)](http://7.daliansky.net/1-main.png)
在要引导的分区卷标上按 `空格` 即可进入
[![space2](http://7.daliansky.net/space2.png)](http://7.daliansky.net/space2.png)
勾选以下选项：
[![space-selected](http://7.daliansky.net/space-selected.png)](http://7.daliansky.net/space-selected.png)
选择 `Boot macOS with selected options` 启动
出现错误画面拍照发群里寻求帮助。

##  Clover Options：Clover选项

> 文章上面已经提到了Clover的选项是它的核心，那么现在就让我们走进Clover选项设置

照例先上一张图：
[![options](http://7.daliansky.net/options.png)](http://7.daliansky.net/options.png)

- Boot Args

- 引导参数传递，比如前面教大家的使用 `-v` 打开啰嗦模式，就是通过它传递的；再比如你使用了不兼容版本的 `Lilu` 和 `AppleALC` 而导致无法进入系统时，可以在这上面手动添加上： `-liluoff` 或者 `-alcoff` 跳过相关的驱动而进入系统

- Configs

  - 配置文件选择。当你有不止一个 `config.plist` 配置文件时，可以通过该选项选择不同的配置文件进入系统
  - 操作过程
    - 光标移动到 `Configs`
      [![configs](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/configs.png)](http://7.daliansky.net/configs.png)
    - 回车后进入子菜单
      [![configselect](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/loading.gif)](http://7.daliansky.net/configselect.png)
    - 通过光标上下移动选择其它的配置文件，回车后按 `ESC` 键退到主菜单

- GUI tuning

  - Clover主题切换，当你有不止一套主题的时候，可以通过该选项切换主题
  - 操作过程
    - 光标移动到 `GUI tuning`
      [![gui](https://blog.daliansky.net/images/loading.gif)](http://7.daliansky.net/gui.png)
    - 回车后进入子菜单
      [![guithemes](https://blog.daliansky.net/images/loading.gif)](http://7.daliansky.net/guithemes.png)
    - 光标移动到 `Themes` ，回车后进入子菜单
      [![guithemeselect](https://blog.daliansky.net/images/loading.gif)](http://7.daliansky.net/guithemeselect.png)
    - 通过光标上下移动选择其它的主题，回车后按 `ESC` 键退到主菜单，Clover主界面已经刷新为选择的新主题

- ACPI patching

  - 电源补丁：进不去电脑的，需要drop tables的，禁用/调用 `DSDT.aml` ，禁用 `hotpatch` ，无关关机的，无法重启的；总之跟电源相关的都在这里边。

  - 操作过程
    光标移动到 `ACPI patching`
    [![acpi](https://blog.daliansky.net/images/loading.gif)](http://7.daliansky.net/acpi.png)

    回车后进入子菜单
    [![acpi-submenu](https://blog.daliansky.net/images/loading.gif)](http://7.daliansky.net/acpi-submenu.png)

    - Debug DSDT
      - 打开DSDT调试模式
    - DSDT name
      - 默认文件名为：DSDT.aml
    - Tables dropping
      - 光标移动到 `Tables dropping` 回车进入
        [![acpi-tablesdrop](https://blog.daliansky.net/images/loading.gif)](http://7.daliansky.net/acpi-tablesdrop.png)
      - 通过移动光标按空格勾选各选项，按 `ESC` 退出该子菜单
        [![acpi-table-dropping](https://blog.daliansky.net/images/loading.gif)](http://7.daliansky.net/acpi-table-dropping.png)
      - 该表格等同于使用 `Clover Configurator` 打开 `config.plist` 后，在 `ACPI` 选项的左下角 `Drop Tables`
        [![Drop-Tables](https://blog.daliansky.net/images/loading.gif)](http://7.daliansky.net/Drop-Tables.png)
    - Drop OEM _DSM
      - 丢弃_DSM
      - 光标移动到 `Drop OEM _DSM` 回车进入
        [![apci-drop-oem_dsm](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/apci-drop-oem_dsm.png)](http://7.daliansky.net/apci-drop-oem_dsm.png)
      - 通过移动光标按空格勾选各选项
        [![acpi-drop-oem_dsm-selected](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/acpi-drop-oem_dsm-selected.png)](http://7.daliansky.net/acpi-drop-oem_dsm-selected.png)
      - 按 `ESC` 退出该子菜单
    - DSDT fix mask
      - DSDT修复遮盖
      - 详细说明如下：

| 参数          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| `AddDTGP`     | 修改 DSDT 添加方法所必须依赖的函数。必不可缺                 |
| `FixDarwin`   |                                                              |
| `Fixshutdown` | 关机修复，主要是添加 _PTS 函数，判断寄存器 arg0 值是否为 5 ，华硕主板建议勾选. |
| `AddMCHC`     | 这个功能是在 dsdt 中添加一装置具体是 DveiceID=0X0044,匹配 Intel Clarkdale 平台。有些芯片需要这个装置来解决 PCI 的电源管理问题，一般不启用 |
| `FixHPET`     | 修复 HPET ，添加 IRQ(0,8,11) 加载原生电源管理，10.9 不需要   |
| `FakeLPC`     | 仿冒 LPC ，一般 Clover 会自动注入合适的芯片参数到 dsdt 中，来达到加载 AppleLPC.kext 的目的。对以 Intel and NForce 芯片，建议勾选。特别是芯片组比较老的如：ICH7,ICH9 |
| `FixIPIC`     | 从 decice IPIC 移除中断语句 (IRQ(2)),有助于电源按钮的工作，对于笔记本而言，更希望增加这个中断功能 |
| `FixSBUS`     | 增加 SMBusControlle 到设备树种，可修复因缺失 SBUS 控制而在系统 log 中出现的警告，建议勾选 |
| `Fixdisplay`  | 增加 GFX0,和 HDMI 音频设置 HADU. 如果设置了 FAKEID 也会增加到这边，建议勾选 |
| `FixIDE`      | 修复在 10.6 事五国出现的 AppleIntelPIIXATA 错误。 一般不启用 |
| `FixSATA`     | 内建磁盘，用 ICH6 的 ID 匹配，解决橙色磁盘问题，一般启用     |
| `FixFIREWIRE` | 在火线控制装置中增加 fwhub 属性。一般不启用                  |
| `FixUSB`      | 注入 USB 属性，帮助内建 USB                                  |
| `FixLAN`      | 注入网卡属性，帮助网卡内建。建议启用                         |
| `FixAirport`  | 为支持 Airport 的无线网卡注入属性，以开启 Airport 功能，无此设备的不启用 |
| `FixHDA`      | 修正 AZAL to HDEF or HDAU, 增加 layout -id 和 pinconfig,MaximumBootBeepVolume 属性 |
| `FixDarwin7`  | 这项补丁只有Darwin OS系统［苹果系统］识别                    |
| `FixRTC`      | 从RTC装置中删除IRQ（0），作用是否与patch里的防RTC重置类似？  |
| `FixTMR`      | 从TMR装置中删除IRQ(8)，适用于较早的dos设备，现代新的计算机不需要补丁，这个问题只是以前没发现［作者］ |
| `AddIMEI`     | 这个设置用于intelHDxxx集成显卡，解决完美注入。这项也需要开启fakeid -> IMEI |
| `FixIntelGfx` | 开启对IntelGFX显卡的新补丁设置，不开启则补丁不会生效［配合imei］ |
| `FixWAK`      | 这个补丁主要是消除警告，如在method_WAK下缺少返回语句则加入Return(Package(0))，我不知道会有什么 |
| `DeleteUnuse` | 从DSDT中删除没有使用的设备如软盘驱动器，打印机端口和其他没用的设备 |
| `FixADP1`     | 将“ AC0 ”设备 重命名为“ ADP1”设备.                           |
| `AddPNLF`     | 添加一个非常实用的PNLF 设置代码：当然只有你可以调节亮度控制时才有用。这个补丁也会对系统良好的睡眠/唤醒 |
| `FixS3D`      | 修正了 _S3Dmethods函数，也解决了一些睡眠/唤醒的问题          |
| `FixACST`     | ACST项含义对于苹果和华硕意义不同，对于华硕是AC适配器状态，而苹果是一个替代_cst，c-states table［CPU 闲置休眠状态的功能］。如果要没有冲突就要将其重名为其他名称的东西 |
| `AddHDMI`     | 修复HDMI音频输出问题（无需修改AppleHDA）                     |
| `FixRegions`  | 因为BIOS当中的一些内容改变了。这个浮动的区域导致无法使用自定义DSDT（custom  DSDT），因为这个区域可移动且不符合当前的状态。这个补丁的目的是找到BIOS中所有这样的区域并在自定义DSDT中加以修正。所以现在你可以生成有错误区域的自定义DSDT然后使用这个补丁 |
| `FixHeaders`  | MACH reboot修复                                              |

- 光标移动到 `DSDT fix mask` 回车进入
  [![acpi-DSDT-fix-mask](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/acpi-DSDT-fix-mask.png)](http://7.daliansky.net/acpi-DSDT-fix-mask.png)

- 通过移动光标按空格勾选各选项

  ![acpi-DSDT-fix-mask-selected](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/acpi-DSDT-fix-mask-selected.png)

  ![acpi-DSDT-fix-mask-selected2](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/acpi-DSDT-fix-mask-selected2.png)

  - 按 `ESC` 退出该子菜单

- Custom DSDT patches

  - 自定义的DSDT补丁
  - 光标移动到 `Custom DSDT patches` 回车进入
    [![acpi-Custom-DSDT-patches](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/acpi-Custom-DSDT-patches.png)](http://7.daliansky.net/acpi-Custom-DSDT-patches.png)
  - 通过移动光标按空格勾选各选项
    [![acpi-Custom-DSDT-patches-selected](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/acpi-Custom-DSDT-patches-selected.png)](http://7.daliansky.net/acpi-Custom-DSDT-patches-selected.png)
  - 按 `ESC` 退出该子菜单

##  Block injected kexts 管理你的驱动程序

通过Clover加载的驱动程序位于 `EFI/CLOVER/kexts/Other` ，也有可能位于 `EFI/CLOVER/kexts/10.13` 或者 `EFI/CLOVER/kexts/10.12` 目录中。它至少包括以下驱动程序：

| 驱动程序                    | 详细信息                                                     | 备注 |
| --------------------------- | ------------------------------------------------------------ | ---- |
| FakeSMC.kext                | 安装hackintosh的核心程序，没有它就没法在你的电脑上面运行macOS | 必备 |
| Lilu.kext                   | 内核扩展程序，离开它，下面的几个程序都无法正常运行           | 必备 |
| WhateverGreen.kext          | 显卡综合修复，整合了核显、AMD、NVIDIA的综合修复，包括 （单卡启动黑屏，唤醒黑屏 等等）(依赖于Lilu) | 必备 |
| AppleALC.kext               | 动态对系统注入必要的文件/打补丁以驱动声卡(依赖于Lilu)        | 可选 |
| IntelGraphicsFixup.kext     | 修补 Intel 核显综合问题 (开机花屏，Haswell/Skylake 因 PAVP 导致的死机等等)(依赖于Lilu) | 可选 |
| IntelGraphicsDVMTFixup.kext | 修正 Broadwell/Skylake 平台核显因 DVMT 不足而导致的死机(依赖于Lilu) | 可选 |
| NvidiaGraphicsFixup.kext    | 修正 N 卡 (可能也适用于 I 卡) 使用某些 SMBios 如 MacPro6,1 等引发黑屏的问题(依赖于Lilu) | 可选 |
| AirportBrcmFixup            | 修补 Broadcom Wi-Fi 综合问题                                 | 可选 |
| FakePCIID.kext              | 仿冒PCI设备核心驱动，部分驱动依赖于它                        | 可选 |
| ACPIBatteryManager.kext     | 笔记本电池管理驱动                                           | 可选 |
| RealtekRTL8xxx.kext         | Realtek 8xxx网卡驱动程序                                     | 可选 |
| VoodooPS2Controller.kext    | Voodoo键盘/鼠标驱动程序                                      | 可选 |

当你面对这么一堆驱动的时候，如何让它们有条不紊地正常工作呢？万一出现问题的时候又如何让这些驱动在Clover引导的时候禁用它们呢？这个时候 `Block injected kexts` 就派上用场了。新版的 `Clover Bootloader` 已经将 `Block injected kexts` 放到了 `macOS` 系统分区的图标下面了。

- 操作方法：
  - 开机进入clover引导界面，要引导的分区卷标上按 `空格` 即可进入
    [![1-main](http://7.daliansky.net/1-main.png)](http://7.daliansky.net/1-main.png)
  - 光标移动到 `Block injected kexts` 回车进入
    [![kim](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/Blockinjectedkexts.png)](http://7.daliansky.net/Blockinjectedkexts.png)
  - 选择当前系统加载的驱动程序目录
    [![kimselect](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/BIKSelect.png)](http://7.daliansky.net/BIKSelect.png)
  - 勾选禁用的驱动程序
    [![kimoptions](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/BIKChoose.png)](http://7.daliansky.net/BIKChoose.png)
  - 按 `ESC` 退出该子菜单

#  后记

Clover Bootloader的使用暂时先写到这里，还有更多的用法等着我们去发掘。您有什么想法或者需要补充的，请点击下面的QQ群与我联系更新。

#  特别感谢

群友 `(￣(工)￣)_小哥哥` 帮忙整理部分资料

#  关于打赏

整整写了两天的博文，如果不希望看到博主停更的话，请点击下方的 `打赏` 支持一下，有钱的捧个钱场，没钱的捧个人场，谢谢大家！

##  感谢名单

- [Apple](https://www.apple.com/) 的 macOS
- [RehabMan](https://github.com/rehabman)维护的项目：[OS-X-Clover-Laptop-Config](https://github.com/RehabMan/OS-X-Clover-Laptop-Config) [Laptop-DSDT-Patch](https://github.com/RehabMan/Laptop-DSDT-Patch) [OS-X-USB-Inject-All](https://github.com/RehabMan/OS-X-USB-Inject-All)等
- [Acidanthera](https://github.com/acidanthera) 维护的项目：[OpenCorePkg](https://github.com/acidanthera/OpenCorePkg) [lilu](https://github.com/acidanthera/Lilu) [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup) [WhateverGreen](https://github.com/acidanthera/WhateverGreen) [VirtualSMC](https://github.com/acidanthera/VirtualSMC) [AppleALC](https://github.com/acidanthera/AppleALC) [BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM) [MaciASL](https://github.com/acidanthera/MaciASL) 等
- [headkaze](https://www.insanelymac.com/forum/profile/1364628-headkaze/) 提供的工具：[hackintool](https://github.com/headkaze/Hackintool) [PinConfigurator](https://github.com/headkaze/PinConfigurator) [BrcmPatchRAM](https://www.insanelymac.com/forum/topic/339175-brcmpatchram2-for-1015-catalina-broadcom-bluetooth-firmware-upload/)
- [CloverHackyColor](https://github.com/CloverHackyColor)维护的项目：[CloverBootloader](https://github.com/CloverHackyColor/CloverBootloader) [CloverThemes](https://github.com/CloverHackyColor/CloverThemes)
- 宪武整理的：[P-little](https://github.com/daliansky/P-little) [OC-little](https://github.com/daliansky/OC-little)
- [chris1111](https://github.com/chris1111)维护的项目：[VoodooHDA](https://github.com/chris1111/VoodooHDA-2.9.2-Clover-V15) [Wireless USB Adapter Clover](https://github.com/chris1111/Wireless-USB-Adapter-Clover)
- [zxystd](https://github.com/zxystd)开发的[itlwm](https://github.com/zxystd/itlwm) [IntelBluetoothFirmware](https://github.com/zxystd/IntelBluetoothFirmware)
- [lihaoyun6](https://github.com/lihaoyun6)提供的工具：[CPU-S](https://github.com/lihaoyun6/CPU-S) [macOS-Displays-icon](https://github.com/lihaoyun6/macOS-Displays-icon) [SidecarPatcher](https://github.com/lihaoyun6/SidecarPatcher)
- [sukka](https://github.com/SukkaW)更新维护的[从 Clover 到 OpenCore —— Clover 迁移 OpenCore 指南](https://blog.skk.moe/post/from-clover-to-opencore/)
- [xzhih](https://github.com/xzhih)提供的工具：[one-key-hidpi](https://github.com/xzhih/one-key-hidpi)
- [Bat.bat](https://github.com/williambj1)更新维护的[精解OpenCore](https://blog.daliansky.net/OpenCore-BootLoader.html)
- [shuiyunxc](https://github.com/shuiyunxc) 更新维护的[OpenCore配置错误、故障与解决办法](https://shuiyunxc.gitee.io/2020/04/06/Faults/index/)
- [athlonreg](https://github.com/athlonreg)更新维护的[OpenCore 0.5+ 部件补丁](https://blog.cloudops.ml/ocbook/) [Common-patches-for-hackintosh](https://github.com/athlonreg/Common-patches-for-hackintosh)
- [github.com](https://blog.daliansky.net/github.com)
- [码云 gitee.io](https://blog.daliansky.net/gitee.io)
- [扣钉 coding.net](https://blog.daliansky.net/coding.net)

##  参考及引用：

- https://deviwiki.com/wiki/Dell
- https://deviwiki.com/wiki/Dell_Wireless_1820A_(DW1820A)
- [Hervé](https://blog.daliansky.net/[https://osxlatitude.com/profile/4953-hervé/](https://osxlatitude.com/profile/4953-hervé/)) 更新的Broadcom 4350:https://osxlatitude.com/forums/topic/12169-bcm4350-cards-registry-of-cardslaptops-interop/
- [Hervé](https://blog.daliansky.net/[https://osxlatitude.com/profile/4953-hervé/](https://osxlatitude.com/profile/4953-hervé/)) 更新的DW1820A支持机型列表:https://osxlatitude.com/forums/topic/11322-broadcom-bcm4350-cards-under-high-sierramojave/
- [nickhx](https://osxlatitude.com/profile/129953-nickhx/) 提供的蓝牙驱动：https://osxlatitude.com/forums/topic/11540-dw1820a-for-7490-help/?do=findComment&comment=92833
- [xjn819](https://blog.xjn819.com/)： [使用OpenCore引导黑苹果](https://blog.xjn819.com/?p=543) [300系列主板正确使用AptioMemoryFix.efi的姿势(重写版）](https://blog.xjn819.com/?p=317)
- [insanelymac.com](https://www.insanelymac.com/)
- [tonymacx86.com](https://www.tonymacx86.com/)
- [远景论坛](http://bbs.pcbeta.com)
- [applelife.ru](https://applelife.ru/)
- [olarila.com](https://www.olarila.com/)

[![黑果小兵 wechat](clover%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.assets/WeChat.png)](https://blog.daliansky.net/uploads/WeChat.png)

微信扫一扫，订阅【黑果小兵的部落阁】

-------------本文结束感谢您的阅读-------------

如果文章对您有帮助，就请站长喝杯咖啡吧 ´◡`

- 

- ## QQ群列表：

- 

- 688324116 [一起黑苹果](http://shang.qq.com/wpa/qunwpa?idkey=6bf69a6f4b983dce94ab42e439f02195dfd19a1601522c10ad41f4df97e0da82) 2000人群 人满为患
  331686786 [一起吃苹果](http://shang.qq.com/wpa/qunwpa?idkey=db511a29e856f37cbb871108ffa77a6e79dde47e491b8f2c8d8fe4d3c310de91) 2000人群 人满为患
  257995340 [一起啃苹果](http://shang.qq.com/wpa/qunwpa?idkey=8a63c51acb2bb80184d788b9f419ffcc33aa1ed2080132c82173a3d881625be8) 2000人群 远景报备群
  875482673 [黑果小兵黑苹果技术群](http://shang.qq.com/wpa/qunwpa?idkey=81c3783ae414ddac233af104d949899edde38aeac60defcb05036fa7d3fa2972) 2000人群收费群 已满员，请加3/4群
  1058822256 [黑果小兵黑苹果技术群2](http://shang.qq.com/wpa/qunwpa?idkey=0565a16f6dbfa1c69159d15b8fa4ee6473fbf61cb8e998d56e807459aa3006a2) 2000人收费群 已满员，请加3/4群
  819662911 [黑果小兵黑苹果技术群3](http://shang.qq.com/wpa/qunwpa?idkey=f7374ff70f7e52442ed6433abfe844791b2ee595e470d74b462143065210b857) 2000人群(新) 付费群
  954098809 [黑果小兵黑苹果技术群4](http://shang.qq.com/wpa/qunwpa?idkey=034013783cd47f32ae35f19d3fc55591170fa66c8449a1d7aeae53f0935ec505) 2000人群(新) 付费群
  701278330 [黑苹果无线网卡交流群](http://shang.qq.com/wpa/qunwpa?idkey=5bfd8b092f5a3f3079eab8bb1a497973dbba78ad785d9520ad090a931aeb06f6) 1000人群 DW1820A技术支持群
  891434070 [Catalina黑苹果交流群](http://shang.qq.com/wpa/qunwpa?idkey=be06e4c13e796e06a5cd3151d7fcc8f2feee8f9b68b1620ce8771111e2822084) 2000人群 远景报备群
  939122730 [Catalina黑苹果交流II群](http://shang.qq.com/wpa/qunwpa?idkey=e7fb8ea793aee10f9e86c70cd134867bde4183cc3eb87424e61e50b3e9cabf72) 2000人群
  891677227 [黑果小兵高级群](http://shang.qq.com/wpa/qunwpa?idkey=9a1eaa552c45d736bb6b19d82ad80e76bf82729f1c1a975b437aa3858473231d) 2000人群
  943307869 [黑果小兵高级群II](http://shang.qq.com/wpa/qunwpa?idkey=7080e6ff936fd2e207439ea18c0a34b4651db81ff45d0edf27b34a21a037871e) 2000人群
  538643249 [OpenCore技术交流群](http://shang.qq.com/wpa/qunwpa?idkey=665ed002721454d2e811535020261a04b0aae2fa3b6a2ffde5778a852f892178) 2000人群 大神众多非OC适配者慎入
  673294583 [小新Pro黑苹果技术群](http://shang.qq.com/wpa/qunwpa?idkey=c00a79b8adbba92c152f71cdc721660cc2de276f62a1d4435c83b884e7f369c0) 2000人群 非专用机型请勿加入
  946132482 [小新Pro黑苹果](http://shang.qq.com/wpa/qunwpa?idkey=d64dff649d6a19f9496aaa472ee3a14450cfbbeddd0e618edf182ffe320a1fd0) 500人群 非专用机型请勿加入
  943181023 [联想小新Air黑苹果交流群](http://shang.qq.com/wpa/qunwpa?idkey=fb772a7e01436d43e1d856a099549551952bb08161ced4a8fc08b4e75e7ab438) 500人群 非专用机型请勿加入

- 

- ## Telegram群：

- 

- 黑果小兵的部落阁 http://t.me/daliansky

- **本文作者：** 黑果小兵
- **本文链接：** https://blog.daliansky.net/clover-user-manual.html
- **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明出处！

[ 教程](https://blog.daliansky.net/tags/教程/) [ clover](https://blog.daliansky.net/tags/clover/) [ 手册](https://blog.daliansky.net/tags/手册/)

[ 惠普HP ENVY Laptop 13-ad1xx笔记本电脑安装MacOS手记及EFI分享](https://blog.daliansky.net/HP-HP-ENVY-Laptop-13-ad1xx-Notebook-PC-Mac-OS-Note-and-EFI-Share.html)



[MAC常用快捷键整理 ](https://blog.daliansky.net/MAC-commonly-used-shortcut-keys-finishing.html)

[0](https://blog.daliansky.net/null) 条评论

未登录用户

Error: Network Error

  



[   ](https://guides.github.com/features/mastering-markdown/)

[   ](https://guides.github.com/features/mastering-markdown/)[   ](https://guides.github.com/features/mastering-markdown/)[  支持 Markdown 语法](https://guides.github.com/features/mastering-markdown/)

来做第一个留言的人吧！

  

- 文章目录
- 站点概览

1. [1. Clover的前世今生](https://blog.daliansky.net/clover-user-manual.html#clover的前世今生)
2. [2. Clover Bootloader详解](https://blog.daliansky.net/clover-user-manual.html#clover-bootloader详解)
3. [3. 后记](https://blog.daliansky.net/clover-user-manual.html#后记)
4. [4. 特别感谢](https://blog.daliansky.net/clover-user-manual.html#特别感谢)
5. [5. 关于打赏](https://blog.daliansky.net/clover-user-manual.html#关于打赏)

[辽ICP备15000696号-3 ](http://www.beian.miit.gov.cn)© 2016 – 2020  黑果小兵 |  610k |  18:29

 7754658  18642439

 0%









# END
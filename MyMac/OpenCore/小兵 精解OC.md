# 精解OpenCore

 2019-06-11 |  2020-05-14 |  [教程](https://blog.daliansky.net/categories/教程/) | 289036

https://blog.daliansky.net/OpenCore-BootLoader.html





 33k |  1:01

> 教程更新于 `2020.3.2`, 基于 `OpenCore 0.5.6` **官方版本**

**由于个人能力有限, 教程中难免会有些疏漏, 这里推荐大家在参阅本教程的同时也阅读以下资料:**

1. [OpenCore 官方文档](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf) – OpenCore 最权威的资料, 没有之一!!!
2. [xjn‘s Blog](https://blog.xjn819.com/?p=543) – xjn 大佬的博客, 对台式机非常友好的教程, 内存管理写的非常详细
3. [OC-little](https://github.com/daliansky/OC-little) – 宪武大佬的 OC ACPI 热补丁示例
4. [Opencore Vanilla Desktop Guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/)

[![img](%E5%B0%8F%E5%85%B5%20%E7%B2%BE%E8%A7%A3OC.assets/OpenCore.png)](http://7.daliansky.net/OpenCore/OpenCore.png)

##  什么是 OpenCore

OpenCore`(简称 OC)` 是一个着眼于未来开源的引导工具, 最初诞生于 HermitCrabs 实验室, 现在接手于 Acidanthera, 其目的是创造一个更加严谨的模组化的轻量引导系统。尽管 OpenCore 的主要用途是黑苹果, 它也支持其它操作系统的引导。

OpenCore 现在处于公测 Beta 阶段, 引导相关功能也已非常稳定, 喜欢折腾的朋友现在已经是动手的时机了。

这个教程只能作为你探索的起点。请仔细阅读并时刻牢记你的硬件可能有不同的配置要求。

##  为什么选择 OpenCore

1. 从 2019 年 9 月以后, Acidanthera 开发的内核驱动 (Lilu, AppleALC 等等) **「不再会」** 在 Clover 上做兼容性测试
2. OpenCore 更加注重系统的安全性, 提供对 OpenCore 自身引导文件对加密, 同时对文件保险箱 (FileVault) 有更强大的支持, 在未来会支持 UEFI 安全启动
3. OpenCore 启动 FileVault (硬盘保险箱) 加密的分区速度远超 Clover
4. OpenCore 支持基于 `boot.efi` 的原生开机快捷键支持
5. OpenCore 使用更加先进的方法注入第三方内核扩展驱动 (Kext) 且与此同时不会破坏系统完整性保护
6. OpenCore 通过读取启动磁盘设置的 NVRAM 变量, 可以像白苹果一样支持在设置的启动磁盘切换默认引导项
7. 支持给其它 .efi 驱动或引导工具加入参数
8. 大量 Acidanthera 维护的独立 [UEFI 驱动](https://blog.daliansky.net/OpenCore-BootLoader.html#附录2-uefi-驱动-efi-及其用途) 被合并入 OpenCore, 未来的开发直接与 OpenCore 绑定, **且不再支持 Clover**

###  OpenCore 常见疑问

1. **OpenCore 不自带精美的主题, 以后会添加吗?**

   - **OpenCore 自带的 GUI 仅用于 debug, 设计初衷是在正常使用的时像白苹果一样隐藏。**
   - 其实 OpenCore 已经提供了大量可以绕开 GUI 的功能, 将 `ShowPicker` 设置为 `NO` 隐藏菜单之后, 可以通过系统自带的启动磁盘设置来切换默认系统, 用苹果原生快捷键来重置 NVRAM 和添加引导表示符, 需要其它功能的时候可以随时按 `ESC/Option` 调出, **主题存在的意义不大**
   - N.D.K 为官方 OpenCore 开发了一个独立的 GUI 插件, 可以在 [这里下载](https://github.com/n-d-k/NdkBootPicker/releases/latest), 食用方法请阅读项目的 [Readme](https://github.com/n-d-k/NdkBootPicker/blob/master/README.md)

2. **OpenCore 为什么不会为其它操作系统忽略 ACPI 补丁?**

   真正正确的 ACPI (DSDT/SSDT) 应当适用于所有的操作系统, 单独为一个操作系统制作 ACPI 是不合理的, 因此 OpenCore 基于  ACPI 标准, 不会在 ACPI 上区别对待操作系统, 如果想学习怎样使用和制作通用于所有操作系统的 ACPI 补丁, 可以参考 [OC-little](https://github.com/daliansky/OC-little)

3. **OpenCore 的 MOD 版本和官方版本有何不同?**

   OpenCore MOD 是 N.D.K 的一个 Fork, 里面加入了不注入除了 macOS 之外的其它操作系统的功能,  可以看作是一个自带主题的懒人版。但是除非在个别极端情况下主板的固件真的不走规范, 正确配置原版 OpenCore 和 ACPI  是不会造成无法引导其它操作系统的情况的。本教程鼓励正确配置 OpenCore, 因此基于 **原版 OpenCore** 编写, 且 **「不推荐」** 使用 MOD 版本来掩盖自身的配置问题。

4. **OpenCore 的配置为什么看起来这复杂?**

   - OpenCore 为了提高兼容性, 为用户开放了更多底层的 Quirk
   - OpenCore 现阶段没有可用的非常直观的 GUI 编辑器

   换位思考, 如果用 Xcode 来编辑 Clover 安装包内自带的 Config 样本, 显然 OpenCore 会更简单。OpenCore 很多功能都有且只有一处设置, 但是 Clover 有大量等效组合互相干扰。

##  配置 OpenCore

讲了这么多, 终于到真正动手的环节了

> 本文较长, 建议配合博客右下角的目录阅读

###  准备工作

我们先来做些简单的准备工作



#### 推荐 BIOS 设置

- **禁用:**

|                 英文                 |                           中文                           |
| :----------------------------------: | :------------------------------------------------------: |
|              Fast Boot               |                         快速启动                         |
| CFG Lock (MSR 0xE2 write protection) |                CFG 锁 (MSR 0xE2 写入保护)                |
|                 VT-d                 | [VT-d](https://zhidao.baidu.com/question/495526512.html) |
|                 CSM                  |                      兼容性支持模块                      |
|              Intel SGX               |                        Intel SGX                         |

------

- **启用:**

|          英文           |                           中文                           |
| :---------------------: | :------------------------------------------------------: |
|          VT-x           | [VT-x](https://zhidao.baidu.com/question/495526512.html) |
|    Above 4G decoding    |                   大于 4G 地址空间解码                   |
|     Hyper Threading     |                       处理器超线程                       |
|   Execute Disable Bit   |                        执行禁止位                        |
|   EHCI/XHCI Hand-off    |                   接手 EHCI/XHCI 控制                    |
| OS type: Windows 8.1/10 |               操作系统类型: Windows 8.1/10               |
|    Legacy RTC Device    |                      传统 RTC 设备                       |

> 将 操作系统类型设置为 `Windows 8.1/10` 是因为部分主板在 `Other` 模式下会将系统认作是 Windows 7 从而禁用 UEFI 的某些功能并开启 CSM, 200 系及以后的主板理论上不存在这个问题

------

####  下载如下文件

- [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases) (建议从 Acidanthera 的官方 Sample 开始构建, 并使用 **Debug** 版本)

  > [OpenCore-Factory](https://github.com/williambj1/OpenCore-Factory/blob/master/README_zh-Hans.md) 提供连续的源码自动编译, 喜欢尝鲜的朋友可以下载, 最新编译为 [![Release](https://blog.daliansky.net/images/loading.gif)](https://github.com/williambj1/OpenCore-Factory/releases)

- [MacInfoPkg](https://github.com/acidanthera/MacInfoPkg/releases)

- [AppleSupportPkg](https://github.com/acidanthera/AppleSupportPkg/releases)

- **Plist 编辑器:**

  - [ProperTree](https://github.com/corpnewt/ProperTree) (基于 Python 的轻量级跨平台 Plist 编辑器, 针对 OpenCore 有优化, Acidanthera 官方推荐)
  - [Xcode](https://itunes.apple.com/cn/app/xcode/id497799835?mt=12) (Acidanthera 官方推荐, 但是 Xcode 11 处理 `` 存在严重问题)
  - Plist Editor Pro (使用远古 Plist 标准, 会破坏 XML 换行而且不会自动转换 Base64, 可用但是不推荐)

  [OpenCore Configurator](https://mackie100projects.altervista.org/opencore-configurator/) `(简称 OCC)` 是一个第三方图形化 OpenCore 编辑器, 经过半年依赖的发展, 如今已经趋近成熟

  **经测试, 软件已经可以正常使用, 但是由于 OpenCore 的配置文件更新频繁, 所以软件不一定永远兼容最新版本, 同时最新的版本为了同步开发中的自编译版本可能也存在不支持当前最新官方 Release 的情况。**

  **使用之前需要了解清楚工具的当前支持到的最高版本, 跨版本编辑会造成 config 损坏, 设置项无法对应等棘手情况。**

  感谢 [`mackie100`](https://github.com/mackie100) 和 [`草原企鹅`](https://github.com/btwise) 的付出

------

####  需要具备的条件

- 一个 U 盘
- **对黑苹果有一定的知识基础, 至少会自己配置 Clover, 清楚各个 内核驱动 `.kext` 和 UEFI `.efi` 驱动的用途, 不清楚可以查看[附录[1\]](https://blog.daliansky.net/OpenCore-BootLoader.html#附录1-opencore-支持的内核驱动-kext-及其用途) 和 [附录[2\]](https://blog.daliansky.net/OpenCore-BootLoader.html#附录2-uefi-驱动-efi-及其用途)**
- 一个完全精简过的 Clover EFI
- 一个正常稳定工作的黑苹果测试平台
- EFI 挂载工具

###  创建 USB 启动盘

> 给自己留一个后手, 先用 USB 尝试和排错, 稳定了再迁移到硬盘里

1. 创建一个 USB 启动盘, 格式化为 GUID 分区图, 分区类型为 macOS 日志式。

   [![FormatUSB](%E5%B0%8F%E5%85%B5%20%E7%B2%BE%E8%A7%A3OC.assets/FormatUSB.png)](http://7.daliansky.net/OpenCore/FormatUSB.png)

2. 挂载完 EFI 分区真有意思的部分就「开始了」

   [![EmptyEFI](%E5%B0%8F%E5%85%B5%20%E7%B2%BE%E8%A7%A3OC.assets/EmptyEFI.png)](http://7.daliansky.net/OpenCore/EmptyEFI.png)

###  基础文件夹结构

1. 解压下载下来的 OpenCore 引导文件, 把 EFI 文件夹里面的 `OC 文件夹` 和 `BOOT/BOOTx64.efi` 对号入座放入 ESP 分区中

   注意⚠️: 与 Clover 不同, OpenCore 的 BOOTx64.efi 和 OpenCore.efi 是两个不同的文件

   添加 UEFI 引导项的时候应当添加 `BOOTx64.efi` 而不是 `OpenCore.efi`!

2. 我们先 **「删除」** 一些不需要的文件

   - drivers 文件夹下

     - AppleUsbKbDxe.efi

       > 这个驱动是给使用模拟 UEFI 的老主板在 OpenCore 界面正常输入用的, 请勿在 Ivy Bridge (3 代酷睿)及以上的主板上使用 ([详见 vit9696 的解释](https://applelife.ru/threads/opencore-obsuzhdenie-i-ustanovka.2944066/page-176#post-856653))

     - NvmExpressDxe.efi

       > 用于在 Haswell (4 代酷睿) 或更老的主板上支持 NVMe 硬盘, 新主板不需要

     - XhciDxe.efi

       > 用于给 Sandy Bridge (2 代酷睿) 或更老的主板上支持 XHCI, 新主板不需要

     - HiiDatabase.efi

       > 用于给 Ivy Bridge (3 代酷睿) 或更老代主板上支持 UEFI 字体渲染, UEFI Shell 中文字渲染异常时使用, 新主板不需要

   - tools 文件夹下

     - BootKicker.efi

       > 调用苹果原生的引导切换 GUI, 黑苹果不支持

     - CleanNvram.efi

       > OpenCore 自带的 NVRAM 清理功能已经足够我们使用

     - GopStop.efi

       > 停止显卡 GOP, 排错时使用

     - HdaCodecDump.efi

       > 导出声卡 Codec, 可用于定制声卡, 需要时可以临时加回来

     - VerifyMsrE2.efi

       > 用于检查主板上 CFG 锁的状态

3. OpenCore 的正确文件结构如下所示

   [![Structure](%E5%B0%8F%E5%85%B5%20%E7%B2%BE%E8%A7%A3OC.assets/Structure.png)](http://7.daliansky.net/OpenCore/Structure.png)

4. 现在, 我们可以把 **AppleSupportPkg** 中必需的 `.efi` 驱动程序放入 `Drivers` 文件夹, 将 你的 `kext` 和 `DSDT/SSDT` 放入各自的文件夹中。**请注意, OpenCore 不支持支持列表以外的 UEFI 驱动程序!**

5. 完成后的效果:

   [![StructureFilled](%E5%B0%8F%E5%85%B5%20%E7%B2%BE%E8%A7%A3OC.assets/StructureFilled.png)](http://7.daliansky.net/OpenCore/StructureFilled.png)

####  配置思路

OpenCore 的配置建议遵循从简原则, 先确保能进系统再来增加其它功能 (不进系统功能都是免谈), 变数越少出错的概率也就越低

- 内核驱动只保留 `Lilu`, `WhateverGreen`, `VirtualSMC` 和必须依赖才能开机的驱动 (删除 AppleALC, VoodooI2C 等功能性驱动)

###  配置 Config.plist

- 请记住 OpenCore 中的 config.plist, 与 Clover 的 config.plist 尽管名称相同但是内容和结构完全不同。**它们不能混合滥用**。
- 如果你不清楚某一个项里应该填什么「数据类型」, 请参考 [OpenCore 官方文档](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs)
- 下面的 `Quirk` 给的都是推荐值, 请根据自身情况修改

> 部分未在文中提及的 Quirk 和 参数保留原样

1. 复制 `Sample.plist`, 将副本重命名为 `config.plist` 并用上文提到的编辑工具打开

   - **如果你打算使用的 SMBIOS 苹果已经停止支持 或者你是用的是戴尔 OEM 笔记本**
   - **请使用 SampleFull.plist 并「认真」补全所有 SMBIOS 信息**

   [![ConfigSample](%E5%B0%8F%E5%85%B5%20%E7%B2%BE%E8%A7%A3OC.assets/ConfigSample.png)](http://7.daliansky.net/OpenCore/ConfigSample.png)

2. 里面`躺`着所有我们需要配置的子项:

   - `ACPI`: 用于加载, 屏蔽和修补 ACPI (DSDT/SSDT) 表
   - `Booter`: 用于设置 FwRuntimeServices.efi (Slide 值计算, KASLR)
   - `DeviceProperties`: 用于设置 PCI 设备属性, 如英特尔缓冲帧补丁, 声卡 Layout ID
   - `Kernel`: 用于说明 OpenCore 的具体加密信息, 配置 Kext 加载顺序以及屏蔽驱动
   - `Misc`: OpenCore 的自身设置
   - `NVRAM`: 用于注入 NVRAM (如引导标识符和 SIP)
   - `Platforminfo`: 用于设置 SMBIOS 机型信息
   - `UEFI`: 用于加载 UEFI 驱动以及以何种顺序加载

   **#WARNING - 1～5** 是开发者的嘱咐和对 Sample 选择的说明, ***在删除之前请务必确保你已经阅读并遵守了这些条例***

   > **Warning - 1:** 这只是一个实例。不要直接拿来开机。
   >
   > **Warning - 2:** 在用 Config 开机之前, 确保你已经理解 Config 内「每一个项的」的意义
   >
   > **Warning - 3:** 在大多数情况下, 建议从 Sample.plist 开始配置
   >
   > **Warning - 4:** SampleFull.plist 用于苹果不再支持的机型 (2011 年或更早)

###  ACPI

**Add:**

- 你需要把这些而例子里内容修改/创建为你的 `EFI/OC/ACPI/` 下的文件

------

**Block:**

- 禁用某个 ACPI 表, 常用于禁用 `DAMR` 来关闭 VT-d

------

**Patch:**

- 对 DSDT (SSDT) 的内容进行查找和替换

------

**Quirks:** ACPI 相关设置

- FadtEnableReset:

  ```
  NO
  ```

  - 在旧硬件上修复重启和关机, 除非需要, 否则不推荐开启

- NormalizeHeaders:

  ```
  NO
  ```

  - 清除 ACPI 头字段, 只有 macOS 10.13 需要

- RebaseRegions:

  ```
  YES
  ```

  - 尝试试探性地重新定位 ACPI 内存区域, **使用自定义 DSDT 则必须开启**

- ResetHwSig:

  ```
  NO
  ```

  - 存在重新启动后因无法维持硬件签名而导致从休眠中唤醒的问题的硬件需要开启

- ResetLogoStatus:

  ```
  NO
  ```

  - 无法在有 `BGRT` 表的系统上显示 OEM Windows 标志的硬件需要开启

###  Booter 启动器

**MmioWhitelist:** 如果开机卡在 `PCI...` 可以尝试开启 Item 1 下的 Patch

------

**Quirks:**

- **AvoidRuntimeDefrag:** `YES`

  - 开启后会修复 UEFI 的运行服务, 例如日期, 时间, NVRAM, 电源控制等

- **DevirtualiseMmio:** `YES`

  - 开启后会**减少 Stolen 内存占用空间**，扩大 `Slide = N` 值的范围, 适用于大多数主板
  - 

- **DisableSingleUser:** `NO`

  - 开启后会禁止 `Cmd + S` 和 `-s` 的使用，使设备更加接近于 T2 白苹果

- **DisableVariableWrite:** `NO`

  - 开启后会禁止 NVRAM 写入, 在 Z390/HM370 等没有原生 macOS 支持 NVRAM 的主板上需要开启

- **DiscardHibernateMap:** `NO`

  - 开启后会重用原始休眠内存映射，仅某些旧硬件需要

- **EnableSafeModeSlide:** `YES`

  - 开启后会允许在安全模式下使用 Slide 值

- **EnableWriteUnprotector:** `YES`

  - 开启后会在执行期间删除 CR0 寄存器中的写入保护

- **ForceExitBootServices:** `NO`

  - 开启后会确保 ExitBootServices 即使在 MemoryMap 发生更改时也能调用成功, 除非有必要, **否则请勿使用**

- **ProtectCsmRegion:** `NO`

  - 开启后会用于修复人为制造和睡眠唤醒的问题, AvoidRuntimeDefrag 已经修复了这个问题所以请尽可能避免使用这个 Quirk

- **ProtectSecureBoot:** `NO`

  - 避免操作系统对 UEFI 安全启动变量 (`db`, `dbx`, `PX`, `KEK`) 进行写入, 这个选项主要用于避免 Insyde 主板和 MacPro5,1 的 NVRAM 问题

- **ProvideCustomSlide:** `YES`

  - 如果 Slide 值存在冲突, 此选项将强制 macOS 执行以下操作:

    > 使用一个伪随机值。 只有在遇到 `Only N/256 slide values are usable!` 时需要

- **SetupVirtualMap:** `YES`

  - 开启后会将 SetVirtualAddresses 调用修复为虚拟地址

- **ShrinkMemoryMap:** `NO`

  - 有巨大且不兼容内存映射的主板需要开启, 非必须不要使用

- **SignalAppleOS:** `NO`

  - 通过 OS Info 将 macOS 加载的信息报告给其它操作系统, 用于在 Windows 中为 MacBook 启用 iGPU
  
- ？？？？RebuildAppleMemoryMap

  - ??????????????

###  DeviceProperties 设备属性

**不同的设备硬件地址不一样! 你需要先通过 Hackintool 或者 Windows 设备管理器 等工具查看 PCI 设备地址!**

> 此处内容可以用 Hackintool 生成然后直接复制过来, 请提前确保内容和结构无误

------

**Add:** 添加 `_DSM` 设备属

【**=====？？？？？？？**】

```
PciRoot(0x0)/Pci(0x2,0x0)
```

- 注入缓冲帧补丁, 查看 [此处](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.cn.md#使用-weg-自定义-fb-和-端口-补丁)来确认你需要的缓冲帧补丁

```
PciRoot(0x0)/Pci(0x1f, 0x3)` → `Layout-id
```

- 应用 AppleALC 音频 Layout ID 注入

------

**Block:** 用于删除/屏蔽设备属性 (可以删除此项, 大多数情况都用不到)

- 这里的设置等同于 Clover 里的 ACPI 重命名 **`_DSM` → `XDSM` + `TgtBridge`**

###  Kernel 内核

这里是指定要加载哪些 Kext 以及仿冒 CPU ID 的地方, 这里的顺序**非常重要**, 所以请确保 `Lilu.Kext` 始终在第一位! 其他优先级更高的 Kext 为 Lilu 的插件, 如 VirtualSMC, AppleALC, WhateverGreen 等

**Add:**

- **BundlePath**

  - 这里填入 Kext 的名称
  - 如: `Lilu.kext`

- **Enabled**

  - 控制 Kext 的启用禁用

- **ExecutablePath**

  - 隐藏在 kext 中的实际可执行文件的路径，可以通过右键单击并选择 `显示包内容` 来查看 Kext 的路径。 通常为 `Contents/MacOS/(Kext)`，但有的时候 `Plugin` 文件夹下也会有 Kext。

  - 如: `Contents/MacOS/Lilu`

    > 空壳 Kext 没有可执行文件 (e.g. USBPorts.kext), 此项留空即可

- **PlistPath**

  - 隐藏在 Kext 中的 `Info.plist` 路径
  - 如: `Contents/Info.plist`

> VoodooI2C 注入样板 [🚪](https://github.com/daliansky/OC-little/blob/master/22-常见驱动加载顺序/config-4-I2C驱动列表.plist)
>
> VoodooPS2 注入样板 [🚪](https://github.com/daliansky/OC-little/blob/master/22-常见驱动加载顺序/config-2-PS2键盘驱动列表.plist)

------

**Emulate:** 仿冒不支持的 CPU, 例如奔腾和赛扬, 或者在不支持的 CPU 上启用 XCPM

- CpuidData:

   设置为零时，将使用原始 CPU 位

  - ` | 00 00 00 00 | 00 00 00 00 | 00 00 00 00`
  - 例如: CPUID `0x0306A9` 就是 `A9 06 03 00 | 00 00 00 00 | 00 00 00 00 | 00 00 00 00`

- CpuidMask:

   CPU 的被仿冒位

  - `FF FF FF FF | 00 00 00 00 | 00 00 00 00 | 00 00 00 00`
  - 如果需要替换更长的位, 将 `00` 替换为 `FF`

------

**Block:** 屏蔽系统里的 Kext

------

**Patch:** 这是你要添加系统内核补丁, Kext 补丁, 和 AMD CPU 补丁的地方。(等同于 Clover 的 KextToPatch 和 KernelToPatch)

------

**Quirks:**

- AppleCpuPmCfgLock:

  ```
  NO
  ```

  - 如果设备的 CFG-Lock 是开启的状态则需要设置为 `YES` (尽可能用 Grub 关闭 BIOS 的 CFG-Lock 并避免开启这个 Quirk)

- AppleXcpmCfgLock:

  ```
  NO
  ```

  - 同上

- AppleXcpmExtraMsrs:

  ```
  NO
  ```

  - 禁用奔腾和某些至强等不支持 CPU 所需的多个 MSR 访问

- AppleXcpmForceBoost:

  ```
  NO
  ```

  - 强制拉高睿频, 建议在长期高负载的专业设备上使用, 至强系列的处理器开启这个选项会受益

- CustomSMBIOSGuid:

  ```
  NO
  ```

  - 对 UpdateSMBIOSMode 自定义模式执行 GUID 修补, 用于戴尔笔记本电脑 (等同于 Clover 的 `DellSMBIOSPatch`)

- DisbaleIOMapper:

  ```
  NO
  ```

  - 需要绕过 VT-d 且 BIOS 中禁用时使用

- DummyPowerManagement:

  ```
  NO
  ```

  - 禁用 AppleIntelCpuPowerManagement 原生电源管理, 用于更好的替代 NullCpuPowerManagement.kext

- **ExternalDiskIcons:**

  - 作为外接硬盘

  ```
  YES
  ```

  - 硬盘图标补丁, macOS 将内部硬盘视为外接硬盘 (黄色) 时使用

- IncreasePciBarSize:

  ```
  NO
  ```

  - 将 IOPCIFamily 中 32 位 PCI Bar 的大小从 1GB 增加到 4GB, 在 BIOS 中启用 Above4GDecoding 是一种更加干净和安全的方法。某些 X99 板可能需要开启, 这些主板通常会在 IOPCIFamily 上遇到内核崩溃

- LapicKernelPanic:

  ```
  NO
  ```

  - 禁用由 AP 核心 lapic 中断造成的内核崩溃, 通常用于「惠普电脑」 (等同于 Clover 的 `Kernel LAPIC`)

- PanicNoKextDump:

  ```
  YES
  ```

  - 在发生内核崩溃时阻止输出 Kext 列表, 提供可供排错参考的崩溃日志, 排错时请务必开启

- PowerTimeoutKernelPanic:

  - 电源

  ```
  YES
  ```

  - 修复 macOS Catalina 中由于设备电源状态变化超时而导致的内核崩溃

- ThirdPartyDrivers:

  ```
  NO
  ```

  - 为 SSD 启用 TRIM 指令, NVMe SSD 会自动被 macOS 加载因此不需要, SATA SSD 可以在终端执行 `sudo trimforce enable` 开启
  - 同时修复 macOS 10.15 下非苹果原厂 SSD 无法使用硬盘休眠 (`hibernatemode 25`)

- XhciPortLimit:

  - ？？？？？？？？？？？？？
  
  ```
YES
  ```
  
  - 这实际上是 15 端口限制补丁, 不建议依赖, 因为这不是 USB 的最佳解决方案。有能力的情况下请选择定制 [USB](https://blog.daliansky.net/Intel-FB-Patcher-tutorial-and-insertion-pose.html#定制usb), 这个选项适用于没有定制 USB 的设备



### Misc 杂项

**BlessOverride:** 用于覆盖 Windows `bootmgfw.efi` 的位置以便识别 Windows 引导项, OpenCore 和 Windows 的引导文件在同一硬盘的同一 ESP 分区下使用

```

▼ Misc                  <Dictionary>
|__ ▼ BlessOverride     <Array>
    |__ Item 0          <String>          \EFI\Microsoft\Boot\bootmgfw.efi
```

**Boot:** 引导界面的设置

- **HibernateMode:** `None`

  > 与系统内的休眠模式 (hibernatemode 25) 配合, 引导进系统会还原休眠前的状态, 这个功能会影响 SSD 寿命, 建议关闭

  - `None`: 关闭休眠支持
  - `Auto`: 自动检测 RTC 和 NVRAM 模式
  - `RTC`: RTC 模式

- **HideAuxiliary:** `YES`

- ???????????????

  - 默认隐藏以下引导项, 按空格键显示全部
    - macOS 恢复
    - 在自定义引导项时定义为 `Auxiliary` 的引导项
    - 在 **Tools** 中添加的操作系统 (如: `Clean NVRAM`)

- **HideSelf:** `NO`

  - 在 OpenCore 的启动选择中隐藏自身 EFI 分区内的其它启动项

- **PickerAttributes:** `0x00`

  > 给 OpenCore 自带的引导选择界面`添加特效`, 食用方法为填入字体颜色和背景颜色的值的 16 进制之和

  - 字体颜色
    - `0x00`: 黑
    - `0x01`: 蓝
    - `0x02`: 绿
    - `0x03`: 青
    - `0x04`: 红
    - `0x05`: 品红
    - `0x06`: 棕
    - `0x07`: 浅灰
    - `0x08`: 深灰
    - `0x09`: 浅蓝
    - `0x0A`: 浅绿
    - `0x0B`: 浅青
    - `0x0C`: 浅红
    - `0x0D`: 浅品红
    - `0x0E`: 黄
    - `0x0F`: 白
  - 背景颜色
    - `0x00`: 黑
    - `0x10`: 蓝
    - `0x20`: 绿
    - `0x30`: 青
    - `0x40`: 红
    - `0x50`: 品红
    - `0x60`: 棕
    - `0x70`: 浅灰

- **PickerAudioAssist:** `NO`

- 设置为 `YES` 时会朗读屏幕上选择项的内容, 需要提提前在 EFI 中放入音频文件并正确设置下文的 **Audio** 章节

- **PollAppleHotKeys:** `YES`

  - ？？？？？？？

  - 设置为 

    ```
    YES
    ```

     后允许在引导过程中使用苹果原生快捷键, 需要与 Quirk 

    ```
    KeySupport=Yes
    ```

     结合使用, 具体体验取决于主板固件。快捷键组合:

    - `Cmd + V`: 启用 `-v` 跑码
    - `Cmd + Opt + P + R`: 重置 NVRAM
    - `Cmd + R`: 启动恢复分区
    - `Cmd + S`: 启动至单用户模式
    - `Option / ALT`: 在 `ShowPicker` 设置成 `NO` 时显示引导项选择界面, `ALT` 不可用时可用 `ESC` 键代替
    - `Cmd + C + 减号`: 关闭主板兼容性检查, 等同于添加引导标识符 `-no_compat_check`
    - `Shift`: 安全模式

- **ShowPicker:** `YES`

  - 显示 OpenCore 的 UI, 用于查看可用引导项
  - **设置为 `NO` 可以跳过倒计时**, 和 `PollAppleHotKeys=Yes` 配合快捷键可以大幅提升体验

- **TakeoffDelay:** `0`

  - 在启动前延迟 `n` 毫秒, 提升键盘快捷键识别的正确率。`n` 的有效范围为大于 `5000 ~ 10000`, 32bit 以内的正整数

- **Timeout:** `0`

  - 设置引导项等待时间, 单位`秒`
  - `0` 为关闭倒计时, 相当于 Clover 的 `-1` (并不是跳过倒计时)

- **PickerMode:** `Builtin`

  - `Builtin`

    > 使用 OpenCore 自带的简单 UI

  - `External`

    > 使用其它 GUI

  - `Apple`

    > 使用苹果原生的 GUI, 黑苹果不支持

------

**Debug:** Debug 有特殊用途, 除非你知道你在做什么, 否则保持原样, 具体参考官方文档

- **DisableWatchDog:** `NO`
  - 如果 macOS 在启动时卡在某些地方, 可能需要设置为 `YES`, 通常用于排除错误干扰
- **DisplayDelay:** `0`
- **DisplayLevel:**
  - `2147483714`: 在屏幕上显示所有 Debug 信息
  - `0`: 隐藏所有 Debug 信息
- **Target:**
  - `0`: 关闭日志记录
  - `3`: 允许屏幕输出日志
  - `19`: 允许屏幕输出 UEFI 变量日志
  - `65`: 在 ESP 分区根目录生成日志文件 `opencore-YYYY-MM-DD-HHMMSS.txt`, 但屏幕上不显示日志

------

**Security:** 安全

- **AllowNvramReset:** `YES`

- ?????

  - 允许在引导选择界面和快捷键 `Cmd + Opt + P + R` 按下时重置 NVRAM

- **AllowSetDefault:** `YES`

- ????

  - 允许使用 `CTRL + 回车` 和 `CTRL + 数字` 锁定默认启动项

- **AuthRestart:** `NO`

  - 允许重启 FileVault2 分区时不用再次输密码, 有安全风险

- **ExposeSensitiveData:**

  - `3` 将 OpenCore 的启动路径和版本储存进 NVRAM
  - `11` 在 `3` 的基础上添加主板 OEM 信息, [HWMonitorSMC2](https://github.com/CloverHackyColor/HWMonitorSMC2) 和 [NVMeFix](https://github.com/acidanthera/NVMeFix) 需要主板 OEM 信息才能正常工作

- **Vault:** `Optional`???????Secure

  > OpenCore 自身的加密和安全保护, 具体参考官方文档

  - `Optional`

    > 不强制要求 `vault.plist` 和 `vault.sig` 文件存在, 但是会在其中任意文件存在时依旧执行验证, 不安全

  - `Basic`

    > 强制要求 `vault.plist` 存在, 开机时会根据里面的内容核对 `OC` 目录下的文件, 防止在 EFI 文件系统崩溃后依旧启动

  - `Secure`

    > 强制要求 `vault.sig` 签名文件 和 `vault.plist` 存在, 用于和 UEFI 安全启动配合

- **ScanPolicy:** `0`

  - `0`: 允许扫描所有可用的硬盘
  - 其它值请参考官方文档, 对给出的选项做 16 进制加法运算

------

**Tools** 用于运行 OC 调试工具, 例如验证 CFG 锁 (VerifyMsrE2)

- Arguments

  - 传递的参数

- Auxiliary:

  ```
  YES
  ```

  - `YES` 默认隐藏

- Name

  - OpenCore 启动项中显示的名称

- Enabled

  - 启用或禁用

- Path

  - `Tools` 文件夹下的文件名
  - 如: [VerifyMsrE2.efi](https://github.com/acidanthera/AppleSupportPkg/releases)

------

**Entires:** 用于指定 OpenCore 无法自动找到的无规律引导路径

- Arguments

  - 传递的参数

- Auxiliary:

  ```
  NO
  ```

  - `YES` 默认隐藏

- Name

  - OpenCore 启动项中显示的名称

- Enabled

  - 启用或禁用

- Path

  - 引导磁盘的 PCI 路径，可以通过 [OpenCoreShell](https://github.com/acidanthera/OpenCoreShell) 的 `map` 命令找到
  - 如: `PciRoot(0x0)/Pci(0x1D,0x4)/Pci(0x0,0x0)/NVMe(0x1,09-63-E3-44-8B-44-1B-00)/HD(1,GPT,11F42760-7AB1-4DB5-924B-D12C52895FA9,0x28,0x64000)/\EFI\Microsoft\Boot\bootmgfw.efi`

###  NVRAM

**Add:**

- **[`4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14`](https://github.com/erikberglund/AppleNVRAM/blob/master/Apple/4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14.md)**

  - UIScale:

     OpenCore UI 和引导第一阶段缩放

    - `01:` 正常大小
    - `02:` HIDPI (使 FileVault 和 苹果标志 在原生 HIDPI 显示器上以正常大小显示)

  - DefaultBackgroundColor:

     默认背景颜色

    - 控制第一阶段苹果标志后面的背景颜色, `加特技`
    - 格式为 16 进制颜色 RGBA: `RR GG BB AA`, 可以随意调整
    - 标准色:
      - 黑: `00 00 00 00`
      - 灰: `BF BF BF 00`

- **[`7C436110-AB2A-4BBB-A880-FE41995C9F82`](https://github.com/erikberglund/AppleNVRAM/blob/master/Apple/7C436110-AB2A-4BBB-A880-FE41995C9F82.md)**

  - boot-args:

    ```
    -v debug=0x100
    ```

    - 引导标识符, 更多请查看 [附录[3\]](https://blog.daliansky.net/OpenCore-BootLoader.html#附录3----常用内核引导标识符合集)

  - csr-active-config:

    ```
    <0x00000000>
    ```

     (SIP 设置)

    - `0x00000000` - SIP 完全开启
    - `0x30000000` - 允许未签名的 Kext 加载并允许写入受保护的文件系统路径
    - `0xE7030000` - SIP 完全关闭
    - `0x67000000` - 不再推荐使用

  - nvda_drv:

    ```
    <>
    ```

    - 设为 `31` 时启用 NVIDIA WebDrivers
    - 没有 N 卡可以删除此项

  - **prev-lang:kbd:** `<0x7a682d48616e733a323532>` (将默认语言设置为简体中文, 留空为英文)

  - SystemAudioVolume:

    ```
    0x46
    ```

    - `0x80` 为静音

------

**Block:** 强制重写 NVRAM 变量, 因为 `Add` 不会覆盖 NVRAM 中已经存在的值, 所以需要用 Block 删除原来的值 (如: `boot-args`) 来起到刷新的作用来注入上方的 `Add`

> 具体工作原理请参考 [Acidanthera bugtracker #575](https://github.com/acidanthera/bugtracker/issues/575#issuecomment-554522261)

------

**LegacyEnable:** 允许从 `nvram.plist` 中读取 NVRAM 变量

- 没有原生 NVRAM 的设备设置为 `YES`
- macOS 下硬件 NVRAM 工作「不」正常的设备设置为 `YES`
- macOS 下硬件 NVRAM 工作正常的设备设置为 `NO`

------

**LegacyOverwrite:** `NO`

- 允许 `nvram.plist` 中的变量覆盖现有 NVRAM 的变量
- 模拟 NVRAM 需要开启

------

**LegacySchema:**

- 选择注入的 NVRAM 变量, 其它未被包含的变量即使存在于 `nvram.plist` 也不会被注入, 与 LegacyEnable 配合使用

------

**WriteFlash:**

- 在有些有 bug 的 UEFI 固件上, 系统运行时不能将易失性变量转换为非易失性变量, 这个选项提供的是一个解决方案, 并没有增加一个新功能

###  PlatformInfo 机型信息

**Auto:** `YES` (基于 Generic 部分生成 PlatformInfo, 而不是 DataHub, NVRAM 和 SMBIOS 部分)

> OpenCore 提供了大量方法来注入 PlatformInfo, 这里只介绍最简单的自动注入法

------

**Generic:**

- AdviseWindows

  ```
  YES
  ```

  - 启用来允许重启到一个 ESP 分区不是磁盘中第一个分区的磁盘中的 Windows

- SpoofVendor:

  ```
  YES
  ```

  - 仿冒制造商为 Acidanthera 来避免出现冲突

- SystemUUID:

  - 填入设备的硬件 UUID 以免造成 Windows 和其它软件的激活问题 (官方不再建议留空)

- MLB:

  - 用 macserial 读取或生成

- ROM:

  ```
  <0x############>
  ```

  - 可以是任意 6 Byte MAC 地址, 如 `0x112233000000`

- SystemProductName:

  - 用 macserial 读取或生成

- SystemSerialNumber:

  - 用 macserial 读取或生成

------

**UpdateDataHub:** `YES` (更新 DataHub)

------

**UpdateNVRAM:** `YES` (更新 NVRAM)

------

**UpdateSMBIOS:** `YES` (更新 SMBIOS)

------

**UpdateSMBIOSMode:** `Create` (用新分配的 EfiReservedMemoryType 替换原有的表, 戴尔笔记本需要使用 `Custom` 并开启 `CustomSMBIOSGuid`)

------

####  macserial

**使用 macserial 查看原有 PlatformInfo:**

1. 打开下载好的 MacInfoPkg

2. 找到里面的 `macserial` 并将其复制到桌面

3. 终端输入: **(建议复制粘贴)**

   ```
   
   ```

1. ```
   cd ~/Desktop
   
   ./macserial | grep -w 'Model:\|Valid:\|Hardware UUID:\|ROM:\|MLB:\|Serial Number:' | sed '/ \- /d' | tr -d ' ' | sed $'s/Model:/SystemProductName:/g' | sed $'s/HardwareUUID:/SystemUUID:/g' | sed $'s/SerialNumber:/SystemSerialNumber:/g' | sed $'s/\:/\: /g'
   ```

2. 此时你现有的 PlatformInfo 就已经列出来了, 对着 Config 对号入座即可

> 如果输出的 `Valid` 不是 `Possibly` 说明这个 PlatformInfo 不符合规范, 很有可能无法激活苹果服务, 建议重新生成

------

**使用 macserial 生成新的 PlatformInfo:**

1. 如果要生成新的 PlatformInfo 请输入如下命令来列出所有支持的设备型号:

   ```
   
   ```

```
cd ~/Desktop

./macserial -l | grep 'Model:' | tr -d ' ' | sed $'s/Model:/\\- /g'
```

然后输入如下命令来生成序列号和 MLB

```

```

1. ```
   ./macserial -m <设备型号>
   ```

   > 请忽略 `<>`, 例子: `./macserial -m MacBookPro15,3`

   输出的结果中, **`|`** 前面的是 `序列号 SystemSerialNumber`, 后面的是 `MLB`

###  UEFI

**ConnectDrivers:** `YES`

- 强制加载 `.efi` 驱动程序, 更改为 `NO` 将自动连接 UEFI 驱动程序, 这样以获得更快的启动速度, 但并非所有驱动程序都可以自行连接, 某些文件系统驱动程序可能无法加载

------

**Drivers:** 在这里添加你的 `.efi` 驱动

------

**Audio:**

> 这里的声卡设置只与 UEFI 环境相关, 不影响系统内音频, 但是需要确保系统内声卡正常工作, 这里填的部分值需要从系统内的 IOReg 提取

- AudioSupport:

  ```
  YES
  ```

  - `YES` 启用音频支持

- AudioDevice:

  - 声卡设备的 PCI 地址 `PciRoot(0x0)/Pci...`
  - 可以在 macOS 下运行 `gfxutil -f HDEF` 获得

- AudioCodec:

  ```
  0
  ```

  - 声卡的 Codec 地址, 可以在 IORegistryExplorer 的 `HDEF` → `IOHDACodecDevice` → `IOHDACodecAddress` 中获得, 一般是 `0x0`
  - 直接搜索 `IOHDACodecDevice` 可以大幅节约时间

- AudioOut:

  - 指定的输出设备, 最简单的方法就是从 `0 到 声卡总输出数` 每个都试一遍
  - `声卡的总输出数` 可以在 Debug 版本的 OpenCore 日志中获得 `(n Outputs)`

- MinimumVolume:

  - 输出最小音量, 有效值为 `0 ~ 100`
  - 如果下方 `VolumeAmplifier` 公式计算出的最小音量小于这里给出的值 `Duang` 就不会被播放

- PlayChime:

  ```
  YES
  ```

  - `YES` 播放 `Duang`

- VolumeAmplifier:

  ```
  1000
  ```

  - `音量在原有基础上放的百分比`
  - 有效范围 `0 ~ 1000`

> 如果要让 Duang 和 VoiceOver 等其它音频功能工作, 需要额外下载语音资源包并放置于 `ESP(分区)/EFI/OC/Resources/Audio` 文件夹下, [下载地址](https://github.com/acidanthera/OcBinaryData/tree/master/Resources/Audio) (注: 使用 `git clone --depth=1` 速度最快)。
>
> 同时 [AudioDxe](https://github.com/acidanthera/OpenCorePkg) 也需要安装在 `Drivers` 文件夹中并通过 config 注入

------

**Input:**

- KeyForgetThreshold:

  ```
  5
  ```

  - 按住按键后每个键之间的时间间隔 (单位: 毫秒)

- KeyMergeTheshold:

  ```
  2
  ```

  - 按住按键被重置的时间间隔 (单位: 毫秒)

- KeySupport:

  ```
  YES
  ```

  - 开启 OC 的内置键盘支持
  - 使用 AppleUsbKbDxe.efi 请设置为 `NO`

- KeySupportMode:

  ```
  Auto
  ```

   键值转换协议模式

  - `V1`: UEFI 旧版输入协议
  - `V2`: UEFI 新输入协议
  - `AMI`: APTIO 输入协议

- KeySwap:

  ```
  NO
  ```

  - 交换 Command 和 Option 键

- PointerSupport:

  ```
  NO
  ```

  - 修复 UEFI 选择器协议

- **PointerSupportMode:** `留空`

- TimerResolution:

  ```
  50000
  ```

  - 固件时钟刷新的频率 (单位: 100纳秒)
  - 华硕主板为自己的界面使用 `60000`
  - 苹果使用 `100000`

------

**Output:**

- **TextRenderer:** `BuiltinGraphics`

  > 文字渲染模式

  - `BuiltinGraphics`: 使用 OpenCore 内置文字渲染的图形模式, 并同时启用 OpenCore 自带的控制台管理, **支持 HIDPI 和全屏范围显示, 通常效果胜于下面的选项**
  - `SystemGraphics`: 使用主板固件自带文字渲染的**图形**模式, 并同时启用 OpenCore 自带的控制台管理
  - `SystemText`: 使用主板固件自带文字渲染的**文字**模式, 并同时启用 OpenCore 自带的控制台管理
  - `SystemGeneric`: 使用主板固件自带文字渲染的**文字**模式和自带的控制台管理

- **ConsoleMode:** `留空`

  > 控制台模式

  - 大多数固件上 `留空` 是最好的选择
  - `Max` 会尽可能使用最大的 ConsoleMode
  - **TextRendere** 使用 `Builtin` 时此项设置不生效

- **Resolution:** `Max`

  - `宽x高@Bpp` (如: `3840x2160@32`), 注意 `Bpp` 不是刷新率, 而是显示位深 (bits per pixel), 如果不清楚可以不填
  - `宽x高` (如: `3840x2160`) 也是支持的
  - `留空` 将不会改变 UEFI 的默认分辨率
  - `Max` 将会尝试开启最大分辨率

  > 这个选项在固件没有 GOP 时会失效

- **ClearScreenOnModeSwitch:** `NO`

- 从图形模式切换到文本模式时, 某些固件仅清除屏幕的一部分, 导致屏幕上残留之前绘制的图片。 此选项会在切换到文本模式之前用黑色填充整个屏幕

- **DirectGopRendering:** `NO`

  - 开启后会直接使用 OpenCore 内置的 GOP 渲染控制台
  - 这个选项主要用于提升或修复 MacPro5,1 的渲染问题
  - 除非开启后能明显感觉到有改进否则不要开启

- **IgnoreTextInGraphics:** `NO`

- 修复不用 `-v` 开机时日志覆盖苹果标志输出的问题

- **ReplaceTabWithSpace:** `NO`

- 取决于固件, 某些设备在 UEFI Shell 中编辑文件使用 `Tab键` 出问题时启用。注意, 此选项只作用于 **TextRenderer:** `System...` (主板固件自带) 的文字渲染

- **ProvideConsoleGop:** `YES`

  - macOS 引导加载程序要求 GOP (图形输出协议) 存在于控制台句柄上
  - 大部分的笔记本都不提供 GOP, 台式机的独立显卡可以单独刷入 GOP
  - 如果选择启动项之后不出现 macOS 启动 Verbose 请启用
  - 开启此选项能最大可能保证 OpenCore UI 和 苹果标志 以正确分辨率显示

- **ReconnectOnResChange:** `NO`

- 有些固件在 GOP 分辨率改变后要求重新连接控制器才能输出文本, 开启这个选项会导致从 UEFI Shell 中启动 OpenCore 时直接黑屏, 尽量避免开启

- **SanitiseClearScreen:** `YES`

  - 修复 OpenCore 在高分屏中以 1024x768 显示的问题, 注意要同时开启带有 OpenCore 自带的控制台管理 (`ConsoleControl`) 的 **TextRenderer** (前三种) 并将 `ConsoleMode` 的内容 **「留空」**

------

**Protocols:** (协议)

- AppleAudio:

  ```
  YES
  ```

  - 安装 Apple Audio 协议以在 OpenCore 中使用 VoiceOver, 目前该协议支持在 macOS 10.13 及以上播放音频, 老系统使用的 AppleHDA 协议暂未支持

- AppleBootPolicy:

  ```
  NO
  ```

  - 用于确保虚拟机或旧白苹果上兼容 APFS

- AppleEvent:

  ```
  NO
  ```

  - 用于在虚拟机或旧白苹果上兼容文件保险箱

- AppleImageConversation:

  ```
  NO
  ```

  - 重新安装 Apple Image Conservation 协议

- AppleKeyMap:

  ```
  NO
  ```

  - 重新安装 Apple Key Map 协议

- AppleSmcIO:

  ```
  YES
  ```

  - 重新安装 Apple SMC I/O 协议
  - **VirtualSmc.efi 已经被替代为此选项**

- AppleUserInterfaceTheme:

  ```
  NO
  ```

  - 重新安装 Apple User Interface Theme 协议

- DataHub:

  ```
  NO
  ```

  - 重新安装 Data Hub 协议

- DeviceProperties:

  ```
  NO
  ```

  - 确保在 VM 或旧白苹果上完全兼容

- FirmwareVolume:

  ```
  NO
  ```

  - 修复 Filevault 的 UI 问题, 设置为 `YES` 可以获得更好地兼容 FileVault

- HashServices:

  ```
  NO
  ```

  - 修复运行 FileVault 时鼠标光标大小不正确的问题, 设置为 `YES` 来在 Aptio IV 或更老的主板上兼容 FileVault

- OSInfo:

  ```
  NO
  ```

  - 用于为主板或者其它程序接收来自 macOS 引导工具的消息

- UnicodeCollation:

  ```
  NO
  ```

  - 一些较旧的固件破坏了 Unicode 排序规则, 设置为 `YES` 可以修复这些系统上 UEFI Shell 的兼容性 (通常为用于 IvyBridge 或更旧的设备)

------

**Quirks:**

- **ExitBootServicesDelay:** `0`
- **IgnoreInvalidFlexRatio:** `NO`
  - BIOS 中无法禁用 `MSR_FLEX_RATIO(0x194)` 时开启
- **ReleaseUsbOwnership:** `NO`
  - 从固件驱动程序中释放 USB 控制器所属权, 除非您不知道自己在做什么, 否则避免使用。Clover 的等效设置是 `FixOwnership`
- **RequestBootVarFallback:** `YES`
  - 用于修复启动项顺序
- **RequestBootVarRouting:** `YES`
  - 从 `EFI_GLOBAL_VARIABLE_GUID` 中为 `OC_VENDOR_VARIABLE_GUID` 请求 redirectBoot 前缀变量
  - 启用此项以便能够在与 macOS 引导项设计上不兼容的固件中可靠地使用 `启动磁盘` 设置
- **UnblockFsConnect:** `NO`
  - 惠普笔记本在 OpenCore 引导界面没有引导项时设置为 `YES`

------

###  解决 Clover 和 OpenCore 的冲突

**在重启进入 OpenCore 之前, 我们还需要解决一些冲突问题:**

- 删除 Clover 设置面板

  > Clover 设置面板会和 OpenCore 产生冲突, 需要删除

  - Clover 设置面板位于 `/Library/PreferencePanes/Clover.prefPane`
  - 终端输入 `sudo rm -rf /Library/PreferencePanes/Clover.prefPane` 删除

- 清理 Clover 的模拟 NVRAM RC 脚本 和 守护程序 `CloverDaemonNew`

  > 在终端中输入:

  ```
  
  ```

- ```
  # 删除 ESP 分区下的 nvram.plist
  rm -rf /Volumes/(你的 ESP 分区)/nvram.plist
  
  # 删除 RC 脚本
  rm -rf "/etc/rc.clover.lib"
  rm -rf "/etc/rc.boot.d/10.save_and_rotate_boot_log.local"
  rm -rf "/etc/rc.boot.d/20.mount_ESP.local"
  rm -rf "/etc/rc.boot.d/70.disable_sleep_proxy_client.local.disabled"
  rm -rf "/etc/rc.boot.d/80.save_nvram_plist.local"
  rm -rf "/etc/rc.shutdown.local"
  rm -rf "/etc/rc.boot.d"
  rm -rf "/etc/rc.shutdown.d"
  
  # 删除 Clover 新开发的 NVRAM 守护程序 `CloverDaemonNew`
  launchctl unload '/Library/LaunchDaemons/com.slice.CloverDaemonNew.plist'
  rm -rf '/Library/LaunchDaemons/com.slice.CloverDaemonNew.plist'
  rm -rf '/Library/Application Support/Clover/CloverDaemonNew'
  rm -rf '/Library/Application Support/Clover/CloverLogOut'
  rm -rf '/Library/Application Support/Clover/CloverWrapper.sh'
  ```

- 重置「硬件」NVRAM

  > 为了尽可能减少问题出现的概率, 建议在 Clover 和 OpenCore 之间切换时重置「硬件」NVRAM

  - 通过 Clover F11 清除

    1. 删除 Clover 内的 `EmuVariableUefi.efi` (通常位于 `Drives/UEFI/` 或 `drivers64UEFI/` 目录下)

       > 如果不删除 `EmuVariableUefi.efi`, Clover 只会重置模拟出来的原本就是空的 NVRAM

    2. 重启进入 Clover 引导项选择界面然后按下 `F11` 或 `Fn+F11` 直到设备自动重启

1. 开机直接选择 UEFI 引导项进入 OpenCore

- 通过 OpenCore 重置

  > 参考下文

### 现在是时候开机见分晓了

[![Success](%E5%B0%8F%E5%85%B5%20%E7%B2%BE%E8%A7%A3OC.assets/Success.png)](http://7.daliansky.net/OpenCore/Success.png)

##  完善与优化

这里只简单写了一些网上暂时还找不到中文解决方案的一些问题, 强烈推荐在看这里的同时参阅 [xjn 大佬的博客](https://blog.xjn819.com/?p=543)

###  排除常见错误

**遇到如下问题可以尝试这样设置:**

1. 进入 OpenCore 时卡在 `no vault provided!`:

   - `Misc → Security`
     - `Vault` = `Optional`

2. 选完启动项后卡在 `EndRandomSeed` 或直接卡死

   - 如果等一会能进入系统但是不跑码「或」不显示第一阶段的苹果标志:

     - `UEFI → Quirks`
       - `ProvideConsoleGop` = `YES`

   - 如果完全卡死:

     - 尝试 `Misc → Boot` 下 `PollAppleHotKeys` = `NO`, `UEFI → Input` 下 `KeySupport` = `NO`, 并换用 `AppleUsbKbDxe.efi`

     - 这个设备的 CFG 很可能有锁, 如下方案 

       「二选一」

       1. 解锁 CFG (建议), [参考 xjn 教程中的 `解锁BIOS中的CFG功能` 小节](https://blog.xjn819.com/?p=317)

       2. `Kernel → Quirks` 下 `AppleCpuPmCfgLock` = `YES`, `AppleCpuPmCfgLock` = `YES`

          > 这个方法的代价是丧失原生电源管理, 「不」建议长期使用

3. 卡在 `PCI Configuration Begin`

   - BIOS 关闭 CSM 并设置操作系统模式为 **Win8.1/10**

   - 依次尝试如下引导标识符

     - npci=0x2000
     - npci=0x3000

   - 如果依旧不行, 将 **`Booter → MmioWhitelist → Item 1 (Generic...)`** 下的 `Enabled` 改为 `True`

     > 4 代 CPU 主板可以尝试同时开启 `Item 0` 下的补丁

4. 开机出现 `Failed to parse real field of type 1`

   > 这是因为 OC 本身不支持 `real` 类型的数据, Xcode 在编辑较长的数字的时候会自动将 `Integer` 转换为 `real`

   1. 用 VS Code 打开 config.plist
   2. Command + F 查找替换所有 `real` 为 `Integer`

5. 开机卡在 `This version of Mac OS X is not supported`

   - 检查 Config 内 PlatformInfo 是否填写无误
     - `Automatic` = `YES`
     - 如果使用的是 SampleFull, 确保所有内容都已完整填写

6. 300 系主板卡在 `apfs_module_start`

   - 较新的主板使用了新的时钟 

     ```
     AWAC
     ```

      来代替 

     ```
     RTC
     ```

     , 这个设备与 macOS 不兼容, 下面的方法 

     「二选一」

     - (推荐) 添加 [SSDT-AWAC](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/SSDT-AWAC.dsl) 来屏蔽 `AWAC` 并启动 `RTC`
     - 用 [SSDT-RTC0](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/SSDT-RTC0.dsl) 仿冒 RTC

7. 华硕或惠普关机后 BIOS 重置或被强制进入安全模式

   - 将 **`Kernel → Patch → Item 0 (com.apple.driver.AppleRTC)`** 下的 `Enabled` 改为 `YES` 来开启 AppleRTC Patch

8. UEFI 引导项选择完 OpenCore 之后直接跳回选择界面

   - 参考上文重新核对 ESP 分区中的文件夹结构, 确保 UEFI 启动项添加的是 OpenCore 的 `Bootx64.efi`

###  模拟 NVRAM

####  开始之前先在这里澄清一些被广泛误解的理论并解释一些常见问题

- **什么是 NVRAM:**

  - 非易失性随机存取存储器 (NVRAM) 是随机存取存储器 (RAM) 的一种; 在依赖主板上 CMOS 电池时, 即使断开电脑电源, 该存储器仍可以保证存储的数据不丢失
  - NVRAM 里存有大量敏感参数, 如: UEFI 安全启动的 `KEK`, `PK`, `DB` 和 `DBX`, 因此 NVRAM 需要是一种提供随机访问功能的非易失性存储器, 且需要通过特定的 [GUID](https://github.com/erikberglund/AppleNVRAM) 读写

- **没有「原生 NVRAM 」的含义:**

  - 所谓 “没有「原生」NVRAM” 实际上指的是 macOS 在 **使用 APTIO 内存修复驱动** 的环境下 NVRAM 的「写入」依旧没有被修复, 导致变量无法存入主板
  - 现在支持 UEFI 的主板都支持 NVRAM, 而且 NVRAM 在 Windows 或者 Linux 下完全正常工作
  - macOS 可以正常读取 NVRAM, 也就是说在 Windows 下给 NVRAM 中的 [`7C436110-AB2A-4BBB-A880-FE41995C9F82:bootargs`](https://github.com/erikberglund/AppleNVRAM/blob/master/Apple/7C436110-AB2A-4BBB-A880-FE41995C9F82.md) 加上 `-v` 后重启进入 macOS 会直接进入啰嗦模式
  - 「没有 原生 NVRAM」很容易误解为这个主板如同传统 `BIOS + MBR` 引导的主板一样完全不支持 NVRAM
  - **「原生 NVRAM」实际上应该理解为 macOS 可以正常读写「硬件」NVRAM**

- **没有原生或模拟 NVRAM 的影响:**

  - 无法使用偏好设置的 `启动磁盘` 为 OpenCore 指定默认引导项
  - FakeSMC 的部分 SMC Key 无法保存
  - AsusSMC.kext 会丢失键盘在上次关机时的亮度

  > 没有原生或模拟 NVRAM 完全不影响系统的正常运作, 更不存在不模拟 NVRAM 就没法开机的谬论

- **哪些主板的硬件 NVRAM 在 macOS 下不正常工作:**

  - 部分 X99 和 X299 芯片组主板
  - 之前 300 系的 NVRAM 问题已经通过 SSDT 添加 PMC 设备的方式解决

- **验证你的主板的 NVRAM 在 macOS 下是否正常工作:**

  > 有时候只参考并不能得出准确的结论, 实际试验一下更加准确

  1. 删除引导工具内的 `VariableRuntimeDxe.efi`, `EmuVariableRuntimeDxe.efi`, `EmuVariableUefi.efi`
  2. 删除 NVRAM 导出脚本 `LogoutHook`(参考下文), `RC Script`(参考上文)
  3. 删除 EFI 分区根目录下的 `nvram.plist`
  4. 重启
  5. 在终端输入 `sudo nvram myvar=test`
  6. 再次重启
  7. 在终端输入 `nvram -p | grep -i myvar`
  8. 如果终端成功输出了 `test` 那就说明你的主板在 macOS 下硬件 NVRAM 正常工作, 反之就是「不」正常工作

- **模拟 NVRAM 是什么, LogoutHook 扮演什么角色:**

  - **模拟 NVRAM** 是通过 `VariableRuntimeDxe`(`EmuVariableRuntimeDxe`/`EmuVariableUefi`) 模拟出一个 macOS 可读写的重启「不记忆」NVRAM, 里面的内容与硬件层面的 NVRAM 完全隔离, 引导工具在加载完这个驱动后自身也会进入这个环境中并失去对硬件 NVRAM 的控制
  - **LogoutHook** 是一个在账户注销前运行的脚本, 运行时会用 `nvram -x` 命令将 NVRAM 中的信息导出至 EFI 分区根目录的 `nvram.plist` 中, OpenCore 在下一次开机时会读取 `nvram.plist` 里面的内容并写进硬件 NVRAM 中

- **可以直接使用别人生成的 `nvram.plist` 吗**

  - 使用别人的 `nvram.plist` 时, 只有 [`7C436110-AB2A-4BBB-A880-FE41995C9F82`](https://github.com/erikberglund/AppleNVRAM/blob/master/Apple/7C436110-AB2A-4BBB-A880-FE41995C9F82.md) 下的内容可以正常工作
  - [`8BE4DF61-93CA-11D2-AA0D-00E098032B8C`](https://github.com/erikberglund/AppleNVRAM/blob/master/EFI/8BE4DF61-93CA-11D2-AA0D-00E098032B8C.md) 下的内容由于 UUID 不同完全无法工作, 完全无法为 OpenCore 指定默认引导项
  - 即使不模拟 NVRAM, OpenCore 和 FwRuntimeServices 提供的 [`7C436110-AB2A-4BBB-A880-FE41995C9F82`](https://github.com/erikberglund/AppleNVRAM/blob/master/Apple/7C436110-AB2A-4BBB-A880-FE41995C9F82.md) 模拟已经足以在 `nvram.plist` 中正确生成此项
  - **所以这么做完全没有意义**

####  用 LogoutHook 模拟 NVRAM 并为 OpenCore 指定默认引导项

确认主板在 macOS 下 NVRAM 不正常工作后我们可以通过安装 OpenCore 提供的 LogoutHook 来模拟 NVRAM

> 这里强烈建议在开始之前用 OpenCore 重置 NVRAM 并通过 `VariableRuntimeDxe.efi` 创建一个虚拟的可正常读写的 NVRAM
>
> 如果严格按照这个流程执行且 Config 内 `BlessOverride` 和启动项的路径均已设置正确, 那么理论上 `启动磁盘` 为 OpenCore 指定默认引导项的功能已经可以正常工作
>
> 如果 OpenCore 无法正确识别 Windows 建议先修复这个问题再模拟 NVRAM

1. 按照上文的方法清理 Clover 的 RC 脚本

2. 彻底删除之前安装的 LogoutHook

   - 终端输入: `sudo defaults read com.apple.loginwindow LogoutHook` 来获取 LogoutHook 的路径

     > 如果终端输出的不是一串路径, 而是
     >
     > `The domain/default pair of (com.apple.loginwindow, LogoutHook) does not exist`
     >
     > 说明系统内没有安装 LogoutHook, **跳过下面的命令**直接继续下一步 Config 的设置

   - 输入如下命令来删除 `LogoutHook.command` 并清空触发设置 **(请按顺序执行!!!)**

     ```
     
     ```

- ```
  # 删除文件 LogoutHook.command
  sudo rm -rf $(sudo defaults read com.apple.loginwindow LogoutHook)
  
  # 清空 LogoutHook 的触发设置
  sudo defaults delete com.apple.loginwindow LogoutHook
  ```

Config 中:

- **`Booter → Quirks → DisableVariableWrite`** 设置为 `YES`
- **`Misc → Security → AllowNvramReset`** 设置为 `YES`
- **`Misc → Security → ExposeSensitiveData`** 设置为 `3`
- **`NVRAM → LegacyEnable`** 设置为 `NO`

删除引导工具内 **config.plist「和」Drivers 目录** 下的 `VariableRuntimeDxe.efi`, `EmuVariableRuntimeDxe.efi`

删除 EFI 分区下的 `nvram.plist`

重启

进入 OpenCore 然后 选择引导界面的重置 NVRAM 选项「或者」按下快捷键 `Command + Option + P + R`

设备自动重启后, 进入 BIOS, Windows 或者 Win PE 重新为 OpenCore 添加引导项

> OpenCore 重置 NVRAM 时相比 Clover 会重置更多的变量, 理论上更干净, 但是会导致 UEFI 引导项丢失

进入 macOS, 在 Config 里将 **`NVRAM → LegacyEnable`** 设置为 `YES`, **`UEFI → Drivers`** 下添加 `VariableRuntimeDxe.efi`

```

▼ UEFI                  <Dictionary>
|__ ▼ Drivers           <Array>
    |__ Item #          <String>          FwRuntimeServices.efi
    |__ Item #+1        <String>          VariableRuntimeDxe.efi
```

在 **`EFI/OC/Drivers`** 文件夹下放入 [`VariableRuntimeDxe.efi`](https://github.com/williambj1/OpenCore-Factory/releases/tag/OpenCore-UEFI-Drivers)

再次重启

进入 macOS, 打开 `偏好设置/启动磁盘`, 解锁后选择你的 macOS 分区然后 **「锁上」**

终端输入命令检查 NVRAM 变量是否成功写入:

```

nvram -x '8BE4DF61-93CA-11D2-AA0D-00E098032B8C:Boot0080' | sed '/\<data\>/,/\<\/data\>/!d;//d' | base64 --decode
```

- 如果输出的「结尾」部分包含 `\System\Library\CoreServices\boot.efi` 我们可以继续执行下面的步骤
- 如果显示 `nvram: Error getting variable - '8BE4DF61-93CA-11D2-AA0D-00E098032B8C:Boot0080': (iokit/common) data was not found` 说明模拟 NVRAM 没有成功, 请回到第一步重来

在终端中输入 `sudo` + `空格` 然后将 LogoutHook 拉进终端的「窗口中」, 按下回车, 输入密码, 再次回车 (临时运行 LogoutHook)

终端中会出现一排上上步提到的错误, 这是正常的因为我们没有其它的引导项 (`Boot0081~3`) 和上次引导的记录 (`BootCurrent`) 等等, 只需要确保 **「没有任何关于」`Boot0080` 和 `BootOrder`** 的错误出现就可以了 (`BootNext` 也会报错但是实测没有影响)

挂载 EFI 分区并打开根目录下的 `nvram.plist`, 检查 **`Boot0080` 和 `BootOrder`** 是否存在

```

▼ ROOT                                          <Dictionary>
|__ ▼ Add                                       <Dictionary>
    |__ ▼ 7C436110-AB2A-4BBB-A880-FE41995C9F82  <Dictionary>
    |   |__ ...                                 <Data>        ...
    |   |__ 用 Hackintool 核对内容是否一致
    |__ ▼ 8BE4DF61-93CA-11D2-AA0D-00E098032B8C  <Dictionary>
        |__ Boot0080                            <Data>        ...
        |__ BootOrder                           <Data>        ...
```

如果没有问题, 在终端输入如下命令「安装」LogoutHook:

```

```

1. ```
   mv (空格, 把 LogoutHook 拖窗口, 然后再空格) $HOME/.LogoutHook.command
   
   sudo $HOME/.LogoutHook.command install
   ```

   > 如果使用 10.12 或更老的系统则需要将 `nvram.mojave` 也复制到 `$HOME` 下
   >
   > `mv (空格, 把 nvram.mojave 拖窗口, 然后再空格) $HOME`

2. 再次删除引导工具内 **config.plist「和」Drivers 目录** 下的 `VariableRuntimeDxe.efi`

> 非常重要! 不要漏了而前功尽弃!

1. 大功告成!

###  修复 OpenCore 引导界面的显示问题

**在 Config 中:**

- `UEFI → Protocols`
  - `ConsoleControl` = `YES`
- `UEFI → Quirks`
  - `ProvideConsoleGop` = `YES`
  - `IgnoreTextInGraphics` = `YES`
  - `SanitiseClearScreen` = `YES`
- `Misc → Boot`
  - `ConsoleMode` = `留空`
  - `Resolution` 设置为显示器的正常分辨率, 如 4k: `3840x2160`
- `UEFI → Output`
  - `TextRenderer` = `BuiltinGraphics`

> 如果 OC 界面出现在显示屏中央, 即使画面没有扭曲也说明分辨率不正常
>
> 将 `ConsoleMode` 设置为 `Max` 并关闭 `SanitiseClearScreen` 在某些固件上也可以做到分辨率正确, 但是这个方法官方 **「不」推荐使用**

###  在 OpenCore 中启用 FileVault 文件保险箱

**在 Config 中:**

- `Misc → Security`
  - `AuthRestart` = `YES`
- `UEFI → Input`
  - `KeySupport` = `YES`
- `UEFI → Protocals`
  - `FirmwareVolume` = `YES`
  - `AppleSmcIo` = `YES`
  - `ProvideConsoleGOP` = `YES`
  - `RequestBootVarRouting` = `YES`
  - (Haswell 或更老的主板中 (包括 X99)) `HashServices` = `YES`
  - (在 Aptio IV 固件的主板上, 如果遇到了 `Still waiting for root device`) `ExitBootServicesDelay` = `5`

###  修改 OpenCore 引导界面的启动项名称

在与引导项的 `.efi` 文件的同一目录下添加纯文本文件 `.contentDetails` 可以修改对应启动项的名称

> **注意⚠️!!! 启动项名称只能是 ASCII 字符!!!**
>
> **下方的命令修改的是当前 macOS 的启动项名称, 修改其它分区的 macOS 前需要先进入其目录, 这里不再赘述**
>
> 删掉 `.contentDetails` 就可以恢复原来的名称

**终端输入:**

```

sudo -s

# 在 macOS 10.15 挂载系统分区为读写
mount -uw / && killall Finder

# macOS
echo "启动项名称" >> /System/Library/CoreServices/.contentDetails

# Windows 没有使用 BlessOverride
echo "启动项名称" >> /Volumes/(你的 ESP 分区)/EFI/Boot/.contentDetails

# Windows 使用了 BlessOverride
echo "启动项名称" >> /Volumes/(你的 ESP 分区)/EFI/Microsoft/Boot/.contentDetails

# 退出 sudo -s
exit
```



## 附录[1] – OpenCore 支持的内核驱动 (Kext) 及其用途

> 参考 https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Kexts.md
>
> **GitHub 项目的下载地址位于 Release 下, 驱动具体支持的设备请进入项目地址查看**

###  有线网卡

|                        驱动及项目地址                        | 备注                                         |
| :----------------------------------------------------------: | -------------------------------------------- |
| [AppleRTL8169Ethernet](https://www.realtek.com/en/directly-download) | Realtek RTL8169 官方驱动                     |
| [AtherosE2200Ethernet.kext](https://github.com/Mieze/AtherosE2200Ethernet) | 高通 Atheros Killer E2200 系列驱动           |
| [AtherosL1cEthernet.kext](https://github.com/al3xtjames/AtherosL1cEthernet) | 高通 Atheros AR813x/815x 驱动                |
| [IntelMausi.kext](https://github.com/acidanthera/IntelMausi) | 英特尔有线网卡 Acidanthera 分支              |
| [IntelMausiEthernet.kext](https://github.com/Mieze/IntelMausiEthernet) | 英特尔有线网卡原作者                         |
| [NullEthernetInjector.kext](https://github.com/RehabMan/OS-X-Null-Ethernet) | 仿冒内建网卡 (没有可用的内建网卡时使用)      |
| [RealtekR1000SL.kext](https://github.com/SergeySlice/RealtekLANv3) | Realtek 8111B/C/D/E/EP/F/G/GU/8411B 系列驱动 |
| [RealtekRTL8100.kext](https://github.com/Mieze/RealtekRTL8100) | Realtek RTL810X 系列驱动                     |
| [RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X) | Realtek RTL8111/8168 系列驱动                |

###  Wi-Fi 和蓝牙

|                        驱动及项目地址                        | 备注                                           |
| :----------------------------------------------------------: | ---------------------------------------------- |
| [AirPortAtheros40.kext](https://i.applelife.ru/2018/12/442854_AirPortAtheros40.kext.zip) | 高通 Atheros AR92xx/AR93xx 驱动, 仅用于 10.14+ |
| [AirportBrcmFixup.kext](https://github.com/acidanthera/AirportBrcmFixup) | 非苹果官方博通网卡修复                         |
|  [ATH9KFixup.kext](https://github.com/chunnann/ATH9KFixup)   | 高通 Atheros AR9xxx 无线网卡修复               |
| [BrcmPatchRAM.kext](https://github.com/acidanthera/BrcmPatchRAM) | 博通网卡蓝牙固件上传                           |
| [BT4LEContinuityFixup.kext](https://github.com/acidanthera/BT4LEContinuityFixup) | IOBluetoothFamily 修补                         |
| [MT7610](https://d86o2zu8ugzlg.cloudfront.net/mediatek-craft/drivers/MT7612_7610U_D5.0.1.25_SDK1.0.2.18_UI5.0.0.27_20151209.zip) | 联发科 MT7610                                  |
| [RT5370](https://d86o2zu8ugzlg.cloudfront.net/mediatek-craft/drivers/RTUSB_D2870-4.2.9.2_UI-4.0.9.6_2013_11_29.zip) | 联发科 RT5370                                  |
| [RTL8192CU](https://drive.google.com/file/d/1ZtdMqlvKBbHULJhl1u9omuLOy6j0vx48/view?usp=sharing) | Realtek RTL8192CU 驱动 (链接为谷歌云盘)        |

###  键盘, 鼠标和触摸设备

|                        驱动及项目地址                        | 备注                                      |
| :----------------------------------------------------------: | ----------------------------------------- |
| [ApplePS2SmartTouchPad.kext](https://osxlatitude.com/forums/topic/1948-elan-focaltech-and-synaptics-smart-touchpad-driver-mac-os-x/) | 触摸板和键盘                              |
|     [AsusSMC.kext](https://github.com/hieplpvip/AsusSMC)     | 华硕 Fn 键, 键盘背光灯和环境光传感器 驱动 |
|  [NoTouchID.kext](https://github.com/al3xtjames/NoTouchID)   | 禁用 Touch ID 检测, 修复输密码时卡顿      |
| [SerialMouse.kext](https://github.com/Goldfish64/SerialMouse) | 使用 Microsoft 串行鼠标协议的串行鼠标驱动 |
|  [VoodooI2C.kext](https://github.com/alexandred/VoodooI2C)   | I2C 触摸板/屏 驱动                        |
| [VoodooPS2Controller.kext](https://github.com/acidanthera/VoodooPS2) | PS2 键盘/触摸板 驱动                      |

###  显卡和声卡

|                        驱动及项目地址                        | 备注                            |
| :----------------------------------------------------------: | ------------------------------- |
|   [AppleALC.kext](https://github.com/acidanthera/AppleALC)   | 定制万能声卡驱动                |
| [NVIDIA CUDA drivers](https://www.nvidia.com/object/mac-driver-archive.html) | NVIDIA CUDA 驱动                |
|   [NVIDIA Web-drivers](https://gfe.nvidia.com/mac-update)    | NVIDIA 显卡驱动                 |
| [SNBGraphicsMojaveInstaller](https://github.com/Andrej-Antipov/SNBGraphicsMojaveInstaller) | 二代酷睿核显驱动, 仅用于 10.14+ |
| [VoodooHDA.kext](https://sourceforge.net/projects/voodoohda/) | 万能声卡驱动                    |
| [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen) | 显卡补丁驱动                    |
| [Polaris22Fixup.kext](https://github.com/osy86/Polaris22Fixup) | Polaris22/VegaM 显卡修复        |

###  CPU 和 SMC

|                        驱动及项目地址                        | 备注                            |
| :----------------------------------------------------------: | ------------------------------- |
|  [CPUFriend.kext](https://github.com/acidanthera/CPUFriend)  | CPU 变频管理                    |
| [FakeSMC.kext and sensors](https://github.com/CloverHackyColor/FakeSMC3_with_plugins) | Clover 官方 FakeSMC             |
|   [HWPEnabler.kext](https://github.com/headkaze/HWPEnable)   | 启用 HWP                        |
| [NullCPUPowerManagement.kext](https://github.com/corpnewt/NullCPUPowerManagement) | AMD 和虚拟机专用版本            |
| [OpcodeEmulator.kext](https://www.insanelymac.com/forum/topic/329704-opcode-emulator-opemu-plug-in-project/) | Opcode 模拟驱动                 |
| [TSCAdjustReset.kext](https://github.com/interferenc/TSCAdjustReset) | TSC 频率同步驱动                |
| [VirtualSMC.kext 及传感器](https://github.com/acidanthera/VirtualSMC) | Acidanthera 的 SMC 和传感器驱动 |
| [VoodooTSCSync.kext](https://github.com/RehabMan/VoodooTSCSync) | TSC 频率同步驱动                |

###  USB 和 其它接口

|                        驱动及项目地址                        | 备注                                  |
| :----------------------------------------------------------: | ------------------------------------- |
| [IOElectrify.kext](https://github.com/the-darkvoid/macOS-IOElectrify) | 在雷电 3 设备上启用常开电源           |
|  [USBWakeFixup.kext](https://github.com/osy86/USBWakeFixup)  | 修复 Skylake 平台 USB 唤醒黑屏        |
| [SASMegaRAID.kext](https://github.com/dukzcry/osx-goodies/tree/master/raid) | LSI MegaRAID SAS 系列 RAID 控制器驱动 |
| [Sinetek-rtsx.kext](https://www.insanelymac.com/forum/topic/321080-sineteks-driver-for-realtek-rtsx-sdhc-card-readers/?do=findComment&comment=2376387) | Realtek `RTSX` SDHC 读卡器驱动        |
| [VoodooSDHC.kext](https://github.com/lvs1974/VoodooSDHCMod)  | SDHC 读卡器驱动                       |

###  其它驱动

|                        驱动及项目地址                        | 备注                                                |
| :----------------------------------------------------------: | --------------------------------------------------- |
| [AppleIntelInfo.kext](https://github.com/headkaze/AppleIntelInfo) | CPU / 核显 变频测试                                 |
| [HibernationFixup.kext](https://github.com/acidanthera/HibernationFixup) | 修复因 RTC 变量和 NVRAM 造成的睡眠问题              |
|       [Lilu.kext](https://github.com/acidanthera/Lilu)       | SDK & Library                                       |
|   [LiluFriend.kext](https://github.com/PMheart/LiluFriend)   | 用于确保 Lilu 在 L/E 下正常加载                     |
| [RTCMemoryFixup.kext](https://github.com/lvs1974/RTCMemoryFixup) | 修复 BIOS CMOS (RTC) 内存和 AppleRTC 之间的冲突问题 |
| [NightShiftUnlocker.kext](https://github.com/0xFireWolf/NightShiftUnlocker) | 解锁 NightShift                                     |
| [WebCamera.kext](https://www.applelife.ru/threads/asus-x550vc-i-asus-x550cc.41752/page-130#post-593586) | 某些旧设备的摄像头驱动                              |

##  附录[2] – UEFI 驱动 (.efi) 及其用途

###  OpenCore 支持的 UEFI 驱动

- ApfsDriverLoader
  - APFS 文件系统引导驱动
- AudioDxe
  - OpenCore 用于在 UEFI 环境播放音频的驱动
- CrScreenshotDxe
  - 增加 OpenCore UI 内截屏功能, 快捷键为 `LCtrl + LAlt + F12` ( 左Ctrl + 左Alt + F12)
- FwRuntimeServices
  - 通过支持只读和只写 NVRAM 变量来提高 OpenCore 和 Lilu 的安全性。有些设置项, 例如 `RequestBootVarRouting`, 需要此驱动程序才能正常运行
- HFSPlus
  - 苹果自带的闭源 HFS 驱动, 不具有 `Bless` 和其它功能, 但是启动速度比它的等效驱动 `VBoxHfs` 快 3 倍
- HiiDatabase.efi
  - 用于给 Ivy Bridge (3 代酷睿) 或更老代主板上支持 UEFI 字体渲染, UEFI Shell 中文字渲染异常时使用, 新主板不需要
- NvmExpressDxe
  - 从 Broadwell 开始的大多数固件中都包含此驱动程序。对于 Haswell 及更早的固件, 如果安装了 NVMe SSD 就需要使用
- MemoryAllocation.efi
  - 为 Z390/X99 等主板预留第一组 512MB 内存, 帮助引导工具注入内核以及内核缓存至第一组 512MB 内存, 需要配合 FwRuntimeServices 和引导标识符 `slide=1`
- AppleUsbKbDxe
  - 添加了对 `AppleKeyMapAggregator` 协议支持的 USB 键盘驱动, 这是 `AppleGenericInput` 的等效驱动, 根据固件的不同, 实际效果会更好或更坏
- VariableRuntimeDxe
  - EDK II 的 NVRAM 驱动, OpenCore 中用于模拟 NVRAM, 需要配合 `FwRuntimeServices`(.efi) 和 `DisableVariableWrite`(Quirk)
- VBoxHfs
  - 具有 `bless` 支持的 HFS 文件系统驱动程序。该驱动程序可以等效替代 Apple 固件中常见的闭源 HFSPlus 驱动。**此驱动虽然功能更加完善, 但启动速度相比大约慢 3 倍，并且尚未经过安全审核**
- XhciDxe
  - 来自 MdeModulePkg 的 XHCI USB 控制器支持驱动程序。从 Sandy Bridge 开始的大多数固件中都包含此驱动程序。在较旧的固件上可以用于支持 USB 3.0 PCI 卡

> **部分网上无法找到的 UEFI 驱动可以在这里下载**
>
> [![Download](%E5%B0%8F%E5%85%B5%20%E7%B2%BE%E8%A7%A3OC.assets/loading.gif)](https://github.com/williambj1/OpenCore-Factory/releases/tag/OpenCore-UEFI-Drivers)

###  OpenCore 不兼容的 UEFI 驱动

- AppleGenericInput

  - 添加了对 `AppleKeyMapAggregator` 协议支持的用户输入驱动。此外, 还解决了某些固件上的鼠标输入问题, 这是 AppleUsbKbDxe 的等效驱动, 根据固件的不同, 实际效果会更好或更坏
  - **已合并入 OpenCore**

- AppleImageCodec

  - 为 Clover 启动 FileVault 2 解码 PNG 和 BMP, **OpenCore 已集成**

- AppleKeyAggregator

  - 为 Clover 支持 FileVault 2 启动 UI 的驱动, **OpenCore 已集成**

- AppleKeyFeeder

  - 为 Clover 支持 FileVault 2 内 PS/2 键盘输入的驱动, **OpenCore 已集成**

- AppleUITheme

  - 为 Clover 支持 FileVault 2 启动 UI 主题的驱动, **OpenCore 已集成**

- AptioInputFix

   & 

  AppleGenericInput

  - 用于解决 UEFI 固件输入问题的驱动, **已与 OpenCore 合并**

- AptioMemoryFix

  - NVRAM 和内存驱动, 用于修复 UEFI 固件上的内存问题, **已与 OpenCore 合并为 `FwRuntimeServices`**

- CsmVideoDxe

  - 用于 Clover GUI 的显卡驱动, 允许使用更多分辨率, 基于 UEFI BIOS 中的 CSM 兼容模块, 并将启用所需的 CSM, **OpenCore 不兼容**

- DataHubDxe

  - macOS 必需的 DataHub 协议, **OpenCore 自带且提供了这个 Quirk**

- EmuVariableUefi

  - Clover 的模拟 NVRAM 驱动, **OpenCore 不兼容, 替代品为 `VariableRuntimeDxe`**

- EnglishDxe

  - 在 UEFI Shell 中支持 UnicodeCollation 协议, **OpenCore 自带且提供了这个 Quirk**

- EnhancedFatDxe

  - 这个驱动已存在于所有 UEFI 固件中, **无法从 OpenCore 直接使用**。由于很多固件的 FAT 支持都有问题, 导致在尝试写入时会损坏文件系统。如果在引导过程中写入 EFI 分区出现问题, 则需要将此驱动用 [UEFITool](https://github.com/LongSoft/UEFITool) 刷入固件中

- FirmwareVolume

  - 为 Clover 启动 FileVault 2 创建 FirmwareVolume 光标的驱动, **OpenCore 已集成**

- FSInject

  - Clover 用于注入内核驱动 (Kext) 的驱动, **OpenCore 自带且使用更先进的方法**

- HashServiceFix

  - 修复 UEFI BIOS 中的哈希支持, **OpenCore 自带且提供了这个 Quirk**

- OsxAptioFixDrv

  - 旧的 Clover NVRAM 和内存驱动, 用于修复 UEFI 固件上的内存问题, **与 FwRuntimeServices 和 OpenCore 不兼容**

- OSXAptioFix2Drv-Free2000.efi

  - Clover 的内存驱动, 用于修复 UEFI 固件上的内存问题, [作者已经声明会损坏硬件](https://blog.daliansky.net/(https://www.reddit.com/r/hackintosh/comments/cfjyla/i_unleashed_a_plague_upon_you_guys_and_i_am_sorry/))

- OsxAptioFix3Drv

  - Clover NVRAM 和内存驱动, 用于修复 UEFI 固件上的内存问题, **与 FwRuntimeServices 和 OpenCore 不兼容**

- OsxFatBinaryDrv

  - Clover 用于支持 OS X 10.9 和更早版本的 FAT Binary 可执行文件的驱动, **与 FwRuntimeServices 和 OpenCore 不兼容**

- OsxLowMemFixDrv

  - 精简版的 OsxAptioFixDrv, 用于修复 UEFI 固件上的内存问题, **与 FwRuntimeServices 和 OpenCore 不兼容**

- PartitionDxe

  - 用于支持非常规分区图的驱动, 如: 混合 GPT/MBR 或 Apple 分区图, **OpenCore 兼容性未知**

- Ps2MouseDxe

  - PS/2 鼠标驱动, **这个驱动已存在于所有 UEFI 固件中**

- SMCHelper

  - UEFI 层面的 SMC 驱动, 用于与 FakeSMC 配合。**与 OpenCore 不兼容**

- VirtualSmc

  - UEFI 层面的 SMC 驱动, **已与 OpenCore 合并**

- **其它未在本文中提到的 UEFI 驱动一律不兼容**

##  附录[3] – 常用内核引导标识符合集

|                          引导标识符                          | 作用                                               |
| :----------------------------------------------------------: | -------------------------------------------------- |
|                     `-amd_no_dgpu_accel`                     | 关闭 AMD 显卡硬件加速                              |
|                           `cpus=#`                           | 启用 `#` 个 CPU 核心                               |
| [`darkwake=0`](http://www.yekki.me/power-nap-and-darkwake-argument/) | 禁用 Power Nap                                     |
|                           `dart=0`                           | 禁用 VT-d                                          |
|                        `debug=0x100`                         | 发生 KP 时不自动重启                               |
|                         `keepsyms=1`                         | 发生 KP 时保留 Debug Symbols, 用于给开发者反馈问题 |
|                      `kext-dev-mode=1`                       | 启用 Kext 开发模式, 非开发者请勿使用               |
|                      `-no_compat_check`                      | 关闭兼容性检查                                     |
|                        `npci=0x2000`                         | 在旧设备上禁用 kIOPCIConfiguratorPFM64             |
|                         `nvda_drv=1`                         | 启用 N 卡驱动的老方法, 在 10.12 及以后失效         |
|                        `nv_disable=1`                        | 关闭 N 卡硬件加速                                  |
|                             `-s`                             | 单用户模式                                         |
|                          `slide=#`                           | 手动设置 KASLR slide 值为 `#`                      |
|                             `-v`                             | verbose 跑码模式                                   |
|                             `-x`                             | 安全模式                                           |

> 内核驱动提供的引导标识符请去对应驱动的 Readme 查看

##  参考及引用

- [Apple](https://www.apple.com) 的 macOS
- [Acidanthera](https://github.com/acidanthera) 的 OpenCore 及其[官方文档](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs)
- [khronokernel](https://github.com/khronokernel) 的 [Getting-Started-With-OpenCore](https://github.com/khronokernel/Getting-Started-With-OpenCore) 和 [Opencore-Vanilla-Desktop-Guide](https://github.com/khronokernel/Opencore-Vanilla-Desktop-Guide)
- [Tonymacx86](https://tonymacx86.com/) 的 [Emulated-NVRAM-Uninstaller](https://www.tonymacx86.com/resources/clover-emulated-nvram-uninstaller.368/)
- [xjn](https://github.com/xjn819) 的博客 [使用OpenCore引导黑苹果](https://blog.xjn819.com/?p=543)

##  鸣谢

- [宪武](https://blog.daliansky.net/) 提供的 OpenCore SSDT Hotpatch 典范 [OC-little](https://github.com/daliansky/OC-little)
- [xlivans](https://github.com/xlivans), [子俊](https://github.com/zijun-he) 对 OpenCore 的大量跟进和研究
- OpenCore 交流群成员进行的大量测试
- [Bat.bat](https://github.com/williambj1) 对教程对审核更新完善

[![黑果小兵 wechat](https://blog.daliansky.net/images/loading.gif)](https://blog.daliansky.net/uploads/WeChat.png)

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
- **本文链接：** https://blog.daliansky.net/OpenCore-BootLoader.html
- **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明出处！






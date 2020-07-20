





```csharp



    How can I have ProperTree open when I double-click a .plist file?

    On macOS you can run buildapp.command located in ProperTree's Scripts directory to build an application bundle which can be associated with .plist files. While this approach works - it sometimes has odd issues when attempting to open multiple .plist files by double-clicking. Typically the first will work as normal, but opening any subsequent .plist file requires using the File -> Open menu.

    On Windows, you can run AssociatePlistFiles.bat located in ProperTree's Scripts directory to associate .plist files with ProperTree.bat, and also to add an Open with ProperTree option to the contextual menu when right-clicking .plist files. This approach is location-dependent, and moving your copy of ProperTree will require you re-run AssociatePlistFiles.bat.

    When I try to run ProperTree, I get [ModuleNotFoundError: No module name 'tkinter']

    That is because the graphical interface library that ProperTree depends on isn't present or cannot be detected, you need to install tkinter from your package manager.

    To install it on Ubuntu (and Ubuntu-based distros), you can run sudo apt-get install python3-tk -y

    ProperTree doesn't run because it doesn't have permissions, what gives?

    This shouldn't happen and it is recommended that you download only from the official ProperTree repository, but if you are confident about your source, then running chmod +x ProperTree.command should sort it out

    I use an international keyboard layout on macOS and some keys crash ProperTree with NSRangeException', reason: '-[__NSCFConstantString characterAtIndex:]: Range or index out of bounds

    This is a bug in the Cocoa implementation of Tcl/Tk on macOS (discussed here). The latest python 2 installer from python.org ships with, and uses Tcl/Tk 8.6.8 which has this issue fixed. Given that the shebang in ProperTree.command leverages #!/usr/bin/env python - the first python 2 binary found should be used.


```



当我双击一个。plist文件时，如何打开ProperTree ?在macOS上，您可以运行位于ProperTree脚本目录中的buildapp.command来构建一个与.plist文件相关联的应用程序包。虽然这种方法可以工作——但当试图通过双击打开多个.plist文件时，有时会出现奇怪的问题。通常第一个可以正常工作，但是打开任何后续的.plist文件需要使用文件-&gt;打开菜单。在Windows上，您可以运行位于ProperTree脚本目录中的AssociatePlistFiles.bat来将.plist文件与ProperTree关联起来。bat，还可以在右键单击.plist文件时在上下文菜单中添加Open with ProperTree选项。这种方法依赖于位置，移动ProperTree的副本将需要重新运行AssociatePlistFiles.bat。当我尝试运行ProperTree时，我得到[ModuleNotFoundError: No module name 'tkinter']这是因为ProperTree所依赖的图形界面库没有出现或者无法检测到，所以需要从包管理器安装tkinter。要在Ubuntu(和基于Ubuntu的发行版)上安装它，可以运行sudo apt-get install python3-tk -yProperTree不能运行，因为它没有权限，是什么?这种情况不应该发生，建议您只从官方的ProperTree存储库下载，但是如果您对自己的源代码有信心，那么运行chmod +x ProperTree.command应该可以对其进行排序我在macOS上使用国际键盘布局和一些键崩溃的属性树NSRangeException'，原因:'-[摔伤的nscfconstantstring characterAtIndex:]:范围或索引越界这是macOS上Tcl/Tk的Cocoa实现中的一个bug(在这里讨论)。来自python.org的最新python2安装程序附带了，使用的Tcl/Tk 8.6.8已经修复了这个问题。考虑到ProperTree.command中的shebang利用了#!/usr/bin/env python—应该使用找到的第一个python 2二进制文件。









# end
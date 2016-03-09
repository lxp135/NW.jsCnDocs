# 打包与分发
---

[TOC]

这篇指南文档将教会你如何打包与分发NW.js应用。

## 快速开始

你可以使用[`nw-builder`](https://github.com/nwjs/nw-builder)来快速的形成一个程序包。

1. 在打包之前[准备好你的应用](#_3)。
2. 输入 `npm install -g nw-builder` 安装  `nw-builder`。
3. 输入 `nwbuild -p linux64 /path/to/your/app` 打包你的应用。

打包完成的应用将在`./build`文件夹中找到。

## 准备好你的应用

在打包之前，你应该准备所有必要的文件。根据下面的清单进行检查，确保没有任何缺失：

* [ ] 源代码与资源文件
* [ ] 使用`npm install`安装NPM模块
* [ ] [重新编译原生Node.js模块](Advanced/Use Native Node Modules.md)
* [ ] [编译 NaCl binaries](Advanced/Use NaCl in NW.js.md)
* [ ] [压缩JS源代码](Advanced/Protect JavaScript Source Code.md)并删除原始文件
* [ ] 在[manifest file](../References/Manifest Format.md#icon)中配置图标

!!! Warning "警告"
    不要假设你的`node_modules`会正确工作在所有平台上，例如`node-email-templates`在Windows和Mac OS X上需要指定的`npm install`命令来安装。而且需要Python环境才能运行，但是Python环境在Windows上默认是不存在的。
    一个经验规则是**在每个平台上将`npm install`命令配置到对应的`package.json`中**，以确保一切正常。

!!! note "文件名与路径"
	大多数Linux与Mac OS X的文件系统对于文件名是**大小写敏感**的，这意味着`test.js` 和 `Test.js` 是截然不同的两个文件。确保代码中的路径与文件名是正确的，否则你的应用在不同的文件系统上面可能会看起来有问题或者直接崩溃。

!!! note "在windows中使用长路径"
        在Windows上使用超过260个字符的路径名称，将可能导致构建失败。通常发生在依赖`npm install`安装且NPM版本小于3.0的情况下。在建立你的应用根目录时使用类似`C:\build\`这样的路径将尽可能避免这个问题。

## 准备 NW.js

为了让你的应用更好的运行，你必须重新编译发布NW.js。针对不同的需求和打包尺寸，NW.js提供了多个 [编译版本](Advanced/Build Flavors.md)。选择适合你的产品编版本格或者 [从源码构建与编译](../For Developers/Building NW.js.md)。

除了SDK版本中的`nwjc` or `nwjc.exe`，下载的压缩包中其他的所有文件可以根据需要有选择的加入到你的产品中。

## 打包你的应用

有两种方式可以打包你的应用：普通文件或者ZIP压缩文件。

### 打包方式1：普通文件 (推荐方式)

在Windows和Linux系统上,你可以将你的应用所有相关文件与NW.js执行文件放在相同文件夹下一起发送给你的用户。确保`nw` (或 `nw.exe`) 与 `package.json`在相同的文件夹（或目录）下。 或者你可以把你的应用的所有相关文件放在一个单独的文件夹下，并将该文件夹命名为`package.nw`，该文件夹需要放在与`nw` (或 `nw.exe`)相同的文件夹（或目录）中。

在Mac系统中，新建名称为`app.nw`的文件夹，把你的应用所有相关文件放入其中，然后将`app.nw`文件夹放在`nwjs.app/Contents/Resources/`目录下即可。

推荐您使用该打包方式。

### 打包方式2：ZIP压缩文件

你可以将应用的所有相关文件打成一个名为`package.nw`的压缩包。在Windows与Linux系统中，将`package.nw`与NW.js可执行文件合并即可。而在Mac系统中，则将`package.nw`放到`nwjs.app/Contents/Resources/`目录下。

!!! warning "打包过大或者文件过多将导致启动缓慢"
	在启动时，NW.js会将压缩包解压缩到一个临时文件夹并读取。所以如果你打包的压缩包过大或者文件过多会导致启动缓慢。

在Windows和Linux系统中，你甚至可以隐藏你的压缩包，将压缩包与`nw`或`nw.exe`合并为一个文件。

在Windows系统中执行以下命令：
```batch
copy /b nw.exe+package.nw app.exe
```

在Linux系统中执行以下命令达到相同效果：
```bash
cat nw app.nw > app && chmod +x app 
```

## 不同平台区别

### Windows

Icon for `nw.exe` can be replaced with tools like [Resource Hacker](http://www.angusj.com/resourcehacker/), [node-webkit-builder](https://github.com/mllrsohn/node-webkit-builder) and [node-winresourcer](https://github.com/felicienfrancois/node-winresourcer).

You can create a installer to deploy all necessary files onto end user's system. You can use [Windows Installer](https://msdn.microsoft.com/en-us/library/cc185688(VS.85).aspx), [NSIS](http://nsis.sourceforge.net/Main_Page) or [Inno Setup](http://www.jrsoftware.org/isinfo.php).

### Linux

On Linux, you need to create proper [`.desktop` file](https://wiki.archlinux.org/index.php/Desktop_Entries).

To create a self-extractable installer script, you can use scripts like [`shar`](https://en.wikipedia.org/wiki/Shar) or [`makeself`](http://stephanepeter.com/makeself/).

To distribute your app through the package management sysmtem, like `apt`, `yum`, `pacman` etc, please follow their official documents to create the packages.

### Mac OS X

On Mac OS X, you need to modify following files to have your own icon and boundle id:

* `Contents/Resources/nw.icns`: icon of your app. `nw.icns` is in [Apple Icon Image Format](https://en.wikipedia.org/wiki/Apple_Icon_Image_format). You can convert your icon in PNG/JPEG format into ICNS by using tools like [Image2Icon](http://www.img2icnsapp.com/).
* `Contents/Info.plist`: the apple package description file. You can view [Implementing Cocoa's Standard About Panel](http://cocoadevcentral.com/articles/000071.php) on how this file will influence your app and what fields you should modify.

And you should sign you Mac app. Or the user won't launch the app if Gatekeeper is turned on. See [Signed Apps or Installer Packages](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/DistributingApplicationsOutside/DistributingApplicationsOutside.html) for details.

## 参考资料

在[wiki of NW.js](https://github.com/nwjs/nw.js/wiki/How-to-package-and-distribute-your-apps)中可以找到更多工具帮助你打包应用。

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

!!! 警告
    Do not assume your `node_modules` that target one platform work _as is_ in all platforms. For instance `node-email-templates` has specific Windows & Mac OS X`npm install` commands. Besides, it requires python to install properly, which is not installed by default on Windows.
    As a rule of thumb **`npm install` your `package.json` on each platform you target** to ensure everything works as expected.

!!! note "文件名与路径"
    On most Linux and some Mac OS X, the file system is **case sensitive**. That means `test.js` and `Test.js` are different files. Make sure the paths and filenames used in your app have the right case. Otherwise your app may look bad or crash on those file systems.

!!! note "在windows中使用长路径"
    The length of path used in your app may exceed the maximum length (260 characters) on Windows. That will cause various build failures. This usually happens during installing dependencies with `npm install` using older version of NPM (<3.0). Please build your app in the root directory, like `C:\build\`, to avoid this issue as much as possible.

## 准备 NW.js

You have to redistribute NW.js with your app to get your app running. NW.js provided multiple [build flavors](Advanced/Build Flavors.md) for different requirements and package size. Choose the proper build flavor for your product or [build it from source code](../For Developers/Building NW.js.md).

All files in the downloaded package should be redistributed with your product, except `nwjc` or `nwjc.exe` in SDK flavor.

## 打包你的应用

There two options to pack your app: plain files or zip file.

### 打包方式1：普通文件 (推荐方式)

On Windows and Linux, you can put the files of your app in the same folder of NW.js binaries and then ship them to your users. Make sure `nw` (or `nw.exe`) is in the same folder as `package.json`. Or you can put the files of your app in a folder named `package.nw` in the same folder as `nw` (or `nw.exe`).

On Mac, put the files of your app into a folder named `app.nw` in `nwjs.app/Contents/Resources/` and done.

It's the recommended way to pack your app.

### 打包方式2：ZIP压缩文件

You can package all the files into a zip file and rename it as `package.nw`. And put it along with NW.js binaries for Windows and Linux. For Mac, put `package.nw` in `nwjs.app/Contents/Resources/`.

!!! warning "Start Slow with Big Package or Too Many Files"
    At starting time, NW.js will unzip the package into temp folder and load it from there. So it will start slower if your package is big or contains too many files.

On Windows and Linux, you can even hide the zip file by appending the zip file to the end of `nw` or `nw.exe`.
You can run following command on Windows to achieve this:
```batch
copy /b nw.exe+package.nw app.exe
```
or following command on Linux:
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

在[wiki of NW.js](https://github.com/nwjs/nw.js/wiki/How-to-package-and-distribute-your-apps)中可以找到更多工具来打包你的应用。

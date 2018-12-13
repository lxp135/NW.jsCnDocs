# 打包与发布
---

[TOC]

这篇指南文档将教会你如何打包与发布NW.js应用。

## 快速开始

您可以使用下列工具自动打包您的NW.js应用并发布。

* [nwjs-builder-phoenix](https://github.com/evshiron/nwjs-builder-phoenix) (推荐)
* [nw-builder](https://github.com/nwjs-community/nw-builder)

您也可以依据下列步骤手动打包并发布您的应用。

## 准备好你的应用

在打包之前，你应该准备所有必要的文件。根据下面的清单进行检查，确保没有任何缺失：

* [ ] 源代码与资源文件
* [ ] 使用`npm install`安装NPM模块
* [ ] [重新编译原生Node.js模块](Advanced/Use Native Node Modules.md)
* [ ] [编译 NaCl 库](Advanced/Use NaCl in NW.js.md)
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

All files in the downloaded package should be redistributed with your product, except tools in SDK flavor including `nwjc`, `payload` and `chromedriver`.
除了SDK版本中的`nwjc` or `nwjc.exe`，下载的压缩包中其他的所有文件可以根据需要有选择的加入到你的产品中。

## 打包你的应用

有两种方式可以打包你的应用：普通文件或者ZIP压缩文件。

### 打包方式1：普通文件 (推荐方式)

在Windows和Linux系统上,你可以将你的应用所有相关文件与NW.js执行文件放在相同文件夹下一起发送给你的用户。确保`nw` (或 `nw.exe`) 与 `package.json`在相同的文件夹（或目录）下。 或者你可以把你的应用的所有相关文件放在一个单独的文件夹下，并将该文件夹命名为`package.nw`，该文件夹需要放在与`nw` (或 `nw.exe`)相同的文件夹（或目录）中。

在Mac系统中，新建名称为`app.nw`的文件夹，把你的应用所有相关文件放入其中，然后将`app.nw`文件夹放在`nwjs.app/Contents/Resources/`目录下即可。

推荐您使用该打包方式。

### 打包方式2：ZIP压缩文件

你可以将应用的所有相关文件打成一个名为`package.nw`的压缩包。在Windows与Linux系统中，将`package.nw`与NW.js可执行文件放到相同目录即可。而在Mac系统中，则将`package.nw`放到`nwjs.app/Contents/Resources/`目录下。

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

`nw.exe` 文件的图标可以用[Resource Hacker](http://www.angusj.com/resourcehacker/)工具来替换, [nw-builder](https://github.com/mllrsohn/node-webkit-builder) 和 [node-winresourcer](https://github.com/felicienfrancois/node-winresourcer)。

你可以创建一个安装程序来部署所有必要文件到用户的系统。你可以使用[Windows Installer](https://msdn.microsoft.com/en-us/library/cc185688(VS.85).aspx)、[NSIS](http://nsis.sourceforge.net/Main_Page) 或 [Inno Setup](http://www.jrsoftware.org/isinfo.php)。

### Linux

在Linux系统中，你需要创建合适的[`.desktop` 文件](https://wiki.archlinux.org/index.php/Desktop_Entries)。

要创建一个自动安装脚本，你可以使用[`shar`](https://en.wikipedia.org/wiki/Shar) 或 [`makeself`](http://stephanepeter.com/makeself/)。

要通过包管理系统分发您的应用程序，比如`apt`、`yum`、`pacman`等，请按照他们的官方说明文档来创建包。

### Mac OS X

在Mac OS X中,你需要修改下列文件，添加你自己的icon和boundle id：

* `Contents/Resources/nw.icns`：MAC应用程序图标。`nw.icns` 遵循 [Apple Icon Image Format](https://en.wikipedia.org/wiki/Apple_Icon_Image_format)标准。你可以使用类似[Image2Icon](http://www.img2icnsapp.com/)的工具，将你的PNG/JPEG格式的图标转换成icns格式。
* `Contents/Info.plist`：MAC应用程序描述文件。你可以看[Implementing Cocoa's Standard About Panel](http://cocoadevcentral.com/articles/000071.php)文档将告诉你需要修改哪些字段。

To rename the application, the following files should be modified:
* `Contents/Info.plist` - CFBundleDisplayName
* `Contents/Resources/en.lproj/InfoPlist.strings` - CFBundleDisplayName
* `package.json` -- add a string field `product_string` (e.g. foobar); the helper application will be shown as 'foobar Helper'. (since v0.24.4)
* `Contents/Versions/n.n.n.n/nwjs Helper.app/Contents/MacOS/nwjs Helper` - rename the file to 'foobar Helper'
* `Contents/Versions/n.n.n.n/nwjs Helper.app/Contents/Info.plist` - change CFBundleDisplayName
* `Contents/Versions/n.n.n.n/nwjs Helper.app` - rename the directory to 'foobar Helper.app'

你应该注册的你mac应用程序，否则当Gatekeeper选项被开启时用户将不能使用你的应用程序。查看[提交到Mac应用商店](Advanced/Support for Mac App Store.md)了解详情。

## 参考资料

在[wiki of NW.js](https://github.com/nwjs/nw.js/wiki/How-to-package-and-distribute-your-apps)中可以找到更多工具帮助你打包应用。

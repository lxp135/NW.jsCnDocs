# 使用开发者工具调试
---

!!! note "仅SDK版本支持调试"
    开发者工具仅在[SDK版本](Advanced/Build Flavors.md)中工作，建议您使用SDK版本进行开发与调试NW.js应用。使用其他版本发布应用。

## 启用开发者工具

您可以在Windows、Linux上使用快捷键<kbd>F12</kbd>呼出开发者工具，或者在Mac上使用<kbd>&#8984;</kbd>+<kbd>&#8997;</kbd>+<kbd>i</kbd>呼出。

此外, 您可以编写JS代码调用NW.js的API打开开发者工具窗口 [win.showDevTools()](../References/Window.md#winshowdevtoolsiframe-callback)。

## Node.js 模块调试

NW.js默认运行在[Separate Context Mode](Advanced/JavaScript Contexts in NW.js.md#separate-context-mode)模式下。调试Node.js模块，您需要在应用中右键单击并选择"Inspect Background Page"菜单。当程序运行到Node.js模块断点时，在后台运行的开发者工具会自动聚焦并暂定在断点上。

如果您的应用运行在[Mixed Context Mode](Advanced/JavaScript Contexts in NW.js.md#mixed-context-mode)模式下，Node.js模块可以在开发者工具中直接进行调试。想知道更详细的两种不同模式的差异，请查看[在NW.js中使用JavaScript上下文](Advanced/JavaScript Contexts in NW.js.md)文档。

## 远程调试

您可以在命令行输入 `--remote-debugging-port=port` 指令并指定端口来启动开发者工具监听。举个例子，输入并运行 `nw --remote-debugging-port=9222`，可以在您自己的浏览器中输入网址http://localhost:9222/来进行远程调试。

## 使用开发者工具扩展

支持开发者工具全部扩展，包括但不限于ReactJS、Vue.js等等。
使用扩展，需要在manifest.json配置文件中添加 "chrome-extension://*" 权限配置项，并在启动nw.exe时添加 `--load-extension=path/to/extension` 执行参数。
这些开发者工具的扩展程序文件可以从Chrome Web商店下载安装后，在Chrome浏览器的扩展文件夹中拷贝出来。

### React 例子

* https://s3-us-west-2.amazonaws.com/nwjs/sample/react-app.zip
* https://s3-us-west-2.amazonaws.com/nwjs/sample/react-devtools.zip

解压缩, 下载SDK版本并带以下参数运行：`nw.exe --load-extension=path/to/devtools path/to/app/folder`

这个是一个包含'package.json'配置文件的简单react应用。开发者工具文件来自Chrome浏览器的扩展文件夹，该文件夹是安装在Chrome Web应用商店中的官方react开发者工具扩展。
仅在配置文件manifest中添加"chrome-extension://*"权限配置项时有效。

### Vue 例子

1. `npm install --save-dev nw-vue-devtools`
1. 在配置文件 `package.json` 中添加以下代码:
    ```js
    "chromium-args": "--load-extension='./node_modules/nw-vue-devtools/extension'",
    ```
1. 您必须在您的应用中引用Vue.js，不能使用被压缩过版本 (使用 `vue.js` 而不是 `vue.min.js`)。

自动下载、编译并安装最新版的Vue开发者工具到NW.js。

如果您使用`nwjs-builder-phoenix`打包工具，添加`"chromium-args"`配置项到`package.json` `build.strippedProperties`配置文件列表中。
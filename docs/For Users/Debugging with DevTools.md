# 使用DevTools调试
---

!!! note "仅SDK版本支持调试"
    开发者工具仅在[SDK flavor](Advanced/Build Flavors.md)版本中工作，建议您使用SDK版本进行开发与调试NW.js应用。使用其他版本发布应用。

## 启用开发者工具

您可以在Windows、Linux上使用快捷键<kbd>F12</kbd>呼出开发者工具，或者在Mac上使用<kbd>&#8984;</kbd>+<kbd>&#8997;</kbd>+<kbd>i</kbd>呼出。

此外, 你可以编写JS代码调用NW.js的API打开开发者工具窗口 [win.showDevTools()](../References/Window.md#winshowdevtoolsiframe-callback)。

## Node.js 模块调试

NW.js is running in [Separate Context Mode](Advanced/JavaScript Contexts in NW.js.md#separate-context-mode) by default. To debug Node.js modules, you can right click the app and choose "Inspect Background Page". When stepping into Node.js modules in the debugger, the DevTools for background page is automatically focused and stopped at certain statement.

If your app is running under [Mixed Context Mode](Advanced/JavaScript Contexts in NW.js.md#mixed-context-mode), Node.js modules can be directly debugged within the same DevTools window of the window. See [JavaScript Contexts in NW.js](Advanced/JavaScript Contexts in NW.js.md) for the differences.

## 远程调试

您可以在命令行输入 `--remote-debugging-port=port` 指令并指定端口来启动开发者工具监听。举个例子，输入并运行 `nw --remote-debugging-port=9222`，可以在你自己的浏览器中输入网址http://localhost:9222/来进行远程调试。

## 使用开发者工具扩展

Devtools extensions are fully supported, including the one for ReactJS, Vue.js, etc. To use it, add the permission "chrome-extension://*" to manifest.json of the devtools extension, and load it with `--load-extension=path/to/extension` when nw is started. The files for devtools extensions can be copied from extension folder of Chrome browser after you install them from Chrome Web Store.

### React 例子

* https://s3-us-west-2.amazonaws.com/nwjs/sample/react-app.zip
* https://s3-us-west-2.amazonaws.com/nwjs/sample/react-devtools.zip

解压缩, 下载SDK版本并带以下参数运行：`nw.exe --load-extension=path/to/devtools path/to/app/folder`

这个是一个包含'package.json'配置文件的简单react应用。The devtools files are from the Chrome browser's extension folder of official react devtools extension installed from Chrome Web store. Only the manifest file is modified to add the permission: "chrome-extension://*".

### Vue 例子

1. `npm install --save-dev nw-vue-devtools`
1. 在配置文件 `package.json` 中添加以下代码:
    ```js
    "chromium-args": "--load-extension='./node_modules/nw-vue-devtools/extension'",
    ```
1. Vue.js must be in use in your app, and cannot be minified (use `vue.js` not `vue.min.js`).

This will automatically download, build, and install the latest Vue-DevTools into NW.js.

If you are using `nwjs-builder-phoenix` then add in `"chromium-args"` to your `package.json` `build.strippedProperties` array.
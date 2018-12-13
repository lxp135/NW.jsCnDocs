# NW.js 中文参考文档翻译计划
鉴于NW.js如此好用的项目没有中文文档，特此建立NW.js文档中文参考文档翻译计划。

## 如何使用
* 直接访问 https://nwjs.liuxp.me
* 点击Download ZIP按钮下载本项目，解压缩后用浏览器打开site文件夹中的index.html。

## 如何编译
中文文档同英文文档相同，使用markdown书写并用mkdocs编译而成。

需要Python和Python package manager pip 来安装MkDocs
使用pip安装mkdocs:
```
$ pip install mkdocs
```
运行 mkdocs -V 以检查是否正确安装
```
$ mkdocs -V
```
进入项目文件夹，执行以下命令编译
```
mkdocs build --clean
```
会基于mkdocs.yml配置将docs中的原始md文件编译到site文件夹中形成静态页面。

## 参与翻译
fork本项目并编辑docs文件夹中的md文件，然后申请提交。 

## 翻译进度
| 标题 | 文件 | 进度 |
| ------ | ------ | ------ |
| - '首页' | 'index.md' | 100% |
|- '用户手册' | | 16.5% |
|  - '快速开始' | 'For Users/Getting Started.md' | 100% |
|  - '使用DevTools调试' | 'For Users/Debugging with DevTools.md' | 0% |
|  - '打包与发布' | 'For Users/Package and Distribute.md' | 100% |
|  - 'FAQ' | 'For Users/FAQ.md' | 100% |
|  - '迁移' | | 0% |
|    - '0.12升级0.13' | 'For Users/Migration/From 0.12 to 0.13.md' | 0% |
|  - '进阶' | | 0% |
|    - '编译' | 'For Users/Advanced/Build Flavors.md' | 0% |
|    - '内容验证' | 'For Users/Advanced/Content Verification.md' | 0% |
|    - '自定义菜单' | 'For Users/Advanced/Customize Menubar.md' | 0% |
|    - '在NW.js中使用JavaScript上下文' | 'For Users/Advanced/JavaScript Contexts in NW.js.md' | 0% |
|    - '保护JavaScript源代码' | 'For Users/Advanced/Protect JavaScript Source Code.md' | 0% |
|    - 'NW.js的安全性' | 'For Users/Advanced/Security in NW.js.md' | 0% |
|    - '提交到Mac应用商店' | 'For Users/Advanced/Support for Mac App Store.md' | 0% |
|    - '使用ChromeDriver测试' | 'For Users/Advanced/Test with ChromeDriver.md' | 0% |
|    - '透明窗口' | 'For Users/Advanced/Transparent Window.md' | 0% |
|    - '在NW.js中使用Flash插件' | 'For Users/Advanced/Use Flash Plugin.md' | 0% |
|    - '在NW.js中使用NaCl' | 'For Users/Advanced/Use NaCl in NW.js.md' | 0% |
|    - '在NW.js中使用原生Node.js模块' | 'For Users/Advanced/Use Native Node Modules.md' | 0% |
|    - '自动更新' | 'For Users/Advanced/Autoupdates.md' | 0% |
|- '开发者手册' | | 0% |
|  - '构建 NW.js' | 'For Developers/Building NW.js.md' | 0% |
|  - '为NW.js贡献代码' | 'For Developers/Contributing to NW.js.md' | 0% |
|  - '打开专有编解码器' | 'For Developers/Enable Proprietary Codecs.md' | 0% |
|  - '资源列表' | 'For Developers/Repositories.md' | 0% |
|  - '了解崩溃机制' | 'For Developers/Understanding Crash Dump.md' | 0% |
|  - '为NW.js编写文档' | 'For Developers/Writing Documents for NW.js.md' | 0% |
|  - '为NW.js编写测试用例' | 'For Developers/Writing Test Cases for NW.js.md' | 0% |
|  - '贡献者列表' | 'For Developers/Contributors of Documents.md' | 0% |
|- '参考资料' | | 0% |
|  - 'App' | 'References/App.md' | 0% |
|  - 'DOM的变化' | 'References/Changes to DOM.md' | 0% |
|  - 'Node的变化' | 'References/Changes to Node.md' | 0% |
|  - '剪切板' | 'References/Clipboard.md' | 0% |
|  - '命令行参数' | 'References/Command Line Options.md' | 0% |
|  - 'Chrome扩展API' | 'References/Chrome Extension APIs.md' | 0% |
|  - '配置文件' | 'References/Manifest Format.md' | 0% |
|  - '菜单' | 'References/Menu.md' | 0% |
|  - '菜单项' | 'References/MenuItem.md' | 0% |
|  - '屏幕' | 'References/Screen.md' | 0% |
|  - 'Shell' | 'References/Shell.md' | 0% |
|  - '快捷键' | 'References/Shortcut.md' | 0% |
|  - '系统托盘' | 'References/Tray.md' | 0% |
|  - 'webview标签' | 'References/webview Tag.md' | 0% |
|  - 'Window' | 'References/Window.md' | 0% |
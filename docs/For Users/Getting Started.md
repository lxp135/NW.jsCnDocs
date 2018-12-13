# 快速开始
---

[TOC]

## NW.js 能做什么?
NW.js 基于[Chromium](http://www.chromium.org)内核与[Node.js](http://nodejs.org/)。
NW.js让您在编写应用时可以使用Node.js及其modules与web开发技术。而且，您可以非常容易的将一个WEB应用打包成一个原生应用。

## 获得 NW.js

您可以从官方网站[http://nwjs.io](http://nwjs.io/)取得最新版本，或者您也可以参考[Building NW.js](../For Developers/Building NW.js.md)自己编译NW.js。

!!! tip
    建议您选择SDK版本，在SDK版本中您可以使用DevTools工具来Debug您的应用。查看不同编译版本的区别[Build Flavors](Advanced/Build Flavors.md)。

## 编写 NW.js 应用

### 例 1 - Hello World

从一个简单的例子来让我们看看如何编写一个NW.js应用。

**第一步** 创建 `package.json`配置文件:

```json
{
  "name": "helloworld",
  "main": "index.html"
}
```

`package.json` 是应用的默认配置文件，书写格式参考[JSON format](http://www.json.org/)。其中 `main` 属性设置例1中的NW.js应用打开的首页是`"index.html"`，`name` 属性设置了NW.js应用的唯一名称，更多配置属性可以查看[Manifest Format](../References/Manifest Format.md)。

!!! tip "Use JS File as Main"
    您可以将 `"main"`属性设置为像 `"main.js"`的js文件，该文件在应用启动时默认不打开窗口并在后台执行。您可以先进行一些初始化动作并稍后使用代码手动打开NWjs窗口。例如：
    ```javascript
    // 初始化您的APP，一般为一些常量定义或权限验证逻辑
    // and ...
    nw.Window.open('index.html', {}, function(win) {});
    ```

**第二步** 创建首页 `index.html`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

这是一个简单的HTML页面文件，您可以使用任何最新浏览器支持的WEB开发技术。

**第三步** 运行您的应用

```bash
cd /path/to/your/app
/path/to/nw .
```

`/path/to/nw` 是NW.js生成的二进制文件。在windows系统上,文件为`nw.exe`；在Linux系统,文件为`nw`;在Mac系统,文件为`nwjs.app/Contents/MacOS/nwjs`.

!!! tip "拖拽 &amp; Drop on Windows"
    在使用windows系统时，您可以拖拽`package.json`文件到`nw.exe`文件之上来运行应用。

### 例 2 - 使用 NW.js APIs

全部NW.js APIs都可以在任何JavaScript文件从`nw`全局对象调用。从[API References](../index.md#references)列表查看所有支持的APIs。

这个例子展示了如何在NW.js应用中创建一个右键菜单，您可以在例1中创建的`index.html`文件中加入如下代码：

```html
<!DOCTYPE html>
<html>
<head>
  <title>Context Menu</title>
</head>
<body style="width: 100%; height: 100%;">

<p>'Right click' to show context menu.</p>

<script>
// 创建一个空菜单
var menu = new nw.Menu();

// 添加一些文本选项
menu.append(new nw.MenuItem({
  label: 'Item A',
  click: function(){
    alert('You have clicked at "Item A"');
  }
}));
menu.append(new nw.MenuItem({ label: 'Item B' }));
menu.append(new nw.MenuItem({ type: 'separator' }));
menu.append(new nw.MenuItem({ label: 'Item C' }));

// 绑定"contextmenu"事件
document.body.addEventListener('contextmenu', function(ev) {
  // 阻止弹出默认菜单
  ev.preventDefault();
  // 在你点击的位置弹出自定义菜单
  menu.popup(ev.x, ev.y);

  return false;
}, false);

</script>  
</body>
</html>
```

然后运行应用:
```bash
cd /path/to/your/app
/path/to/nw .
```

!!! tip "require('nw.gui')"
	依然支持使用`require('nw.gui')`来加载NW.js APIs，同样返回`nw`对象。

### 例 3 - 使用 Node.js API

You can call node.js and modules directly from the DOM. So it enables endless possibilities for writing apps with nw.js.

这个例子展示了如何使用Node.js中的`os`模块来调用系统接口，您可以在`index.html`文件中加入如下代码：

```html
<!DOCTYPE html>
<html>
<head>
  <title>My OS Platform</title>
</head>
<body>
<script>
// 使用node.js取得系统接口
var os = require('os');
document.write('You are running on ', os.platform());
</script>
</body>
</html>
```

您可以在NW.js中使用所有由[`npm`](https://www.npmjs.com/)安装的Node.js模块。

!!! note "Native Node Modules"
    本地使用`npm install`安装的原生Node模块，并不能在NW.js中直接使用。想使用某个Node模块的话，您必须使用[`nw-gyp`](https://github.com/nwjs/nw-gyp)从该模块源代码重新编译。有关详细信息，请参阅[使用原生Node.js模块](Advanced/Use Native Node Modules.md)。

## 下一步？

查看[使用DevTools进行Debug](Debugging with DevTools.md)，了解如何调试NW.js应用。

查看[打包与分发](Package and Distribute.md)，了解如何将您的NW.js应用打包成成品并发布。

查看[FAQ](FAQ.md)，了解常见问题。

查看[the migration notes](Migration/From 0.12 to 0.13.md)，了解如何从0.12或者更老版本升级。

## 取得帮助

更多有用的信息请查看[NW.js wiki](https://github.com/nwjs/nw.js/wiki)。该wiki对所有人开放，欢迎您参与编辑分享您所知道关于NW.js的知识。

您也可以在[mail list on Google group](https://groups.google.com/forum/#!forum/nwjs-general)这个邮件列表中提出您的问题，或者在[Gitter](https://gitter.im/nwjs/nw.js)中参与讨论。

请在[GitHub](https://github.com/nwjs/nw.js/issues)上报告BUG和提交需求，使NW.js变得更强大。
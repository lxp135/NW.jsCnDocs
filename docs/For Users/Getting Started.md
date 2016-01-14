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

!!! note "Chromium核心特性"
    由于NW.js基于Chromium核心,她允许您使用Chrome浏览器所支持独特功能，比如[File System API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API), 与带`-webkit-`前缀的实验性CSS样式. **但是请小心使用这些非标准特性，非标准特性有可能会在新的版本中不被支持。**

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

This example shows how to create a native context menu in your NW.js app. You can create `index.html` with following content:
```html
<!DOCTYPE html>
<html>
<head>
  <title>Context Menu</title>
</head>
<body style="width: 100%; height: 100%;">

<p>'Right click' to show context menu.</p>

<script>
// Create an empty context menu
var menu = new nw.Menu();

// Add some items with label
menu.append(new nw.MenuItem({
  label: 'Item A',
  click: function(){
    alert('You have clicked at "Item A"');
  }
}));
menu.append(new nw.MenuItem({ label: 'Item B' }));
menu.append(new nw.MenuItem({ type: 'separator' }));
menu.append(new nw.MenuItem({ label: 'Item C' }));

// Hooks the "contextmenu" event
document.body.addEventListener('contextmenu', function(ev) {
  // Prevent showing default context menu
  ev.preventDefault();
  // Popup the native context menu at place you click
  menu.popup(ev.x, ev.y);

  return false;
}, false);

</script>  
</body>
</html>
```

... then run your app:
```bash
cd /path/to/your/app
/path/to/nw .
```

!!! tip "require('nw.gui')"
    The legacy way of loading NW.js APIs using `require('nw.gui')` is also supported. It returns the same `nw` object.

### Example 3 - Using Node.js API

You can call node.js and modules directly from the DOM. So it enables endless possibilities for writing apps with nw.js.

This example shows how to query the OS platform with `os` module of Node.js. Simpley create the `index.html` file with following content and run it with NW.js.

```html
<!DOCTYPE html>
<html>
<head>
  <title>My OS Platform</title>
</head>
<body>
<script>
// get the system platform using node.js
var os = require('os');
document.write('You are running on ', os.platform());
</script>
</body>
</html>
```

You could also use the modules installed by [`npm`](https://www.npmjs.com/) with NW.js.

!!! note "Native Node Modules"
    Native Node modules, built when running `npm install`, are not compatible with NW.js ABI. To use them, you have to rebuild it from source code with [`nw-gyp`](https://github.com/nwjs/nw-gyp). See [Use Native Node Modules](Advanced/Use Native Node Modules.md) for details.

## What's next

See [Debugging with DevTools](Debugging with DevTools.md) for debugging NW.js apps.

See [Package and Distribute](Package and Distribute.md) for packaging and reditribute your app in production.

See [FAQ](FAQ.md) for issues you may encounter.

See [the migration notes](Migration/From 0.12 to 0.13.md), if you are migrating your app from NW.js 0.12 or older versions.

## Getting Help

There are lots of useful information on [NW.js wiki](https://github.com/nwjs/nw.js/wiki). The wiki is also open for everyone, and you are encouraged to publish your knowledge on wiki.

You can also ask questions on [mail list on Google group](https://groups.google.com/forum/#!forum/nwjs-general) or chat on [Gitter](https://gitter.im/nwjs/nw.js).

Please report bugs or submit requirements on [GitHub](https://github.com/nwjs/nw.js/issues) to make NW.js more powerful.
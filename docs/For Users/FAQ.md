# FAQ 常见问题
---

[TOC]

## 在Linux终端不能使用 console.log 输出信息
在命令行设置 `--enable-logging=stderr` 参数，更多信息请参考 https://www.chromium.org/for-testers/enable-logging 文档。

## `var crypto = require('crypto')` 获得了一个错误的对象
`crypto`是Chromium所使用的全局对象，它不能被覆盖。所以您不能使用`crypto`这个变量名。改写您自定义的变量名，比如改为`nodeCrypto`即可。

## 图片在 AngularJS 中不能正确显示，在开发者工具中报 `Failed to load resource XXX net::ERR_UNKNOWN_URL_SCHEME` 错误信息
为了抵御XSS攻击AngularJS添加了 `unsafe:`未知前缀机制。NW.js和Chrome应用程序的URL都以`chrome-extension:`前缀开头，AngularJS还不能正确识别。解决的办法是使用如下代码修改AngularJS的白名单配置，添加`chrome-extension:`：

```javascript
myApp.config(['$compileProvider',
  function($compileProvider) {
    $compileProvider.imgSrcSanitizationWhitelist(/^\s*((https?|ftp|file|blob|chrome-extension):|data:image\/)/);
    $compileProvider.aHrefSanitizationWhitelist(/^\s*(https?|ftp|mailto|tel|file|chrome-extension):/);
  }]);
```

## 在 AngularJS 2 中不能正确显示异常信息
AngularJS 2试图创建名为global的全局对象作为异常处理器。然而，global变量名已被NW.js使用，故不能在开发者工具中正确显示异常信息。
在加载AngularJS库之前，重命名NW.js中的global全局对象为nw_global，空出global名称给AngularJS使用，例如：
```html
<script>
window.nw_global = window.global;
window.global = undefined;
</script>
<!-- Angular 2 Dependencies -->
```

## 如何使用ESC键退出全屏模式？
通常用户习惯使用ESC键退出全屏模式。在默认情况下，NW.js不支持 ESC 快捷键退出全屏模式。但是，给予了开发者退出全屏模式的API接口，开发者可以自己控制是否退出全屏和如何退出全屏。

例子：按 ESC 键退出全屏模式配置，更多请参考[快捷键](../References/Shortcut.md):

```javascript
nw.App.registerGlobalHotKey(new nw.Shortcut({
  key: "Escape",
  active: function () {
    // decide whether to leave fullscreen mode
    // then ...
    nw.Window.get().leaveFullscreen();
  }
}));
```
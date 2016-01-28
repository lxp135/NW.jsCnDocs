# FAQ 常见问题
---

[TOC]

## 1. `var crypto = require('crypto')` 获得了一个错误的对象
`crypto`是Chromium所使用的全局对象，它不能被覆盖。所以您不能使用`crypto`这个变量名。改写您自定义的变量名，比如改为`nodeCrypto`即可。

## 2. 在AnugarJS中图片不可用并在DevTools中可以看到`Failed to load resource XXX net::ERR_UNKNOWN_URL_SCHEME`错误信息。
为了抵御XSS攻击AngularJS添加了 `unsafe:`未知前缀机制。NW.js和Chrome应用程序的URL都以`chrome-extension:`前缀开头，AnuglarJS还不能正确识别。解决的办法是使用如下代码修改AngularJS的白名单配置添加`chrome-extension:`：

```javascript
myApp.config(['$compileProvider',
  function($compileProvider) {
    $compileProvider.imgSrcSanitizationWhitelist(/^\s*((https?|ftp|file|blob|chrome-extension):|data:image\/)/);
    $compileProvider.aHrefSanitizationWhitelist(/^\s*(https?|ftp|mailto|tel|file:chrome-extension):/);
  }]);
```



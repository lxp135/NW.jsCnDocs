# 编译版本
---

[TOC]

NW.js通过支持不同的编译配置方式来消减应用程序大小。目前NW.js支持以下编译配置方式：

* SDK版本：内建支持开发者工具与NaCl插件。 SDK版本与0.13.0之前的编译配置方式类似。
* 常规版本：最小编译版本，不支持开发者工具和NaCl插件。

在编码过程中，你可以代码中使用 `process.versions['nw-flavor']` 方法来查看当前应用运行在哪个编译版本。

查看 [Build Flavors section in Building NW.js](../../For Developers/Building NW.js.md#build-flavors) 教你如何从源码编译。

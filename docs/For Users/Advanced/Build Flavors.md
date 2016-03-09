# Build Flavors
---

[TOC]

NW.js通过支持不同的编译配置方式来消减应用程序大小。目前NW.js支持以下编译配置方式：

* SDK flavor：内建支持DevTools调试工具与NaCl插件。 SDK flavor与0.13.0之前的编译配置方式类似。
* NaCL flavor：支持 Native Client (NaCl) 插件，但是不包含DevTools调试工具。
* Normal flavor：最小编译版本，不支持DevTools调试工具和NaCl插件。

查看 [Build Flavors section in Building NW.js](../../For Developers/Building NW.js.md#build-flavors) 教你如何从源码编译。
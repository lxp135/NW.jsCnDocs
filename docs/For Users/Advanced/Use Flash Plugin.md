# 在NW.js中使用Flash插件
---

[TOC]

NW.js支持Pepper Flash插件。Just put the plugin files (including 'manifest.json' of it) in the 'PepperFlash' subdirectory under NW.js binary. On Mac OSX, the subdirectory should be in the "Internet Plug-Ins" directory in the framework bundle.

Flash插件加载过程可以使用 `/path/to/nwjs --url='chrome://plugins'` 命令验证。
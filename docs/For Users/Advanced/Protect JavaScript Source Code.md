# 保护JavaScript源代码
---

[TOC]

你的应用中的所有JavaScript源代码可以通过编译为机器代码的方式加密，NW.js可以正确读取编译后的JavaScript源代码。你在发布应用的时候可以仅加载编译后的机器代码而不需要源代码。

### 编译

使用`nwjc`工具来编译你的JS源代码为机器代码，which is provided in the binary download.

开始使用:
```bash
nwjc source.js binary.bin
```

编译后`*.bin`文件必须包含在你发布的应用中，有必要的话你可以重新命名该文件。

### 加载编译后的JavaScript代码

```javascript
nw.Window.get().evalNWBin(frame, 'binary.bin');
```
The arguments of the [win.evalNWBin()](../../References/Window.md#winevalnwbin) method are similar with the `Window.eval()` method, where the first parameter is the target iframe (`null` for main frame), and the 2nd parameter is the binary code file.

!!! note "注意"
	编译后的机器代码在[Browser Context](JavaScript Contexts in NW.js.md#browser-context)中执行。你可以像使用其他脚本一样使用任何Web APIs（例如DOM）和[access NW.js API and Node API](JavaScript Contexts in NW.js.md#access-nodejs-and-nwjs-api-in-browser-context)运行在[Browser Context](JavaScript Contexts in NW.js.md#browser-context)中。

### 已知问题

在v8引擎中，编译后的机器代码执行效率会**降低大概30%左右**，其他没编译的JS代码不受影响。

编译后的机器代码是**不能跨平台的**，版本之间并不兼容。所以，你需要在每个平台执行`nwjc`来编译对应不同平台的机器代码。

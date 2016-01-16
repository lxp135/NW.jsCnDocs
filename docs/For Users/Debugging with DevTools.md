# 使用DevTools调试
---

!!! note "SDK版本特性"
    DevTools仅在[SDK flavor](Advanced/Build Flavors.md)版本中工作，建议您使用SDK版本进行开发与调试NW.js应用。使用其他版本发布应用。

## 如何打开DevTools调试工具

您可以在Windows、Linux和<kbd>&#8984;</kbd>+<kbd>&#8997;</kbd>+<kbd>i</kbd>上使用快捷键<kbd>F12</kbd>调出DevTools。

你还可以使用DevTools来调试你所使用的NW.js 窗口API [win.showDevTools()`](../References/Window.md##winshowdevtoolsiframe-headless-callback)。

## 远程调试

您可以在命令行输入`--remote-debugging-port=port`并指定端口来启动DevTools监听。举个例子，输入并运行`nw --remote-debugging-port=9222`，可以在你自己的浏览器中输入网址http://localhost:9222/来进行远程调试。
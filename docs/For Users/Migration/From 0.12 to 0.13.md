# 从0.12升级到0.13
---

[TOC]

## 架构变化

+ NW.js应用运行于Chrome内核之上，所有Chrome支持的API与特性NW.js同样支持。默认协议地址从 `file://` 改为 `chrome-extension://`，where the host part of the URL is the generated id. 在0.12中使用 `app://` 的地方改为使用 `chrome-extension://` 。
+ 全部 NW 独有的 API, 原使用 `require()` 的情况现在需要从 `nw.gui` 库中new一个 `nw` 对象来使用。 无论如何, 我们提供了一个内建的库来兼容使用0.12开发的应用。在0.14或更迟的版本我们不建议使用之后，您需要使用`nw.gui`库。
+ The Node.js context is put in the DOM context of the background page, which is shared between opening windows as in 0.12 and before. The difference is you have access to all DOM features and chrome.* platform APIs in the Node context in 0.13.
+ To debug Node.js modules, you have to open DevTools for background page in Separate Context Mode by default. See [Debugging with DevTools](../Debugging with DevTools.md) for details.
+ The entry of the application is either JS or HTML as in 0.12, but as the application is internally a Chrome App, the first window is supposed to be launched by JS from the background page. If you specify a HTML file as the entry with "main" field in package.json, NW will use a default JS to open the first window and load it.
+ If NW.js is running under [Mixed Context Mode](../Advanced/JavaScript Contexts in NW.js.md#mixed-context-mode) (boot NW.js with `--mixed-context` argument), `nw.*` is kind of mirror of `window.*`. In this mode, you **CANNOT** share variables among frames or windows by assigning it to Node context. So do **NOT** turn on Mixed Context mode if your application is heavily depending on this variable sharing feature.

## Node.js 变化

+ Node.js is bumped to 6.x in latest build. Check your NPM modules to make sure they support Node.js 6.x **especially for native modules**. There is [a list of native modules](https://github.com/nodejs/node/issues/2798) which should be migrated to latest [NaN 2](https://github.com/nodejs/nan).
+ Add NW version information to process.versions[`nw`]. process.versions[`node-webkit`] will be deprecated later.

## API 变化

### 编译版本

+ 不同的编译版本支持不同的API与功能，可以通过查看 [编译版本](../Advanced/Build Flavors.md) 文档选择合适的NW.js版本，或者查看 [构建 NW.js](../../For Developers/Building NW.js.md) 文档自己编译NW.js。

### 快捷键

+ `Shortcut` API does **NOT** map <kbd>Ctrl</kbd> modifier to <kbd>&#8984;</kbd> on Mac OS X. However 0.13.0 supports `Command` modifier in cross platform way. So it's your responsible to detect the OS and choose the right modifier when registering hotkeys. See [Shortcut.key](../../References/Shortcut.md#shortcutkey) for details.

### 菜单
+ Menus on Mac is created with default menubar, including `app-name`, `Edit` and `Window`, instead of minimal menubar in 0.12.
+ To fix the name of application menu, you will need to modify  `nwjs.app/Contents/Resources/en.lproj/InfoPlist.strings` instead of `nwjs.app/Contents/Info.plist`. See [Customize Menubar](../Advanced/Customize Menubar.md#mac-os-x).

### Manifest Format

+ [`single-instance`](../../References/Manifest Format.md#single-instance) 选项已 **弃用** ，现在这个选项永远是 `true`。 您 **不能** 再使用多个 instances for your app unless you're using different user data directory (by `--user-data-dir`). You may also want to use the [open event](../../References/App.md#event-openargs): the first instance will be notified with this event when user tries to launch the second instance.
+ [`toolbar`](../../References/Manifest Format.md#toolbar) 选项已 **弃用** 现在这个选项永远是 `false`。The traditional toolbar will **NOT** be supported including the reload buttons, location bar and DevTools buttons. As a workaround, you can open / close DevTools with <kbd>F12</kbd> (Windows & Linux) or <kbd>&#8984;</kbd>+<kbd>&#8997;</kbd>+<kbd>i</kbd> (Mac). And use [`win.reload()`](../../References/Window.md#winreload) and [`win.reloadDev()`](../../References/Window.md#winreloaddev) to simulate the reload buttons.
+ [`no-edit-menu`](../../References/Manifest Format.md#no-edit-menu-mac) is **deprecated**.
+ `snapshot` is **deprecated**. Use [`win.evalNWBin()`](../../References/Window.md#winevalnwbinframe-path) instead.
+ The format of [`node-remote`](../../References/Manifest Format.md#node-remote) is changed to array of [match patterns](https://developer.chrome.com/extensions/match_patterns) used by Chrome extension.
+ Window options `always-on-top`, `visible-on-all-workspaces`, `new-instance`, `inject-js-start` and `inject-js-end` is renamed to [`always_on_top`](../../References/Manifest Format.md#always_on_top), [`visible_on_all_workspaces`](../../References/Manifest Format.md#visible_on_all_workspaces-mac-linux), `new_instance`, `inject_js_start` and `inject_js_end` respectively in `package.json` or as argument of `Window.open()`.
+ `--data-path` command line argument is replaced by `--user-data-dir`.

### Window

+ There is an `id` to identify each window. This will be used to remember the size and position of the window and restore that geometry when a window with the same `id` is later opened. It could be specified in [Window.open](../../References/Window.md#windowopenurl-options-callback) or [`id` of window subfields in manifest](../../References/Manifest Format.md#id)
+ Event [`capturepagedone`](../../References/Window.md#event-capturepagedone) of `Window` API is **deprecated**.Use the callback with the [`win.capturePage(callback [, config ])`](../../References/Window.md#wincapturepagecallback-config) instead.
+ [Window.open](../../References/Window.md#windowopenurl-options-callback) is changed to passing the created window as the argument of the callback.
+ [Window.showDevtools](../../References/Window.md#winshowdevtoolsiframe-callback) is changed to passing the created window as the argument of the callback.
+ [win.setTransparent](../../References/Window.md#winsettransparenttransparent) is **deprecated**. You can't change the transparency after window is created.
+ `unmaximize` and [`leave-fullscreen`](../../References/Window.md#event-leave-fullscreen) events of `Window` object is **deprecated** and replaced by [restore](../../References/Window.md#event-restore). When window is restored from minimized, maximized or fullscreen, `restore` event is triggered instead.
+ Window options `always-on-top` and `visible-on-all-workspaces` is renamed to [`always_on_top`](../../References/Manifest Format.md#always_on_top) and [`visible_on_all_workspaces`](../../References/Manifest Format.md#visible_on_all_workspaces-mac-linux) respectively in `package.json` or as argument of `Window.open()`.
+ Window is not inherited from `EventEmitter` anymore, but the methods `on()`, `once()`, `removeListener()` and `removeAllListeners()` are still supported.

### Screen

+ The `id` obtained by `added`, `orderchanged`, `namechanged`, `thumbnailchanged` should be registered and use the stream id returned by [`registerStream(id)`](../../References/Screen.md#screendesktopcapturemonitorregisterstreamid) before passing to `getUserMedia`. See [Synopsis](../../References/Screen.md#synopsis_1) for the usage.

### 已知问题

+ The following window options passed to `nw.Window.open()` is not effective on **Linux**: `resizable` for now; try to set them in the callback.
+ `reloadDev()` and `isDevToolsOpen()` of `nw.Window` are not supported for now
+ `closed` event of `nw.Window`: `App.quit()` doesn't trigger this event.
+ `devtools-closed` event of `nw.Window` is not supported for now.
+ `as_desktop` option is not supported for now
+ `webkit.{plugin|java|page-cache}` option in `package.json` is not supported for now: plugins are enabled by default.
+ `nwUserAgent` attribute of `<iframe>` is not supported for now.
+ `tooltip` of `MenuItem` is not supported for now.
+ `nw.App.setCrashDumpDir()` is not supported; crash dump is stored in `app-data-path/Crash Reports`

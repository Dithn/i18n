# 鍵盤快速鍵

## 概述

This feature allows you to configure local and global keyboard shortcuts for your Electron application.

## 範例

### 局域快速鍵

Local keyboard shortcuts are triggered only when the application is focused. To configure a local keyboard shortcut, you need to specify an [`accelerator`] property when creating a [MenuItem](../api/menu-item.md) within the [Menu](../api/menu.md) module.

Starting with a working application from the [Quick Start Guide](quick-start.md), update the `main.js` file with the following lines:

```javascript fiddle='docs/fiddles/features/keyboard-shortcuts/local'
const { Menu, MenuItem } = require('electron')

const menu = new Menu()
menu.append(new MenuItem({
  label: 'Electron',
  submenu: [{
    role: 'help',
    accelerator: process.platform === 'darwin' ? 'Alt+Cmd+I' : 'Alt+Shift+I',
    click: () => { console.log('Electron rocks!') }
  }]
}))

Menu.setApplicationMenu(menu)
```

> NOTE: In the code above, you can see that the accelerator differs based on the user's operating system. For MacOS, it is `Alt+Cmd+I`, whereas for Linux and Windows, it is `Alt+Shift+I`.

After launching the Electron application, you should see the application menu along with the local shortcut you just defined:

![Menu with a local shortcut](../images/local-shortcut.png)

If you click `Help` or press the defined accelerator and then open the terminal that you ran your Electron application from, you will see the message that was generated after triggering the `click` event: "Electron rocks!".

### 全域快速鍵

To configure a global keyboard shortcut, you need to use the [globalShortcut](../api/global-shortcut.md) module to detect keyboard events even when the application does not have keyboard focus.

Starting with a working application from the [Quick Start Guide](quick-start.md), update the `main.js` file with the following lines:

```javascript fiddle='docs/fiddles/features/keyboard-shortcuts/global'
const { app, globalShortcut } = require('electron')

app.whenReady().then(() => {
  globalShortcut.register('Alt+CommandOrControl+I', () => {
    console.log('Electron loves global shortcuts!')
  })
}).then(createWindow)
```

> NOTE: In the code above, the `CommandOrControl` combination uses `Command` on macOS and `Control` on Windows/Linux.

After launching the Electron application, if you press the defined key combination then open the terminal that you ran your Electron application from, you will see that Electron loves global shortcuts!

### BrowserWindow 內的快速鍵

#### Using web APIs

If you want to handle keyboard shortcuts within a [BrowserWindow](../api/browser-window.md), you can listen for the `keyup` and `keydown` [DOM events](https://developer.mozilla.org/en-US/docs/Web/Events) inside the renderer process using the [addEventListener() API](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener).

```js
window.addEventListener('keyup', doSomething, true)
```

Note the third parameter `true` indicates that the listener will always receive key presses before other listeners so they can't have `stopPropagation()` called on them.

#### Intercepting events in the main process

網頁中的 `keydown` 及 `keyup` 事件觸發前，會先送出 [`before-input-event`](../api/web-contents.md#event-before-input-event) 事件。 可用來攔截、處理沒出現在選單上的自訂快捷鍵。

##### 範例

Starting with a working application from the [Quick Start Guide](quick-start.md), update the `main.js` file with the following lines:

```javascript fiddle='docs/fiddles/features/keyboard-shortcuts/interception-from-main'
const { app, BrowserWindow } = require('electron')

app.whenReady().then(() => {
  const win = new BrowserWindow({ width: 800, height: 600, webPreferences: { nodeIntegration: true } })

  win.loadFile('index.html')
  win.webContents.on('before-input-event', (event, input) => {
    if (input.control && input.key.toLowerCase() === 'i') {
      console.log('Pressed Control+I')
      event.preventDefault()
    }
  })
})
```

After launching the Electron application, if you open the terminal that you ran your Electron application from and press `Ctrl+I` key combination, you will see that this key combination was successfully intercepted.

#### Using third-party libraries

If you don't want to do manual shortcut parsing, there are libraries that do advanced key detection, such as [mousetrap](https://github.com/ccampbell/mousetrap). Below are examples of usage of the `mousetrap` running in the Renderer process:

```js
Mousetrap.bind('4', () => { console.log('4') })
Mousetrap.bind('?', () => { console.log('顥示快速鍵!') })
Mousetrap.bind('esc', () => { console.log('離開') }, 'keyup')

// 組合鍵
Mousetrap.bind('command+shift+k', () => { console.log('command shift k') })

// 將不同的組合鍵對應到同一個 callback
Mousetrap.bind(['command+k', 'ctrl+k'], () => {
  console.log('command k or control k')

  // 回傳 false 防止預設行為被觸發，並避免事件向外傳遞
  return false
})

// gmail 風格的按鍵組合
Mousetrap.bind('g i', () => { console.log('回收件匣') })
Mousetrap.bind('* a', () => { console.log('全選') })

// konami 秘技!
Mousetrap.bind('up up down down left right left right b a enter', () => {
  console.log('konami 秘技')
})
```
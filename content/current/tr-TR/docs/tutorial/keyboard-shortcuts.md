# Klavye Kısayolları

## Genel Bakış

Bu özellik, Electron uygulamanızın yerel ve genel klavye kısayollarını ayarlamanıza izin verir.

## Örnek

### Yerel kısayollar

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

### Genel kısayollar

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

### BrowserWindow içindeki kısayollar

#### Using web APIs

If you want to handle keyboard shortcuts within a [BrowserWindow](../api/browser-window.md), you can listen for the `keyup` and `keydown` [DOM events](https://developer.mozilla.org/en-US/docs/Web/Events) inside the renderer process using the [addEventListener() API](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener).

```js
window.addEventListener('keyup', doSomething, true)
```

Note the third parameter `true` indicates that the listener will always receive key presses before other listeners so they can't have `stopPropagation()` called on them.

#### Intercepting events in the main process

[`before-input-event`](../api/web-contents.md#event-before-input-event) etkinliği, sayfaya `keydown` ve `keyup` etkinlikleri gönderilmeden önce yayınlanır. Bu olabilir menüde görünmeyen özel kısayolları yakalamak ve işlemek için kullanılabilir.

##### Örnek

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
Mousetrap.bind('?', () => { console.log('show shortcuts!') })
Mousetrap.bind('esc', () => { console.log('escape') }, 'keyup')

// combinations
Mousetrap.bind('command+shift+k', () => { console.log('command shift k') })

// Aynı callback için birden fazla komut belirle
Mousetrap.bind(['command+k', 'ctrl+k'], () => {
  console.log('command k veya control k')

  // return false vererek olayı durdurma ve birikmesini önlemek
  return false
})

// gmail şeklindeki prosedürler
Mousetrap.bind('g i', () => { console.log('gelen kutusuna git') })
Mousetrap.bind('* a', () => { console.log('hepsini seç') })

// konami kodu!
Mousetrap.bind('yukarı aşağı sola sağa sola b a girin', () => {
  console.log('konami code')
})
```
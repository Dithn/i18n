# Bezpečnost, národní schopnosti a Vaše odpovědnost

jako vývojáři webu, Obvykle se těší silné bezpečnostní síti prohlížeče - rizika spojená s kódem, který píšeme, jsou relativně malá. Naše internetové stránky jsou v pískovce uděleny omezené pravomoci, a věříme, že naši uživatelé požívají prohlížeče vytvořeného velkým týmem inženýrů, který je schopen rychle reagovat na nově objevené bezpečnostní hrozby.

Při práci s Electronem je důležité pochopit, že Electron není webový prohlížeč. Umožňuje vytvářet funkční desktopové aplikace s dobře známými webovými technologiemi, ale váš kód má mnohem větší energii. JavaScript má přístup k souborovému systému, uživatelskému shellu a dalším. To vám umožní vybudovat vysoce kvalitní nativní aplikace, ale vlastní stupeň bezpečnostních rizik s dodatečnými pravomocemi udělenými vašemu kódu.

S ohledem na to být si vědomi toho, že zobrazení libovolného obsahu z nedůvěryhodných zdrojů představuje vážné bezpečnostní riziko, které Electron nemá řešit. Nejpopulárnější Electron aplikace (Atom, Slack, Visual Studio Code, atd.) zobrazují především lokální obsah (nebo důvěryhodný, bezpečný vzdálený obsah bez integrace s uzlem ) – pokud vaše aplikace spustí kód z online zdroje, je vaší odpovědností zajistit, aby kód nebyl zlovolný.

## Hlášení bezpečnostních problémů

Informace o tom, jak správně zveřejnit zranitelnost Electronu, viz [SECURITY.md](https://github.com/electron/electron/tree/master/SECURITY.md)

## Problémy a vylepšení zabezpečení Chromu

Electron průběžně aktualizuje uvolňování střídavého chromu. Více informací najdete [Electron Release Cadence blog post](https://electronjs.org/blog/12-week-cadence).

## Bezpečnost je zodpovědná všem

Je důležité mít na paměti, že bezpečnost vaší Electron aplikace je výsledkem celkové bezpečnosti rámcové základny (*Chromium*, *Uzel. s*), Electron samotný, všechny NPM závislosti a váš kód. Je proto vaší odpovědností řídit se několika důležitými nejlepšími postupy:

* **Udržujte vaši aplikaci aktuální s nejnovější verzí Electron frameworku.** Při vydávání vašeho produktu také odesíláte balíček složený z Electronu, sdílené knihovny a Node.js. Zranitelnost ovlivňující tyto komponenty může mít vliv na bezpečnost vaší aplikace. Aktualizací Electron na nejnovější verzi, zajistíte, že kritická zranitelná místa (jako např. *nodeIntegration obejde*) jsou již upravena a nemohou být využita ve vaší aplikaci. Pro více informací viz "[Použijte aktuální verzi Electron](#17-use-a-current-version-of-electron)".

* **Vyhodnoťte své závislosti.** Zatímco NPM poskytuje půl milionu opakovaně použitelných balíčků, je vaší odpovědností zvolit důvěryhodné knihovny třetích stran. Pokud používáte zastaralé knihovny ovlivněné známými slabými místy nebo spoléháte na špatně udržovaný kód, může být ohrožena bezpečnost vaší aplikace.

* **Přijmout bezpečné postupy kódování.** První obranná linka pro vaši aplikaci je tvůj vlastní kód. společné slabé stránky jako například Cross-Site Scripting (XSS), mají větší vliv na bezpečnost aplikací Electron, proto je velmi doporučeno přijmout osvědčené postupy vývoje bezpečného softwaru a provádět bezpečnostní testy.


## Izolace pro nedůvěryhodný obsah

Bezpečnostní problém existuje vždy, když obdržíte kód z nedůvěryhodného zdroje (např. vzdáleného serveru) a proveďte jej lokálně. As an example, consider a remote website being displayed inside a default [`BrowserWindow`][browser-window]. Pokud útočník nějakým způsobem změní zmíněný obsah (buď útokem na zdroj, nebo tím, že sedí mezi vaší aplikací a skutečným cílem) budou moci spustit nativní kód na počítači uživatele.

> :warning: byste neměli načíst a spustit vzdálený kód s integrací Node.js. Namísto toho použijte pouze lokální soubory (balené společně s vaší aplikací) pro spuštění Node.js kódu. To display remote content, use the [`<webview>`][webview-tag] tag or [`BrowserView`][browser-view], make sure to disable the `nodeIntegration` and enable `contextIsolation`.

## Upozornění na elektronické zabezpečení

Od Electronu 2.0 uvidí vývojáři vytištěná varování a doporučení do vývojářské konzoly. Zobrazují se pouze tehdy, když je binární název Electron, označující, že vývojář v současné době hledá konzoli.

Tyto výstrahy můžete vynutit nebo vynutit nastavením `ELECTRON_ENABLE_SECURITY_WARNINGS` nebo `ELECTRON_DISABLE_SECURITY_WARNINGS` v procesu `. nv` nebo `okno` objekt.

## Kontrolní seznam: Bezpečnostní doporučení

Alespoň byste měli postupovat podle těchto kroků, abyste zlepšili bezpečnost vaší aplikace:

1. [Načíst pouze zabezpečený obsah](#1-only-load-secure-content)
2. [Zakázat integraci Node.js ve všech přehrávačích, které zobrazují vzdálený obsah](#2-do-not-enable-nodejs-integration-for-remote-content)
3. [Povolit izolaci kontextů ve všech přehrávačích, které zobrazují vzdálený obsah](#3-enable-context-isolation-for-remote-content)
4. [Použít `ses.setPermissionRequestHandler()` ve všech relacích, které načítají vzdálený obsah](#4-handle-session-permission-requests-from-remote-content)
5. [Zakázat `webSecurity`](#5-do-not-disable-websecurity)
6. [Definujte `Content-Security-Policy`](#6-define-a-content-security-policy) a použijte restriktivní pravidla (tj. `script-src 'self'`)
7. [Nenastavovat `allowRunningInsecureContent` na `true`](#7-do-not-set-allowrunninginsecurecontent-to-true)
8. [Nepovolit experimentální funkce](#8-do-not-enable-experimental-features)
9. [Nepoužívat `enableBlinkFeatures`](#9-do-not-use-enableblinkfeatures)
10. [`<webview>`: Nepoužívejte `povolené popupy`](#10-do-not-use-allowpopups)
11. [`<webview>`: Ověřte možnosti a parametry](#11-verify-webview-options-before-creation)
12. [Zakázat nebo omezit navigaci](#12-disable-or-limit-navigation)
13. [Zakázat nebo omezit vytváření nových oken](#13-disable-or-limit-creation-of-new-windows)
14. [Nepoužívejte `openExter` s nedůvěryhodným obsahem](#14-do-not-use-openexternal-with-untrusted-content)
15. [Zakázat vzdálený `` modul](#15-disable-the-remote-module)
16. [Filtrovat `vzdálený` modul](#16-filter-the-remote-module)
17. [Použít aktuální verzi Electronu](#17-use-a-current-version-of-electron)

Chcete-li automatizovat detekci chybných konfigurací a nezabezpečených vzorů, je možné použít [elektronegativitu](https://github.com/doyensec/electronegativity). Pro další podrobnosti o možných slabinách a chybách při implementaci při vývoji aplikací pomocí Electronu, prosím, podívejte se na tuto [příručku pro vývojáře a auditory](https://doyensec.com/resources/us-17-Carettoni-Electronegativity-A-Study-Of-Electron-Security-wp.pdf)

## 1) Načíst pouze bezpečný obsah

Veškeré zdroje nezahrnuté do vaší aplikace by měly být načteny pomocí zabezpečeného protokolu jako `HTTPS`. Jinými slovy, nepoužívejte nezabezpečené protokoly jako `HTTP`. Podobně doporučujeme používat `WSS` přes `WS`, `FTPS` po `FTP`a tak dále.

### Proč?

`HTTPS` má tři hlavní výhody:

1) Ověřuje vzdálený server, zajišťuje, že se aplikace připojí ke správnému hostiteli namísto impersonátoru. 2) Zajišťuje integritu dat a tvrdí, že data nebyla upravena během přenosu mezi vaší aplikací a hostitelem. 3) Šifruje přenos mezi uživatelem a cílovým hostitelem, ztěžuje odposlechu informací odeslaných mezi vaší aplikací a hostitelem.

### Jak?

```js
// Špatný
browserWindow.loadURL('http://example.com')

// Good
browserWindow.loadURL('https://example.com')
```

```html<!-- Špatné --><script crossorigin src="http://example.com/react.js"></script>
<link rel="stylesheet" href="http://example.com/style.css">

<!-- Good -->
<script crossorigin src="https://example.com/react.js"></script>
<link rel="stylesheet" href="https://example.com/style.css">
```


## 2) Nepovolovat integraci Node.js pro vzdálený obsah

_Toto doporučení je výchozí chování v Electronu od 5.0.0._

It is paramount that you do not enable Node.js integration in any renderer ([`BrowserWindow`][browser-window], [`BrowserView`][browser-view], or [`<webview>`][webview-tag]) that loads remote content. Cílem je omezit pravomoci, které udělujete na vzdálený obsah, tak je dramaticky obtížnější pro útočníka, aby poškodil vaše uživatele, pokud získají schopnost spustit JavaScript na vašich webových stránkách.

Poté můžete určitým hostitelům udělit další oprávnění. Například, pokud otevíráte BrowserWindow na `https://example. om/`, můžete dát webové stránce přesně ty schopnosti, které potřebuje, ale už více.

### Proč?

Útok XSS) přes stránky je nebezpečnější, pokud útočník může vyskočit z procesu vykreslování a spustit kód na počítači uživatele. Útoky napříč místy jsou poměrně běžné - a zatímco problém, jejich výkon je obvykle omezen na zasílání zpráv na webových stránkách, na kterých jsou spuštěny. Vypnutí integrace Node.js pomáhá zabránit eskalaci XSS na tzv. útok "Remote Code Execution" (RCE).

### Jak?

```js
// Špatné
const mainWindow = nový BrowserWindow({
  webPreferences: {
    nodeIntegration: true,
    nodeIntegrationInWorker: true
  }
})

mainWindow.loadURL('https://example.com')
```

```js
// Good
const mainWindow = new BrowserWindow({
  webPreferences: {
    preload: path.join(app.getAppPath(), 'preload.js')
  }
})

mainWindow.loadURL('https://example.com')
```

```html<!-- Špatné --><webview nodeIntegration src="page.html"></webview><!-- Dobré --><webview src="page.html"></webview>
```

Při zakázání integrace Node.js můžete stále vystavit API vašim webům, které spotřebovávají moduly nebo funkce Node.js. Přednačtené skripty mají i nadále přístup k `vyžadují` a další uzel. funkce umožňující vývojářům vystavit vlastní API vzdálenému načtenému obsahu.

V následujícím příkladu skriptu bude mít později načtený web přístup k metodě `window.readConfig()` , ale žádné funkce Node.js.

```js
const { readFileSync } = require('fs')

window.readConfig = funkce () {
  const data = readFileSync('./config.json')
  return data
}
```


## 3) Povolit kontextovou izolaci pro vzdálený obsah

Kontextová izolace je funkce Electron, která umožňuje vývojářům spustit kód v přednažených skriptech a v Electron API v vyhrazeném kontextu JavaScriptu. V praxi to znamená, že globální objekty jako `Array.prototype. ush` nebo `JSON.parse` nemůže být upraven skripty běžícími v procesu renderer.

Electron používá stejnou technologii jako Chromium [Content skripts](https://developer.chrome.com/extensions/content_scripts#execution-environment) pro povolení tohoto chování.

I když používáte `nodeIntegration: false` k prosazování silné izolace a zabraňují použití primitivních uzlů, `musí být také použity kontextIzolace`.

### Proč?

Kontextová izolace umožňuje každému ze skriptů, které jsou spuštěny v přehrávači, provádět změny ve svém prostředí JavaScriptu bez obav, že by byly v rozporu s skripty v Electron API nebo skriptu před načtením.

Zatímco je stále experimentální funkce Electronu, izolace přidává další vrstvu bezpečnosti. Vytváří nový JavaScript svět pro Electron API a preload skripty, který zmírňuje takzvané útoky "Prototype Pollution".

At the same time, preload scripts still have access to the  `document` and `window` objects. In other words, you're getting a decent return on a likely very small investment.

### Jak?

```js
// Main process
const mainWindow = new BrowserWindow({
  webPreferences: {
    contextIsolation: true,
    preload: path.join(app.getAppPath(), 'preload.js')
  }
})
```

```js
// Přednačte skript

// Nastavte proměnnou na stránce před jejím načtením
webFrame.executeJavaScript('window. oo = "foo"; )

// Nahraná stránka nebude mít přístup k tomuto nastavení, je k dispozici pouze
// v tomto kontextu
okno. dokument ar = 'bar'

. ddEventListener('DOMContentLoaded', () => {
  // Will log out 'undefined' od okna. oo je dostupné pouze v hlavní konzoli
  // context
  . og(window.foo)

  // Bude odhlásit 'bar', protože window.bar je v tomto kontextu k dispozici
  console.log(window.bar)
})
```


## 4) Zpracovávat žádosti o oprávnění relace z vzdáleného obsahu

Možná jste při používání Chromeu viděli žádosti o oprávnění: Vyskakují pokaždé, když se stránka pokusí použít funkci, kterou musí uživatel ručně schválit ( jako oznámení).

API je založeno na [Chromium permissions API](https://developer.chrome.com/extensions/permissions) a implementuje stejné typy oprávnění.

### Proč?

Ve výchozím nastavení Electron automaticky schválí všechny žádosti o povolení, pokud vývojář nemá manuálně nakonfigurovaný vlastní obsluhovatele. Zatímco solidní výchozí hodnota, vývojáři si vědomi bezpečnosti by mohli chtít předpokládat pravý opak.

### Jak?

```js
const { session } = require('electron')

session
  .fromPartition('some-partition')
  . etPermissionRequestHandler(((webContents, permission, callback) => {
    const url = webobsah. etURL()

    if (permission === 'notifications') {
      // approves the permissions request
      callback(true)
    }

    // Verify URL
    if (! rl. tartsWith('https://example. om/')) {
      // Denies request
      return callback(false)
    }
})
```


## 5) Zakázat WebSecurity

_Doporučení je výchozí_

You may have already guessed that disabling the `webSecurity` property on a renderer process ([`BrowserWindow`][browser-window], [`BrowserView`][browser-view], or [`<webview>`][webview-tag]) disables crucial security features.

Ve produkčních aplikacích nevypínejte `webSecurity`.

### Proč?

Zakázáním `webSecurity` zakážete politiku stejného původu a nastavíte `allowRunningInsecureContent` na `true`. Jinými slovy, umožňuje provedení nezabezpečeného kódu z různých domén.

### Jak?
```js
// Špatné
const mainWindow = nový BrowserWindow({
  webPreference: {
    webSecurity: false
  }
})
```

```js
// Good
const mainWindow = nový prohlížeč Window()
```

```html<!-- Špatné --><webview disablewebsecurity src="page.html"></webview><!-- Dobré --><webview src="page.html"></webview>
```


## 6) Definujte bezpečnostní politiku obsahu

Bezpečnostní politika obsahu (CSP) je další vrstvou ochrany proti útokům přes stránky a útokům při injektáži dat. Doporučujeme je povolit na libovolných webových stránkách, které nahrajete v Electronu.

### Proč?

CSP umožňuje serveru obsluhujícímu obsah omezit a ovládat zdroje Electron se může načíst pro danou webovou stránku. `https://example.com` by mělo být povoleno načíst skripty z původu, který jste definovali během skriptů z `https://evil. ttacker.com` by nemělo být dovoleno běžet. Definování CSP je snadný způsob, jak zlepšit bezpečnost vaší aplikace.

Následující CSP umožní Electronu spustit skripty z aktuální webové stránky a z `apis.example.com`.

```plaintext
// Bad
Content-Security-Policy: '*'

// Good
Content-Security-Policy: script-src 'self' https://apis.example.com
```

### Hlavička CSP HTTP

Electron respektuje hlavičku [`Content-Security-Policy` HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) , kterou lze nastavit pomocí Electronu [`webRequest.onHeadersReceived`](../api/web-request.md#webrequestonheadersreceivedfilter-listener) handler:

```javascript
const { session } = require('electron')

session.defaultSession.webRequest. nHeadersReceived((detaily, callback) => {
  callback({
    responseHeaders: {
      . .details.responseheaders,
      'Content-Security-Policy': ['default-src \'none\'']
    }
  })
})
```

### CSP Meta Tag

Upřednostňovaným dodacím mechanismem ověřovatele je HTTP hlavička, však není možné použít tuto metodu při načítání zdroje pomocí `souboru://` protokolu. V některých případech může být užitečný, například pomocí `souboru://` protokolu, pro nastavení zásad na stránce přímo v značce pomocí značky `<meta>`:

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'none'">
```


## 7) Nenastavujte `allowRunningInsecureContent` na `true`

_Doporučení je výchozí_

Ve výchozím nastavení Electron neumožní načíst webové stránky načtené přes `HTTPS` a spustit skripty, CSS, nebo pluginy z nezabezpečených zdrojů (`HTTP`). Nastavením vlastnosti `povolit RunningInsecureContent` na `true` zakáže tuto ochranu.

Načítání počátečního HTML webu přes `HTTPS` a pokusu o načtení následných zdrojů přes `HTTP` je také známo jako "smíšený obsah".

### Proč?

Načítání obsahu přes `HTTPS` zajišťuje autentičnost a integritu načtených zdrojů při šifrování samotného provozu. Více informací naleznete v části na [zobrazující pouze bezpečný obsah](#1-only-load-secure-content).

### Jak?

```js
// Špatné
const mainWindow = nový BrowserWindow({
  webPreference: {
    allowRunningInsecureContent: true
  }
})
```

```js
// Good
const mainWindow = new BrowserWindow({})
```


## 8) Nezapínejte experimentální funkce

_Doporučení je výchozí_

Pokročilí uživatelé Electronu mohou povolit experimentální funkce Chromium pomocí vlastnosti `experimentalFeatures`.

### Proč?

Experimentální funkce jsou, jak naznačuje název, experimentální a nebyly povoleny pro všechny uživatele chromu. Navíc jejich dopad na Electron jako celek pravděpodobně nebyl testován.

Legitimní případy použití existují, ale pokud nevíte, co děláte, byste tuto vlastnost neměli povolit.

### Jak?

```js
// Špatné
const mainWindow = nový BrowserWindow({
  webPreference: {
    experimentalFeatures: true
  }
})
```

```js
// Good
const mainWindow = new BrowserWindow({})
```


## 9) Nepoužívat `enableBlinkFeatures`

_Doporučení je výchozí_

Blink je název vykreslujícího motoru za Chromiem. Stejně jako v `experimentalFeatures`umožňuje vlastnosti `enableBlinkFeatures` vývojářům povolit vlastnosti, které byly ve výchozím nastavení zakázány.

### Proč?

Obecně řečeno, existují pravděpodobně dobré důvody, pokud funkce nebyla ve výchozím nastavení povolena . Existují případy oprávněného použití pro povolení určitých funkcí. Jako vývojář byste měli přesně vědět, proč potřebujete povolit funkci, jaké dopady jsou a jak to ovlivňuje bezpečnost vaší žádosti. Za žádné okolnosti byste neměli povolit funkce spekulativně.

### Jak?
```js
// Špatné
const mainWindow = nový BrowserWindow({
  webPreference: {
    enableBlinkFeatures: 'ExecCommandInJavaScript'
  }
})
```

```js
// Good
const mainWindow = nový prohlížeč Window()
```


## 10) Nepoužívat `povolené vyskakovací okna`

_Doporučení je výchozí_

If you are using [`<webview>`][webview-tag], you might need the pages and scripts loaded in your `<webview>` tag to open new windows. The `allowpopups` attribute enables them to create new [`BrowserWindows`][browser-window] using the `window.open()` method. `<webview>` tagy nemají povoleno vytvářet nová okna.

### Proč?

If you do not need popups, you are better off not allowing the creation of new [`BrowserWindows`][browser-window] by default. Toto je v souladu se zásadou minimálně vyžadovaného přístupu: nenechte vytvářet nová vyskakovací okna, pokud nevíte, že tato funkce potřebuje.

### Jak?

```html<!-- Špatné --><webview allowpopups src="page.html"></webview><!-- Dobré --><webview src="page.html"></webview>
```


## 11) Ověřit možnosti WebView před vytvořením

WebView vytvořený v procesu renderer, který nemá povolenou integraci Node.js , nebude schopen povolit integraci samotnou. WebView však vždy vytvoří nezávislý proces vykreslování s vlastními `nastavením`.

It is a good idea to control the creation of new [`<webview>`][webview-tag] tags from the main process and to verify that their webPreferences do not disable security features.

### Proč?

Od `<webview>` žije v DOM, mohou být vytvořeny skriptem běžícím na vašich webových stránkách, i když Node. s integrace je jinak zakázána.

Electron umožňuje vývojářům zakázat různé bezpečnostní funkce, které ovládají proces vykreslování. In most cases, developers do not need to disable any of those features - and you should therefore not allow different configurations for newly created [`<webview>`][webview-tag] tags.

### Jak?

Before a [`<webview>`][webview-tag] tag is attached, Electron will fire the `will-attach-webview` event on the hosting `webContents`. Pomocí události zabráníte vytváření `webViews` s potenciálně nejistými možnostmi.

```js
app.on('web-contents-created', (událost, obsah) => {
  obsah. n('will-attach-webview', (případ, webPreference, params) => {
    // Strip out out skripty preload pokud nejsou použity, nebo ověříte, že jejich umístění je legitimní
    smazat webPreference. znovu načtěte
    odstranit webová nastavení. znovu načíst URL

    // Zakázat integraci Node.js
    webPreferences. odeIntegrace = false

    // Ověřte načtenou adresu URL
    , pokud (!params. rc.startsWith('https://example.com/')) {
      event.preventDefault()
    }
  })
})
```

Tento seznam opět jen minimalizuje riziko, ale neodstraňuje jej. Pokud je vaším cílem zobrazit webovou stránku, bude prohlížeč bezpečnější.

## 12) Zakázat nebo omezit navigaci

Pokud vaše aplikace nepotřebuje navigaci nebo potřebuje přejít pouze na známé stránky, je dobrý nápad omezit navigaci přímo na známý rozsah a zakázat jakoukoliv jinou navigaci.

### Proč?

Navigace je běžným útočným vektorem. Pokud útočník dokáže přesvědčit vaši aplikaci na odjíždět od své aktuální stránky, mohou donutit vaši aplikaci k otevření webových stránek na internetu. I když jsou vaše `webContents` nakonfigurovány tak, aby byly bezpečnější (jako když je vypnuto `nodeIntegration` nebo `contextIsolation` , povoleno), tak, aby vaše aplikace otevřela náhodné webové stránky, výrazně usnadní práci při využívání vaší aplikace .

Běžným útokovým vzorcem je, že útočník přesvědčí uživatele vaší aplikace k interakci s aplikací tak, že se přesune na jednu z stránek útočníka. To se obvykle provádí prostřednictvím odkazů, pluginů nebo jiného uživatelsky generovaného obsahu.

### Jak?

If your app has no need for navigation, you can call `event.preventDefault()` in a [`will-navigate`][will-navigate] handler. Pokud víte, na které stránky se vaše aplikace může pohybovat, zkontrolujte URL v ovladači událostí a nechte navigaci nastat, pokud se shoduje s URL adresami, které očekáváte.

Doporučujeme použít pro URL parser Node. Jednoduchý porovnávání řetězců může být někdy zmařeno - `startsWith('https://example.com')` test by umožnil `https://example.com.attacker.com` skrz to.

```js
const URL = require('url').URL

app.on('web-contents-created', (událost, obsah) => {
  obsah. n('will-navigate', (event navigationUrl) => {
    const parsedUrl = new URL(navigationUrl)

    if (parsedUrl. rigin !== 'https://example.com') {
      event.preventDefault()
    }
  })
})
```

## 13) Zakázat nebo omezit vytváření nových oken

Pokud máte známou sadu oken, je dobré omezit vytváření dalších oken ve vaší aplikaci.

### Proč?

Tak jako navigace, vytváření nového `webContents` je běžný útok vektor. Útočníci se pokoušejí přesvědčit vaši aplikaci o vytvoření nových oken, snímků, nebo jiných procesů vykreslování s více oprávněními než předtím; nebo s otevřenými stránkami, které nemohly být dříve otevřeny.

Pokud nepotřebujete vytvářet okna navíc k těm, které víte, budete muset vytvořit, Vypnutí tvorby vám koupí trochu více bezpečí bez poplatků. Toto je obvykle případ aplikací, které otevírají jeden `BrowserWindow` a nepotřebují otevírat libovolný počet dalších oken v průběhu času.

### Jak?

[`webContents`][web-contents] will emit the [`new-window`][new-window] event before creating new windows. That event will be passed, amongst other parameters, the `url` the window was requested to open and the options used to create it. Doporučujeme použít událost ke kontrole vytváření oken a omezit ji pouze na to, co potřebujete.

```js
const { shell } = require('electron')

app.on('web-contents-created', (událost, obsah) => {
  obsah. n('new-window', async (event navigationUrl) => {
    // V tomto příkladu požádáme operační systém
    // o otevření url této události ve výchozím prohlížeči.
    event.preventDefault()

    čeká na shell.openExternal(navigationUrl)
  })
})
```

## 14) Nepoužívejte `openExternal` s nedůvěryhodným obsahem

Shell's [`openExternal`][open-external] allows opening a given protocol URI with the desktop's native utilities. On macOS, for instance, this function is similar to the `open` terminal command utility and will open the specific application based on the URI and filetype association.

### Proč?

Improper use of [`openExternal`][open-external] can be leveraged to compromise the user's host. Pokud je openExterní použit s nedůvěryhodným obsahem, může být pákový efekt k provádění libovolných příkazů.

### Jak?

```js
// Špatné
const { shell } = require('electron')
shell.openExternal(USER_CONTROLLED_DATA_HERE)
```
```js
// Good
const { shell } = require('electron')
shell.openExternal('https://example.com/index.html')
```

## 15) Zakázat `vzdálený` modul

Modul `vzdálený` poskytuje způsob, jak procesy vykreslování přistupovat k API normálně pouze v hlavním procesu. Použitím renderer může vyvolat metody hlavního procesního objektu bez výslovného odeslání interprocesních zpráv. Pokud vaše desktopová aplikace nepoužívá nedůvěryhodný obsah, toto může být užitečný způsob, jak mít přístup k vašim procesům a pracovat s moduly, které jsou dostupné pouze v hlavním procesu, jako moduly související s GUI (dialogy, nabídky atd.).

However, if your app can run untrusted content and even if you [sandbox][sandbox] your renderer processes accordingly, the `remote` module makes it easy for malicious code to escape the sandbox and have access to system resources via the higher privileges of the main process. Za takových okolností by proto mělo být vypuštěno.

### Proč?

`vzdálený` používá interní IPC kanál pro komunikaci s hlavním procesem. Útoky „Prototype pollution“ mohou umožnit přístup k vnitřnímu IPC kanálu, které pak mohou být použity k úniku ze sandboxu napodobením `vzdálených` IPC zpráv a získáním přístupu k hlavním procesním modulům běžícím s vyššími oprávněními.

Kromě toho je možné, aby přednačítané skripty náhodně unikly moduly do pískovaného přehrávače . Uvolnění `vzdáleného` zbrojního škodlivého kódu s velkým počtem hlavních procesních modulů, pomocí kterých lze provést útok.

Vypnutí `vzdáleného` modulu eliminuje tyto útočné vektory. Enabling context isolation also prevents the "prototype pollution" attacks from succeeding.

### Jak?

```js
// Chybné, pokud může renderer spustit nedůvěryhodný obsah
const mainWindow = nový BrowserWindow({})
```

```js
// Good
const mainWindow = new BrowserWindow({
  webPreferences: {
    enableRemoteModule: false
  }
})
```

```html<!-- Chybné, pokud může renderer spustit nedůvěryhodný obsah --><webview src="page.html"></webview>

<!-- Good -->
<webview enableremotemodule="false" src="page.html"></webview>
```

## 16) Filtrovat `vzdálený` modul

Pokud nemůžete zakázat `vzdálený` modul, měli byste filtrovat globály, Node, a Electron moduly (tzv. vestavěné moduly) přístupné prostřednictvím `vzdáleného` , které vaše aplikace nevyžaduje. To lze provést blokováním některých modulů zcela a nahrazením jiných proxy servery, které odhalují pouze funkce, které vaše aplikace potřebuje.

### Proč?

V důsledku přístupových práv systému v hlavním procesu, funkčnost poskytovaná hlavními procesními moduly může být nebezpečná v rukou škodlivého kódu, který běží v narušeném procesu vykreslování. Omezením souboru přístupných modulů na minimum, které aplikace potřebuje, a filtrování ostatních, omezíte sadu nástrojů, kterou může použít škodlivý kód k útoku na systém.

Všimněte si, že nejbezpečnější volba je [plně zakázat vzdálený modul](#15-disable-the-remote-module). Pokud zvolíte filtrování přístupu namísto úplného vypnutí modulu, musíte být velmi opatrní, abyste zajistili, že žádná eskalace privilegia nebude možná prostřednictvím modulů, které povolujete přes filtr.

### Jak?

```js
const readOnlyFsProxy = require(/* ... */) // exposes only file read functionality

const allowedModules = new Set(['crypto'])
const proxiedModules = new Map(['fs', readOnlyFsProxy])
const allowedElectronModules = new Set(['shell'])
const allowedGlobals = new Set()

app.on('remote-require', (event, webContents, moduleName) => {
  if (proxiedModules.has(moduleName)) {
    event.returnValue = proxiedModules.get(moduleName)
  }
  if (!allowedModules.has(moduleName)) {
    event.preventDefault()
  }
})

app.on('remote-get-builtin', (event, webContents, moduleName) => {
  if (!allowedElectronModules.has(moduleName)) {
    event.preventDefault()
  }
})

app.on('remote-get-global', (event, webContents, globalName) => {
  if (!allowedGlobals.has(globalName)) {
    event.preventDefault()
  }
})

app.on('remote-get-current-window', (event, webContents) => {
  event.preventDefault()
})

app.on('remote-get-current-web-contents', (event, webContents) => {
  event.preventDefault()
})
```

## 17) Použít aktuální verzi Electronu

Měli byste se snažit vždy používat nejnovější dostupnou verzi Electronu. Kdykoli je vydána nová hlavní verze, měli byste se pokusit co nejrychleji aktualizovat svou aplikaci.

### Proč?

Aplikace vytvořená s starší verzí Electron, Chromium a Node. s je snazší cíl než aplikace, která používá novější verze těchto komponentů. Obecně řečeno, bezpečnostní otázky a využívání starších verzí chromu a Node.js jsou široce dostupné.

Chromium i Node.js jsou působivými honoráři strojírenství postavenými tisíci talentovaných vývojářů. Vzhledem k jejich popularitě je jejich bezpečnost pečlivě testována a analyzována stejně kvalifikovanými výzkumnými pracovníky v oblasti bezpečnosti. Many of those researchers [disclose vulnerabilities responsibly][responsible-disclosure], which generally means that researchers will give Chromium and Node.js some time to fix issues before publishing them. Vaše aplikace bude bezpečnější, pokud spustí nejnovější verzi Electronu (a tedy Chromium a Node. ) pro , které potenciální bezpečnostní otázky nejsou tak obecně známy.


[browser-window]: ../api/browser-window.md


[browser-window]: ../api/browser-window.md
[browser-view]: ../api/browser-view.md
[webview-tag]: ../api/webview-tag.md
[web-contents]: ../api/web-contents.md
[new-window]: ../api/web-contents.md#event-new-window
[will-navigate]: ../api/web-contents.md#event-will-navigate
[open-external]: ../api/shell.md#shellopenexternalurl-options-callback
[sandbox]: ../api/sandbox-option.md
[responsible-disclosure]: https://en.wikipedia.org/wiki/Responsible_disclosure
# Installatie

To install prebuilt Electron binaries, use [`npm`][npm]. De beste methode is om Electron te installeren als ontwikkelingsafhankelijkheid in je app:

```sh
npm install electron --save-dev
```

Kijk in de [Electron versie documentatie][versioning] om meer informatie te vinden over hoe je verschillende versies kunt beheren in je project.

## Globale Installatie

Je kan `electron` ook globaal installeren in je `$PATH`:

```sh
npm install electron -g
```

## Maatwerk

Als je de gedownloade architectuur wilt wijzigen (bijv. `ia32` op een `x64` machine), kan je de `--arch` gebruiken met npm install of de `npm_config_arch` environment variable:

```shell
npm install --arch=ia32 electron
```

Naast het wijzigen van de architectuur kun je ook het platform specificeren (bijv. `win32`, `linux`, etc.) met behulp van de `--platform` markering:

```shell
npm install --platform=win32 electron
```

## Proxies

Als u een HTTP-proxy wilt gebruiken, moet u de `ELECTRON_GET_USE_PROXY` variabele instellen op elke waarde. plus extra omgevingsvariabelen afhankelijk van de Node versie van uw hostsysteem:

* [Knooppunt 10 en hoger][proxy-env-10]
* [Voor Node 10][proxy-env]

## Custom Mirrors en Caches
During installation, the `electron` module will call out to [`@electron/get`][electron-get] to download prebuilt binaries of Electron for your platform. Het zal dit doen door contact op te nemen met GitHub's release download pagina (`https://github. om/electron/releases/tag/v$VERSION`, waarbij `$VERSION` de exacte versie van Electron) is.

Als je geen toegang hebt tot GitHub of je moet een aangepaste versie aanleveren, je kan dit doen door het aanbieden van een kopie of een bestaande cachemap.

#### Spiegel
Je kunt omgevingsvariabelen gebruiken om de basis-URL te vervangen, het pad waarnaar te zoeken naar Electron binaries en de binaire bestandsnaam. The url used by `@electron/get` is composed as follows:

```plaintext
url = ELECTRON_MIRROR + ELECTRON_CUSTOM_DIR + '/' + ELECTRON_CUSTOM_FILENAAM
```

For instance, to use the China mirror:

```plaintext
ELECTRON_MIRROR="https://cdn.npm.taobao.org/dist/electron/"
```

#### Cachegeheugen
Je kunt ook de lokale cache overschrijven. `@electron/get` zal gedownloade binaries in een lokale map cachen om uw netwerk niet te stress geven. Je kunt die cachemap gebruiken om aangepaste builds van Electron aan te bieden of om te voorkomen dat er contact wordt met met het netwerk.

* Linux: `$XDG_CACHE_HOME` of `~/.cache/electron/`
* macOS: `~/Library/Caches/electron/`
* Windows: `$LOCALAPPDATA/electron/Cache` or `~/AppData/Local/electron/Cache/`

Op een omgeving die oudere versies van Electroon heeft gebruikt, vindt u misschien de cache ook in `~/.electron`.

Je kunt ook de lokale cache locatie overschrijven door het aanbieden van een `electron_config_cache` omgevingsvariabele.

De cache bevat zowel het officiële zip-bestand van de versie als een checksum, opgeslagen als tekstbestand. Een typische cache zou er zo uit kunnen zien:

```sh
● githubgithub.comelectronelectronreleasesdownload1.7.9electron-v1.7.9-darwin-x64.zip
githubgithub.com.com9.darwin-x64.zip
/01githubgithub.comelectronelectronelectronreleasesdownloesdownloadv1.7.9SHASUMS256.txt
(IN(IN(IN2/githubSHASUMS256.txt
capable github.comelectronreleasesdownload1.8.1electron-v1.1.1-dar-x64. IP IP
taxes, taxes, electron-v1.8.1-darwin-x64.zip
![httpsgithub.comelectronelectronelectronelectronreleasesdownloesdownloast1.8.1SHASUMS256.txt
ľraised, raised, SHASUMS256.txt
UK, httpsgithub. electronomelectronreleasesdownloast1.8.2-beta.1electron-v1.8.2-beta.1-darwin-x64.zip
ghaeable eable electron-v1.8.2-beta.1-darwin-x64.zip
Verify httpsgithub. electronreleasesdownload-adv1.8.2-beta.1SHASUMS256.txt
Watsves, SHASUMS256.txt
![httpsgithub.comelectronelectronreleasesdownloesdownload 1.8.2-beta.2electron-v1.8.2-beta.2-darwin-x64.zip
(GA) (GA) Electron-v1.8.2-beta.2-dar-x64.zige
(Chrimpelplatform.1.8.2-beta.2-beta.2-beta.2-betaa. SHASUMS256.txt
Amsterdam
 ・SHASUMS256.txt
github.comelectronelectronreleasesdownloesdownload 1.8.2-beta.3electron-v1.8.2-beta.3-darwin-x64. Rust uit
×××electron-v1.8.2-beta.3-darwin-x64.zip
github.comelectronelectronelectronreleasesdownload 1.8.2-beta.3SHASUMS256.txt
    LudLudLudLud6.txt
```

## Overslaan binaire download
Bij het installeren van het `electron` NPM pakket, downloadt het automatisch de electron binary.

Dit kan soms onnodig, bijvoorbeeld in een CI-omgeving, bij het testen van een ander component.

Om te voorkomen dat het binary wordt gedownload wanneer je alle npm afhankelijkheden installeert, kun je de omgevingsvariabele `ELECTRON_SKIP_BINARY_DOWNLOAD` instellen. Bijv.:
```sh
ELECTRON_SKIP_BINARY_DOWNLOAD=1 npm installatie
```

## Probleemoplossen

Bij het uitvoeren van `npm install electron`, krijgen sommige mensen een error.

In bijna alle gevallen zijn de fouten het resultaat van een netwerk probleem en niet een probleem met de `electron` npm package. Errors zoals `ELIFECYCLE` `EAI_AGAIN`, `ECONNRESET` en `ETIMEDOUT` zijn indicaties van dergelijke netwerkproblemen. De beste oplossing is om te proberen de netwerkverbinding te wijzigen of even te wachten en de installatie opnieuw uit te voeren.

You can also attempt to download Electron directly from [electron/electron/releases][releases] if installing via `npm` is failing.

If installation fails with an `EACCESS` error you may need to [fix your npm permissions][npm-permissions].

If the above error persists, the [unsafe-perm][unsafe-perm] flag may need to be set to true:

```sh
sudo npm install electron --unsafe-perm=true
```

Op langzamere netwerken kan het aangeraden worden om de `--verbose` vlag te gebruiken om de download voortgang te tonen:

```sh
npm install --verbose electron
```

Als je een herdownload van het bestand en het SHASUM bestand wilt forceren zet je de `force_no_cache` environment variabele op `true`.

[npm]: https://docs.npmjs.com
[versioning]: ./electron-versioning.md
[releases]: https://github.com/electron/electron/releases
[proxy-env-10]: https://github.com/gajus/global-agent/blob/v2.1.5/README.md#environment-variables
[proxy-env]: https://github.com/np-maintain/global-tunnel/blob/v2.7.1/README.md#auto-config
[electron-get]: https://github.com/electron/get
[npm-permissions]: https://docs.npmjs.com/getting-started/fixing-npm-permissions
[unsafe-perm]: https://docs.npmjs.com/misc/config#unsafe-perm
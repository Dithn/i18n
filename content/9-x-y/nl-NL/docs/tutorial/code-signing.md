# Code signeren

Code signing is a security technology that you use to certify that an app was created by you.

Op macOS kan het systeem wijzigingen in de app detecteren, of de wijziging nu per ongeluk of door kwaadwillende code is geïntroduceerd.

In Windows wijst het systeem een vertrouwensniveau toe aan uw code die certificaat tekent die als u dat niet hebt, of als uw trust level laag is, zullen er beveiligingsdialogen verschijnen wanneer gebruikers uw applicatie beginnen te gebruiken.  Vertrouw level bouwt na verloop van tijd zodat het beter is om zo vroeg mogelijk te beginnen met code-ondertekening.

Hoewel het mogelijk is niet-ondertekende apps te verspreiden, wordt het niet aanbevolen. Zowel Windows als macOS zal standaard de download of de uitvoering van niet-ondertekende applicaties verhinderen. Starting with macOS Catalina (version 10.15), users have to go through multiple manual steps to open unsigned applications.

![macOS Catalina Gatekeeper waarschuwing: de app kan niet worden geopend omdat de ontwikkelaar niet kan worden geverifieerd](../images/gatekeeper.png)

Zoals je kunt zien, krijgen gebruikers twee opties: de app direct naar de prullenbak verplaatsen of het uitvoeren ervan annuleren. U wilt niet dat uw gebruikers dat dialoogvenster zien.

Als je een Electron app aan het maken bent die je van plan bent te verpakken en te verspreiden, het moet code-signed zijn. De Mac en Windows app opslag staat niet niet-ondertekende apps toe.

# MacOS builds ondertekenen

Voordat je macOS-versies tekent, moet je het volgende doen:

1. Enroll in the [Apple Developer Program][] (requires an annual fee)
2. Download and install [Xcode][]
3. Generate, download, and install [signing certificates][]

Er zijn een aantal tools voor het ondertekenen van je verpakte app:

- [`electron-osx-sign`][] is a standalone tool for signing macOS packages.
- [`electron-packager`][] bundles `electron-osx-sign`. Als je `electron-packager`gebruikt, geef dan de `--osx-sign=true` vlag om je bouwwerk te ondertekenen.
  - [`electron-forge`][] uses `electron-packager` internally, you can set the `osxSign` option in your forge config.
- [`electron-builder`][] has built-in code-signing capabilities. Zie [electron.build/code-signing](https://www.electron.build/code-signing)

## Notarificatie

Vanaf macOS Catalina vereist Apple dat applicaties worden genoteerd. "Notarization" as defined by Apple means that you upload your previously signed application to Apple for additional verification _before_ distributing the app to your users.

To automate this process, you can use the [`electron-notarize`][] module. Je hoeft niet noodzakelijk deze stap te voltooien voor elke constructie die je maakt - alleen de builds die je van plan bent te verzenden naar gebruikers.

## Mac App Store

See the [Mac App Store Guide][].

# Windows-versies worden ondertekend

Voordat u Windows-versies tekent, moet u het volgende doen:

1. Een Windows Authenticode code ondertekeningscertificaat (jaarlijkse kosten vereist)
2. Installeer Visual Studio 2015/2017 (om de ondertekening te krijgen)

Je kunt een code krijgen die een certificaat tekent van veel resellers. Prijzen variëren, dus het kan de moeite waard zijn om te winkelen. Populaire resellers zijn:

* [digicert](https://www.digicert.com/code-signing/microsoft-authenticode.htm)
* [Comodo](https://www.comodo.com/landing/ssl-certificate/authenticode-signature/)
* [GoDaddy](https://au.godaddy.com/web-security/code-signing-certificate)
* Koop er onder andere één om er één te vinden die bij u past. Google is uw vriend :)

Er zijn een aantal tools voor het ondertekenen van je verpakte app:

- [`electron-winstaller`][] will generate an installer for windows and sign it for you
- [`electron-forge`][] can sign installers it generates through the Squirrel.Windows or MSI targets.
- [`electron-builder`][] can sign some of its windows targets

## Windows Store

See the [Windows Store Guide][].

[Apple Developer Program]: https://developer.apple.com/programs/
[`electron-builder`]: https://github.com/electron-userland/electron-builder
[`electron-forge`]: https://github.com/electron-userland/electron-forge
[`electron-osx-sign`]: https://github.com/electron-userland/electron-osx-sign
[`electron-packager`]: https://github.com/electron/electron-packager
[`electron-notarize`]: https://github.com/electron/electron-notarize
[`electron-winstaller`]: https://github.com/electron/windows-installer
[Xcode]: https://developer.apple.com/xcode
[signing certificates]: https://github.com/electron/electron-osx-sign/wiki/1.-Getting-Started#certificates
[Mac App Store Guide]: mac-app-store-submission-guide.md
[Windows Store Guide]: windows-store-guide.md
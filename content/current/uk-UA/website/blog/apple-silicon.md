---
title: Підтримка Apple Silicon
author: Маршаллофсун
date: '2020-10-15'
---

З випуском апаратного забезпечення Apple Silicon в цьому році, як виглядає шлях, щоб ви отримали ваш додаток Electron працюючи на новому обладнанні?

---

З випуском Electron 11.0.0-бети. , команда Electron зараз доставляє збірки Electron, що працюють на новому апараті Apple Silicon для Apple, який планує доставки пізніше цього року. Ви можете захопити останню бета-версію за допомогою `npm встановити electron@beta` або завантажити її безпосередньо з нашого [релізів](https://electronjs.org/releases/stable).

## Як це працює?

Починаючи з Electron 11, ми будемо доставляти окремі версії Electron для Intel Macs та Apple Silicon Mac. До цих змін ми вже були доставлені два артефакти: `darwin-x64` і `mas-x64`під час використання сумісності в Mac App Store. Зараз ми постачаємо ще один два артефакти, `вапняк-arm64` і `mas-arm64`, які є еквівалентами Apple Silicon для вищезгаданих артефактів.

## Що мені потрібно зробити?

Вам потрібно буде відправити дві версії вашого застосунку: один для x64 (Intel Mac) і один для arm64 (Apple Silicon). Доброю новиною є те, що [`електрон-пакет`](https://github.com/electron/electron-packager/) [`electron-rebuild`](https://github.com/electron/electron-rebuild/) і [`electron-forge`](https://github.com/electron-userland/electron-forge/) вже підтримує розробку `arm64` архітектури. До тих пір, поки ви використовуєте останні версії цих пакетів, ваш застосунок має працювати бездоганно після оновлення цільової архітектури до `arm64`.

В майбутньому, ми випустимо пакет, який дозволить вам "об'єднати" ваші `arm64` та `x64` програми з одним універсальним двійковим файлом але варто зазначити, що ця бінарна версія може бути _величезною_ і, ймовірно, не є ідеальною для доставки споживачам.

## Можливі проблеми

### Нативні модулі

Ви націлюєте на нову архітектуру, вам потрібно оновити декілька залежностей, що можуть спричинити проблеми з ними. Для вашої довідки наведено мінімальну версію залежностей.

| Залежність              | Вимога версії |
| ----------------------- | ------------- |
| X-код                   | `>=12.2.0` |
| `вуздечний гір`         | `>=7.1.0`  |
| `електро-реконструкція` | `>=1.12.0` |
| `електро-пакувальник`   | `>=15.1.0` |

В результаті цих вимог версії залежностей, вам, можливо, доведеться виправити/оновити певні вбудовані модулі.  Одна річ — це те, що оновлення Xcode представить нову версію macOS SDK, що може призвести до збоїв для ваших нативних модулів.


## Як це перевірити?

В даний час програми Apple Silicon працюють лише на апараті Apple Silicon , яке не є комерційно доступним під час написання цього блогу посту. Якщо у вас [набір для розробників](https://developer.apple.com/programs/universal/), ви можете протестувати вашу програму на цьому. В іншому випадку вам доведеться дочекатися випуску виробничого обладнання Apple Silicon для перевірки, якщо ваша програма працює.

## А Розетта 2?

Розетта 2 є останньою ітерацією Apple їх технології [Rosetta](https://en.wikipedia.org/wiki/Rosetta_(software)) , що дозволяє запускати додатки з x64 Intel на їх новому пристрої для arm64 Apple Silicon (Apple). Хоча ми вважаємо, що додатки x64 будуть працювати через Розетта 2, є деякі важливі речі для відмітки (і причини чому ви повинні доставити на рідний броні 64 двійковий).

* Продуктивність вашого додатку буде значно погіршена. Electron / V8 використовує [JIT](https://en.wikipedia.org/wiki/Just-in-time_compilation) компіляцію для JavaScript, і через те, як працює Rosetta, ви ефективно запустите JIT двічі (один раз у V8 і ще раз у Розетті).
* Ви втрачаєте користь нових технологій у Apple Silicon, наприклад, збільшення розміру сторінки з пам'яттю.
* Чи згадували ми про те, що продуктивність **значно погіршиться**?
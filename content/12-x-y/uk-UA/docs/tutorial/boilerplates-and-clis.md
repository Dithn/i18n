# Шаблони Коду vs CLI

Розробка Electron є ненаписаною - для розробки не існує "один істинний шлях", збирати, упаковку чи випустити застосунок на Electron. Додаткові можливості для Electron, обидва для збірки та під час виконання, зазвичай можна знайти на [npm](https://www.npmjs.com/search?q=electron) в окремих пакетах, дозволяти розробникам будувати як програму, так і будувати конвеєр.

Цей рівень модулярності і розширення гарантує, що всі розробники працюють з Electron, як великий, так і малий у обсязі команди, ніколи не обмежуються тим, що вони можуть або не можуть робити в будь-який час під час свого життєвого циклу розробки. Тим не менш, для багатьох розробників, один з бойлери або командний рядок може значно полегшити компіляцію, пакунок і вивільніть додаток .

## Шаблони Коду vs CLI

Паросток бойлера, це тільки початкова точка - полотно, так щоб озвучуватись: з якої ви створюєте свій додаток. Зазвичай вони приходять у вигляді репозиторію, який ви можете клонувати і налаштувати налаштування вашого серця.

Інструмент командного рядка з іншого боку продовжує підтримувати вас протягом розробки і релізу. Вони є більш корисними та підтримуючими, але застосовують рекомендації щодо того, як код потрібно структурувати та будувати. *Особливо для початківців, використовуючи інструмент командного рядка, ймовірно, буде корисним*.

## electron-forge

Це "повний інструмент для створення сучасних програм Electron". Electron Forge об'єднує існуючі (і добре підтримуються) конструкційні інструменти для розробки Electron в згуртований пакет, так щоб кожен міг перейти прямо до розробки Electron.

Forge походить з [шаблоном готового використання](https://electronforge.io/templates) , використовуючи Webpack як пакет. До неї входять приклад конфігурації typescript і надається два файли конфігурації, щоб забезпечити легке настроювання. Він використовує ті ж основні модулі, що використовуються більшою спільнотою Electron (наприклад, [`electron-packager`](https://github.com/electron/electron-packager)) - зміни зроблені розробленими розробниками Electron maintainers (як Slack) користувачами Forge для отримання корисних копалин, теж.

Більше інформації та документації по [electronforge.io](https://electronforge.io/).

## electron-builder

Це "повне рішення для пакунку та створити готовий додаток Electron для дистрибуції" ", який фокусується на інтегрованому досвіді. [`electron-builder`](https://github.com/electron-userland/electron-builder) додає одну окрему залежність, орієнтовану на простоту, а також на обробку всіх подальших вимог внутрішньо.

`electron-builder` замінює можливості та модулі, які використовуються в Electron супроводжуючих (наприклад, автооновлення). Вони загалом більш інтегровані але будуть рідше в популярних додатках Electron : Atom, Visual Studio Code, або Slack.

Ви можете знайти більше інформації та документації в [в репозиторії](https://github.com/electron-userland/electron-builder).

## electron-react-boilerplate

Якщо вам не потрібні якісь інструменти, але тільки суцільний бойлер, з якого можна будувати, CT Linux [`electron-react-boilerplate`](https://github.com/chentsulin/electron-react-boilerplate) може коштувати . Воно дуже популярне у спільноті і використовує `електрон-конструктор` .

## Інші Інструменти та Шаблони Коду

Список ["Awome Electron"](https://github.com/sindresorhus/awesome-electron#boilerplates) містить більше інструментів і бойлитиць на вибір. If you find the length of the list intimidating, don't forget that adding tools as you go along is a valid approach, too.
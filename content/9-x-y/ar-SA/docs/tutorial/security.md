# Security, Native Capabilities, and Your Responsibility

As web developers, we usually enjoy the strong security net of the browser - the risks associated with the code we write are relatively small. مواقعنا مُنحت صلاحيات محدودة في صندوق الرمل، ونحن على ثقة بأن مستخدمينا يستمتعون بـ متصفح قام ببناؤه فريق كبير من المهندسين القادرين على الرد بسرعة على التهديدات الأمنية المكتشفة حديثا.

عند العمل مع إلكترون، من المهم أن نفهم أن إلكترون ليس متصفح ويب. يسمح لك ببناء تطبيقات سطح المكتب الغنية بالميزة مع تكنولوجيا الويب المألوفة، ولكن التعليمات البرمجية الخاصة بك تمتلك قوة أكبر بكثير. يمكن لـ JavaScript الوصول إلى نظام الملفات، وقصة المستخدم، والمزيد. هذا يسمح لك ببناء تطبيقات محلية عالية الجودة، ولكن حجم المخاطر الأمنية المتأصلة مع الصلاحيات الإضافية الممنوحة للكود الخاص بك.

وإذ تضع ذلك في اعتبارها، كن على علم بأن عرض المحتوى التعسفي من المصادر غير الموثوق بها يشكل خطرا أمنيا شديدا ليس المقصود من إلكترون التعامل معه. في الواقع، تطبيقات إلكترون الأكثر شعبية (Atom, Slack, Visual Studio Code، إلخ) تعرض المحتوى المحلي في المقام الأول (أو موثوق به، تأمين المحتوى البعيد بدون دمج العقدة - إذا نفذ تطبيقك التعليمات البرمجية من مصدر على الإنترنت، تقع على عاتقك مسؤولية التأكد من أن التعليمات البرمجية ليست خبيثة.

## الإبلاغ عن المشكلات الأمنية

للحصول على معلومات حول كيفية الكشف بشكل صحيح عن ضعف إلكترون، راجع [SECURITY.md](https://github.com/electron/electron/tree/master/SECURITY.md)

## المسائل الأمنية المتعلقة بالكروم وتحديثاته

يبقي إلكترون على اطلاع مع إصدارات الكروميوم بالتناوب. للحصول على مزيد من المعلومات، انظر [مشاركة مدونة إصدار الإلكترون Cadence](https://electronjs.org/blog/12-week-cadence).

## الأمن هو مسؤولية الجميع

من المهم أن نتذكر أن أمان تطبيق إلكترون الخاص بك هو نتيجة الأمان العام للأساس الإطاري (*كروميوم*، *العقدة. s*)، إلكترون نفسه، جميع التبعيات NPM و الرمز الخاص بك. وعلى هذا النحو، تقع على عاتقك مسؤولية اتباع بعض أفضل الممارسات المهمة:

* **ابق تطبيقك على اطلاع بأحدث اصدار اطر إلكترون.** عند تحرير المنتج الخاص بك، تقوم أيضا بشحن حزمة مؤلفة من Electron، مكتبة Chromium المشتركة و Node.js. نقاط الضعف التي تؤثر على هذه المكونات قد تؤثر على أمن التطبيق الخاص بك. عن طريق تحديث إلكترون إلى أحدث إصدار ، تأكد من أن نقاط الضعف الحرجة (مثل *تخطي عقد التكامل*) قد تم تصحيحها بالفعل ولا يمكن استغلالها في تطبيقك. للمزيد من المعلومات، أنظر "[استخدم الإصدار الحالي من إلكترون](#17-use-a-current-version-of-electron)".

* **تقييم التبعيات الخاصة بك.** بينما يقدم NPM نصف مليون حزمة قابلة لإعادة الاستخدام، تقع على عاتقك مسؤولية اختيار مكتبات موثوق بها من طرف ثلاثة. إذا كنت تستخدم مكتبات قديمة متأثرة بأوجه ضعف معروفة أو تعتمد على رمز رديء الصيانة، قد يكون أمن التطبيق في خطر.

* **تبني ممارسات البرمجة الآمنة** خط الدفاع الأول لتطبيقك هو الكود الخاص بك. نقاط الضعف الشائعة على الشبكة، مثل سكريبت عبر الموقع، (XSS)، لها تأثير أمان أعلى على تطبيقات إلكترون ومن ثم يوصى بشدة باعتماد أفضل ممارسات تطوير البرمجيات الآمنة وإجراء الاختبارات الأمنية.


## عزل الحتوى غير الموثوق به

توجد مشكلة أمنية عندما تتلقى تعليمة برمجية من مصدر غير موثوق به (على سبيل المثال خادم بعيد) وتقوم بتنفيذها محلياً. As an example, consider a remote website being displayed inside a default [`BrowserWindow`][browser-window]. إذا تمكن مهاجم بطريقة ما من تغيير المحتوى المذكور (إما عن طريق مهاجمة المصدر مباشرة، أو عن طريق الجلوس بين التطبيق الخاص بك ووجهتك الفعلية)، فإنهم سيكونون قادرين على تنفيذ التعليمات البرمجية الأصلية على جهاز المستخدم.

> :warning: لا ينبغي تحت أي ظرف من الظروف تحميل وتنفيذ التعليمات البرمجية البعيدة باستخدام تمكين تكامل Node.js. بدلاً من ذلك، استخدم فقط الملفات المحلية (المجمعة معاً مع التطبيق الخاص بك) لتنفيذ رمز Node.js. To display remote content, use the [`<webview>`][webview-tag] tag or [`BrowserView`][browser-view], make sure to disable the `nodeIntegration` and enable `contextIsolation`.

## تحذيرات أمنية إلكترون

من إلكترون 2.0 على ذلك، سيشاهد المطورين تحذيرات وتوصيات مطبوعة إلى وحدة المطور. تظهر فقط عندما يكون اسم بيناري هو إلكترون ، يشير إلى أن المطور ينظر حاليا إلى وحدة التحكم.

يمكنك تمكين أو تعطيل هذه التحذيرات بالقوة عن طريق إعداد `ELECTRON_ENABLE_SECURITY_WARNINGS` أو `ELECTRON_DISABLE_SECURITY_WARNINGS` عن إمّا `. nv` أو `نافذة` كائن.

## قائمة: التوصيات الأمنية

يجب عليك على الأقل متابعة هذه الخطوات لتحسين أمن تطبيقك:

1. [تحميل المحتوى الآمن فقط](#1-only-load-secure-content)
2. [تعطيل دمج Node.js في جميع المعلنين الذين يعرضون المحتوى البعيد](#2-do-not-enable-nodejs-integration-for-remote-content)
3. [تمكين عزل السياق في جميع مقدمي عرض المحتوى البعيد](#3-enable-context-isolation-for-remote-content)
4. [Use `ses.setPermissionRequestHandler()` in all sessions that load remote content](#4-handle-session-permission-requests-from-remote-content)
5. [لا تقم بتعطيل `webSecurity`](#5-do-not-disable-websecurity)
6. [تحديد `سياسة المحتوى - الأمن`](#6-define-a-content-security-policy) واستخدام قواعد تقييدية (أي `script-src 'نفسها'`)
7. [لا تقم بتعيين `allowRunningInSecureContent` إلى `true`](#7-do-not-set-allowrunninginsecurecontent-to-true)
8. [لا تقم بتمكين الميزات التجريبية](#8-do-not-enable-experimental-features)
9. [لا تستخدم `تمكين ميزات Blinks`](#9-do-not-use-enableblinkfeatures)
10. [`<webview>`: لا تستخدم `سماح المنبثقات`](#10-do-not-use-allowpopups)
11. [`<webview>`: التحقق من الخيارات والمشاركات](#11-verify-webview-options-before-creation)
12. [تعطيل التنقل أو الحد منه](#12-disable-or-limit-navigation)
13. [تعطيل أو تقييد إنشاء نوافذ جديدة](#13-disable-or-limit-creation-of-new-windows)
14. [لا تستخدم `openExternal` مع محتوى غير موثوق به](#14-do-not-use-openexternal-with-untrusted-content)
15. [تعطيل وحدة `البعيد`](#15-disable-the-remote-module)
16. [تصفية وحدة `عن بعد`](#16-filter-the-remote-module)
17. [استخدام نسخة حالية من إلكترون](#17-use-a-current-version-of-electron)

للتشغيل الآلي للكشف عن سوء التكوينات والأنماط غير الآمنة، من الممكن استخدام [التجاوز الإلكتروني](https://github.com/doyensec/electronegativity). للحصول على تفاصيل إضافية عن نقاط الضعف المحتملة وأخطاء التنفيذ عند تطوير التطبيقات باستخدام إلكترون، يرجى الرجوع إلى هذا الدليل [لـ المطورين ومراجعي الحسابات](https://doyensec.com/resources/us-17-Carettoni-Electronegativity-A-Study-Of-Electron-Security-wp.pdf)

## 1) تحميل المحتوى الآمن فقط

يجب تحميل أي موارد غير مشمولة بتطبيقك باستخدام بروتوكول آمن مثل `HTTPS`. بعبارة أخرى، لا تستخدم بروتوكولات غير آمنة مثل `HTTP`. وبالمثل، نوصي باستخدام `WSS` على `WS`، `FTPS` حول `FTP`، وما إلى ذلك.

### لما؟

`HTTPS` لديه ثلاث مزايا رئيسية:

1) يقوم بالتصديق على الخادم البعيد، وضمان اتصال التطبيق بالمضيف الصحيح بدلاً من انتحال الشخص. 2) يضمن سلامة البيانات، ويؤكد أن البيانات لم يتم تعديلها أثناء الانتقال بين التطبيق الخاص بك والمضيف. 3) أنها تشفر حركة المرور بين المستخدم الخاص بك والمضيف المقصود، جعله أكثر صعوبة للإسقاط على المعلومات المرسلة بين تطبيقك و المضيف.

### كيف؟

```js
// سيئ
المتصفح Window.loadURL('http://example.com')

///Good
browserWindow.loadURL('https://example.com')
```

```html<!-- سيء --><script crossorigin src="http://example.com/react.js"></script>
<link rel="stylesheet" href="http://example.com/style.css"><!-- جيد --><script crossorigin src="https://example.com/react.js"></script>
<link rel="stylesheet" href="https://example.com/style.css">
```


## 2) لا تقم بتمكين تكامل Node.js للمحتوى البعيد

_هذه التوصية هي السلوك الافتراضي في إلكترون منذ 5.0.0._

It is paramount that you do not enable Node.js integration in any renderer ([`BrowserWindow`][browser-window], [`BrowserView`][browser-view], or [`<webview>`][webview-tag]) that loads remote content. الهدف هو الحد من الصلاحيات التي تمنحها للمحتوى البعيد، وبالتالي تجعل من الصعب بشكل كبير على المهاجم أن يضر بمستخدميك إذا ما اكتسبوا القدرة على تنفيذ جافا سكريبت على موقع الويب الخاص بك.

بعد ذلك، يمكنك منح أذونات إضافية للمضيفين المحددين. على سبيل المثال، إذا كنت تفتح نافذة المتصفح المشار إليها في `https://example. om/`، يمكنك إعطاء هذا الموقع بالضبط القدرات التي يحتاجها، ولكن لا أكثر.

### لما؟

هجوم البرنامج النصي عبر الموقع (XSS) يكون أكثر خطورة إذا تمكن المهاجم من القفز من عملية العارض وتنفيذ التعليمات البرمجية على جهاز الكمبيوتر الخاص بالمستخدم. وهجمات الكتابة عبر المواقع شائعة إلى حد ما - وإن كانت مشكلة. قوتهم عادة ما تقتصر على التلاعب بالموقع الذي يتم تنفيذه على الموقع. تعطيل تكامل Node.js يساعد على منع تصعيد XSS إلى هجوم يسمى "تنفيذ التعليمات البرمجية البعيدة".

### كيف؟

```js
// سيئ
const mainWindow = متصفح جديد ({
  webPreferences: {
    nodeIntegration: true,
    nodeIntegrationInWorker: true
  }
})

mainWindow.loadURL('https://example.com')
```

```js
// Good
const mainWindow = New BrowserWindow({
  webPreferences: {
    preload: path.join(app.getAppPath(), 'preload.js'
  }
})

mainWindow.loadURL('https://example.com')
```

```html<!-- سيء --><webview nodeIntegration src="page.html"></webview><!-- جيد --><webview src="page.html"></webview>
```

عند تعطيل دمج Node.js ، لا يزال بإمكانك عرض APIs على موقع الويب الخاص بك الذي يستهلك Node.js الوحدات أو الميزات. لا يزال التحميل المسبق للبرامج النصية الوصول إلى `يتطلب` وعقدة أخرى. s ميزات ، مما يسمح للمطورين بالكشف عن محتوى مخصص API للمحتوى المحمّل عن بعد.

في المثال التالي البرنامج النصي للتحميل المسبق، الموقع الذي تم تحميله لاحقاً سيكون لديه الوصول إلى طريقة `window.readConfig()` ولكن لا توجد ميزات Node.js.

```js
const { readFileSync } = require('fs')

window.readConfig = function () {
  const data = readFileSync('./config.json')
  إعادة البيانات
}
```


## 3) تمكين العزل السياقي للمحتوى البعيد

عزل السياق هو ميزة إلكترون التي تسمح للمطورين بتشغيل التعليمات البرمجية في البرامج النصية السابقة التحميل وفي تطبيقات إلكترون APIs في سياق جافا سكريبت مخصص. في الممارسة، هذا يعني أن الكائنات العالمية مثل `Array.prototype. اضغط` أو `JSON.parse` لا يمكن تعديله بواسطة البرامج النصية قيد التشغيل في عملية المعرض.

يستخدم إلكترون نفس التكنولوجيا مثل برامج كروموم [لمحتوى البرامج البرمجية](https://developer.chrome.com/extensions/content_scripts#execution-environment) لتمكين هذا السلوك.

حتى عندما تستخدم `تكميل العقدة: خاطئ` لفرض العزلة القوية و منع استخدام بدائيات العقدة ، `يجب أيضا استخدام العزلة السياقية`.

### لما؟

يسمح المحتوى المعزول لكل سكريبت مشغل في المصيير لعمل تغييرات لبيئة JavaScript بدون القلق حول التعارض مع السكريبتات في الـElectron API أو السكريبت المحمل من قبل.

في حين لا تزال ميزة إلكترون التجريبية، فإن عزلة السياق تضيف طبقة إضافية من الأمان. إنه ينشئ عالم جافا سكريبت جديد لـ إلكترون APIs و النصوص المسبقة التحميل، مما يخفف من حدة ما يسمى بهجمات "نموذج التلوث الأولي".

At the same time, preload scripts still have access to the  `document` and `window` objects. In other words, you're getting a decent return on a likely very small investment.

### كيف؟

```js
// العملية الرئيسية
const mainWindow = New BrowserWindow({
  webPreferences: {
    contextIsolation: true,
    preload: path.join(app.getAppPath(), 'preload.js')

})
```

```js
// تحميل البرنامج النصي المسبق

// تعيين متغير في الصفحة قبل أن يحمّل
webFrame.executeJavaScript('window). oo = "foo"؛ )

// الصفحة المحملة لن تتمكن من الوصول إلى هذا، فهي متاحة فقط
/ / في هذا السياق
النافذة. r = 'الشريط'

المستند. dEventListener('DOMContentLoaded', () => {
  // سيتم تسجيل الخروج 'غير محدد' منذ النافذة. oo متوفر فقط في وحدة التحكم الرئيسية
  // السياق
  . og(window.foo)

  // سوف يسجل خروج 'بار' لأن window.bar متاح في هذا السياق
  console.log(window.bar)
})
```


## 4) التعامل مع طلبات إذن الجلسة من المحتوى البعيد

You may have seen permission requests while using Chrome: They pop up whenever the website attempts to use a feature that the user has to manually approve ( like notifications).

API مبني على [أذونات كروميوم API](https://developer.chrome.com/extensions/permissions) وينفذ نفس أنواع الأذونات.

### لما؟

بشكل افتراضي، سيوافق إلكترون تلقائياً على جميع طلبات الأذونات ما لم يقوم المطور بتكوين معالج مخصص يدوياً. على الرغم من وجود إفتراضي متين، المطورين الواعين بالأمن قد يريدون أن يفترضوا العكس تماما.

### كيف؟

```js
const { session } = مطلوبة ('electron')

جلسة
  .fromPartition('some-section')
  . etPermissionrequestHandler(webcontents, perauthore, perauthortHandler) => {
    const url = webcontts. etURL()

    if (إذن === 'الإشعارات') {
      // يوافق على الأذونات المطلوبة
      الرد (true)
    }

    // التحقق من عنوان URL
    إذا (! ر.ل tartsWith('https://example. om/')) {
      // رفض الإذن بطلب
      رد المكالمة (alse)
    }
})
```


## 5) عدم تعطيل أمن الويب

_التوصية هي الافتراضي لـ Electron's_

You may have already guessed that disabling the `webSecurity` property on a renderer process ([`BrowserWindow`][browser-window], [`BrowserView`][browser-view], or [`<webview>`][webview-tag]) disables crucial security features.

لا تقم بتعطيل `webSecurity` في تطبيقات الإنتاج.

### لما؟

سيؤدي تعطيل `webSecurity` إلى تعطيل سياسة نفس الأصل وتعيين `allowRunningInsecure content` إلى `true`. بعبارة أخرى، فإنه يسمح بتنفيذ التعليمات البرمجية غير الآمنة من نطاقات مختلفة.

### كيف؟
```js
//
سيئ النوافذ الرئيسية = متصفح جديد ({
  تفضيلات الويب: {
    webSecurity: false
  }
})
```

```js
// جيد
const mainWindow = متصفح جديد()
```

```html<!-- سيء --><webview disablewebsecurity src="page.html"></webview><!-- جيد --><webview src="page.html"></webview>
```


## 6) تحديد سياسة أمن المحتوى

سياسة أمن المحتوى (CSP) هي طبقة حماية إضافية ضد هجمات الكتابة عبر الموقع وهجمات حقن البيانات. نوصي بتمكينهم من خلال أي موقع تحميله داخل إلكترون.

### لما؟

يسمح CSP للمحتوى الذي يخدم الخادم لتقييد ومراقبة الموارد إلكترون الذي يمكن تحميله لتلك الصفحة المعطاة. `https://example.com` يجب أن يسمح بتحميل البرامج النصية من الأصول التي حددتها بينما البرامج النصية من `https://evil. لا ينبغي السماح لـ ttacker.com` بتشغيل. تعريف CSP هو طريقة سهلة لتحسين أمن التطبيق الخاص بك.

سيسمح CSP التالي لإلكترون بتنفيذ البرامج النصية من الموقع الحالي ومن `apis.example.com`.

```plaintext
// سيئ
المحتوى - سياسة الأمن: '*'

///Good
Content-Security-Policy: script-src 'self' https://apis.example.com
```

### رأس HTTP CSP

يحترم إلكترون [`سياسة أمن المحتوى` رأس HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) والذي يمكن تعيينه باستخدام Electron's [`webrequest.onHeadersReceived`](../api/web-request.md#webrequestonheadersreceivedfilter-listener) المعالج :

```javascript
const { session } = require('electron')

session.defaultSession.webrequest. nheadersreceieived(التفاصيل، استرجاع المكالمة) => {
  رد المكالمة ({
    الردات: {
      .details.responseheaders,
      'Content-Security-Policy': ['default-src \'none\'']
    }
  })
})
```

### CSP Meta Tag

آلية التوصيل المفضلة لـ CSP، هي عنوان HTTP، ومع ذلك، ليس من الممكن استخدام هذه الطريقة عند تحميل مورد باستخدام بروتوكول `الملف:/`. يمكن أن يكون مفيدا في بعض الحالات، مثل استخدام ملف `/ /` بروتوكول ، لتعيين سياسة على صفحة مباشرة في العلامة باستخدام وسم `<meta>`:

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'none'">
```


## 7) لا تقم بتعيين `allowRunningInSecureContent` إلى `true`

_التوصية هي الافتراضي لـ Electron's_

بشكل افتراضي، لن يسمح إلكترون لمواقع الويب المحملة فوق `HTTPS` بتحميل و تنفيذ البرامج النصية ، CSS، أو الإضافات من مصادر غير آمنة (`HTTP`). تعيين الخاصية `allowRunningInsecure ureContent` إلى `true` يعطل تلك الحماية.

تحميل HTML المبدئي لموقع عبر `HTTPS` ومحاولة تحميل موارد لاحقة عبر `HTTP` يعرف أيضًا باسم "محتوى مختلط".

### لما؟

تحميل المحتوى عبر `HTTPS` يضمن صحة وسلامة الموارد التي تم تحميلها أثناء تشفير حركة المرور نفسها. راجع القسم على [فقط عرض المحتوى الآمن](#1-only-load-secure-content) لمزيد من التفاصيل.

### كيف؟

```js
//
سيئ النوافذ الرئيسية = متصفح جديد ({
  تفضيلات الويب: {
    allowRunningInsecureContent: true
  }
})
```

```js
// جيد
const mainWindow = متصفح جديد ({})
```


## 8) عدم تمكين الميزات التجريبية

_التوصية هي الافتراضي لـ Electron's_

يمكن للمستخدمين المتقدمين في إلكترون تمكين ميزات كروميوم التجريبية باستخدام خاصية `اختبارية`

### لما؟

الميزات التجريبية هي، كما يقترح الاسم، تجريبية ولم يتم تمكين لجميع مستخدمي Chromium. علاوة على ذلك، فإن تأثيرها على إلكترون ككل لم يتم اختباره.

حالات الاستخدام المشروعة موجودة، ولكن ما لم تعرف ما تقوم به، يجب عدم تمكين هذه الخاصية.

### كيف؟

```js
//
سيئ النوافذ الرئيسية = متصفح جديد ({
  تفضيلات الويب: {
    experimentalFeatures: true
  }
})
```

```js
// جيد
const mainWindow = متصفح جديد ({})
```


## 9) لا تستخدم `ميزات تمكين Blinkures`

_التوصية هي الافتراضي لـ Electron's_

البوميض هو اسم محرك التقديم خلف كروميوم. كما هو الحال مع `ميزات تجريبية`، خاصية `تمكين ميزات Blinkatures` تسمح للمطورين بـ تمكين الميزات التي تم تعطيلها بشكل افتراضي.

### لما؟

بشكل عام، هناك أسباب وجيهة محتملة إذا لم يتم تمكين الميزة بشكل افتراضي. توجد حالات استخدام مشروعة لتمكين خصائص محددة. باعتبارك مطور ، يجب أن تعرف بالضبط لماذا تحتاج إلى تمكين ميزة، ما هي تداعيات وكيف يؤثر ذلك على أمن تطبيقك. تحت لا يجب على أي ظروف تمكين الميزات بشكل مخيف.

### كيف؟
```js
//
سيئ النوافذ الرئيسية = متصفح جديد ({
  تفضيلات الويب: {
    enableBlinkFeatures: 'ExecCommandInJavaScript'
  }
})
```

```js
// جيد
const mainWindow = متصفح جديد()
```


## 10) لا تستخدم `السماح المنبثقات`

_التوصية هي الافتراضي لـ Electron's_

If you are using [`<webview>`][webview-tag], you might need the pages and scripts loaded in your `<webview>` tag to open new windows. The `allowpopups` attribute enables them to create new [`BrowserWindows`][browser-window] using the `window.open()` method. `<webview>` العلامات غير مسموح لولا ذلك بإنشاء نوافذ جديدة .

### لما؟

If you do not need popups, you are better off not allowing the creation of new [`BrowserWindows`][browser-window] by default. هذا يتبع مبدأ الحد الأدنى من الوصول المطلوب: لا تدع الموقع ينشئ نوافذ منبثقة جديدة ما لم تعرف أنه يحتاج إلى هذه الميزة.

### كيف؟

```html<!-- سيء --><webview allowpopups src="page.html"></webview><!-- جيد --><webview src="page.html"></webview>
```


## 11) التحقق من خيارات WebView قبل الإنشاء

سيتم إنشاء WebView في عملية إعادة عرض التي لا تحتوي على تكامل Node.js تمكين لن تكون قادرة على تمكين التكامل نفسه. ومع ذلك، فإن WebView سوف دائما ينشئ عملية عرض مستقلة مع `تفضيلات الويب` الخاصة به.

It is a good idea to control the creation of new [`<webview>`][webview-tag] tags from the main process and to verify that their webPreferences do not disable security features.

### لما؟

منذ `<webview>` يعيش في DOM، يمكن إنشاؤها بواسطة برنامج نصي يعمل على موقع الويب الخاص بك حتى لو كانت العقدة. يتم تعطيل التكامل لولا ذلك.

يتيح إلكترون للمطورين تعطيل مختلف ميزات الأمان التي تتحكم في عملية المعرض. In most cases, developers do not need to disable any of those features - and you should therefore not allow different configurations for newly created [`<webview>`][webview-tag] tags.

### كيف؟

Before a [`<webview>`][webview-tag] tag is attached, Electron will fire the `will-attach-webview` event on the hosting `webContents`. استخدم الحدث لمنع إنشاء `مشاهدات ويب` مع خيارات محتملة غير آمنة.

```js
المحتويات app.on('web-contents-created'، (الحدث، المحتويات) => {
  . لا ('will-attach-webview', (الحدث, webPreferenes), Params) => {
    // قطع البرامج النصية للتحميل المسبق إذا كانت غير مستخدمة أو تحقق من موقعها مشروع
    حذف تفضيلات الويب. إعادة تحميل
    حذف تفضيلات الويب. إعادة تحميل URL

    // تعطيل تكامل Node.js
    webPreferences. odeintegration = خطأ

    //التحقق من عنوان URL الذي يتم تحميله
    إذا (!params. rc.startsWith('https://example.com/')) {
      event.preventDefault()

  })
})
```

ومرة أخرى، فإن هذه القائمة لا تؤدي إلا إلى التقليل من المخاطر، كما أنها لا تحذفها. إذا كان هدفك هو عرض موقع، فسيكون المتصفح خيارًا أكثر أمانًا.

## 12) تعطيل الملاحة أو الحد منها

إذا كان التطبيق الخاص بك لا يحتاج إلى التنقل أو يحتاج فقط إلى الانتقال إلى الصفحات المعروفة، هي فكرة جيدة للحد من التنقل فوراً إلى هذا النطاق المعروف، وعدم السماح بأي نوع آخر من التنقل.

### لما؟

الملاحة هي ناقل هجوم شائع. إذا كان بإمكان مهاجم أن يقنع تطبيقك بـ التنقل بعيدا عن صفحته الحالية، قد يجبرون تطبيقك على فتح مواقع الويب على الإنترنت. حتى إذا تم تكوين محتويات الويب `الخاصة بك` لتكون أكثر أماناً (مثل وجود `عقد اندماج` معطلة أو `سياق` ممكناً)، جعل التطبيق الخاص بك لفتح موقع ويب عشوائي سيجعل عمل استغلال تطبيق الخاص بك أسهل بكثير.

نمط الهجوم الشائع هو أن المهاجم يقنع مستخدمي تطبيقك بالتفاعل مع التطبيق بطريقة تمكنه من الانتقال إلى إحدى الصفحات للمهاجم. ويتم ذلك عادة عن طريق الروابط، أو الإضافات، أو أي محتوى آخر ينشئه المستخدم.

### كيف؟

If your app has no need for navigation, you can call `event.preventDefault()` in a [`will-navigate`][will-navigate] handler. إذا كنت تعرف صفحات التطبيق الخاص بك قد ينتقل إلى: تحقق من عنوان URL في معالج الحدث ودع التنقل يحدث فقط إذا كان يتطابق مع عناوين URL التي تتوقعها.

ننصح بأن تستخدم "مركز العقد" لعناوين URLs. يمكن إجراء مقارنات سلاسل بسيطة في بعض الأحيان خداع - `startsWith('https://example.com')` اختبار يسمح `https://example.com.attacker.com` من خلال.

```js
const URL = مطلوب('url').URL

app.on('web-contents-created', (الحدث, المحتويات) => {
  . n('سنسح'، (الحدث، الملاحةUrl) => {
    const parsedUrl = URL(avigationUrl) الجديد

    إذا (parsedUrl. rigin !== 'https://example.com') {
      event.preventDefault()
    }
  })
})
```

## 13 - تعطيل إنشاء نوافذ جديدة أو الحد منها

إذا كان لديك مجموعة معروفة من النوافذ، فمن الجيد الحد من إنشاء نوافذ إضافية في التطبيق الخاص بك.

### لما؟

Much like navigation, the creation of new `webContents` is a common attack vector. يحاول المهاجمون إقناع التطبيق الخاص بك بإنشاء نوافذ جديدة أو إطارات أو أو أي عمليات أخرى من عمليات العارض مع امتيازات أكثر من ذي قبل؛ أو مع فتح صفحات لا يمكن فتحها من قبل.

إذا لم تكن بحاجة إلى إنشاء نوافذ بالإضافة إلى النوافذ التي تعرفها فستحتاج إلى إنشائها، تعطيل إنشاء يشتريك القليل من الأمان الإضافي بدون تكلفة. هذه هي عادة الحال بالنسبة للتطبيقات التي تفتح واحدة `نافذة المتصفح` ولا تحتاج إلى فتح عدد اعتباطي من النوافذ الإضافية عند التشغيل.

### كيف؟

[`webContents`][web-contents] will emit the [`new-window`][new-window] event before creating new windows. سيتم تمرير هذا الحدث، ضمن معلمات أخرى ، عنوان url `` المطلوب من النافذة فتحه والخيارات المستخدمة لإنشائه. نوصي بأن تستخدم الحدث لتمحيص إنشاء نوافذ، وتقصره على ما تحتاجه فقط.

```js
const { shell } = require('electron')

app.on('web-contents-created'، (الحدث، المحتويات) => {
  المحتويات. n('new-window', async (الحدث, avigationUrl) => {
    // في هذا المثال سوف نطلب من نظام التشغيل
    // فتح عنوان URL لهذا الحدث في المتصفح الافتراضي.
    event.preventDefault()

    انتظار shell.openExternal(navigationUrl)
  })
})
```

## 14) لا تستخدم `الفتح الخارجي` مع محتوى غير موثوق به

Shell's [`openExternal`][open-external] allows opening a given protocol URI with the desktop's native utilities. على سبيل المثال macOS هذه الوظيفة مشابهة لجهاز الأوامر الطرفية `المفتوح` وسوف تفتح التطبيق المحدد استناداً إلى رابط URI ونوع الملف.

### لما؟

Improper use of [`openExternal`][open-external] can be leveraged to compromise the user's host. عند استخدام openExternal مع محتوى غير موثوق به، يمكن استخدام لتنفيذ الأوامر التعسفية.

### كيف؟

```js
//
سيئة { shell } = مطلوبة ('electron')
shell.openExternal(USER_CONTROLLED_DATA_HERE)
```
```js
// جيد
const { shell } = require('electron')
shell.openExternal('https://example.com/index.html')
```

## 15) تعطيل وحدة `عن بعد`

توفر وحدة `البعيد` طريقة لعمليات المعرض لـ الوصول إلى واجهات برمجة التطبيقات المتاحة عادة في العملية الرئيسية فقط. باستخدامه، المعرض يمكنه استدعاء طرق لعنصر عملية رئيسي دون إرسال رسائل بين العمليات صراحة. إذا كان تطبيق سطح المكتب الخاص بك لا يعمل محتوى غير موثوق به، هذا يمكن أن يكون طريقة مفيدة للحصول على إمكانية الوصول إلى عمليات العارض و العمل مع الوحدات النمطية المتاحة فقط للعملية الرئيسية. مثل وحدات ذات صلة بـ GUI (حوارات، قوائم، إلخ).

However, if your app can run untrusted content and even if you [sandbox][sandbox] your renderer processes accordingly, the `remote` module makes it easy for malicious code to escape the sandbox and have access to system resources via the higher privileges of the main process. لذلك، يجب أن يكون معطلا في مثل هذه الظروف.

### لما؟

`البعيد` يستخدم قناة IPC الداخلية للتواصل مع العملية الرئيسية. هجمات "نموذج التلوث" يمكن أن تمنح الوصول إلى التعليمات البرمجية الضارة إلى قناة IPC الداخلية ، والذي يمكن استخدامه بعد ذلك للهروب من صندوق الرمل عن طريق محاكاة `البعيد` رسائل IPC والوصول إلى وحدات العمليات الرئيسية التي تعمل مع امتيازات أعلى .

بالإضافة إلى ذلك، من الممكن تحميل البرامج النصية مسبقاً إلى وحدات تسرب عن طريق الخطأ إلى عارض مربع رملي. تسريب `` رمز خبيث للأسلحة مع عدد كبير من وحدات العمليات الرئيسية للقيام بالهجوم.

تعطيل وحدة `البعيد` يزيل ناقلات الهجوم هذه. تمكين عزل السياق يمنع أيضًا هجمات "النموذج الأولي" من بنجاح.

### كيف؟

```js
// سيئ إذا كان المعرض يستطيع تشغيل محتوى غير موثوق به
مشغل mainWindow = متصفح جديد ({})
```

```js
// جيد
const mainWindow = متصفح جديد Window({
  webPreferences: {
    enableRemoteModule: false
  }
})
```

```html<!-- سيئ إذا كان المعرض يستطيع تشغيل محتوى غير موثوق به --><webview src="page.html"></webview><!-- جيد --><webview enableremotemodule="false" src="page.html"></webview>
```

## 16) تصفية وحدة `البعيد`

إذا لم تستطع تعطيل وحدة `عن بعد` ، يجب عليك تصفية الكومبيت, العقدة, و وحدات إلكترون (ما يسمى بوحدات مدمجة) يمكن الوصول إليها عن طريق `عن بعد` التي لا يحتاج إليها تطبيقك. يمكن القيام بذلك عن طريق حظر وحدات معينة بالكامل وعن طريق استبدال وحدات أخرى بوكلاء يعرضون فقط الوظيفة التي يحتاج إليها تطبيقك.

### لما؟

ونظرا لامتيازات الدخول إلى النظام في العملية الرئيسية، الوظيفة التي توفرها وحدات العملية الرئيسية قد تكون خطيرة في أيدي التعليمات البرمجية الخبيثة التي تعمل في عملية عرض معرضة للخطر. من خلال الحد من مجموعة الوحدات التي يمكن الوصول إليها إلى الحد الأدنى الذي يحتاج إليه تطبيقك و تصفية الآخرين، يمكنك تقليل مجموعة الأدوات التي يمكن أن يستخدمها الرمز الخبيث لمهاجمة النظام.

لاحظ أن الخيار الأكثر أمنا هو [تعطيل الوحدة النمطية عن بعد](#15-disable-the-remote-module) بالكامل. إذا اخترت تصفية الوصول بدلاً من تعطيل الوحدة تمامًا، يجب أن تكون حذرا جدا للتأكد من أنه لا يوجد تصعيد للامتيازات ممكن من خلال الوحدات التي تسمح بتجاوز عامل التصفية.

### كيف؟

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

## 17) استخدام نسخة حالية من إلكترون

يجب عليك أن تسعى دائما إلى استخدام أحدث إصدار متاح من إلكترون. كلما تم إصدار نسخة رئيسية جديدة، يجب عليك محاولة تحديث تطبيق الخاص بك في أسرع وقت ممكن.

### لما؟

تم بناء تطبيق مع نسخة قديمة من إلكترون وكروميوم والعقدة. s هدف أسهل من التطبيق الذي يستخدم إصدارات أحدث من هذه المكونات. بشكل عام، مشاكل الأمن واستغلال الإصدارات القديمة من Chromium و Node.js متوفرة على نطاق أوسع.

كل من Chromium و Node.js هي ميزات مثيرة للإعجاب للهندسة بنيت بواسطة آلاف المطورين الموهوبين. ونظرا لشعبيتهم، فإن أمنهم يتم اختباره بعناية وتحليله من قبل باحثين أمنيين متساويين في المهارة. Many of those researchers [disclose vulnerabilities responsibly][responsible-disclosure], which generally means that researchers will give Chromium and Node.js some time to fix issues before publishing them. سيكون تطبيقك أكثر أمنا إذا كان يقوم بتشغيل إصدار حديث من إلكترون (ومن ثم الكروميوم والعقدة). ) لـ أي مشاكل أمنية محتملة ليست معروفة على نطاق واسع.


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
---
title: تغييرات API القادمة في إلكترون 1.0
author: zcbenz
date: '2015-11-17'
---

منذ بداية إلكترون، بدءاً من الوراء عندما كانت تسمى الذاكرة - شل، لقد قمنا بتجربة توفير واجهة برمجة تطبيقات جافا سكريبت اللطيفة عبر المنصة لوحدة محتوى كروميوم ومكونات واجهة المستخدم الأصلية. بدأ تطبيق APIs بشكل عضوي جداً، ومع مرور الوقت قمنا بعدة تغييرات لتحسين التصاميم الأولية.

---

الآن مع تجهيز إلكترون لإصدار 1.0، نود أن نغتنم الفرصة للتغيير من خلال معالجة آخر تفاصيل API الملتهبة. التغييرات الموصوفة أدناه مدرجة في **0.35.**، مع APIs القديمة الإبلاغ عن تحذيرات المهمشة حتى تتمكن من التحديث لإصدار 1.0 في المستقبل. لن يكون إلكترون 1.0 خارجا لبضعة أشهر لذلك لديك بعض الوقت قبل أن تصبح هذه التغييرات مكسورة.

## تحذيرات الإهمال

بشكل افتراضي، ستظهر التحذيرات إذا كنت تستخدم APIs المهملة. لإيقاف تشغيلهم، يمكنك تعيين `process.noDegiation` إلى `true`. لتتبع مصادر استخدامات API المهملة، يمكنك تعيين `العملية. مهمل` إلى `صحيح` لطرح استثناءات بدلاً من طباعة التحذيرات، أو تعيين `العملية. الإهمال العرقي` إلى `صحيح` لطباعة آثار الإهمال.

## طريقة جديدة لاستخدام الوحدات المدمجة

الوحدات المدمجة يتم الآن تجميعها في وحدة واحدة بدلاً من تقسيمها إلى وحدات مستقلة، حتى تتمكن من استخدامهم [دون تضارب مع وحدات أخرى](https://github.com/electron/electron/issues/387):

```javascript
var app = require('electron').app
var BrowserWindow = require('electron').BrowserWindow
```

الطريقة القديمة لـ `مطلوبة ('app')` لا تزال مدعومة للتوافق الخلفي، ولكن يمكنك أيضًا إيقاف تشغيل:

```javascript
يتطلب ('electron').hideInternalModules()
متطلب ('app') // رمي خطأ.
```

## طريقة أسهل لاستخدام وحدة `البعيد`

وبسبب الطريقة التي تغير بها استخدام الوحدات المدمجة، جعلنا من الأسهل استخدام وحدات الجانب الرئيسي للمعالجة في عملية العرض. يمكنك الآن فقط الوصول إلى `عن بعد`سمات لاستخدامها:

```javascript
// طريقة جديدة.
var app = require('electron').remote.app
var BrowserWindow = require('electron').remote.BrowserWindow
```

بدلاً من استخدام سلسلة طلب طويلة:

```javascript
// الطريقة القديمة.
var app = require('electron').remote.require('app')
var BrowserWindow = require('electron').remote.require('BrowserWindow')
```

## تقسيم وحدة `ipc`

وحدة `ipc` موجودة على كل من العملية الرئيسية وعملية العرض و API مختلفة على كل جانب. وهو أمر مربك للغاية بالنسبة للمستخدمين الجدد. لقد قمنا بإعادة تسمية الوحدة إلى `ipcMain` في العملية الرئيسية، و `ipcRenderer` في عملية العرض لتجنب الخلط:

```javascript
// In main process.
var ipcMain = مطلوبة ('electron').ipcMain
```

```javascript
// في عملية العرض.
var ipcRenderer = require('electron').ipcRenderer
```

وبالنسبة لوحدة `ipcRenderer` ، تم إضافة عنصر `إضافي` عند تلقي الرسائل، لمطابقة كيفية التعامل مع الرسائل في `ipcMain` وحدة:

```javascript
ipcRenderer.on('message', function (event) {
  console.log(event)
})
```

## توحيد خيارات `نافذة المتصفح`

خيارات `نافذة المتصفح` لها أنماط مختلفة استنادًا إلى خيارات API الأخرى، وكان من الصعب نوعا ما استخدامها في جافا سكريبت بسبب `-` في الأسماء. تم توحيدها الآن مع أسماء جافا سكريبت التقليدية:

```javascript
نافذة متصفح جديدة({ minWidth: 800, minHeight: 600 })
```

## متابعة اتفاقيات DOM لأسماء API

أسماء API في إلكترون المستخدمة لتفضيل إميل حرف الجمال لجميع أسماء API، مثل `Url` إلى `URL`، ولكن DOM لديها اتفاقياتها الخاصة بها، ويفضلون `URL` على `URL`، أثناء استخدام `معرف` بدلاً من `ID`. لقد قمنا بإعادة تسمية API التالية لمطابقة أنماط DOM:

* `عنوان URL` أعيد تسميته إلى `URL`
* `Csp` أعيد تسميتها إلى `CSP`

ستلاحظ الكثير من الإهمال عند استخدام إلكترون v0.35.0 للتطبيق الخاص بك بسبب هذه التغييرات. طريقة سهلة لإصلاحها هي استبدال جميع مثيلات `URL` بـ `URL`.

## تغييرات على أسماء الحدث `Tray`

نمط `Tray` اسم الحدث كان مختلفا بعض الشيء عن وحدات أخرى لذلك تم إعادة الاسم لجعله مطابقا للأخرى.

* `انقر` أعيد تسميتها إلى `انقر`
* `ضغطة مزدوجة` أعيد تسميتها إلى `ضغطة مزدوجة`
* `انقر بالزر الأيمن` أعيدت تسميتها إلى `انقر بالزر الأيمن`

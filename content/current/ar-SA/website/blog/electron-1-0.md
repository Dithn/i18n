---
title: Electron 1.0
author: سيد
date: '2016-05-11'
---

خلال السنتين الماضيتين، ساعد إلكترون المطورين على بناء تطبيقات سطح المكتب عبر المنصة باستخدام HTML و CSS و جافا سكريبت. نحن الآن متحمسون لمشاركة معلم رئيسي لإطار عملنا وللمجتمع الذي أنشأه. الإصدار من إلكترون 1.0 متاح الآن من [electronjs.org](https://electronjs.org).

---

![Electron 1.0](https://cloud.githubusercontent.com/assets/378023/15007352/315f5eea-1213-11e6-984e-21f5dab31267.png)

يمثل إلكترون 1.0 معلما رئيسيا في استقرار API ونضجه. هذا الإصدار يسمح لك ببناء تطبيقات تعمل و تشعر بأنها أصلية حقا على ويندوز، Mac، و لينوكس. بناء تطبيقات إلكترون أسهل من أي وقت مضى مع مستندات جديدة، أدوات جديدة، وتطبيق جديد للمشي من خلال واجهة برمجة تطبيقات إلكترون.

إذا كنت على استعداد لبناء تطبيق إلكترون الأول الخاص بك، إليك [دليل البداية السريعة](https://electronjs.org/docs/tutorial/quick-start) لمساعدتك على البدء.

نحن متحمسون لرؤية ما تبنيه بعد ذلك باستخدام إلكترون.

## مسار اليكتروني

لقد أصدرنا إلكترون عندما أطلقنا [Atom](https://atom.io) قبل أكثر من عامين بقليل. إلكترون ، الذي كان يعرف آنذاك باسم آتوم شيل ، كان الإطار الذي بنيناه آتوم فوق في تلك الأيام، Atom كانت القوة الدافعة وراء الميزات والوظائف التي وفرها إلكترون كما قمنا بالضغط للحصول على الإصدار الأولي من Atom

الآن قيادة إلكترون هي مجموعة متنامية من المطورين والشركات تبني كل شيء من [البريد الإلكتروني](https://nylas.com)، [المحادثة](https://slack.com)، و [تطبيقات Git](https://www.gitkraken.com) إلى [أدوات تحليل SQL](https://www.wagonhq.com)، [عملاء التورينت](https://webtorrent.io/desktop)و [الروبوتات](https://www.jibo.com).

في هاتين السنتين الماضيتين رأينا كلا من الشركات والمشاريع المفتوحة المصدر اختر إلكترون كأساس لتطبيقاتها. فقط في العام الماضي، تم تنزيل إلكترون أكثر من 1.2 مليون مرة. [قم بجولة](https://electronjs.org/apps) لبعض تطبيقات إلكترون المدهشة وأضف الخاص بك إذا لم تكن موجودة بالفعل.

![تنزيلات إلكترون](https://cloud.githubusercontent.com/assets/378023/15037731/af7e87e0-12d8-11e6-94e2-117c360d0ac9.png)

## عروض API إلكترون

إلى جانب 1. اصدار، نحن نقوم بإصدار تطبيق جديد لمساعدتك على استكشاف إلكترون APIs وتعلم المزيد حول كيفية جعل تطبيق إلكترون الخاص بك يشعر بالأم. يحتوي تطبيق [إلكترون API التجريبي](https://github.com/electron/electron-api-demos) على كتل برمجية لمساعدة على بدء التطبيق الخاص بك وإرشادات على نحو فعال باستخدام واجهة برمجة تطبيقات إلكترون.

[![عروض API إلكترون](https://cloud.githubusercontent.com/assets/378023/15138216/590acba4-16c9-11e6-863c-bdb0d3ef3eaa.png)](https://github.com/electron/electron-api-demos)

## Devtron

لقد أضفنا أيضا ملحق جديد لمساعدتك على تصحيح أخطاء تطبيقات إلكترون الخاصة بك. [Devtron](https://electronjs.org/devtron) هو ملحق مفتوح المصدر لـ [أدوات مطور كروم](https://developer.chrome.com/devtools) مصممة لمساعدتك في التفتيش، تصحيح الأخطاء، واستكشاف الأخطاء في تطبيق إلكترون.

[![Devtron](https://cloud.githubusercontent.com/assets/378023/15138217/590c8b06-16c9-11e6-8af6-ef96299e85bc.png)](https://electronjs.org/devtron)

### الميزات

  * **يتطلب الرسم البياني** الذي يساعدك على تصور تبعيات مكتبتك الداخلية والخارجية في كل من عمليتي العرض الرئيسية
  * **مراقبة IPC** التي تقوم بتعقب وعرض الرسائل المرسلة و المتلقاة بين العمليات في التطبيق الخاص بك
  * **مفتش الحدث** الذي يظهر لك الأحداث والمستمعين المسجلين في التطبيق الخاص بك على واجهة برمجة تطبيقات إلكترون الأساسية مثل النافذة، التطبيق والعمليات
  * **شتاء التطبيق** الذي يتحقق من تطبيقاتك عن الأخطاء الشائعة ويفتقد وظائف

## Spectron

أخيرا، نحن نصدر نسخة جديدة من [Spectron](https://electronjs.org/spectron)، إطار اختبار التكامل لتطبيقات إلكترون.

[![Spectron](https://cloud.githubusercontent.com/assets/378023/15138218/590d50c2-16c9-11e6-9b54-2d73729fe189.png)](https://electronjs.org/spectron)

3 - سيكترونا لديه دعم شامل لكامل واجهة برمجة تطبيقات إلكترون يسمح لك بكتابة اختبارات أكثر سرعة للتحقق من سلوك تطبيقك في مختلف السيناريوهات والبيئات. Spectron مبني على [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver) و [WebDriverIO](http://webdriver.io) حتى يكون لديه أيضًا APIs كاملة للتنقل في الصفحات، مستخدم مدخلات، وتنفيذ جافا سكريبت.

## المجتمع

إلكترون 1.0 هو نتيجة لجهود مجتمعية بذلها مئات من المطورين. خارج الإطار الأساسي، كان هناك مئات المكتبات والأدوات التي تم إصدارها لجعل البناء والتعبئة ونشر تطبيقات إلكترون أسهل.

هناك الآن صفحة مجتمع [جديدة](https://electronjs.org/community) تسرد العديد من الأدوات والتطبيقات والمكتبات والإطارات الرائعة التي يجري تطويرها. يمكنك أيضًا التحقق من [إلكترون](https://github.com/electron) و [مستخدم إلكترون](https://github.com/electron-userland) منظمة لرؤية بعض هذه المشاريع الرائعة.

جديد في إلكترون؟ مشاهدة فيديو البريد الإلكتروني 1.0:

<div class="video"><iframe src="https://www.youtube.com/embed/8YP_nOCO-4Q?rel=0" frameborder="0" allowfullscreen></iframe></div>

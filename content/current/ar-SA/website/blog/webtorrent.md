---
title: 'مشروع الأسبوع: WebTorrent'
author:
  - الأخاديد
  - zeke
date: '2017-03-14'
---

هذا الاسبوع حصلنا على [@feross](https://github.com/feross) و [@dcposch](https://github.com/dcposch) للتحدث عن WebTorrent، عميل تورنت الذي يعمل على الويب والذي يربط المستخدمين معا لتكوين شبكة متصفح موزعة لامركزية إلى متصفح.

---

## ما هو WebTorren؟

[WebTorrent](https://webtorrent.io) هو أول عميل تورنت يعمل في المتصفح. تم كتابتها بالكامل في جافا سكريبت ويمكنها استخدام WebRTC لنقل الأقران إلى الأقران. لا حاجة إلى إضافة أو إضافة أو تثبيت المتصفح.

باستخدام معايير الويب المفتوحة، يقوم WebTorrent بربط مستخدمي الموقع معاً لتكوين شبكة متصفح موزعة لامركزية إلى متصفح لنقل الملفات بكفاءة.

يمكنك مشاهدة عرض تجريبي لـ WebTorrent في العمل هنا: [webtorrent.io](https://webtorrent.io/).

<a href="https://webtorrent.io/">
  <img alt="صفحة الويب الرئيسية" src="https://cloud.githubusercontent.com/assets/2289/23912149/1543d2ce-089c-11e7-8519-613740c82b47.jpg">
</a>

## لماذا هذا رائع؟

تخيل موقع فيديو مثل يوتيوب، ولكن حيث يساعد الزوار على استضافة محتوى الموقع. كلما زاد عدد الناس الذين يستخدمون موقع الويب بواسطة WebTorrent، كلما أصبح أسرع وأكثر مرونة.

التواصل بين المتصفح و المتصفح يقلل من الوسيط و يتيح للناس التواصل بشروطهم الخاصة. لا يوجد المزيد من العملاء/الخادم - فقط شبكة من الأقران، كلها متساوية. إن WebTorrent هي الخطوة الأولى في الرحلة لإعادة إضفاء الطابع اللامركزي على الإنترنت.

## أين يأتي إلكترون إلى الصورة؟

قبل حوالي عام، قررنا بناء [سطح مكتب WebTorrent](https://webtorrent.io/desktop/)، نسخة من WebTorrent التي تعمل كتطبيق سطح المكتب.

[![نافذة مشغل سطح المكتب WebTorrent](https://cloud.githubusercontent.com/assets/2289/23912152/154aef0a-089c-11e7-8544-869b0cd642b1.jpg)](https://webtorrent.io/desktop/)

أنشأنا سطح المكتب لـ WebTorrent لثلاثة أسباب:

1. أردنا تطبيق تورنت مفتوح المصدر نظيف خفيف الوزن وخالي من الإعلانات
2. أردنا تطبيق تورنت مع دعم جيد للبث
3. نحن بحاجة إلى "عميل مختلط" يربط شبكات BitTorrent و WebTorrent

## إذا كان بإمكاننا بالفعل تنزيل التورينت في متصفح الويب الخاص بي، فلماذا تطبيق سطح المكتب؟

أولاً، القليل من الخلفية حول تصميم WebTorrent.

<a href="https://webtorrent.io/desktop/">
  <img alt="شعار سطح المكتب webtorrent" src="https://cloud.githubusercontent.com/assets/2289/23912151/154657e2-089c-11e7-9889-6914ce71ebc9.png" width="200" align="right">
</a>

في الأيام الأولى، استخدم BitTorrent TCP كبروتوكول نقل. وفي وقت لاحق، أتى البرنامج على أمل تحقيق أداء أفضل ومزايا إضافية على البرنامج التركي للبضائع. كل عميل تورنت اعتمد في نهاية المطاف uTP ، واليوم يمكنك استخدام BitTorrent على أي من البروتوكولين. ويمثل بروتوكول WebRTC الخطوة المنطقية التالية. وهو يجلب الوعد بإمكانية التشغيل المتبادل مع متصفحات الويب - شبكة P2P عملاقة تتألف من جميع عملاء BitTorrent على سطح المكتب وملايين متصفحات الويب.

"أقران الويب" (أقران التورنت الذين يعملون في متصفح الويب) يجعلون شبكة BitTorrent أقوى من خلال إضافة الملايين من الأقران الجدد، ونشر BitTorrent إلى عشرات من حالات الاستخدام الجديدة. ويتبع WebTorrent سرعة BitTorrent قدر الإمكان، لجعل من السهل على عملاء BitTorrent الحاليين إضافة دعم إلى WebTorrent.

بعض تطبيقات التورنت مثل [Vuze](https://www.vuze.com/) تدعم بالفعل أقران الويب، لكننا لم نرغب في الانتظار حتى يقوم الباقي بإضافة الدعم. **بشكل أساسي، سطح المكتب WebTorrent كان طريقنا لتسريع اعتماد بروتوكول WebTorrent** من خلال إنشاء تطبيق تورنت رائع يريد الناس حقاً استخدامه، نحن نزيد عدد الأقران في الشبكة الذين يمكنهم مشاركة التورنت مع أقران الويب (١). . مستخدمون على مواقع الويب).

## ما هي بعض حالات الاستخدام المثيرة للاهتمام للسورنت بما يتجاوز ما يعرف الناس بالفعل أنه يمكنهم فعله؟

أحد أكثر الاستخدامات إثارة لـ WebTorrent هو تقديم المساعدة من الأقران. يمكن للمشاريع التي لا تستهدف الربح مثل [ويكيبيديا](https://www.wikipedia.org/) و [أرشيف الإنترنت](https://archive.org/) أن تقلل من عرض النطاق الترددي وتكاليف الاستضافة من خلال السماح للزوار برقائق دخول. يمكن خدمة المحتوى المشهور في المتصفح إلى المتصفح، بسرعة وبتكلفة زهيدة. يمكن خدمة المحتوى الذي يمكن الوصول إليه بشكل موثوق عبر HTTP من خادم الأصل.

أرشيف الإنترنت في الواقع قام بالفعل بتحديث ملفاتهم التورنت بحيث يعملون بشكل رائع مع WebTorrent. لذا إذا كنت ترغب في تضمين محتوى أرشيف الإنترنت على موقعك، يمكنك القيام بذلك بطريقة تقلل من تكاليف استضافة الأرشيف، السماح لهم بتكريس المزيد من المال لأرشفة الشبكة في الواقع!

وهناك أيضا حالات مثيرة لاستخدام الأعمال التجارية، من CDNs إلى توصيل التطبيقات عبر P2P.

## ما هي بعض مشاريعك المفضلة التي تستخدم WebTorren؟

![لقطة شاشة تطبيق Gaia](https://cloud.githubusercontent.com/assets/2289/23912148/154392c8-089c-11e7-88a8-3d4bcb1d2a94.jpg)

أروع شيء تم بناؤه باستخدام WebTorrent، ربما هو [خريطة 3D للنجوم](http://charliehoey.com/threejs-demos/gaia_dr1.html). إنها محاكاة تفاعلية ثلاثية الأبعاد لدرب اللبان. تحميل البيانات من تورنت, مباشرة في المتصفح الخاص بك. من الملهم أن تطير عبر نظامنا النجمي وتدرك كيف أننا بشر قليلون مقارنة مع اتساع الكون.

يمكنك أن تقرأ كيف تم صنع هذا في [تورينتنغ المجرة](https://medium.com/@flimshaw/torrenting-the-galaxy-extracting-2-million-3d-stars-from-180gb-of-csvs-457ff70c0f93)، منشور مدونة حيث يشرح المؤلف، تشارلي هوي، كيف بنى خريطة النجمة مع WebGL و WebTorrent.

<a href="https://brave.com/">
  <img alt="شعار شجاع" src="https://cloud.githubusercontent.com/assets/2289/23912147/1542ad4a-089c-11e7-8106-15c8e34298a9.png" width="150" align="left">
</a>

نحن أيضًا مشجعين ضخمين على [الشجعان](https://brave.com/). الشجاعة هي متصفح يقوم تلقائيا بحظر الإعلانات والتتبع لجعل الويب أسرع وأكثر أمانا. دعم التورنت الذي تم إضافته مؤخرا، بحيث يمكنك [عرض التورنت التقليدية دون استخدام تطبيق منفصل](https://torrentfreak.com/brave-a-privacy-focused-browser-with-built-in-torrent-streaming-170219/). هذه الميزة مدعومة من قبل WebTorrent.

لذا ، تماما مثل كيف يمكن لمعظم المتصفحات أن تقدم ملفات PDF ، يمكن للشجاعة أن تجعل روابط المغناطيس وملفات التورنت . إنها مجرد نوع آخر من المحتوى يدعمه المتصفح أصلاً.

أحد مؤسسي الشجعان هو في الواقع بريندان إيش، مخلوق جافاسكريبت، اللغة التي كتبناها في WebTorrent ، لذلك نعتقد أنه من الرائع جداً أن الشجعان اختاروا دمج WebTorrent.

## لماذا اخترت بناء سطح مكتب WebTorrent على Electron؟

<a href="https://webtorrent.io/desktop/">
  <img alt="نافذة سطح المكتب الرئيسية WebTorrent" src="https://cloud.githubusercontent.com/assets/2289/23912150/15444542-089c-11e7-91ab-7fe3f1e5ee43.jpg" align="right" width="450">
</a>

هناك ميم بأن تطبيقات إلكترون "متضخمة" لأنها تتضمن وحدة محتوى كروم بأكملها في كل تطبيق. في بعض الحالات، هذا صحيح جزئياً (مثبت تطبيق إلكترون عادة ~40 ميغابايت، حيث مثبت تطبيق محدد OS-عادة ~20 ميغابايت).

ومع ذلك، في حالة سطح المكتب WebTorrent ، نستخدم كل خاصية إلكترون تقريبا، والعديد من ميزات كروم في أثناء التشغيل العادي. إذا أردنا تنفيذ هذه الميزات من الصفر لكل منصة، كان من الممكن أن يستغرق بناء تطبيقنا شهوراً أو سنوات أطول، أو كنا فقط قادرين على إصدار منصة واحدة.

فقط للحصول على فكرة، نحن نستخدم دمج حساب إلكترون [](https://electronjs.org/docs/api/app/#appdockbouncetype-macos) (لإظهار تقدم التحميل)، [تكامل شريط القوائم](https://electronjs.org/docs/api/menu) (للتشغيل في الخلفية)، [تسجيل معالج البروتوكول](https://electronjs.org/docs/api/app/#appsetasdefaultprotocolclientprotocol-path-args-macos-windows) (لفتح الروابط المغناطيسية)، [مانع توفير الطاقة](https://electronjs.org/docs/api/power-save-blocker/) (لمنع النوم أثناء تشغيل الفيديو)، و [التحديث التلقائي](https://electronjs.org/docs/api/auto-updater) بالنسبة لميزات Chrome ، نحن نستخدم الكثير: العلامة `<video>` (لتشغيل العديد من صيغ الفيديو المختلفة)، `<track>` علامة (لدعم التسميات التوضيحية المغلقة)، دعم السحب والإفلات و WebRTC (والذي ليس تافهاً لاستخدامه في تطبيق أصلي).

لا تذكر: محرك التورنت الخاص بنا مكتوب في JavaScript ويفترض وجود الكثير من الـ Nالعقدة API، لكن على وجه الخصوص `مطلوب('net')` و `مطلوب('dgram')` لدعم TCP و UDP.

في الأساس، إلكترون هو فقط ما كنا بحاجة إليه وكان لدينا مجموعة دقيقة من الميزات التي كنا بحاجة إليها لشحن تطبيق صلب، مصقول في وقت قياسي.

## ما هي الأشياء المفضلة لديك عن إلكترون؟

وما برحت مكتبة ويبتورنت في طور التطوير كمشروع جانبي مفتوح المصدر منذ سنتين. **قمنا بصنع سطح المكتب على WebTorrent خلال أربعة أسابيع.** إلكترون هو السبب الرئيسي في أننا تمكنا من بناء وشحن تطبيقنا بهذه السرعة.

تماما مثل العقدة. جعل برمجة الخادم متاحة لجيل من المبرمجين الأماميين باستخدام jQuery، و Electron يجعل تطوير التطبيق الأصلي متاحا لأي شخص على دراية بالإنترنت أو العقدة. (ق) التطوير. إلكترون في غاية التمكين.

## هل يشترك الموقع ورمز عميل سطح المكتب؟

نعم، تعمل حزمة [`webtorrent` npm](https://npmjs.com/package/webtorrent) في Node.js، وفي المتصفح، وفي Electron. يمكن تشغيل نفس التعليمات البرمجية بالضبط في جميع البيئات - هذا هو جمال جافا سكريبت. إنه وقت التشغيل العام اليوم. ووعد جافا أبلتس بتطبيقات "الكتابة مرة واحدة، تشغيل أي مكان"، ولكن هذه الرؤية لم تتحقق أبدا لعدد من الأسباب. إلكترون ، أكثر من أي منصة أخرى ، في الواقع ، أصبحت مظلمة جدا بهذا المثل الأعلى .

## ما هي بعض التحديات التي واجهتها أثناء بناء WebTorren؟

في الإصدارات الأولى من التطبيق، كافحنا لجعل واجهة المستخدم تؤدي وظيفتها. وضعنا محرك التورنت في نفس عملية العارض التي ترسم نافذة التطبيق الرئيسية التي، يمكن التنبؤ بها، يؤدي إلى البطء في أي وقت كان هناك نشاط مكثف لوحدة المعالجة المركزية من محرك التورنت (مثل التحقق من قطع التورنت المتلقاة من الأقران).

أصلحنا هذا عن طريق نقل محرك التورنت إلى عملية ثانية غير مرئية للعروض التي نتواصل معها مع [IPC](https://electronjs.org/docs/api/ipc-main/). بهذه الطريقة، إذا استخدمت هذه العملية بإيجاز الكثير من وحدة المعالجة المركزية، فإن خيط واجهة المستخدم لن يتأثر. تمرير اليانصيب وسلسة الرسوم المتحركة مرضية جداً.

ملاحظة: كان علينا أن نضع محرك التورنت في عملية العرض، بدلا من العملية "الرئيسية"، لأننا بحاجة إلى الوصول إلى WebRTC (وهو متاح فقط في العارض).

## في أي مجالات ينبغي تحسين إلكترون؟

شيء واحد نحب رؤيته هو توثيق أفضل حول كيفية بناء وشحن التطبيقات الجاهزة للإنتاج، خاصة حول مواضيع صعبة مثل توقيع التعليمات البرمجية والتحديث التلقائي. كان علينا أن نتعلم عن أفضل الممارسات من خلال الحفر إلى شفرة المصدر والسؤال حول تويتر!

## هل تم تشغيل سطح المكتب WebTorren؟ إن لم يكن الأمر كذلك، ما الذي سيأتي بعد ذلك؟

نحن نعتقد أن الإصدار الحالي من سطح المكتب WebTorrent ممتاز، ولكن هناك دائما مجال للتحسين. نحن نعمل حاليا على تحسين الصقول, الأداء, دعم الترجمة و ترميز الفيديو.

إذا كنت مهتما بالمشاركة في المشروع، تحقق من [صفحة GitHub الخاصة بنا](https://github.com/feross/webtorrent-desktop)!

## أي نصائح لتطوير إلكترون قد تكون مفيدة للمطورين الآخرين؟

[Feross](http://feross.org/)، أحد المساهمين بسطح المكتب في WebTorrent ، في الآونة الأخيرة *"العالم الحقيقي: بناء تطبيقات سطح المكتب عبر المنصة باستخدام جافا سكريبت"* في NodeConf Argentina يحتوي على نصائح مفيدة لإطلاق تطبيق إلكترون مصقول. الحديث مفيد بشكل خاص إذا كنت في المرحلة التي لديك فيها تطبيق عمل أساسي وتحاول أن تأخذه إلى المستوى التالي من الصقل و الاحتراف.

[شاهد هنا](https://www.youtube.com/watch?v=YLExGgEnbFY): <iframe width="100%" height="360" src="https://www.youtube.com/embed/YLExGgEnbFY?rel=0" frameborder="0" allowfullscreen mark="crwd-mark"></iframe>

[الشرائح هنا](https://speakerdeck.com/feross/real-world-electron):

<script async class="speakerdeck-embed" data-id="5aae08bb7c5b4dbd89060cff11bb1300" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

[DC](https://dcpos.ch/)، مساهم آخر في WebTorrent ، كتب [قائمة تحقق من الأشياء التي يمكنك فعلها](https://blog.dcpos.ch/how-to-make-your-electron-app-sexy) لجعل التطبيق الخاص بك يشعر بأنه مصقول و أصلي. يأتي مع أمثلة التعليمات البرمجية ويغطي أشياء مثل دمج رصيف macOS، وسحب وإسقاط وإشعارات سطح المكتب، والتأكد من تحميل التطبيق بسرعة.

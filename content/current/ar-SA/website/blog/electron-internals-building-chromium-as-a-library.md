---
title: 'إلكترون الداخلية: بناء Chromium كمكتبة'
author: zcbenz
date: '2017-03-03'
---

يعتمد إلكترون على Chromium مفتوح المصدر في جوجل، وهو مشروع ليس بالضرورة مصمم لاستخدامه من قبل مشاريع أخرى. يقدم هذا المنشور كيف يتم بناء Chromium كمكتبة لاستخدام Electron's وكيف تطور نظام الإنشاء عبر السنين.

---

## استخدام CEF

إطار الكروم المدمج (CEF) هو مشروع يحول الكروميوم إلى مكتبة، ويوفر واجهة برمجة تطبيقات مستقرة استنادًا إلى كود الكروميوم. الإصدارات الأولى من محرر Atom و NW.js استخدمت CEF.

للحفاظ على API مستقر، يخفي CEF جميع تفاصيل Chromium و wraps Chromium's APIs مع واجهة خاصة به. إذاً عندما نحتاج إلى الوصول إلى أساس APIs لـ Chromium، مثل دمج Node.js في صفحات الويب، أصبح ميزات CEF مانع.

لذا في النهاية قام كل من إلكترون و NW.js بالتبديل إلى استخدام واجهات برمجة تطبيقات Chromium's مباشرة.

## البناء كجزء من الكروميوم

على الرغم من أن كروميوم لا تدعم رسمياً المشاريع الخارجية، الرمز هو وحدة نمطية ومن السهل إنشاء حد أدنى من المتصفح استناداً إلى Chromium. الوحدة النمطية التي توفر واجهة المتصفح تسمى وحدة المحتوى.

لتطوير مشروع مع وحدة المحتوى، أسهل طريقة هي بناء مشروع كجزء من الكروميوم. This can be done by first checking out Chromium's source code, and then adding the project to Chromium's `DEPS` file.

NW.js والإصدارات المبكرة جدا من إلكترون تستخدم هذه الطريقة في البناء.

الجانب السلبي هو، الكروميوم هو رمز كبير جدا ويحتاج إلى آلات قوية جدا لبنائها. بالنسبة للحواسيب المحمولة العادية، قد يستغرق ذلك أكثر من 5 ساعات. لذلك يؤثر هذا بشكل كبير على عدد المطورين الذين يمكن أن يساهموا في مشروع كما أنه يجعل التطوير أبطأ.

## بناء Chromium كمكتبة مشتركة واحدة

كمستخدم لوحدة المحتوى، لا يحتاج إلكترون إلى تعديل كود Chromium في معظم الحالات، لذا فإن طريقة واضحة لتحسين بناء إلكترون هي بناء كروميوم كمكتبة مشتركة، ثم اربط به في إلكترون. بهذه الطريقة لم يعد المطورون بحاجة إلى بناء كل من Chromium عند المساهمة في Electron.

تم إنشاء مشروع [libchromiumcontent](https://github.com/electron/libchromiumcontent) بواسطة [@aroben](https://github.com/aroben) لهذا الغرض. يقوم ببناء وحدة المحتوى لكروميوم كمكتبة مشتركة، ثم يوفر رأسي كروميوم وثنائيات تم بناؤها مسبقاً للتحميل. يمكن العثور على رمز الإصدار الأولي من libchromiumcontent [في هذا الرابط](https://github.com/electron/libchromiumcontent/tree/873daa8c57efa053d48aa378ac296b0a1206822c).

وُلد مشروع [الإضاءة](https://github.com/electron/brightray) أيضا كجزء من محتوى الذي يوفر طبقة رقيقة حول وحدة المحتوى.

من خلال استخدام ليبكروم محتوى وإضاءة معا، يمكن للمطورين بناء متصفح بسرعة دون الدخول في تفاصيل بناء كروميوم. وتزيل الحاجة إلى شبكة سريعة وآلة قوية لبناء المشروع.

بالإضافة إلى إلكترون، كان هناك أيضا مشاريع أخرى قائمة على كروم بنيت بهذه الطريقة مثل [متصفح الخروق](https://www.quora.com/Is-Breach-Browser-still-in-development).

## تصفية الرموز المصدرة

على Windows هناك حد لعدد الرموز التي يمكن لمكتبة مشتركة واحدة أن تصدرها . مع تنامي رمز الكروميوم، سرعان ما تجاوز عدد الرموز المصدرة في ليبكروم المحتوى الحد الأقصى.

وكان الحل هو تصفية الرموز غير اللازمة عند إنشاء ملف DLL. عمل من قبل [توفير `. ف` ملف إلى الرابط](https://github.com/electron/libchromiumcontent/pull/11/commits/85ca0f60208eef2c5013a29bb4cf3d21feb5030b)، ثم باستخدام برنامج نصي إلى [يقرر ما إذا كان ينبغي تصدير الرموز تحت فضاء الاسم ](https://github.com/electron/libchromiumcontent/pull/47/commits/d2fed090e47392254f2981a56fe4208938e538cd).

باتباع هذا النهج، على الرغم من أن Chromium استمر في إضافة رموز جديدة مصدرة، فإن libchromiumcontent يمكن مع ذلك إنشاء ملفات مكتبة مشتركة عن طريق تجريد المزيد من رموز .

## بناء المكون

قبل التحدث عن الخطوات التالية المتخذة في محتوى، من المهم تقديم مفهوم بناء المكون في Chromium أولاً.

كمشروع ضخم، تستغرق خطوة الربط وقتا طويلا جدا في كروميوم عند البناء. عادة عندما يقوم المطور بتغيير صغير، قد يستغرق الأمر 10 دقائق لمشاهدة المخرج النهائي . لحل هذه المشكلة، قدم Chromium بناء المكون، الذي يبني كل وحدة في Chromium كمكتبات مشتركة منفصلة، حتى الوقت الذي يقضيه في خطوة الربط النهائية يصبح غير ملحوظ.

## شحن ثنائيات أولية

مع استمرار نمو الكروميوم، كان هناك العديد من الرموز المصدرة في Chromium، حتى رموز وحدة المحتوى و Webkit كانت أكثر من حد . كان من المستحيل إنشاء مكتبة مشتركة قابلة للاستخدام بمجرد رموز التجريد من الرموز.

في النهاية، كان علينا [شحن ثنائيات الكروم الخام](https://github.com/electron/libchromiumcontent/pull/98) بدلاً من إنشاء مكتبة مشتركة واحدة.

وكما تم تقديمه سابقا، هناك نوعان من طرق البناء في Chromium. نتيجة شحن ثنائيات خام، علينا شحن توزيعين مختلفين من ثنائي النوع في ليبكروم محتوى. إحداها تسمى بناء `Static_libr` ، الذي يتضمن جميع المكتبات الثابتة لكل وحدة تم إنشاؤها عن طريق البناء العادي من Chromium. والآخر هو `شارك_مكتبة`، الذي يتضمن جميع المكتبات المشتركة لكل وحدة تم إنشاؤها من خلال بناء المكونات.

في Electron، يتم ربط إصدار التصحيح مع إصدار `شارك_libr` من libchromiumcontent، لأنه صغير التنزيل ولا يستغرق إلا القليل من الوقت عند ربط إمكانية التنفيذ النهائي. و إصدار الإصدار من إلكترون مرتبط بنسخة `Static_libr` من ليبكروم المحتوى، حتى يتمكن المترجم من إنشاء رموز كاملة مهمة لتصحيح الأخطاء، والربط يمكن أن يحقق تحسينا أفضل بكثير لأنه يعرف أي ملفات الكائنات مطلوبة وأيها غير مطلوبة.

لذلك من أجل التطوير الطبيعي، يحتاج المطورين فقط إلى بناء إصدار التصحيح، الذي لا يتطلب شبكة جيدة أو آلة قوية. على الرغم من أن الإصدار الإصدار يتطلب الكثير من الأجهزة الأفضل لبنائه، فإنه يمكن أن يولد أحسن ثنائيات محسنة.

## تحديث `gn`

كونها أحد أكبر المشاريع في العالم، معظم الأنظمة العادية ليست مناسبة لبناء كروميوم، ويقوم فريق Chromium بتطوير أدوات بناء الخاصة به.

الإصدارات السابقة من Chromium كانت تستخدم `gyp` كنظام للبناء، لكنها تعاني من البطء، ويصبح من الصعب فهم ملف التكوين الخاص به لمشاريع معقدة بعد سنوات من التطوير، تحول Chromium إلى `gn` كنظام بناء وهو أسرع بكثير ولديه هندسة معمارية واضحة.

أحد تحسينات `gn` هو تقديم `source_set`، الذي يمثل مجموعة من ملفات الكائنات. في `gyp`، تم تمثيل كل وحدة إما `ثابت_مكتبة` أو `شارك_مكتبة`، ومن أجل البناء العادي لـ Chromium، قامت كل وحدة بتوليد مكتبة ثابتة وتم ربطها معا في القابل للتنفيذ النهائي. باستخدام `gn`، تقوم كل وحدة الآن بإنشاء مجموعة من ملفات الكائنات، والجهاز التنفيذي النهائي يربط فقط جميع ملفات الكائنات معا، حتى لم تعد ملفات المكتبة الثابتة الوسيطة قد تم إنشاؤها.

غير أن هذا التحسين تسبب في مشكلة كبيرة في ليبكروم المحتوى، لأن ملفات المكتبة الساكنة الوسيطة كانت في الواقع بحاجة إليها من قبل ليبكروم محتوى.

أول محاولة لحل هذا هو [تصحيح `gn` لتوليد ملفات المكتبة الثابتة ](https://github.com/electron/libchromiumcontent/pull/239)، وهو حل للمشكلة، ولكنه بعيد كل البعد عن إيجاد حل لائق .

تمت المحاولة الثانية بواسطة [@alespergl](https://github.com/alespergl) إلى [إنتاج مكتبات ثابتة مخصصة من قائمة ملفات الكائنات](https://github.com/electron/libchromiumcontent/pull/249). لقد استخدم خدعة لتشغيل بناء سماوي أولا لجمع قائمة من ملفات الكائنات التي تم إنشاؤها ، ومن ثم قم في الواقع ببناء المكتبات الثابتة عن طريق تغذية `gn` مع القائمة. It only made minimal changes to Chromium's source code, and kept Electron's building architecture still.

## Summary

كما ترون، مقارنة ببناء إلكترون كجزء من Chromium، بناء Chromium كمكتبة يتطلب جهوداً أكبر ويتطلب صيانة مستمرة ومع ذلك، يزيل الأخير متطلبات الأجهزة القوية لبناء إلكترون، وبالتالي تمكين مجموعة أكبر بكثير من المطورين من البناء و المساهمة في إلكترون. وهذا الجهد يستحق منا كل الاهتمام.

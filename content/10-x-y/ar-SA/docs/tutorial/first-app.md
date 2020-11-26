# كتابة أول تطبيق إلكترون

يمكنك الكترون من إنشاء تطبيقات لأجهزة الحاسب المكتبي باستخدام الجافا سكريبت (JavaScript) بشكل خالص عن طريق تزويد المطور ببيئة تشغيلية غنية بمكونات أصلية (API) لكل نظام تشغيل. تستطيع اعتبار إلكترون كنكهة مختلفة من نود. جي اس (Node.js) تركز على انشاء تطبيقات سطح المكتب بدلاً من سيرفرات الويب. سيرفرات الويب هي من اختصاص نود. جي اس، بينما تطبيقات سطح المكتب هي من اختصاص إلكترون.

هذا لا يعني أن Electron هو ربط جافا سكريبت لمكتبات واجهة المستخدم الرسومية (GUI). بدلاً من ذلك ، يستخدم Electron صفحات الويب كجهاز GUI ، لذا يمكنك أيضًا رؤيتها كمتصفح صغير على Chromium ، يتم التحكم فيه بواسطة جافا سكريبت.

**ملاحظة**: هذا المثال متاح أيضًا كمستودع يمكنك</a> تنزيله وتشغيله على الفور .

</p> 

وفيما يتعلق بالتطوير ، فإن تطبيق الإلكترون هو في الأساس تطبيق Node.js. نقطة البداية `package.json ` هي مماثلة لتلك الخاصة بوحدة Node.js. سيكون تطبيق الإلكترون على بنية المجلد التالية:



```plaintext
تطبيقك
<unk> <unk> <unk> <unk> <unk> <unk> <unk> package.json
<unk> <unk> <unk> ', main.js
<unk> <unk> <unk> ', index.html
```


أنشئ مجلدًا فارغًا جديدًا لتطبيقك الإلكتروني الجديد. و إفتح موجه الأوامر الخاص بك وقم بتشغيله و أكتب فيه ` npm init ` من هذا المجلد.



```sh
npm init
```


سوف يرشدك npm من خلال إنشاء `package.json` في الملف الأساسي . النص البرمجي المحدد بواسطة `main` الملف الأساسي لتشغيل تطبيقك ، والذي سيشغل العملية الرئيسية. مثال على `package.json` قد يبدو مثل هذا:



```json
{
  "name": "your-app",
  "الإصدار": "0.1.0",
  "main": "main.js"
}
```


__ملاحظة__ : إذا كان `main` الحقل غير موجود في `package.json` ، سيحاول الإلكترون تحميل `index.js` (كما يفعل Node.js) . إذا كان هذا في الواقع تطبيقًا بسيطًا للعقدة ، فيمكنك إضافة برنامج `start` نصي يوجه `node` لتنفيذ الحزمة الحالية:



```json
{
  "name": "your-app",
  "version": "0.1.0",
  "main": "main.js",
  "scripts": {
    "start": "node ."
  }
}
```


تحويل تطبيق العقدة هذا إلى تطبيق إلكتروني بسيط للغاية - نحن فقط نستبدل  `node` بــ  `electron` 



```json
{
  "name": "your-app",
  "version": "0.1.0",
  "main": "main.js",
  "scripts": {
    "start": "electron ."
  }
}
```




## تنصيب إلكترون (Electron)

في هذه المرحلة ، ستحتاج إلى تثبيت `electron` نفسه. والطريقة الموصى بها للقيام بذلك هي تثبيته كإعتماد على التطوير في تطبيقك ، مما يسمح لك بالعمل على تطبيقات متعددة بنسخ إلكترون مختلفة. للقيام بذلك ، قم بتشغيل الأمر التالي من دليل التطبيق الخاص بك:



```sh
npm install --save-dev electron
```


توجد وسائل أخرى لتثبيت الكترون. يرجى الرجوع إلى </a>دليل التثبيت للتعرف على الاستخدام مع البروكسي والتخزين المؤقت المخصص.</p> 



## اسلوب تطوير إلكترون باختصار

يتم تطوير تطبيقات الإلكترون في جافا سكريبت باستخدام نفس المبادئ والأساليب الموجودة في تطوير Node.js. يمكن الوصول إلى جميع واجهات برمجة التطبيقات والميزات الموجودة في Electron من خلال `electron` الوحدة النمطية ، والتي يمكن أن تكون مطلوبة مثل أي وحدة Node.js أخرى:



```javascript
إلكترون = مطلوبة ('electron')
```


تعرض الوحدة النمطية ` electron </ 0> ميزات في مساحات الأسماء. كأمثلة ، دورة الحياة
يتم إدارة التطبيق من خلال <code> electron.app </ 0> ، يمكن إنشاء النوافذ
باستخدام فئة <code> electron.BrowserWindow </ 0>. قد ينتظر ملف <code> main.js </ 0> بسيط
لكي يكون التطبيق جاهزًا وفتح نافذة:</p>

<pre><code class="javascript">const { app, BrowserWindow } = require('electron')
  
  function createWindow () {
    // إنشاء نافذة طولها 800 وعرضها 600.
  اسمح للفوز = متصفح ويندوز جديد ({
    عرض: 800,
    طول: 600,
    تفضيلات الويب: {
      nodeIntegration: true
    }
  })

  // وتحميل الفهرس .html من التطبيق.
  win.loadFile('index.html')
}

app.whenReady().then(createWindow)
`</pre> 

يجب أن تقوم ` main.js </ 0> بإنشاء النوافذ والتعامل مع جميع أحداث النظام الخاصة بك
قد يواجه التطبيق. نسخة أكثر اكتمالا من المثال أعلاه
قد يفتح أدوات مطوري البرامج أو يعالج النافذة المغلقة أو يعاد إنشاءها
النوافذ على نظام MacOS إذا نقر المستخدم على رمز التطبيق في قفص الاتهام.</p>

<pre><code class="javascript">const { app, BrowserWindow } = require('electron')
  
  function createWindow () {
    // إنشاء نافذة طولها 800 وعرضها 600.
  فوز = متصفح ويندوز جديد ({
    عرض: 800,
    ارتفاع 600,
    تفضيلات الويب: {
      nodeIntegration: true
    }
  })

  // وتحميل index.html من التطبيق.
  win.loadFile('index.html')

   // افتح DevTools.
  win.webContents.openDevTools()
}

// سيتم استدعاء هذه الطريقة عند انتهاء إلكترون
// تهيئة وهي مستعدة لإنشاء نوافذ المتصفح.
// لا يمكن استخدام بعض APIs إلا بعد حدوث هذا الحدث.
app.whenReady().then(createWindow)

// إنهاء عند إغلاق جميع النوافذ.
app.on('window-all-closed', () => {
// على macOS من الشائع للتطبيقات وشريط القوائم
   // للبقاء نشطًا حتى يتم إنهاء المستخدم بشكل صريح باستخدام Cmd + Q
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  //على macOS من الشائع إعادة إنشاء نافذة في التطبيق عندما
  //يتم النقر فوق رمز قفص الاتهام وليس هناك نوافذ أخرى مفتوحة.
  إذا (BrowserWindow.getAllWindows). ength === 0) {
    createWindow()

})

// في هذا الملف يمكنك تضمين بقية العملية الرئيسية الخاصة بتطبيقك
// code. يمكنك أيضًا وضعها في ملفات منفصلة وطلبها هنا.
`</pre> 

وأخيرًا ، فإن ` index.html </ 0> هي صفحة الويب التي تريد إظهارها:</p>

<pre><code class="html"><!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
    <!-- https://electronjs.org/docs/tutorial/security#csp-meta-tag -->
    <meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-inline';" />
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using node <script>document.write(process.versions.node)</script>,
    Chrome <script>document.write(process.versions.chrome)</script>,
    and Electron <script>document.write(process.versions.electron)</script>.
  </body>
</html>
`</pre> 



## تشغيل تطبيقك الأول

بمجرد إنشائك الأولية ` main.js </ 0> ، <code> index.html </ 0> ، و <code> package.json </ 0>
الملفات ، يمكنك تجربة التطبيق الخاص بك عن طريق تشغيل <code> npm start </ 0> من التطبيق الخاص بك
دليل.</p>

<h2 spaces-before="0">جرب هذا المثال</h2>

<p spaces-before="0">استنساخ وتشغيل الكود في هذا البرنامج التعليمي باستخدام <a href="https://github.com/electron/electron-quick-start" f-id="quick-start" fo="2"><code>electron/electron-quick-start`</a> repository.

**Note**: Running this requires [Git](https://git-scm.com) and [npm](https://www.npmjs.com/).



```sh
# استنساخ المستودع
$ git نسخة https://github. om/electron/electron-quick-start
# انتقل إلى المستودع
$ cd electron-start
# تثبيت الإعتمادات
$ npm مثبت
# تشغيل التطبيق
$ npm
```


للحصول على قائمة بألواح الصفيح والأدوات لبدء عملية التطوير ، راجع [Boilerplates and CLIs documentation][boilerplates].

[boilerplates]: ./boilerplates-and-clis.md
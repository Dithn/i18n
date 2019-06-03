# شئ JumpListCategory

* `type` رشته (اختیاری) - یکی از موارد زیر: 
  * `وظایف` - آیتم های درون این دسته بندی، در دسته بندیِ استانداردِ `وظایف` قرار خواهند گرفت. تنها یک دسته بندی این چنینی می تواند وجود داشته باشد، و همیشه در پایین لیست پرشی نمایش داده خواهد شد.
  * `غالب` - یک لیست از اپ هایی که اغلب باز شده اند نمایش می دهد، نام دسته بندی و آیتم های آن با ویندوز تنظیم می شوند.
  * `اخیر` - یک لیست از اپ هایی که اخیرا باز شده اند نشان می دهد، نام دسته بندی و آیتم های آن با ویندوز تنظیم می شوند. آیتم ها می توانند به صورت غیر مستقیم و با استفاده از `app.addRecentDocument(path)` به دسته بندی اضافه شوند.
  * `مشخص شده` - لینک فایل و وظایف را نمایش می دهد، `نام` باید توسط اپ تنظیم شده باشد.
* `نام` رشته (اختیاری) - تنها در صورتی که `نوع` `مشخص شده` است باید تنظیم شود، در غیر این صورت نادیده گرفته شود.
* `آیتم ها` JumpListItem[] (اختیاری) - آرایه ای از اشیای [`آیتم های لیست پرشی`](jump-list-item.md) تنها در صورتی که `نوع``وظایف`یا`مشخص شده` است، در غیر این صورت باید نادیده گرفته شود.

**نکته:** اگر شی یک `دسته بندیِ لیست پرشی` نه `نوع` و نه `نام` مشخص شده داشته باشد، `نوع` آن، `وظایف` فرض خواهد شد. اگر خاصیتِ `نام` تنظیم شده باشد ولی `نوع` خاصیت نه، `نوع` به صورت `مشخص شده` فرض خواهد شد.
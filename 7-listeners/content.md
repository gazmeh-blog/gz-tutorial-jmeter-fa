## ماهیت شنودگرها یا Listenerها
پس از ضبط و طراحی سناریوهای آزمون‌های عملکردی یا کارایی در جی‌میتر و اعمال پیکربندی‌های مورد نیاز، با اجرای تست، جی‌میتر درخواست‌ها را به سمت سرور ارسال می‌کند. سپس باید نتیجه اجرای هر دستور مورد بررسی قرار گیرد که آیا درخواست‌ها با موفقیت اجرا شده‌اند یا خیر؟ همچنین در تست کارایی، باید بعضی فاکتورها ذخیره، تجزیه و تحلیل شوند تا بتوان از مشخصه‌های عملکردی سیستم مطلع شد. به عنوان مثال فاکتورهایی مانند:
* **Throughput:** که در فارسی به آن «توان عملیاتی» می‌گوییم که برابر با تعداد درخواست‌های پردازش‌شده در واحد زمان است.
* **Response Time:** در فارسی به آن «زمان‌پاسخ» می‌گوییم و تعیین کننده زمان لازم برای پردازش درخواست ارسالی توسط سرور است. به عبارتی دیگر هنگامی که درخواستی به سمت سرور ارسال می‌شود، مدت زمانی که طول می‌کشد تا پاسخ از سرور دریافت شود را زمان پاسخ در نظر می‌گیریم که یکی از معیارهای مهم در تست کارایی است. 

علاوه بر دو فاکتور بالا، موارد مفید دیگری نیز وجود دارند که محاسبه‌ آن‌ها ممکن است کاری دشوار و زمان‌بر باشد. بدین منظور مولفه‌ای به نام Listener در جی‌میتر طراحی شده است تا بتوان با استفاده از انواع مختلف آن، نتایج تست را از جنبه‌های مختلف، محاسبه و بررسی کرد. در واقع با استفاده از Listenerها می‌توان پاسخ‌ها و نتایج تست را از نمونه‌گیر یا همان Sampler دریافت کرده و اطلاعات مورد نیاز را نمایش داد، که در ادامه به آموزش آن می‌پردازیم.

## انواع Listener در جی‌میتر
در جی‌میتر Listener‌های مختلفی وجود دارند که هر کدام برای هدف خاصی طراحی شده‌اند و کاربر براساس نیاز خود می‌تواند از هر کدام از آنها استفاده کند. 

طبق شکل زیر، Listener نتایج را در قالب گراف، درخت، لاگ فایل و جدول ذخیره کرده و نمایش می‌دهد:

![Types of Listener](./resources/types-of-listeners.png?raw=true "Types of Listener")

Listener‌های متنوعی در پکیج جی‌میتر وجود دارند و همراه جی‌میتر نصب شده‌اند. برخی از پرکاربردترین آن‌ها عبارتند از:
1. **View Results Tree:** نمایش درختی درخواست‌های ارسال شده و پاسخ‌های دریافتی
2. **Summary Report:** نمایش مواردی مانند حداقل زمان دریافت پاسخ و … در قالب جدول
3. **Aggregate Report:** این listener مانند summary report خلاصه نتایج تست را نمایش می‌دهد. در این نوع گزارش نتایج تجمیع شده به تفکیک تراکنش‌های مختلف ارائه می‌شود.
4. **Aggregate Graph:** اطلاعاتی مانند Aggregate Report را به صورت آماری و گرافیکی نمایش می‌دهد.
5. **Assertion Results:** نمایش نتیجه اجرای Assertion‌ها
6. **Generate Summary Results:** ذخیره خلاصه ای از اجرای تست در یک فایل لاگ
7. **Response Time Graph:** ارائه یک گراف از زمان دریافت پاسخ
8. **Save Responses to a file:** ذخیره نتایج تست در یک یا چند فایل
9. **View Results in Table:** نمایش بدنه و header پاسخ دریافتی در قالب جدول

> در جی‌میتر listenerهای دیگری مانند JSR223 Listener و BeanShell Listener  وجود دارند که می‌توان با اسکریپت نویسی نتایج استخراج شده را پردازش کرد و بر اساس نیاز خود یک listener شخصی شده ساخت.

## نحوه استفاده از listener‌ها در جی‌میتر
می‌توان listener‌ها را به کامپوننت‌های مختلفی اضافه کرد. در نتیجه فقط نتایج بخش‌هایی که در حوزه آن‌ها قرار دارند را نمایش می‌دهند. برای فهم بهتر این موضوع مثال زیر در جی‌میتر را در نظر بگیرید.

![Listeners Scope](./resources/listeners-scope.png?raw=true "Listeners Scope")

در شکل بالا، سه listener در حوزه‌های مختلف تعریف شدند؛ بنابراین هر کدام نتایج و داده‌های مختلفی نمایش می‌دهند:
* listener 1: نمایش نتایج مربوط به درخواست GET /ads
* listener 2: نمایش نتایج مربوط به ترد گروپ Get Ads
* listener 3: نمایش نتایج مربوط به Test Plan. یعنی این listener نتایج اجرای همه ی عناصری که در این تست پلن قرار دارند را نمایش می‌دهد.

بنابراین listener می‌تواند بر اساس جایگاه خود موارد مختلفی را جمع آوری کرده و نشان دهد. به عبارتی listener‌ها نتایج را از عناصر هم سطح خود و سطوح داخلی جمع آوری کرده و نمایش می‌دهند. به همین خاطر، توصیه می‌شود که listener در سطح تست پلن تعریف شود تا نتیجه همه thread group‌ها و عناصر داخلی آن‌ها را بتوان جمع آوری و ذخیره کرد.

### عناصر والد یک listener
listener‌ها را می‌توان به عناصر زیر اضافه کرد:
* Test Plan
* Thread Group
* Test Fragment
* Sampler
* Logic Controller
* Non-Test Element

> در نظر داشته باشید که هیچ عنصری را نمی‌توان به listener اضافه کرد، بنابراین فرزندی نخواهد داشت.

برای اضافه کردن یک listener در جی‌میتر، با توجه به نیاز خود روی Test Plan، Thread Group، Samplers و یا … کلیک راست کنید و از منوی باز شده گزینه listener را انتخاب کرده و در نهایت listener‌های مورد نیاز خود را اضافه کنید. 

![Add Listeners](./resources/add-listeners.png?raw=true "Add Listeners")

## listener‌های کاربردی و مهم جی‌میتر
در این بخش با ذکر یک مثال به بررسی انواع listenerهای مختلف و کاربرد آنها در جی‌میتر می‌پردازیم. در این مثال سعی داریم درخواست‌های زیر را اجرا کرده و نتایج را مشاهده کنیم:
* دریافت لیست آگهی‌های مربوط به آپارتمان در سایت دیوار 

![Divar Real Estate](./resources/divar-real-estate.png?raw=true "Divar Real Estate")

* دریافت اطلاعات کامل مربوط به یک آگهی

![Divar House Detail](./resources/divar-house-detail.png?raw=true "Divar House Detail")

1. **View Results Tree**

View Results Tree یکی از مهم‌ترین و پرکاربردترین listener‌هایی است که در جی‌میتر استفاده می‌شود. این listener درخواست‌ها و پاسخ‌های آنها را به صورت درخت نمایش می‌دهد. در واقع View Results Tree درخواست ارسالی به سمت سرور، پاسخ دریافتی از سرور، header‌ها و به طور کلی یک ارزیابی از API را نمایش می‌دهد. 

با استفاده از قسمت drop down می‌توان پاسخ API را در قالب فرمت‌های زیر مشاهده کرد: 

![View Results Tree Formats](./resources/types-of-view-result-tree.png?raw=true "View Results Tree Formats")

* **Text:** نمایش به صورت متن خام و بدون فرمت
* **Boundary Extractor Tester:** با استفاده از Boundary Extractor می‌توان بخشی از پاسخ دریافتی را استخراج و به عنوان متغیر ذخیره کرد و در درخواست های بعدی و یا برای اعتبارسنجی پاسخ از آن استفاده کرد. با استفاده از این فرمت و با مشخص کردن Left Boundary و Right Boundary می‌توان نحوه استخراج کردن بخش مورد نظر از پاسخ دریافتی را تست کرد. 
* **HTML:** نمایش پاسخ با فرمت HTML
* **JSON:** نمایش به صورت فرمت json

در View Results Tree، اگر درخواست به درستی ارسال نشود و یا پاسخ دریافتی با پاسخ مورد انتظار ما متفاوت باشد با رنگ قرمز و در غیر این صورت با رنگ سبز مشخص می‌شود:

![View Results Tree Error](./resources/view-results-tree-error.png?raw=true "View Results Tree Error")

در تصویر بالا، درخواست اول به درستی اجرا شده (رنگ سبز) و برای درخواست دوم خطای " 404 Not Found" دریافت شده است که با رنگ قرمز نمایش داده شده است.

همچنین این listener شامل تب‌های زیر است:
* **تب Sampler result:** شامل کد پاسخ (response code)، پیام پاسخ (response message) و اطلاعاتی در مورد زمان ارسال درخواست، تاخیر، سایز پاسخ به بایت و … است.

![View Results Tree Sampler Tab](./resources/view-results-tree-sampler-tab.png?raw=true "View Results Tree Sampler Tab")

* **تب Request:** شامل جزئیات درخواستی است که جی‌میتر به سمت سرور ارسال می‌کند. در واقع اطلاعاتی مانند URL، header، بدنه درخواست و کوکی‌هاست.

![View Results Tree Request Tab](./resources/view-results-tree-request-tab.png?raw=true "View Results Tree Request Tab")

* **تب Response:** شامل اطلاعات دریافتی از سرور است، مانند بدنه پاسخ و header

![View Results Tree Response Tab](./resources/view-results-tree-response-tab.png?raw=true "View Results Tree Response Tab")

می‌توانید نتیجه تست شامل مواردی مانند URL، Response Code، Response Message و … را در یک فایل ذخیره کنید. با کلیک روی Browse… فایلی را که می‌خواهید نتایج تست روی آن ذخیره شوند را انتخاب کنید. سپس مشخص کنید که می‌خواهید خطاها و یا نتایج درخواست‌های با اجرای موفق را ذخیره کنید. همچنین با استفاده از دکمه Configure می‌توانید موارد بیشتری را انتخاب کنید که در فایل مورد نظر ذخیره شوند:

![View Results Tree Save Results](./resources/view-results-tree-save-results.png?raw=true "View Results Tree Save Results") 

**مزیت:**

مزیت این listener این است که می‌توانیم با استفاده از آن همزمان جزئیات درخواست ارسال شده و پاسخ دریافت شده را مشاهده کنیم که این باعث می‌شود هر گاه تستی به درستی اجرا نشد، علت آن را پیدا کنیم.

**عیب:**

این listener برای تست‌های load/stress که چندین کاربر همزمان سناریوها را اجرا می‌کنند مناسب نیست. در واقع این listener حافظه و CPU بسیاری را مصرف می‌کند زیرا همه نتایج را در حافظه اصلی نگه می‌دارد. 

> بنابراین این listener برای دیباگ کردن مناسب است، که با استفاده از آن می‌توان مطمئن شد که درخواست درستی به سمت سرور ارسال شده و پاسخ درستی نیز از سمت سرور دریافت شده است. بنابراین در اجرای نهایی باید این listener را حذف یا غیرفعال کرد.

2. **Save Response to a file**

با استفاده از save response to a file می‌توان به راحتی نتایج تست را در یک یا چندین فایل ذخیره کرد. طبق شکل زیر، می‌توانید تنظیمات مربوطه را انجام دهید تا فقط نتایج مورد انتظار شما در فایل ذخیره شود:
* **Name:** یک نام برای listener انتخاب کنید.
* **Filename prefix (can include folders):** پیشوند نام فایل‌های ایجاد شده
* **save conditions:** می‌توانید با استفاده از گزینه‌های زیر مشخص کنید که کدام نتایج (پاسخ‌های با اجرای موفق/ناموفق) ذخیره شوند:
  * Save Successful Responses only
  * Save Failed Responses only
  * Don’t save Transaction Controller SampleResult
* **Variable Name containing saved file name:** نام متغیری که در آن نام فایل ایجاد شده ذخیره می‌شود.
* **Don’t add number to prefix:** اگر این مورد را انتخاب کنید، هیچ عددی به پیشوند اسم فایل ایجاد شده اضافه نمی‌شود. یعنی یک فایل ایجاد می‌شود و اگر چندین درخواست داشته باشید، نتایج هر درخواست روی نتایج درخواست قبلی ذخیره می‌شود.
* **Don’t add content type suffix:** در صورت انتخاب این گزینه، پسوندی مبنی بر نمایش نوع محتوای فایل به نام فایل ذخیره شده اضافه نمی‌شود.
* **Add timestamp:** با انتخاب این مورد، تاریخ با فرمت yyyyMMdd-HHmm به اسم فایل اضافه می‌شود.

برای درک بهتر به شکل‌های زیر توجه کنید:

![Save Response To a File](./resources/save-response-to-file.png?raw=true "Save Response To a File")

اگر در قسمت Filename prefix مسیر ذخیره فایل را مشخص نکنید و تنها پیشوند نام فایل‌ها را وارد کنید، فایل‌ها در دایرکتوری bin جی‌میتر ذخیره خواهند شد. در این جا مسیر ذخیره فایل‌ها را مشخص کردیم و طبق شکل زیر در همین مسیر ذخیره شدند:

![Save Response To a File (Results)](./resources/save-response-to-file-result.png?raw=true "Save Response To a File (Results)")

همانطور که پیش‌تر گفتیم، در مثال این آموزش 2 API داریم؛ بنابراین پس از اجرا، طبق تنظیماتی که در بالا انجام دادیم، برای هر درخواست یک فایل مجزا ایجاد شده که حاوی نتیجه اجرای این درخواست‌هاست.

همچنین در تنظیمات این listener گزینه‌های Don’t add number to prefix و Don’t add content type suffix را تیک نزده ایم. بنابراین همانطور که در شکل بالا مشخص است، در ادامه پیشوند اسم فایل، عدد و نوع محتوای فایل را هم داریم.

اگر گزینه Add timestamp را انتخاب کرده باشید، تاریخ لحظه اجرای درخواست‌ها را هم در ادامه پیشوند اسم فایل‌ها خواهید داشت:

![Save Response To a File (TimeStamp)](./resources/save-response-to-file-result-timestamp.png?raw=true "Save Response To a File (TimeStamp)") 

سپس می‌توانید این فایل‌ها را با استفاده از notepad++، مرورگر و … باز کنید و محتوای آن‌ها را مشاهده و استفاده کنید:

![Show Response in firefox](./resources/response-in-firefox.png?raw=true "Show Response in firefox")  


3. **Summary Report**

Summary Report نتایج مورد نیاز برای تجزیه و تحلیل تست را در قالب جدول نمایش می‌دهد. اطلاعاتی مانند:

![Summary Repoer](./resources/summary-report.png?raw=true "Summary Repoer")

* **Label:** اسم درخواست یا تراکنش را نمایش می‌دهد.
* **#Samples:** تعداد sample‌ها تعداد تراکنش‌ها یا درخواست‌های اجرا شده است. در شکل بالا درخواست های Get House List  و Get House Detail هر کدام 7 بار اجرا شده‌اند.
* **Average:** میانگین زمان پاسخ (Response Time) مصرف شده توسط همه sample‌ها برای اجرای label مورد نظر است. به عنوان مثال در عکس بالا، average time برای تراکنش Get House List برابر با 3373 میلی‌ثانیه است.
* **Min:** حداقل زمانی است که برای اجرای یک درخواست مصرف می‌شود. در شکل بالا برای تراکنش Get House List از 7 تا درخواستی که اجرا شده، یکی از درخواست‌ها در کمترین زمان یعنی 2865 میلی ثانیه اجرا شده است.
* **Max:** حداکثر زمانی که برای اجرای یک درخواست مصرف می‌شود. در شکل بالا برای تراکنش Get House List از 7 تا درخواستی که اجرا شده، یکی از درخواست‌ها در بیشترین زمان یعنی 3618 میلی ثانیه اجرا شده است.
* **Std. Dev.:** میزان انحراف استاندارد از متوسط زمان پاسخ. 
* **Error %:** درصد درخواست‌های با اجرای ناموفق 
* **Throughput:** توان عملیاتی تعداد درخواست‌هایی است که در واحد زمان (ثانیه) توسط سرور پردازش می‌شود. این زمان از شروع اولین sample تا پایان اجرای آخرین sample محاسبه می‌شود. هر چه این مقدار بیشتر باشد بیانگر آن است که سیستم کارایی بهتری دارد.
* **Recieved KB/Sec:** نمایش دهنده میزان داده‌های دانلود شده در طول تست است.
* **Sent KB/Sec:** نمایش دهنده میزان داده‌های ارسال شده در طول تست است.
در این listener هم مانند View Results Tree می‌توان نتایج را در یک فایل ذخیره کرد.


4. **Aggregate Report**

Aggregate Report همانند Summary Report نتایج را در قالب جدول نمایش می‌دهد. با این تفاوت که حافظه مصرفی این Listener از Summary Report بیشتر است، زیرا یک کپی از هر نمونه را در حافظه ذخیره می‌کند. بنابراین توصیه می‌شود برای تست کارایی از Aggregate Report استفاده نشود.

همانطور که در شکل زیر مشاهده می‌کنید، aggregate report اطلاعات زیر را نمایش می‌دهد:

![Aggregate Repoer](./resources/aggregate-report.png?raw=true "Aggregate Repoer")

* **Label:** اسم درخواست یا تراکنش را نمایش می‌دهد.
* **#Samples:** تعداد sample‌ها تعداد تراکنش‌ها یا درخواست‌های اجرا شده است. در این‌جا درخواست های Get House List  و Get House Detail هر کدام 7 بار اجرا شده‌اند.
* **Average:** میانگین زمان صرف شده توسط همه sample‌ها برای اجرای label مورد نظر است. به عنوان مثال در عکس بالا، average time برای تراکنش GET House List برابر با 3373 میلی ثانیه است.
* **Min:** حداقل زمانی که برای اجرای یک درخواست صرف می‌شود. در شکل بالا برای تراکنش GET House List از 7 تا درخواستی که اجرا شده، یکی از درخواست‌ها در کمترین زمان یعنی 2865 میلی ثانیه اجرا شده است.
* **Max:** حداکثر زمانی که برای اجرای یک درخواست صرف می‌شود. در شکل بالا برای تراکنش GET House List از 7 تا درخواستی که اجرا شده، یکی از درخواست‌ها در بیشترین زمان یعنی 3618 میلی ثانیه اجرا شده است.
* **Error%:** درصد درخواست‌های با اجرای ناموفق 
Throughput: توان عملیاتی تعداد درخواست‌هایی است که در واحد زمان (ثانیه) توسط سرور پردازش می‌شود. این زمان از شروع اولین sample تا پایان اجرای آخرین sample محاسبه می‌شود. هر چه این زمان بیشتر باشد بهتر است.
* **Recieved KB/Sec:** نمایش دهنده میزان داده‌های دانلود شده در طول تست است.
* **Sent KB/Sec:** نمایش دهنده میزان داده‌های ارسال شده در طول تست است.
* **Median:**  میانه یک معیار آماری استاندارد است. به عبارتی sample‌ها را به دو قسمت مساوی تقسیم می‌کند، نیمی از آن‌ها کوچکتر از میانه و نیمی دیگر بزرگتر از میانه است. بنابراین در اینجا median نشان دهنده این است که 50٪ درخواست‌ها در کمتر یا برابر این زمان اجرا شدند. 
* **90%Line:** ۹۰٪ درخواست‌ها کمتر یا برابر این زمان هستند. (90امین صدک)
* **95%Line:** ۹۵٪ درخواست‌ها کمتر یا برابر این زمان هستند. (95امین صدک)
* **99%Line:** ۹۹٪ درخواست‌ها کمتر یا برابر این زمان هستند. (99امین صدک)

> معمولا از median به عنوان میانه زمان پاسخ و از 99 یا 95 درصد به عنوان بیشینه زمان پاسخ در نتایج تست‌های کارایی استفاده می‌شود.

در این listener هم مانند View Result Tree می‌توان نتایج را در یک فایل ذخیره کرد.

## Plugin‌ها
listener‌های دیگری نیز طراحی شده‌اند که در پکیج اصلی خود جی‌میتر وجود ندارند و می‌توانید آنها را به راحتی به جی‌میتر اضافه و استفاده کنید. 
پلاگین‌هایی مانند:
* **PerfMon Metrics Collector:** برای مانیتور کردن منابعی مانند CPU، حافظه و …
* **Response Codes per Second:** مشخص می‌کند که در طول اجرای تست چه response code هایی دریافت شده است.
* **Response Times Over Time Listener:** نمایش میانگین زمان پاسخ برای هر نمونه در طول تست (میلی ثانیه)

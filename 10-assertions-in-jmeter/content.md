پس از ضبط کردن سناریوی مورد نظر، پارامتربندی، اجرای تست و دریافت پاسخ از سرور، نیاز است تا از درستی پاسخ دریافت شده اطمینان حاصل کنیم. وقتی درخواستی اجرا می‌شود می‌توان با بررسی کد و پاسخ دریافت شده از سرور متوجه شد که درخواست با موفقیت اجرا شده است یا خیر. گاهی اوقات بررسی کد و پیام پاسخ دریافتی کافی نیست. به عنوان مثال، ممکن است با وجود دریافت کد 200 و پیام OK، پاسخ دریافت شده با پاسخ مورد انتظار ما متفاوت باشد و یا گاهی نیاز است که فیلدهای مختلف در بدنه پاسخ و یا موارد دیگری مانند مدت زمان دریافت پاسخ و … را بررسی کنیم تا مطمئن شویم که پاسخ دریافتی مورد انتظار است. جی‌میتر و ابزارهای دیگر به تنهایی نمی‌توانند این گونه خطاها را تشخیص دهند، زیرا در بعضی مواقع تعیین درستی پاسخ به شما بستگی دارد. بدین منظور عنصری به نام Assertion در جی‌میتر طراحی شده است تا با استفاده از آن بتوان درستی پاسخ دریافتی از سرور را بررسی کرد که در این مقاله به آموزش آن می‌پردازیم.

## Assertion در جی‌میتر چیست؟
Assertion یکی از مهمترین بخش‌های جی‌میتر است که کاربران با استفاده از آن‌ها می‌توانند درستی پاسخ دریافتی از سرور را بررسی کنند. به خصوص در تست کارایی بسیار کاربردی هستند، زیرا که با استفاده از آن‌ها می‌توانیم بررسی کنیم که همه درخواست ها به درستی اجرا شده‌اند و تحت تاثیر بار زیاد قرار نگرفته‌اند. در واقع Assertion‌ها پس از اجرای درخواست و دریافت پاسخ از سرور، اجرا می‌شوند، سپس نتایج واقعی را با نتایج مورد انتظار ما مقایسه کرده و در نهایت مشخص می‌کنند که این مقادیر با هم برابر هستند و یا خیر.

## انواع Assertion
در جی‌میتر Assertionهای مختلفی وجود دارند که هر کدام برای هدف خاصی طراحی شده‌اند و کاربر براساس نیاز خود می‌تواند از هر کدام از آن‌ها استفاده کند.برخی از Assertion هایی که در پکیج جی‌میتر وجود دارند، عبارت اند از:
1. **Response Assertion:** بررسی مقادیری مانند کد و پیام پاسخ دریافتی
2. **JSON Assertion:** ارزیابی پاسخ دریافتی از سرور که به صورت JSON است.
3. **JSR223 Assertion:** بررسی پاسخ دریافتی با استفاده از اسکریپت نویسی با زبان های java، groovy  و …
4. **XPath Assertion:** ارزیابی پاسخ دریافتی XML با استفاده از عبارت های XPath


## نحوه استفاده از Assertionها در جی‌میتر
پیش از استفاده از Assertion به موارد زیر توجه کنید:
1. **هزینه Assertion**
   
   هر Assertionای هزینه متفاوتی دارد، به عبارتی میزان مصرف منابعی مانند حافظه، CPU و … متفاوت است. بعضی از Assertionها مانند Response Assertion، Duration Assertion و … منابع کمتر و Assertionهایی مانند Compare Assertion، XPath Assertion و … منابع بیشتری مصرف می‌کنند.

2. **حوزه تعریفی Assertion**

   می‌توان Assertion ها را به عنصرهای مختلف اضافه کرد. در نتیجه فقط بر روی بخش‌هایی که در scope آن‌ها قرار دارند اعمال می‌شوند.
   برای درک بهتر به مثال‌های زیر توجه کنید:
   
![Assertion Scope](./resources/assertion-scope.png?raw=true "Assertion Scope")
   
   در شکل بالا، چهار Assertion در حوزه‌های مختلف تعریف شدند؛ بنابراین هر کدام برای درخواست‌های متفاوتی اعمال می‌شوند:
   * **Assertion 1:** به GET /categories Sampler اضافه شده است، بنابراین تنها بر روی درخواست GET /categories اعمال میشود.
   * **Assertion 2:** به GET /ads Sampler اضافه شده است، بنابراین تنها بر روی درخواست GET /ads اعمال میشود.
   * **Assertion 3:** به Get Categories اضافه شده است، بنابراین بر روی تمام Sampler هایی که در حوزه آن قرار دارند اعمال می‌شود. (یعنی بر روی GET /categories و GET /ads)
   * **Assertion 4:** به Create a new Ad اضافه شده، بنابراین بر روی تمام Sampler هایی که در حوزه آن قرار دارند اعمال می‌شود. (یعنی بر روی GET /ads ،GET /categories و POST /ongoingposts)

بنابراین Assertion می‌تواند بر اساس جایگاه خود بر روی درخواست‌های مختلفی اعمال شود. به عنوان مثال در اینجا Fail شدن 3 Assertion باعث می‌شود کل transaction controllerای که Assertion در آن تعریف شده است fail شده و آنالیز تست و پیدا کردن مشکل زمانبرتر می‌شود.، بنابراین موقع اضافه کردن Assertion توجه کنید که به کدام کامپوننت اضافه می‌کنید. توصیه می‌شود که برای هر درخواست Assertionهای جداگانه تعریف کنید، زیرا که هم قابل فهم‌تر می‌شود و هم در آینده راحت‌تر می‌توانید طراحی خود را ویرایش کنید.

**عناصر والد یک Assertion**
* Test Plan
* Thread Group
* Test Fragment
* Sampler
* Logic Controller
* Non-Test Element

> در نظر داشته باشید که هیچ عنصری را نمی‌توان به Assertion اضافه کرد، بنابراین فرزندی نخواهد داشت.

3. **استفاده همزمان از چندین Assertion**
   
   می‌توانید برای اطمینان بیشتر از درستی پاسخ دریافتی از چندین Assertion به صورت همزمان استفاده کرده و موارد متفاوتی را همزمان چک کنید.

**چهار گام برای استفاده از Assertionها در طول تست**

چهار گام استفاده از هر کدام از Assertionها به شرح زیر است:

1. **اضافه کردن Assertion مورد نظر**

   براساس نیاز خود یک یا چندین Assertion را به عنصر مورد نظر خود اضافه کنید. بر روی عنصری که می‌خواهید Assertion را به آن اضافه کنید، کلیک راست کرده و از منوی باز شده گزینه Assertions را انتخاب کرده و Assertion مورد نظر خود را اضافه کنید:
   
   ![Add Assertion](./resources/add-assertion.png?raw=true "Add Assertion")
   
2. **اضافه کردن فیلد و الگویی که باید تست شود**

   مشخص کنید که قصد دارید کدام فیلد و چه بخشی از پاسخ دریافتی (به عنوان مثال کد پاسخ، پیام پاسخ، زمان پاسخ و … ) را بررسی کنید.
   
3. **اضافه کردن مقدار مورد انتظار**
 
 مقادیر مورد انتظار خود را برای فیلدهایی که در بالا مشخص کردید وارد کنید. به عنوان مثال انتظار دارید که کد پاسخ دریافتی 202 باشد.
 
4. **اضافه کردن Listener مناسب و اجرای تست**

Listener مناسب را اضافه و تست را اجرا کنید. سپس نتایج اجرای Assertionها را با استفاده از Listener مناسب مشاهده کنید.

در حالت GUI، برای مشاهده نتیجه اجرای Assertionها می‌توانید از Listenerهای زیر استفاده کنید:
* View Results Tree Listener
* Assertion Result Listener

در ادامه این آموزش، هر 4 مرحله را برای رایج‌ترین و پرکاربردترین Assertionها به طور کامل بررسی می‌کنیم.


## Assertion های کاربردی و مهم جی‌میتر
در این بخش قصد داریم چند نمونه از مهمترین Assertion ها را با ذکر مثال بررسی کنیم.

### 1. Response Assertion

Response Assertion یکی از رایج‌ترین Assertionهاست که از آن برای ارزیابی بخش‌های مختلف پاسخ دریافتی از سرور مانند بدنه، کد، پیام، header و … استفاده می‌شود.

   ![Response Assertion](./resources/response-assertion.png?raw=true "Response Assertion")

این assertion شامل موارد زیر است که براساس نیاز خود می‌توانیم آن‌ها را تغییر دهیم:
* **Apply to:** مشخص کنید که فقط نمونه اصلی، sub-sampleها و یا … چک شوند.
* **Field to Test:** با استفاده از این بخش می‌توان مشخص کرد که کدام فیلد بررسی شود.
* **Ignore Status:** همان‌طور که می‌دانید، درخواست‌های HTTP در بازه **4 و **5 به عنوان خطا شناخته می‌شوند. گاهی اوقات نیاز است تا negative scenario اجرا کنیم، یعنی اگر کد پاسخ درخواستی در بازه **4 و **5 بود، آن را pass در نظر بگیریم. بنابراین اگر این گزینه را تیک نزنیم و تست را اجرا کنیم با وجود اینکه مقدار مورد انتظار ما برای کد پاسخ در بازه **4 یا **5 بوده است Assertion با شکست مواجه می‌شود. اما اگر این مقدار را تیک بزنیم این Assertion با موفقیت اجرا خواهد شد.
* **Pattern Matching Rules و Patterns to Test:** با استفاده از این دو مورد مشخص می‌کنیم که فیلد مورد نظر بر اساس چه الگویی تست شود.
  
به عنوان مثال درخواست صفحه اول سایت گزمه را در نظر بگیرید:

https://gazmeh.ir/home

قصد داریم با استفاده از این Assertion بررسی کنیم که آیا بخش "خدمات ما" در پاسخ دریافتی وجود دارد یا خیر. بنابراین طبق مراحل زیر پیش می‌رویم:
1. یک Response Assertion به GET /home اضافه می‌کنیم.
2. می‌خواهیم عبارتی را در نمونه اصلی بررسی کنیم. بنابراین در قسمت apply to فیلد Main sample only و فیلد Text Response در بخش Field to Test را انتخاب می‌کنیم.
3. در این مثال انتظار داریم عبارت "خدمات ما" در بدنه اصلی پاسخ دریافتی وجود داشته باشد. بنابراین از قسمت Pattern Matching Rules فیلد Contains را انتخاب می‌کنیم. در بخش Patterns to Test با استفاده از دکمه Add یک ردیف ایجاد کرده و مقدار آن را برابر "خدمات ما" قرار می‌دهیم.
   
   ![Response Assertion Sample](./resources/response-assertion-sample.png?raw=true "Response Assertion Sample")

4. تست را اجرا می‌کنیم و با استفاده از View Results Tree Listener نتیجه اجرای تست را بررسی می‌کنیم. اگر عبارت مورد نظر در متن پاسخ وجود نداشته باشد، این Assertion با شکست مواجه می‌شود و نمونه مورد نظر با رنگ قرمز نمایش داده می‌شود.
   
   ![Response Assertion Failure](./resources/response-assertion-failure.png?raw=true "Response Assertion Failure")

و اگر عبارت مورد نظر در پاسخ وجود داشته باشد Assertion با موفقیت اجرا شده و درخواست با رنگ سبز مشخص می‌شود.

   ![Response Assertion Pass](./resources/response-assertion-pass.png?raw=true "Response Assertion Pass")

برای بررسی موارد دیگر مانند Response Message، Response Code نیز براساس نیاز خود، به همین صورت عمل می‌کنیم.

### 2. JSON Assertion

با استفاده از JSON Assertion می‌توان پاسخ دریافتی که به صورت JSON است را ارزیابی کرد.

   ![JSON Assertion](./resources/json-assrtion.png?raw=true "JSON Assertion")

* **Assert JSON Path exists:** مسیر فیلد مورد نظر که می‌خواهیم مقدار آن را بررسی کنیم.
* **Additionally assert value:** اگر می‌خواهید مقدار فیلد مشخص شده در پاسخ دریافتی از سرور را با مقداری که خود تعیین می‌کنید مقایسه کنید، این مورد را تیک بزنید.
* **Expected Value:** مقدار و یا عبارت مورد انتظاری که قصد دارید با فیلد مشخص شده در پاسخ مقایسه کنید را در این قسمت وارد کنید.
* **Expect null:** اگر توقع دارید فیلد مشخص شده در پاسخ null باشد، این گزینه را تیک بزنید.
* **Invert assertion (will fail if above conditions met):** در صورتی که این مورد را تیک بزنید، اگر شرط مشخص شده برقرار باشد این Assertion با خطا مواجه می‌شود.

در صورتی که Assertion تعریفی شما یکی از شرایط زیر را داشته باشد، با خطا مواجه خواهد شد:
* اگر پاسخ دریافتی شما JSON نباشد.
  
   ![Json Assertion - Response Not Json](./resources/json-assertion-response-not-json.png?raw=true "Json Assertion - Response Not Json")
  
* اگر مسیری که در قسمت Assert JSON Path exists وجود نداشته باشد.
  
   ![JSON Assertion - Response No Field](./resources/json-assertion-response-no-field.png?raw=true "JSON Assertion - Response No Field")
  
* اگر فیلد مورد نظر در مسیر مشخص شده با مقداری که در Expected Value تعریف کرده‌اید برابر نباشد.
  
   ![JSON Assertion - Response Not Match](./resources/json-assertion-response-not-match.png?raw=true "JSON Assertion - Response Not Match")

به عنوان مثال درخواست ورود در سایت گزمه را در نظر بگیرید. حال قصد داریم با استفاده از JSON Assertion مقدار فیلد confirmed موجود در بدنه پاسخ را با مقدار مورد انتظار خود (true) مقایسه کنیم:
1. یک JSON Assertion به POST /create new ad اضافه میکنیم.
2. برای ارزیابی فیلد confirmed، در قسمت Assert JSON Path exists، مسیر آن را مشخص می‌کنیم. (پیش از مسیر فیلد، عبارت $. را وارد می کنیم.)
3. در این مثال برای فیلد confirmed، مقدار مورد انتظار ما true است. بنابراین مورد Additionally assert value را تیک می‌زنیم و در بخش Expected Value مقدار true را وارد می‌کنیم.
   
   ![JSON Assertion Sample](./resources/json-assertion-sample.png?raw=true "JSON Assertion Sample")

4. تست را اجرا می‌کنیم و پاسخی مانند شکل زیر دریافت می‌کنیم:
   
   ![JSON Assertion Failure Response](./resources/json-assertion-failure-response.png?raw=true "JSON Assertion Failure Response")

همانطور که در شکل بالا مشخص است، درخواست با مشکل مواجه شده و فیلد confirmed در پاسخ وجود ندارد، بنابراین Assertion با خطا مواجه شده و با رنگ قرمز نمایش داده می‌شود. همچنین در listener پیغامی مبنی بر عدم وجود فیلد مورد نظر نمایش داده می‌شود.

   ![JSON Assertion Failure](./resources/json-assertion-failure.png?raw=true "JSON Assertion Failure")

اگر درخواست با موفقیت اجرا شود و مقدار دریافتی با مقدار مورد انتظار برابر باشد، assertion با موفقیت اجرا شده و درخواست با رنگ سبز مشخص می‌شود.

   ![JSON Assertion Pass](./resources/json-assertion-pass.png?raw=true "JSON Assertion Pass")

### 3. JSR223 Assertion

با استفاده از JSR223 Assertion و اسکریپت‌نویسی می‌توان پاسخ دریافتی مورد نظر را بررسی و ارزیابی کرد. گاهی اوقات لازم است موارد خاص و پیچیده‌تری را بررسی کرد. استفاده از JSR223 Assertion نیازمند دانش استفاده از زبان‌های اسکریپت‌نویسی مانند groovy و … است.

میزان مصرف منابعی مانند CPU و حافظه بستگی به اسکریپتی که نوشته شده دارد، بنابراین سعی کنید اسکریپی که می‌نویسید پیچیدگی کمتری داشته باشد.

JSR223 Assertion نسبت به BeanShell Assertion انعطاف‌پذیری بالاتری دارد، منابع کمتری مصرف می‌کند و سریع‌تر است.

این Assertion شامل بخش‌های زیر است:
* **Language:** زبانی که قصد دارید با استفاده از آن اسکریپت‌نویسی کنید.
* **Parameters:** پارامترهایی که میخواهید به اسکریپت خود ارسال کنید.
* **Script file:** فایل حاوی اسکریپت برای اجرا
* **Script:** اسکریپتی که می‌خواهید اجرا شود را در این بخش بنویسید. این بخش در صورتی که در قسمت Script file فایلی را مشخص نکرده باشید اجباری است.
  
در ادامه چند مورد از متغیر هایی که میتوانید در اسکریپت خود استفاده کنید را بررسی میکنیم:
* **log:** برای نوشتن در لاگ فایل
* **vars:** برای گرفتن دسترسی خواندن/نوشتن متغیرها
* **sampler:** گرفتن دسترسی برای Sampler جاری
* **AssertionResult:** نتیجه Assertion

برای درک بهتر، در ادامه قصد داریم برای درخواست دریافت لیست آموزش‌های جی‌میتر در سایت گزمه، با استفاده از JSR233 Assertion پاسخ دریافتی را بررسی کنیم. وقتی API مربوطه را اجرا می‌کنیم، آرایه ای از اشیا دریافت می‌کنیم که شامل اطلاعات مربوط به آموزش‌ها مانند مواردی مانند ، tagName title و … هستند. 

   ![Gazmeh Jmeter Posts](./resources/gazmeh-jmeter-posts.png?raw=true "Gazmeh Jmeter Posts")

به عنوان مثال اسکریپتی با استفاده از groovy می‌نویسیم تا به کمک آن بررسی کنیم فیلد tagName تمامی آموزش‌ها برابر با "جی‌میتر" باشد:
```groovy
import groovy.json.JsonSlurper

error=false; msg="";

try{
   jsonSlurper = new JsonSlurper();
   _jsonObj = jsonSlurper.parseText(prev.getResponseDataAsString());


   for(int i=0 ; i<_jsonObj.items.size() ; i++)
   {
       if(!_jsonObj.items[i].tags[0].tagName.equals("جی‌میتر"))
           {
               error=true;
               msg=msg+ "\n post " + _jsonObj.items[i].id + ": the tag name is incorrect," + " the expected is جی‌میتر but actual tag name is: " + _jsonObj.items[i].tags[0].tagName;
           }
   }
}

catch(Exception ex){
   error=true;
   msg=msg+"\nException: "+ex.getMessage();   
}

if(error){
   AssertionResult.setFailure(true);
    AssertionResult.setFailureMessage(msg);
}
```

اگر شرط مورد نظر برقرار باشد، این Assertion با موفقیت اجرا می‌شود و نتیجه آن در View Results Tree Listener به صورت زیر خواهد بود:

   ![JSR223 Assertion Pass](./resources/jsr223-assertion-pass.png?raw=true "JSR223 Assertion Pass")

حال اگر در پاسخ دریافتی مقدار tagName برابر با جی‌میتر نباشد، این Assertion با شکست مواجه می‌شود:

   ![JSR223 Assertion Failure](./resources/jsr223-assertion-fail.png?raw=true "JSR223 Assertion Failure")


### 4. Duration Assertion

با استفاده از این Assertion می‌توان مشخص کرد که آیا سرور در زمان مشخص شده پاسخ داده است یا خیر. بنابراین برای درخواست‌هایی از Duration Assertion استفاده می‌کنیم که ارزیابی زمان پاسخ مهم است. استفاده از این Assertion برای تست کارایی و سنجش عملکرد سیستم توصیه می‌شود.

   ![Duration Assertion](./resources/duration-assertion.png?raw=true "Duration Assertion")

* **Duration in Milliseconds:** بیشترین مدت زمانی که مجاز است تا پاسخ از سرور دریافت شود. (بر حسب میلی‌ثانیه)
  
اگر پاسخ در مدت زمانی بیشتر از مدت زمان مشخص شده از سرور دریافت شود، این Assertion با خطا مواجه می‌شود.

   ![Final Response Time Schematic](./resources/final-res-time-schematic.png?raw=true "Final Response Time Schematic")
   
  اجرای با موفقیت → زمان مورد انتظار => زمان پاسخ
  
اجرای با شکست → زمان مورد انتظار < زمان پاسخ

به عنوان مثال چندین کاربر همزمان، درخواست دریافت لیست آموزش‌های جی‌میتر سایت گزمه را اجرا می‌کنند. حال می‌خواهیم بررسی کنیم که آیا سرور هر درخواست را در کمتر از 450 میلی‌ثانیه پاسخ می‌دهد و یا تحت تاثیر بار زیاد قرار گرفته و مدت زمان ارسال پاسخ افزایش می‌یابد. بنابراین مقدار Duration in millisecond را برابر با 450 قرار می‌دهیم:

   ![Duration Assertion Sample](./resources/duration-assertion-sample.png?raw=true "Duration Assertion Sample")

پس از اجرا اگر مدت زمان دریافت پاسخ کمتر از 450 میلی‌ثانیه باشد، با موفقیت اجرا می‌شود. همانطور که در شکل زیر مشاهده می‌کنید، پاسخ در مدت زمان 441 میلی‌ثانیه دریافت شده، بنابراین این Assertion در View Result Tree Listener با رنگ سبز مشخص می‌شود:

   ![Duration Assertion Pass](./resources/duration-assertion-pass.png?raw=true "Duration Assertion Pass")

Assertion درخواست هایی که پاسخ آن‌ها در مدت زمانی بیشتر از 450 میلی ثانیه دریافت شود، با خطا مواجه شده و با رنگ قرمز مشخص می‌شوند. در شکل زیر پاسخ درخواستی در 511 میلی‌ثانیه دریافت شده، بنابراین این Assertion با شکست مواجه می‌شود.

   ![Duration Assertion Failure](./resources/duration-assertion-failure.png?raw=true "Duration Assertion Failure")

## Plugin ها

Assertion‌های دیگری نیز در پکیج اصلی خود جی‌میتر وجود دارند که می‌توانید براساس نیاز خود، هر کدام را اضافه و استفاده کنید.



 

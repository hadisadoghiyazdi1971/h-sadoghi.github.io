# گزارش معماری و کشف محلی h-sadoghi.github.io

تاریخ: `2026-10-17`؛ منطقهٔ زمانی کاربر: `Asia/Tehran`.
مبنا: فایل‌های همین workspace و Git محلی، بدون اینترنت، نصب، build، اجرای workflow، commit، push یا deploy. اطلاعات محرمانه و مقادیر ایمیل بازنشر نشده‌اند. این گزارش سلامت یا خرابی سایت زنده را اثبات نمی‌کند.

## ۱. خلاصهٔ اجرایی — Executive summary

این مخزن منبع یک سایت دانشگاهی شخصی برای **Hadi Sadoghi Yazdi** و معرفی **Pattern Recognition Laboratory** در Ferdowsi University of Mashhad است. صفحهٔ اصلی زندگی‌نامه، خبرها، نوشته‌های اخیر، مقالات منتخب و ارتباطات را ترکیب می‌کند. blog، publications، projects، repositories، cv، teaching و people بخش‌های اصلی‌اند.

| نتیجه | اطمینان و شواهد |
|---|---|
| سایت ایستا با Jekyll، Markdown، YAML، Liquid و Sass | بالا؛ `Gemfile:1-29`، `_config.yml:192-200,217-282` و `assets/css/main.scss:1-25` |
| مبنای قالب al-folio و دارای محتوای شخصی‌سازی‌شده | بالا؛ `README.md:1-7`، `_config.yml:12-15` و محتوای واقعی `_pages/about.md` و `_pages/profiles.md` |
| مشتق از al-folio | بالا از نظر محتوا؛ وضعیت رسمی GitHub fork و parent repository از تنها remote محلی معلوم نیست؛ template/copy هم ممکن است |
| build در GitHub Actions و انتشار پوشهٔ `_site` با یک action انتشار شاخه‌ای | بالا برای تعریف کد؛ `.github/workflows/deploy.yml:63-95` |
| مقصد مورد انتظار `gh-pages` و Pages با روش Deploy from a branch | متوسط رو به بالا؛ `INSTALL.md:24-31,118-126` و `.github/workflows/prettier-html.yml:10-13`؛ workflow اصلی branch مقصد را صریح نمی‌نویسد |
| موفقیت فعلی deploy، تنظیمات حساب و دامنه | نامعلوم؛ GitHub UI، لاگ Actions و سایت زنده بررسی نشده‌اند |

نسخهٔ دقیق al-folio از فایل‌های بررسی‌شده مشخص نشد. شماره‌های نمونه در `INSTALL.md:38,76,224` و badge پویای README تعیین‌کنندهٔ نسخهٔ این checkout نیستند. Jekyll قفل‌شده `4.3.3` است؛ این عدد نسخهٔ قالب نیست.

## ۲. هویت مخزن — Repository identity

- workspace: `J:\SadoghiSite2\h-sadoghi.github.io`
- branch جاری: `master`؛ تنها شاخهٔ محلی فهرست‌شده نیز master بود.
- HEAD: `8512a9f`؛ subject: `Update profiles.md`؛ اطلاعات نویسنده/ایمیل Git بررسی نشده است.
- تنها remote، پاک‌سازی‌شده: `origin = https://github.com/h-sadoghi/h-sadoghi.github.io.git` برای fetch و push.
- وجود `_config.yml`، `Gemfile`، `_pages/` و `.github/` پیش از ادامه تأیید شد.
- گزارش در شروع وجود نداشت؛ هیچ تنظیم Git تغییر نکرد.

خروجی آغازین `git status --short`:

```text
?? CODEX_H_SADOGHI_SITE_DISCOVERY.md
```

فایل دستور از ابتدا untracked بود. نام حساب origin با نام فرد در config سازگار است، ولی اختیار کاربر نسبت به حساب‌های `h-sadoghi` و `LaboratoryPatternRecognition` از مخزن اثبات نمی‌شود. سایت آموزشی لینک‌شده متعلق به مسیر حساب جداگانه است.

## ۳. معماری سیستم — System architecture

```text
_config.yml                  هویت، URL، قابلیت‌ها، collections و Scholar
Gemfile / Gemfile.lock       وابستگی‌های Ruby و نسخه‌های قفل‌شده
_pages/                     صفحه‌ها و متن‌های جزء people
_posts/                     نوشته‌های محلی
_projects/                  پروژه‌ها، ترتیب و thumbnail
_news/                      خبرهای inline و دارای لینک
_data/                      CV، repositories، coauthors و venues
_bibliography/papers.bib     بانک اصلی مقالات
_layouts/                   قالب‌های صفحات و مقالات
_includes/                  منو، footer، figure، CV، loaderهای JS
_plugins/                   tag/filter و دریافت داده هنگام build
_sass/                      رنگ‌ها، typography، layout و سبک اجزا
assets/css/main.scss        نقطهٔ ورود Sass با متغیر Liquid
assets/js/                  theme، جست‌وجو و رفتار مرورگر
assets/img/                 تصاویر واقعی و نمونه
assets/pdf/                 PDF محلی
.github/workflows/          انتشار و کنترل‌ها و Docker
bin/                        توسعه و انتشار قدیمی
_site/                      مقصد build؛ در شروع موجود نبود
```

Jekyll، front matter را با config و داده‌های YAML/BibTeX ترکیب می‌کند؛ Liquid layout/includeها را می‌سازد؛ Markdown با kramdown/GFM و کد با Rouge پردازش می‌شود. زنجیرهٔ رایج `about/page/post/cv → default → head/header/footer/scripts` است. `profiles → page → default`، و `bib` قالب هر مقاله است. `none` فقط content می‌نویسد. Distill قالب HTML جدا دارد، ولی هیچ‌یک از پنج پست محلی فعلی از آن استفاده نمی‌کند.

`assets/css/main.scss:7-24`، site.max_width و partialهای Sass را مصرف می‌کند. JS در مرورگر اجرا می‌شود؛ pluginها و Scholar زمان build کار می‌کنند. توصیف Flask/Flutter/SQLite در پروژهٔ IPS شرح آن پروژه است و وجود backend فعال در این سایت را نشان نمی‌دهد. مسیر خروجی از permalink و قواعد collectionهاست؛ نام فایل لزوماً نام صفحهٔ منتشرشده نیست.

## ۴. نقشهٔ صفحات — Published page map

جدول، هر ۵۱ فایل `_pages/*.md` را شامل می‌شود. «ندارد» یعنی آن فیلد در YAML تعریف نشده است. مسیرهای صریح قرارداد خروجی‌اند، نه مشاهدهٔ سایت زنده.

| فایل و شاهد | title صریح | permalink صریح | navbar / ترتیب | layout | نقش و وضعیت |
|---|---|---|---|---|---|
| `_pages/404.md:1-7` | Page not found | `/404.html` | تعریف نشده | page | خطا؛ redirect به خانه پس از ۳ ثانیه؛ بیرون منو |
| `_pages/about.md:1-19` | about | `/` | تعریف نشده | about | خانه؛ لینک about اختصاصی در header |
| `_pages/about_einstein.md:1` | ندارد | ندارد | ندارد | ندارد | نمونهٔ upstream؛ exclude صریح در `_config.yml:246`؛ در profiles مصرف نشده |
| `_pages/about_kabir.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Aghajanzadeh.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Ashkezari.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Bagherzadeh.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Bahreini.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/blog.md:1-17` | blog | `/blog/` | true / 1 | default | فهرست وبلاگ؛ ۵ پست در هر صفحه |
| `_pages/cv.md:1-12` | cv | `/cv/` | true / 5 | cv | رزومه با TOC سمت چپ؛ cv_pdf خالی |
| `_pages/Drlotfizad.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/dropdown.md:1-16` | submenus | ندارد | true / 8 | page | تعریف submenus؛ بدون permalink؛ مسیر مستقیم پیش‌فرض بدون build تثبیت نشده |
| `_pages/Farahi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Forghani.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Gharib.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Ghasemigol.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Ghazi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Ghazikhani.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Haghighi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Hamedani.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Hamidzadeh.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/HomaForoughi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Hosseini_40.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Izanloo.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Javidi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Kamel.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Karimpour.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Keshvari.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Mahmoodi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/majidi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/MaryamBaradaran.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Mehrizi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Mirghasemi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/MonaZamiri.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Naeimi.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Nasiri.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Nayyeri.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/news.md:1-5` | news | `/news/` | تعریف نشده | page | فهرست خبرها؛ لینک از خانه، بیرون navbar |
| `_pages/Pakdaman.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/profiles.md:1-324` | people | `/people/` | true / 7 | profiles | people؛ ۳۹ پروفایل به ترتیب آرایه |
| `_pages/projects.md:1-10` | projects | `/projects/` | true / 3 | page | کارت پروژه، دسته‌ها و عکس گروه |
| `_pages/publications.md:1-8` | publications | `/publications/` | true / 2 | page | کتاب‌شناسی کامل با فیلتر |
| `_pages/Rahgooy.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Razavi_Mahnaz.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/repositories.md:1-8` | repositories | `/repositories/` | true / 4 | page | آمار حساب‌ها و مخازن GitHub |
| `_pages/Sabzekar.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/shafighparsazad.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Shams.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/Shekofteh.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |
| `_pages/teaching.md:1-9` | teaching | `/teaching/` | true / 6 | page | پنج درس با تصویر و لینک بیرونی |
| `_pages/Zabihzadeh.md:1` | ندارد | ندارد | ندارد | ندارد | زندگی‌نامهٔ جزء در `/people/`؛ بدون front matter و nav مستقل |

۱۱ فایل front matter، ۳۹ متن درج‌شده در people و یک نمونهٔ exclude هستند. هیچ `published: false` در چهار پوشهٔ محتوا یافت نشد و `_drafts/` در شروع موجود نبود. بیرون navbar بودن به معنی خصوصی یا draft نیست؛ news و 404 مسیر صریح دارند. fragmentها صفحهٔ HTML مستقل تعریف نمی‌کنند؛ احتمال کپی متن خام به‌عنوان static file با توجه به `include: ["_pages"]` تنها پس از build قطعی می‌شود. برای dropdown مسیر خیالی نسبت داده نشده؛ منو از children آن ساخته می‌شود. بین permalinkهای صریح این ۱۱ فایل تکرار نیست؛ تمام مسیرهای تولیدی بدون build بررسی نشده‌اند.

## ۵. موجودی محتوا — Content inventory

### پست‌ها

هر پنج پست `layout: post` و `categories: sample-posts` دارند. تاریخ زیر از YAML است. الگوی خروجی `_config.yml:138` برابر `/blog/:year/:title/` است؛ slug از نام فایل می‌آید، نه لزوماً عنوان انسانی. مسیر نهایی بدون build گزارش نمی‌شود.

| فایل | title | date | موضوع و thumbnail |
|---|---|---|---|
| `_posts/2024-09-24-AliForouzan.md:1-9` | Ali Forouzan near the end | 2024-09-24 | breakout بازار؛ تصویر بدنه Blog/Breakout.jpg؛ `assets/img/Blog/Breakout_ElasticMatch_Hisory.jpg` |
| `_posts/2024-09-25-IPS_Team.md:1-9` | the Indoor Positioning System Team | 2024-09-25 | تیم IPS؛ تصویر Blog/IPS_3d.jpg؛ placeholderهای Member Name باقی است؛ `assets/img/Blog/IPS_1.jpg` |
| `_posts/2024-09-25-Jaafari.md:1-9` | Javad finishes the Volume profile | 2024-09-25 | Volume Profile و فرمول KDE؛ giscus_comments فعال، related_posts غیرفعال؛ `assets/img/Blog/VP.jpg` |
| `_posts/2024-09-26-Naji.md:1-9` | Naji graduate in Indoor Positioning System | 2024-09-25 | Wi-Fi fingerprinting، کاربردها و معماری؛ تاریخ YAML با روز نام فایل متفاوت است؛ `assets/img/Blog/imagesNaji/ips_applications.jpg` |
| `_posts/2024-10-02-LossConcept.md:1-9` | Loss function Lecture 1 | 2024-10-02 | absolute/robust/pinball loss و equality/equity؛ چند فرمول و ۷ تصویر بدنه؛ `assets/img/Blog/LossBlog/MainPage_Loss.JPG` |

### پروژه‌ها

همه `layout: page`، category برابر work و بدون تاریخ صریح‌اند. importance از ۱ تا ۶ ترتیب کارت‌هاست. الگوی `_config.yml:198-200` مسیر مورد انتظار `/projects/1_project/` تا `/projects/6_project/` را می‌سازد.

| فایل | title | importance | thumbnail و خلاصه |
|---|---|---|---|
| `_projects/1_project.md:1-9` | Interactive wall | 1 | `assets/img/InteractiveWall_1.jpg`؛ Kinect، Unity و بازی تعاملی؛ related_publications فعال ولی cite در بدنه نیست |
| `_projects/2_project.md:1-9` | IPS project | 2 | `assets/img/EngineeringFUM.jpg`؛ IPS دانشگاه، fingerprinting و شرح GeoJSON/Flutter/Flask/SQLite؛ نظرات فعال |
| `_projects/3_project.md:1-9` | Smart Mirror | 3 | `assets/img/SmartMirrorMain.jpg`؛ آینهٔ هوشمند؛ ساخت، هتل و حرم؛ redirect خالی |
| `_projects/4_project.md:1-9` | Air Pollution Monitoring | 4 | `assets/img/AirPolution_Sensor.jpg`؛ حسگر متحرک آلودگی، LoRa و اپ |
| `_projects/5_project.md:1-9` | Intelligent Driver Violation Detection System | 5 | `assets/img/Violate_BusDriver.jpg`؛ تشخیص تخلف راننده؛ تصاویر دوربین و پنل |
| `_projects/6_project.md:1-9` | Real Time emotion detection system | 6 | `assets/img/EmotionalDetector.jpg`؛ تشخیص هیجان؛ دوربین، recognizer، monitoring و ذخیرهٔ داده |

`_pages/projects.md:8,14-38` دسته‌های work و fun را حلقه می‌زند؛ هر شش پروژه در work هستند و fun کارت محلی ندارد. گرید فعال Bootstrap یک ستون و از breakpoint متوسط سه ستون است (`:32`)؛ فعال‌بودن تنظیم Masonry به معنی استفادهٔ این گرید مشخص از آن نیست.

### خبرها

| فایل | title صریح | date صریح | رفتار |
|---|---|---|---|
| `_news/announcement_1.md:1-8` | ندارد | 2024-09-24 | inline:true؛ تبریک شروع نیم‌سال |
| `_news/announcement_2.md:1-16` | Instruction of Homework and Mini-Project | September 24, 2024 | inline:false؛ دستور تکلیف، Notebook، مهلت و اطلاعات نویسنده |
| `_news/announcement_3.md:1-13` | Midterm | 2024-09-25 | inline:true؛ زمان نسبی میان‌ترم سه درس و نمرهٔ ۵ از ۲۰ |

همه layout:post و related_posts:false دارند. news دارای output:true است؛ inline فقط شیوهٔ درج در فهرست است و خروجی مستقل را خاموش نمی‌کند. مسیرها از `/news/:title/` و لینک از item.url می‌آیند. `_includes/news.liquid:11` collection را reverse می‌کند و sort صریح براساس date ندارد.

### مقالات و داده‌ها

`_bibliography/papers.bib`، ۱۶۷۴ خط و **۱۲۳ رکورد خام** دارد: ۱۱۴ article، ۸ inproceedings و ۱ incollection. ۱۱۱ کلید یکتا و ۱۲ کلید دوبار تکراری وجود دارد؛ تعداد آثار علمی یکتا با این عدد تعیین نمی‌شود. سال‌های صریح از ۲۰۰۸ تا ۲۰۲۴‌اند. ۱۹ رکورد selected=true و یک selected=false دارند؛ خروجی منتخب نهایی با وجود duplicateها نیاز به بررسی دارد.

در هر ۱۲۳ رکورد title/author/year تشخیص داده شد. ۵۲ google_scholar_id، ۱۷ abstract، ۱۷ doi و ۱۳ html غیرخالی و ۱۵ dimensions=true وجود دارد. ۱۵ فیلد pdf همگی خالی‌اند؛ هیچ preview یا PDF/poster/slides/supp محلیِ غیرخالی در بانک اصلی تشخیص داده نشد. این شمارش ساختاری از متن است، نه اعتبارسنجی با parser رسمی BibTeX.

مسیر نمایش: `papers.bib → jekyll-scholar و _config.yml:315-380 → _pages/publications.md:14-18 → _layouts/bib.liquid`. سبک apa، locale=en، query=@* و group_by=year با ترتیب نزولی است. منتخب‌های خانه از `_includes/selected_papers.liquid:1-3` با query selected=true و group_by none می‌آیند. فیلتر مقالات از `_includes/bib_search.liquid` و `assets/js/bibsearch.js` متن DOM را می‌گردد و hash URL هم مقدار فیلتر است؛ `/publications/#...` الزاماً anchor مقاله نیست.

تشخیص نویسندهٔ صاحب سایت exact-match است (`_layouts/bib.liquid:60-99`). config فقط last_name=[Sadoghi Yazdi] و first_name=[Hadi,H.] دارد، ولی شکل‌های Yazdi, Hadi Sadoghi و Sadoghi-Yazdi, Hadi هم در داده هست. `_data/coauthors.yml` هشت کلید و ۹ حالت نامِ نمونه دارد؛ از people خودکار پر نمی‌شود. `_data/venues.yml` سه کلید AJP/PhysRev/Vision دارد؛ abbrهای دیگر بدون metadata اختصاصی badge متنی می‌گیرند.

دکمهٔ HTML از entry.html ساخته می‌شود، نه صرفاً url. PDF/slide/poster/supp نسبی در کد واقعی به **assets/pdf/** می‌رود (`_layouts/bib.liquid:208-245`)؛ `CUSTOMIZE.md:83` برخلاف کد یک بار assets/pdfs/ می‌گوید. preview از assets/img/publication_preview/ می‌آید؛ فعلاً تصویر و فیلد فعال preview وجود ندارد. `assets/bibliography/2018-12-22-distill.bib` نمونهٔ جداگانه است و منبع publications فعلی نیست.

### افراد، CV، تدریس و GitHub

۳۹ profile در `_pages/profiles.md:9-323` هست؛ هرکدام یک متن در _pages و تصویر در assets/img دارند. layout با include_relative و markdownify متن را درج می‌کند (`_layouts/profiles.liquid:25-28`). همهٔ ۳۹ متن و ۳۹ تصویر با case دقیق موجودند. ترتیب و alignment از YAML است و ارتباط خودکار با نویسندگان BibTeX ندارد.

| ترتیب | متن زیر _pages/ | تصویر زیر assets/img/ | alignment |
|---|---|---|---|
| 1 | `about_kabir.md` | `Kabir.JPG` | right |
| 2 | `Drlotfizad.md` | `Drlotfizad.jpg` | left |
| 3 | `Hosseini_40.md` | `Hosseini_40.jpg` | right |
| 4 | `Ashkezari.md` | `Ashkezari.jpg` | left |
| 5 | `Shams.md` | `Shams.JPG` | right |
| 6 | `Izanloo.md` | `Izanloo.JPG` | left |
| 7 | `Razavi_Mahnaz.md` | `Razavi_Mahnaz.JPG` | right |
| 8 | `Bahreini.md` | `Bahreini.JPG` | left |
| 9 | `Gharib.md` | `Gharib.JPG` | right |
| 10 | `Keshvari.md` | `Keshvari.JPG` | left |
| 11 | `Ghazi.md` | `Ghazi.JPG` | right |
| 12 | `Nayyeri.md` | `Nayyeri.JPG` | left |
| 13 | `Hamedani.md` | `Hamedani.jpg` | right |
| 14 | `Naeimi.md` | `Naeimi.JPG` | left |
| 15 | `Mahmoodi.md` | `Mahmoodi.JPG` | right |
| 16 | `Forghani.md` | `Forghani.JPG` | left |
| 17 | `Aghajanzadeh.md` | `Aghajanzadeh.JPG` | right |
| 18 | `Zabihzadeh.md` | `Zabihzadeh.JPG` | left |
| 19 | `HomaForoughi.md` | `HomaForoughi.JPG` | right |
| 20 | `Farahi.md` | `Farahi.JPG` | left |
| 21 | `Nasiri.md` | `Nasiri.jpg` | right |
| 22 | `Ghazikhani.md` | `Ghazikhani.JPG` | left |
| 23 | `Pakdaman.md` | `Pakdaman.JPG` | right |
| 24 | `MaryamBaradaran.md` | `MaryamBaradaran.JPG` | left |
| 25 | `Shekofteh.md` | `Shekofteh.JPG` | right |
| 26 | `Rahgooy.md` | `Rahgooy.JPG` | left |
| 27 | `Kamel.md` | `Kamel.JPG` | right |
| 28 | `Mirghasemi.md` | `Mirghasemi.JPG` | left |
| 29 | `Ghasemigol.md` | `Ghasemigol.JPG` | right |
| 30 | `Javidi.md` | `Javidi.JPG` | left |
| 31 | `Haghighi.md` | `Haghighi.JPG` | right |
| 32 | `Sabzekar.md` | `Sabzekar.JPG` | left |
| 33 | `MonaZamiri.md` | `MonaZamiri.JPG` | right |
| 34 | `Hamidzadeh.md` | `Hamidzadeh.jpg` | left |
| 35 | `Mehrizi.md` | `Mehrizi.jpg` | right |
| 36 | `Karimpour.md` | `Karimpour.jpg` | left |
| 37 | `majidi.md` | `majidi.jpg` | right |
| 38 | `shafighparsazad.md` | `shafighparsazad.jpg` | left |
| 39 | `Bagherzadeh.md` | `Bagherzadeh.jpg` | right |

CV در `_pages/cv.md` و `_layouts/cv.liquid` دو مسیر دارد: اگر site.data.resume باشد JSON وگرنه `_data/cv.yml`. در checkout، resume.json غایب و تنها assets/json/table_data.json موجود است. YAML شخصی چهار بخش General Information، Education، Job و Responsibility دارد. با این حال `_config.yml:634-636` هنوز resume.json غایب را درخواست می‌کند؛ برخورد افزونه با آن بدون build معلوم نیست. fallback قالب قطعی است ولی رسیدن موفق build به آن تأیید نشده. cv_pdf خالی است؛ PDF اختصاصی CV معرفی نشده.

Teaching فهرست پنج درس با عکس موجود و لینک بیرونی است؛ محتویات درس‌ها در این مخزن نیست. repositories از `_data/repositories.yml` و تصویرهای آماری خارجی ساخته می‌شود؛ صفحه API اختصاصی برای استخراج خودکار مخازن ندارد.

## ۶. صفحهٔ اصلی و ناوبری — Homepage and navigation

ورودی خانه `_pages/about.md:1-19` است: layout=about، permalink=/، تصویر Sadoghi.jpg در راست و غیرمدور. نام از `_config.yml:5-8` می‌آید؛ title:blank یک sentinel برای نمایش نام کامل است (`_layouts/about.liquid:7-12`). متن bio در `_pages/about.md:21-60` و تصویر واقعی در `assets/img/Sadoghi.jpg` است؛ وجود prof_pic.jpg یا prof_pic_color.png به معنی استفادهٔ آن‌ها در خانه نیست.

ترتیب اجزای خانه در `_layouts/about.liquid:41-73`: متن، news، latest posts، selected publications و social/contact note. news به page.news و site.announcements.enabled وابسته است؛ حد ۵ و scrolling بیش از سه مورد تعریف شده. latest posts از تنظیم سراسری فعال و حد ۳ می‌آید و به page.latest_posts وابسته نیست. selected_papers و social در front matter روشن‌اند.

`_includes/header.liquid:45-110` عنوان about را از صفحهٔ permalink=/ می‌گیرد و لینک خانه را جدا می‌سازد. سپس site.pages را با nav_order مرتب و فقط موارد nav با autogen==null را نشان می‌دهد. ترتیب فعلی: about، blog(1)، publications(2)، projects(3)، repositories(4)، cv(5)، teaching(6)، people(7)، submenus(8). submenus لینک‌های تکراری publications/projects/blog و دو divider دارد. about بدون nav:true نیز در منوست.

navbar از navbar-expand-sm و collapse با navbarNav استفاده می‌کند؛ در عرض کوچک hamburger دارد (`header:3,28-43`). search با Ninja Keys و میانبر ctrl k اضافه می‌شود؛ `assets/js/shortcut-key.js` نمایش میانبر را برای Mac عوض می‌کند. search عمومی metadata صفحات منو، پست‌ها، collectionها، socialها و دستور theme را فهرست می‌کند؛ جست‌وجوی کامل متن همهٔ زندگی‌نامه‌ها نیست (`_includes/scripts/search.liquid:28-120,547-579`).

عرض سایت 930px است؛ profile در موبایل ۱۰۰٪ و از 576px برابر ۳۰٪ می‌شود (`_config.yml:57`، `_sass/_base.scss:193-228`). header و footer ثابت‌اند؛ padding در `_sass/_layout.scss:5-32` است. رنگ اصلی روشن بنفش و تاریک cyan است (`_sass/_themes.scss:5-21,70-85`). theme با localStorage و حالت system/light/dark کار می‌کند (`assets/js/theme.js:3-22,208-252`). MathJax، medium zoom، progress bar و back-to-top فعال‌اند؛ analytics، newsletter، Open Graph و Schema.org در config خاموش‌اند. metadata استاندارد و canonical در head تولید می‌شوند.

عنوان‌های «Navbar customization» یا «post Customization» در Sass اثبات نمی‌کنند این بخش‌ها تغییر شخصی صاحب سایت‌اند؛ بدون مقایسه با upstream نباید همهٔ Sass/JS را به ایشان نسبت داد. شخصی‌سازی‌های روشن، متن‌ها، داده‌ها و عکس‌های واقعی‌اند. JS مشترک abstract/award/BibTeX را باز می‌کند، TOC می‌سازد و theme جداول/Notebook را هماهنگ می‌کند؛ scriptهای نمودار و Distill امکانات قالب‌اند و لزوماً در محتوای فعلی مصرف نشده‌اند.

پیوندهای مهم صریح، بدون آزمون شبکه:

| مبدأ | مقصد |
|---|---|
| `_pages/about.md:5` | href='#'؛ نام دانشگاه فعلاً مقصد بیرونی واقعی ندارد |
| `_pages/about.md:59` | `http://prof.um.ac.ir/h-sadoghi/`؛ HTTP؛ سلامت/redirect آزموده نشده |
| `_pages/about.md:60` و `_config.yml:99` | Google Scholar با user ID برابر wEBTP4oAAAAJ؛ لینک دستی و config باید همسو بمانند |
| `_pages/teaching.md:11` | `https://laboratorypatternrecognition.github.io/PatternRecognition_S/pattern_recognition.html` |
| `_pages/teaching.md:12` | `https://laboratorypatternrecognition.github.io/MachineLearningS/machine_learning.html` |
| `_pages/teaching.md:13` | `https://laboratorypatternrecognition.github.io/CircuitElectronics/Introduction_CircuitElectronics.html` |
| `_pages/teaching.md:14` | `https://laboratorypatternrecognition.github.io/SignalSystem/SignalSystemStart.html` |
| `_pages/teaching.md:15` | `https://laboratorypatternrecognition.github.io/AI-Essentials/Home_Page.html` |
| `_pages/projects.md:17-18` | anchorهای work و fun؛ id و لینک .#category از یک مقدار تولید می‌شوند |
| `_news/announcement_2.md:13` | `https://myst-parser.readthedocs.io/en/latest/syntax/typography.html` |

اغلب روابط صفحات از relative_url و URLهای Jekyll ساخته می‌شوند. teaching و عکس گروه با Markdown و ../assets/img/... لینک می‌شوند؛ روی مسیرهای فعلی /teaching/ و /projects/ به /assets/img/... می‌رسند. انتقال صفحه به عمق بیشتر نیاز به بازبینی دارد. figure.liquid برای img از فایل اصلی و برای WebP از srcset با عرض‌های ۴۸۰، ۸۰۰ و ۱۴۰۰ استفاده می‌کند؛ وجود اصل تصویر با وجود خروجی WebP تولیدی یکسان نیست. تصویرهای پروژه از figure و thumbnail وبلاگ از img در `_pages/blog.md:182` می‌آیند.

social از `_includes/social.liquid` و config می‌آید: ایمیل، Scholar، Inspire HEP و RSS فعال‌اند؛ github_username سراسری خالی است، هرچند حساب در repositories.yml تعریف شده. این دو ورودی مستقل‌اند. ایمیل در `_config.yml:9`، `_pages/profiles.md:41` و `_posts/2024-09-26-Naji.md:16` دیده شد؛ مقدارشان بازنشر نشده. footer از نام config، سال زمان build و footer_text استفاده می‌کند (`_includes/footer.liquid:1-35`). last_updated:false است.

## ۷. ساخت و انتشار — Build and deploy

### وابستگی‌ها

`Gemfile:2-24` افزونه‌های Jekyll، mini_racer، unicode_utils و webrick و گروه دیگر css_parser/feedjira/httparty را تعریف می‌کند. Gemfile.lock واقعاً tracked است، با وجود `.gitignore:9`؛ ignore فایل tracked را حذف نمی‌کند. نسخه‌های قفل‌شده: Jekyll 4.3.3 (`lock:82`)، Liquid 4.0.4 (`:153`)، Jekyll Scholar 7.1.3 (`:122`)، Sass converter 3.0.0 (`:120`) و Bundler 2.5.7 (`:243-244`). platformهای lock، Linux/macOS هستند و Windows native در فهرست نیست (`:211-215`).

CI، Ruby 3.2.2 انتخاب می‌کند؛ نسخهٔ Node صریح تعیین نشده. package.json فقط prettier=3.1.1 و @shopify/prettier-plugin-liquid=1.4.0 دارد؛ اپ Node یا script ساخت وب‌اپ تعریف نمی‌کند. npm در CI برای PurgeCSS و formatting است. Python/Jupyter در workflow نصب می‌شود. ImageMagick در config فعال و در Dockerfile صریحاً نصب می‌شود؛ workflow اصلی نصب صریح آن ندارد و وجود نسخهٔ لازم روی runner از فایل محلی معلوم نیست.

### مسیر اصلی deploy.yml

۱. push و pull_request به master/main با فیلتر مسیر و workflow_dispatch (`:3-58`). assets، BibTeX، Markdown، Liquid، JS، YAML و Gemfileها در فیلترند؛ اسناد راهنما و چند workflow مستثنا شده‌اند.

۲. checkout روی ubuntu-latest، Ruby 3.2.2 و bundler-cache؛ درخواست contents:write (`:60-74`).

۳. giscus.repo با github.repository فقط در workspace runner و commitChange:false تنظیم می‌شود (`:75-81`)؛ این تغییر لزوماً در branch منبع ذخیره نمی‌شود.

۴. نصب Jupyter، تنظیم JEKYLL_ENV=production و `bundle exec jekyll build`؛ سپس نصب سراسری PurgeCSS و `purgecss -c purgecss.config.js` (`:82-90`). purgecss.config.js، HTML/JS خروجی را می‌خواند و CSS در _site/assets/css را پالایش می‌کند.

۵. برای رویداد غیرPR، JamesIves/github-pages-deploy-action@v4 پوشهٔ _site را منتشر می‌کند (`:91-95`). branch و repository مقصد در فایل تعیین نشده؛ پیش‌فرض واقعی action نیازمند کد همان action یا لاگ اجراست. اسناد محلی انتظار gh-pages دارند، نه اثبات وضعیت کنونی سرور.

در workflow اصلی actions/upload-pages-artifact یا actions/deploy-pages وجود ندارد. روش مورد انتظار راهنمای محلی Deploy from a branch روی gh-pages است (`INSTALL.md:24-31`). environment مخصوص Pages در workflow اصلی تعریف نشده. فایل CNAME در ریشه موجود نبود؛ keep_files:[CNAME,.nojekyll] در config وجود دامنهٔ اختصاصی را اثبات نمی‌کند.

### تمام workflowهای موجود

| فایل | trigger و کارکرد تعریف‌شده |
|---|---|
| `.github/workflows/deploy.yml` | push/PR فیلترشده و دستی؛ build و انتشار مشروط |
| `.github/workflows/broken-links.yml:3-52` | push/PR؛ lychee روی Markdown/HTML منبع با استثناهای قالب |
| `.github/workflows/broken-links-site.yml:3-46` | پس از موفقیت Deploy site؛ build مجدد و lychee آفلاین روی HTML؛ crawl سایت زنده نیست |
| `.github/workflows/axe.yml:3-73` | دستی؛ build، Chrome، http-server و axe روی آدرس محلی |
| `.github/workflows/prettier.yml:3-48` | push/PR؛ check فرمت، در failure ساخت diff/artifact و در PR ارسال repository_dispatch |
| `.github/workflows/prettier-comment-on-pr.yml:3-18` | repository_dispatch؛ درج کامنت PR؛ در این ممیزی اجرا نشده |
| `.github/workflows/prettier-html.yml:3-36` | دستی؛ checkout gh-pages، اصلاح HTML، commit و push؛ ابزار preview نیست |
| `.github/workflows/lighthouse-badger.yml:17-30,49-62` | page_build/دستی؛ URL هنوز upstream al-folio است و token secret می‌خواهد |
| `.github/workflows/deploy-image.yml:3-44` | push فیلترشده؛ فقط owner=alshedivat؛ push image به amirpourmand/al-folio |
| `.github/workflows/deploy-docker-tag.yml:3-50` | tag با v*؛ Docker چندمعماری؛ guard مالک مشابه deploy-image ندارد و نیازمند Docker secrets است |
| `.github/workflows/docker-slim.yml:5-57` | push فایل خود و workflow_run از Docker Image CI؛ job مشروط به success و owner=alshedivat؛ push image slim |

DOCKER_USERNAME، DOCKER_PASSWORD و LIGHTHOUSE_BADGER_TOKEN نام ارجاع secret هستند، نه مقدار credential. موجودبودن/اعتبار آن‌ها بررسی نشده. تنظیمات واقعی Pages source/path، دسترسی Actions، branch protection، Giscus و Discussions و آخرین لاگ موفق نیاز به GitHub UI دارند.

### پیش‌نمایش محلی؛ فقط پیشنهاد

هیچ فرمان setup/preview اجرا نشده است. برای مرحلهٔ مجاز بعدی، یک کپی آزمایشی، Ruby/Bundler سازگار، Python/Jupyter و ImageMagick آماده شود. راهنمای محلی روی Windows، WSL را پیشنهاد می‌کند (`INSTALL.md:40-42`). فرمان‌های ذکرشده در راهنما:

```bash
bundle install
pip install jupyter
bundle exec jekyll serve
```

آدرس این مسیر http://localhost:4000 است (`INSTALL.md:87-96`). برای همسانی با CI، Ruby 3.2.2 و lock فعلی بررسی شود. bundle install می‌تواند lock/cache را تغییر دهد و جزو این ممیزی نیست.

Docker در INSTALL.md:44-76 پیشنهاد شده و port 8080 دارد، اما docker-compose.yml:9-10 پوشهٔ فعلی را bind-mount می‌کند و **bin/entry_point.sh:5,18، Gemfile.lock را حذف می‌کند**؛ فقط در کپی آزمایشی با آگاهی از این رفتار استفاده شود. bin/deploy ابزار preview نیست: checkout/حذف branch، حذف فایل‌ها و force-push دارد (`:77-115`)؛ فقط متن آن خوانده شد.

build پیش‌فرض آفلاین نیست: external-posts.rb منابع خارجی config را دریافت می‌کند و tag Scholar درخواست شبکه و تأخیر تصادفی دارد. third_party_libraries.download:false مانع مصرف CDN در مرورگر نمی‌شود. ساخت موفق در آینده باید مستقل ثبت شود.

## ۸. راهنمای تغییر و شخصی‌سازی — Customization map / change guide

| درخواست | ورودی اصلی | وابستگی و نکته |
|---|---|---|
| نام/عنوان/affiliation/bio | `_config.yml:5-17` و `_pages/about.md:5,21-60` | title:blank رفتار ویژه دارد؛ layout/metadata مصرف‌کننده‌اند |
| landing و بخش‌ها | `_pages/about.md:7-18`، `_config.yml:202-210` | فقط برای ساختار جدید `_layouts/about.liquid` |
| تصویر اصلی | `_pages/about.md:9` و `assets/img/Sadoghi.jpg` | case دقیق؛ figure مسئول responsive |
| منو | nav/nav_order در front matter همان صفحه | children در `_pages/dropdown.md`؛ رفتار در `_includes/header.liquid` |
| تدریس | `_pages/teaching.md:10-15` و تصاویر | محتوای درس در سایت جداست |
| مقاله | `_bibliography/papers.bib` | کلید یکتا؛ `_config.yml:315-380` تنظیم Scholar |
| لینک نویسنده/مجله | `_data/coauthors.yml` و `_data/venues.yml` | از people پر نمی‌شود؛ spelling باید با BibTeX هماهنگ باشد |
| Google Scholar | `_config.yml:99,315-317,342-346` و google_scholar_id رکوردها | لینک دستی `_pages/about.md:60` نیز هماهنگ شود |
| پروژه | `_projects/*.md`: title/description/img/importance/category | landing در `_pages/projects.md`؛ کارت در `_includes/projects.liquid` |
| وبلاگ | `_posts/YYYY-MM-DD-slug.md` | date/thumbnail/tags؛ عنوان/منابع در `_config.yml:136-177` |
| خبر | `_news/*.md` | date/inline/title؛ limit در `_config.yml:202-205` |
| CV | `_data/cv.yml` و `_pages/cv.md` | ابتدا تصمیم JSON/YAML؛ `_config.yml:634-649` و `_layouts/cv.liquid` |
| PDF رزومه | `assets/pdf/` و cv_pdf در `_pages/cv.md:7` | فعلاً PDF اختصاصی معرفی نشده |
| عضو/استاد/تیم | `_pages/profiles.md` + `_pages/<person>.md` + `assets/img/<photo>` | ترتیب/align در YAML؛ متن جزء بدون front matter |
| عکس گروه/پژوهش | `_pages/projects.md:67-68`، `_projects/*.md` و `_posts/*.md` | فایل مصرف‌شده را تغییر دهید، نه تصویر نمونهٔ ظاهراً مشابه |
| footer/social | `_config.yml:12-24,74-116` | `_includes/footer.liquid` و `_includes/social.liquid` نمای مشترک |
| CSS/رنگ/عرض | `_sass/_themes.scss`، `_variables.scss`، `_base.scss` و `_layout.scss` | entry در `assets/css/main.scss`؛ عرض config |
| URL/دامنه | `_config.yml:20-21` | CNAME/Pages settings بیرونی و لینک‌های دستی بررسی شوند |
| GitHub repos | `_data/repositories.yml` | owner/repo با include سازگار؛ social GitHub جدا در config |
| نظرات | `_config.yml:151-165` و giscus_comments پست/صفحه | CI فقط repo را پر می‌کند؛ نصب و repo/category ID جداست |
| search/JS | `_config.yml:52-54`، `_includes/scripts/search.liquid` و `assets/js/bibsearch.js` | search عمومی و فیلتر مقالات مستقل‌اند |

اولویت با ورودی‌های Markdown/YAML/BibTeX است؛ تغییر قالب مشترک فقط برای نیاز ساختاری پوشش‌داده‌نشده. هیچ تغییر پیشنهادی اجرا نشده است.

## ۹. نگرانی‌های فنی مستند — Specific technical concerns

شدت نسبت به اثر بر داده/تجربهٔ کاربر است؛ هیچ مورد به معنی اثبات شکست deploy زنده نیست.

| شناسه / شدت | شاهد | اثر قابل استنتاج | اقدام آینده |
|---|---|---|---|
| T1 / زیاد برای صحت علمی | `_bibliography/papers.bib:30,270` و ۱۱ جفت دیگر در جدول پایین | کلید تکراری؛ خطر overwrite/ابهام metadata و selected در پردازش؛ تعداد HTML نهایی تأیید نشده | ادغام رکوردها با حفظ DOI/Scholar ID/selected و کلید یکتا |
| T2 / متوسط | `_data/repositories.yml:8` در برابر `_includes/repository/repo.liquid:1,15-24` | URL کامل سازمان به href دارای پیشوند دوبارهٔ https://github.com/ و username/repo نامناسب تبدیل می‌شود | انتخاب مخزن واقعی owner/repo یا معرفی سازمان با بخش مناسب کاربران/لینک عادی |
| T3 / کم تا متوسط | `_data/repositories.yml:3` و `_pages/repositories.md:15-16,23-29` | عنصر null در github_users؛ loop حذف null ندارد و می‌تواند کارت بی‌نام بسازد | حذف عنصر خالی یا guard متناسب |
| T4 / متوسط | `_projects/2_project.md:57`: WiFiLocation.jpg؛ فایل واقعی `assets/img/WiFiLocation.JPG` | اختلاف case در میزبان حساس؛ Windows ممکن است پنهانش کند | هماهنگ‌کردن case و بررسی خروجی |
| T5 / متوسط، نیازمند build | `_config.yml:634-636` و `_layouts/cv.liquid:4-50` | JSON درخواست‌شده غایب؛ fallback قالب هست ولی رفتار افزونه با فایل غایب معلوم نیست | تصمیم صریح YAML/JSON و build آزمایشی؛ غیبت JSON اثبات خرابی CV نیست |
| T6 / متوسط | `_config.yml:171-177` و `_plugins/external-posts.rb:12-29,68-100` | دریافت Medium/Google Blog نمونه و افزودن به site.posts؛ شمار کل بیش از فایل‌های محلی می‌شود؛ درخواست‌ها rescue محلی ندارند | تأیید هدف منابع و مدیریت خطا/حذف/جایگزینی در مرحلهٔ مجاز |
| T7 / کم تا متوسط | `_config.yml:316-317`، `_bibliography/papers.bib:33,206` و `_layouts/bib.liquid:64-66` | شکل‌های مختلف نام همیشه exact-match نمی‌شوند؛ بعضی تأکیدهای نام ممکن است اعمال نشوند | یکدست‌کردن داده و نام‌های مجاز و بررسی خروجی parse |
| T8 / کم تا متوسط | `_includes/scripts/search.liquid:48-54` در برابر `_pages/dropdown.md:8-15` | search از child.url می‌خواند ولی داده child.permalink دارد؛ publications/projects مقصد تعریف‌شده را نمی‌گیرند؛ navbar خودش permalink می‌خواند | همسان‌سازی هدف search و بررسی مرورگر |
| T9 / کم تا متوسط | `assets/js/bibsearch.js:59-64` | setTimeout(filterItems(searchTerm),300) تابع را فوراً اجرا می‌کند؛ debounce شرح‌داده‌شده انجام نمی‌شود | ارسال callback و آزمون typing |
| T10 / متوسط برای cache | `_plugins/cache-bust.rb:22-24,45-46` و `_includes/head.liquid:68` | hash مسیر assets/_sass را می‌گیرد ولی Sass واقعی در _sass است؛ query bust با تغییر Sass لزوماً تغییر نمی‌کند | hash ورودی درست/خروجی CSS و بررسی رفتار cache |
| T11 / کم، تصمیم مالک | `_config.yml:10-16,84,115-116,165,308-309` | description/keywords/contact note و بعضی شناسه‌ها نمونه مانده‌اند؛ note واقعاً در خانه چاپ می‌شود؛ مالکیت Inspire HEP معلوم نیست | شخصی‌سازی و تأیید شناسه‌ها؛ Disqus فقط با flag پست اثر دارد |
| T12 / کم تا متوسط برای اعتبار سنجش | `.github/workflows/lighthouse-badger.yml:17-19` و `README.md:198-210` | هدف سنجش و badgeها upstream است، پس شاهد عملکرد سایت فعلی نیست | هدف درست و نیاز token بررسی شود |
| T13 / متوسط برای توسعه | `docker-compose.yml:9-10` و `bin/entry_point.sh:5,18` | bind mount و حذف lock می‌تواند Gemfile.lock tracked را حذف کند | اصلاح روال در مرحلهٔ مجاز یا اجرای صرفاً در کپی آزمایشی |
| T14 / کم، بازبینی محتوا | `_pages/profiles.md:211`، `_pages/Haghighi.md:1`، `_pages/cv.md:8` و `_posts/2024-09-25-IPS_Team.md:21,24` | نویسهٔ اضافی ]، نام Mehdi Salkhordeh در فایل Haghighi، عبارت 1994 of experience و placeholder تیم؛ نام شخص لزوماً خطا نیست | اصلاح نویسه/متن و تأیید انسانی هویت |
| T15 / کم، WebP تأیید نشده | `_includes/figure.liquid:1,14-24` فقط پسوند lowercase را remove می‌کند؛ `_pages/profiles.md:13` دارای Kabir.JPG است | srcset برای uppercase شامل .JPG در ریشهٔ نام می‌شود؛ انطباق با خروجی ImageMagick نامعلوم | بررسی نام فایل WebP تولیدی؛ وجود .JPG اثبات شکستن img اصلی نیست |

دوازده کلید تکراری؛ شماره‌ها خط خود سرآغاز @ در `_bibliography/papers.bib` هستند:

| کلید | خطوط |
|---|---|
| `gholinejad2017improving` | 30, 270 |
| `nayyeri2017universal` | 44, 434 |
| `ghafarian2021prepare` | 92, 597 |
| `zamiri2014temporal` | 106, 672 |
| `khalkhali2021situation` | 120, 696 |
| `farahi2020probabilistic` | 134, 882 |
| `khalkhali2019multi` | 147, 905 |
| `azimpour2021hyperspectral` | 161, 917 |
| `izanloo2016kalman` | 191, 1639 |
| `ashkezari2019incorporating` | 545, 1574 |
| `ashkezari2019emotional` | 783, 1521 |
| `ashkezari2019robust` | 828, 1548 |

عنوان تکراری زیر کلیدهای متفاوت هم هست: s00034-018-0792-x/majidi2018kalman و monsefi2019sparse/zabihzadeh2019sparse نمونه‌اند. تطابق عنوان نرمال‌شده مجوز حذف نیست؛ DOI، نویسنده و نسخه باید مقایسه شوند. author رکورد اول (`papers.bib:6`) به‌جای جداکنندهٔ and با کاما چند نویسنده را آورده؛ parse/display آن نیاز به بازبینی دارد.

در تطبیق ۸۷ مسیر یکتای تصویر از چهار پوشهٔ محتوا، با احتساب نام واقعی مورد T4، فایل اصلی کاملاً غایب دیگری یافت نشد. اعتبار WebP، anchorهای HTML نهایی، URLهای خارجی و PDF بیرونی آزموده نشده‌اند. permalink صریح تکراری در صفحات اصلی پیدا نشد؛ این بررسی محدود، broken-link check کامل نیست.

## ۱۰. کارهای آینده به ترتیب اولویت — Prioritized future work

۱. **must-fix** — `_bibliography/papers.bib`: حل ۱۲ کلید تکراری و مرور author/titleها با حفظ metadata؛ اولویت صحت آثار علمی است.

۲. **must-fix** — `_data/repositories.yml:1-8`: schema سازگار owner/repo و حذف عضو خالی؛ href و آمار از دادهٔ فعلی نادرست ساخته می‌شوند.

۳. **must-fix** — `_projects/2_project.md:57`: تطبیق case با assets/img/WiFiLocation.JPG برای میزبانی حساس به حروف.

۴. **verify** — `_config.yml:634-649` و `_layouts/cv.liquid`: تصمیم منبع CV و build آزمایشی برای JSON غایب و fallback.

۵. **verify** — `.github/workflows/deploy.yml:60-95` و GitHub Pages settings: source، branch خروجی، مجوز نوشتن و آخرین اجرای موفق؛ فایل محلی وضعیت سرور را نشان نمی‌دهد.

۶. **verify** — `_config.yml:171-177`، `_plugins/external-posts.rb` و `_plugins/google-scholar-citations.rb:31-35`: مطلوب‌بودن منابع نمونه، شمار پست تولیدی و وابستگی/زمان شبکهٔ build.

۷. **nice-to-have** — `_includes/scripts/search.liquid:48-54` و `assets/js/bibsearch.js:64`: مقصد فرزندان dropdown در search و debounce.

۸. **nice-to-have** — `_config.yml:10-16,84,115-116`، `_data/coauthors.yml` و `_data/venues.yml`: جایگزینی metadata نمونه و همسان‌سازی نام با BibTeX.

۹. **verify** — `_plugins/cache-bust.rb:45-46`، `_includes/figure.liquid:1-24` و assets/img: cache CSS و نام WebPهای uppercase در خروجی.

۱۰. **verify** — `bin/entry_point.sh` و workflowهای Docker/Lighthouse: حذف lock در preview، target upstream و ضرورت secrets پیش از انتخاب روال توسعه.

۱۱. **nice-to-have** — `_pages/profiles.md:211`، `_pages/Haghighi.md`، `_pages/cv.md` و `_posts/2024-09-25-IPS_Team.md`: تصحیح/تأیید موارد متنی مشخص و سپس مرور انسانی مطالب تاریخ‌دار ۲۰۲۴.

۱۲. **verify** — `_config.yml:229-253`: تصمیم دربارهٔ انتشار فایل دستور ممیزی و همین گزارش؛ فعلاً در exclude نیستند و Markdown ریشهٔ بدون front matter ممکن است static کپی شود. در این فاز exclude تغییر نکرد.

هیچ‌یک پیاده‌سازی نشده‌اند؛ must-fix اولویت اصلاح آینده است و به معنی خرابی قطعی سایت زنده نیست.

## ۱۱. پرسش‌های باز برای صاحب سایت — Open questions

- آیا هر دو حساب h-sadoghi و LaboratoryPatternRecognition تحت اختیار/مجوز شما هستند و تغییر درس‌ها باید در مخزن جدا انجام شود؟
- Pages واقعاً روی gh-pages و ریشه تنظیم است؟ آخرین اجرای موفق Deploy site کدام است؟ دامنهٔ اختصاصی دارید؟
- blog فقط محتوای آزمایشگاه را نشان دهد یا Medium/Google Blog نمونه عمداً مطلوب‌اند؟
- Inspire HEP متعلق به شماست و نمایش آن مطلوب است؟ Scholar فعلی شناسهٔ اصلی است؟
- منبع مطلوب CV، YAML است یا JSON و PDF تأییدشده‌ای باید اضافه شود؟
- Giscus باید فعال باشد و دسترسی نصب برنامه، Discussions و شناسهٔ repo/category فراهم است؟
- اطلاعات/عکس افراد با تأییدشان منتشر می‌شود؟ هویت Haghighi/Salkhordeh و placeholderهای تیم چه باید باشد؟
- هدف طراحی بعدی چیست: حفظ سبک فعلی، تمرکز بر شخص یا آزمایشگاه؟ سایت انگلیسی بماند یا نسخهٔ فارسی هم لازم است؟
- خبرها/پروژه‌های ۲۰۲۴ آرشیوی بمانند یا وضعیت تازه دارید؟ fun و submenus باید باقی بمانند؟

## ۱۲. نمایهٔ شواهد و راستی‌آزمایی — Evidence index

### شمارش‌های محاسبه‌شده

| مورد | شمارش و تعریف |
|---|---|
| فایل‌های شروع | ۴۷۰ فایل خارج .git؛ شامل tracked/untracked/ignored موجود؛ شمارش بازگشتی واقعی |
| _pages | ۵۱ فایل؛ ۱۱ front matter، ۳۹ fragment مصرف people و یک نمونهٔ exclude |
| پست/پروژه/خبر | ۵/۶/۳ فایل محلی؛ شمار site.posts نهایی به‌علت external_sources نامعلوم |
| افراد | ۳۹ عضو آرایه؛ تمام متن‌ها/تصویرها با نام دقیق موجود |
| داده/قالب/افزونه/workflow | ۴ فایل YAML، ۱۲ layout، ۹ plugin Ruby و ۱۱ workflow |
| تصویر | ۱۲۹ فایل زیر assets/img، ۱۴ زیر readme_preview و ۲ SVG سایر مسیرها؛ مجموع ۱۴۵ |
| پسوند تصویر | در کل ۱۰۸ jpg با تجمیع case، ۳۵ png و ۲ svg؛ تعداد فایل است، نه عکس یکتا |
| PDF | ۱ فایل assets/pdf/example_pdf.pdf؛ محتوای PDF و render آن موضوع این ممیزی نبود |
| ارجاع تصویر در محتوا | ۹۴ رخداد، ۸۷ مسیر یکتا؛ ۸۶ تطبیق دقیق و ۱ اختلاف case؛ اصل همه با نام صحیح موجود |
| تصویر خارج این ارجاع‌ها | ۴۲ فایل از ۱۲۹؛ نبود در ارجاع مستقیم این چهار پوشه، حکم unused بودن در تمام قالب/JS/اسناد نیست |
| بانک اصلی BibTeX | ۱۲۳ رکورد، ۱۱۱ کلید یکتا، ۱۲ کلید دوبار تکراری، ۱۹ selected=true خام |

هیچ عدد جدول تخمینی نیست؛ تعداد HTML منتشرشده، آثار علمی یکتا، پست بیرونی و WebP تولیدی محاسبه نشده و از اعداد بالا استنتاج نمی‌شود. تعریف فایل تصویری شامل jpg/jpeg/png/gif/webp/svg/tif/tiff/ico/avif بود؛ در موجودی فقط jpg/png/svg یافت شد.

### شواهد اصلی

| موضوع | مسیر و خطوط |
|---|---|
| هویت و URL | `_config.yml:5-24` |
| قابلیت و ابعاد | `_config.yml:50-57,399-422,428-441` |
| collections/news | `_config.yml:192-210` |
| پردازش و exclude/plugin | `_config.yml:217-282` |
| archive و فیلترها | `_config.yml:297-309`، `_pages/blog.md:32-57` |
| Scholar و CV config | `_config.yml:315-380,634-649` |
| خانه و تصویر | `_pages/about.md:1-19`، `_layouts/about.liquid:7-73` |
| ناوبری | `_includes/header.liquid:45-110` و جدول تمام صفحات در بخش ۴ |
| درج افراد | `_pages/profiles.md:9-323`، `_layouts/profiles.liquid:7-33` |
| CV | `_layouts/cv.liquid:4-50,72-113`، `_data/cv.yml:1-61` |
| مقاله | `_layouts/bib.liquid:28-48,60-99,185-248,308-329` |
| تصویر | `_includes/figure.liquid:1-78` |
| responsive/theme | `_sass/_base.scss:193-228`، `_sass/_themes.scss:5-21,70-85`، `assets/js/theme.js:208-252` |
| deploy | `.github/workflows/deploy.yml:3-95`، `INSTALL.md:24-31,118-126` |
| ابزار محلی | `bin/entry_point.sh:5,18`، `bin/deploy:77-115`، `purgecss.config.js:1-6` |
| robots/sitemap | `robots.txt:1-7`؛ Disallow خالی و sitemap از url/baseurl |
| مستندات upstream | `README.md:1-7,272-306,467-471`، `CUSTOMIZE.md:3,45-83`، `INSTALL.md:185-231` |

### ثبت ارجاع‌های رسانه با خط منبع

پروفایل‌ها در جدول ۳۹ ردیفی بخش ۵ آمده‌اند. دیگر تصویرها در جدول زیر با case عین منبع ثبت شده‌اند؛ تطبیق نام اصل فایل است، نه render یا crawl سایت.

| منبع | مسیر و خط |
|---|---|
| `_pages/about.md` | `assets/img/Sadoghi.jpg` (:9) |
| `_pages/projects.md` | `assets/img/PR_Goup.jpg` (:68) |
| `_pages/teaching.md` | `assets/img/PR1.JPG` (:11)؛ `assets/img/machinelearning1.JPG` (:12)؛ `assets/img/startPic.jpg` (:13)؛ `assets/img/ECG_1.jpg` (:14)؛ `assets/img/DeepDream2.JPG` (:15) |
| `_posts/2024-09-24-AliForouzan.md` | `assets/img/Blog/Breakout_ElasticMatch_Hisory.jpg` (:8)؛ `assets/img/Blog/Breakout.jpg` (:24) |
| `_posts/2024-09-25-IPS_Team.md` | `assets/img/Blog/IPS_1.jpg` (:8)؛ `assets/img/Blog/IPS_3d.jpg` (:37) |
| `_posts/2024-09-25-Jaafari.md` | `assets/img/Blog/VP.jpg` (:10) |
| `_posts/2024-09-26-Naji.md` | `assets/img/Blog/imagesNaji/ips_applications.jpg` (:8)؛ `assets/img/Blog/imagesNaji/ips_applications.jpg` (:28) |
| `_posts/2024-10-02-LossConcept.md` | `assets/img/Blog/LossBlog/MainPage_Loss.JPG` (:8)؛ `assets/img/Blog/LossBlog/LossinLife.jpg` (:19)؛ `assets/img/Blog/LossBlog/PerspectiveofLoss.jpg` (:30)؛ `assets/img/Blog/LossBlog/LossTypes_1.jpg` (:53)؛ `assets/img/Blog/LossBlog/Robust_lncos.jpg` (:64)؛ `assets/img/Blog/LossBlog/Equality_Equity1.jpg` (:75)؛ `assets/img/Blog/LossBlog/PinBall_Loss.jpg` (:84)؛ `assets/img/Blog/LossBlog/Inequality_Equality_Justice_Equity.jpg` (:96) |
| `_projects/1_project.md` | `assets/img/InteractiveWall_1.jpg` (:5)؛ `assets/img/catalog-front-new.jpg` (:69)؛ `assets/img/catalog-back-new.jpg` (:72)؛ `assets/img/InteractiveWall_1.jpg` (:80)؛ `assets/img/InteractiveWall_2.jpg` (:94)؛ `assets/img/InteractiveWall_3.jpg` (:97)؛ `assets/img/InteractiveWall_4.jpg` (:109)؛ `assets/img/InteractiveWall_5.jpg` (:112) |
| `_projects/2_project.md` | `assets/img/EngineeringFUM.jpg` (:5)؛ `assets/img/EngineeringFUM.jpg` (:28)؛ `assets/img/FingerprintingDiagram.jpg` (:31)؛ `assets/img/mobileapppic2.jpg` (:34)؛ `assets/img/ENG_FUM.jpg` (:43)؛ `assets/img/ComputerGroup_ENG_FUM.jpg` (:54)؛ `assets/img/WiFiLocation.jpg` (:57) |
| `_projects/3_project.md` | `assets/img/SmartMirrorMain.jpg` (:5)؛ `assets/img/SmartMirrorMain.jpg` (:32)؛ `assets/img/SmartMirror_1.jpg` (:35)؛ `assets/img/SmartMirrorColor.jpg` (:38)؛ `assets/img/SmartMirror_Construction.jpg` (:46)؛ `assets/img/SmartMirror_Hotel.jpg` (:60)؛ `assets/img/SmartMirror_DaroSsalam.jpg` (:69) |
| `_projects/4_project.md` | `assets/img/AirPolution_Sensor.jpg` (:5)؛ `assets/img/AirPolution_Sensor.jpg` (:39)؛ `assets/img/SavingRealTimeSensor.jpg` (:42)؛ `assets/img/ApplyingTocity.jpg` (:45) |
| `_projects/5_project.md` | `assets/img/Violate_BusDriver.jpg` (:5)؛ `assets/img/Embede_AI_Camera.jpg` (:15)؛ `assets/img/ReportPanel.jpg` (:18)؛ `assets/img/ReportPanel_1.jpg` (:21)؛ `assets/img/Violate_BusDriver.jpg` (:29) |
| `_projects/6_project.md` | `assets/img/EmotionalDetector.jpg` (:5)؛ `assets/img/EmotionalDetector.jpg` (:30) |

### دامنهٔ بررسی و کنترل فایل‌ها

هر ۶۵ فایل محتوایی در _pages/_posts/_projects/_news خوانده شد؛ front matter، title، date، heading، لینک و رسانه بررسی شد. فایل‌های ریشهٔ الزامی، هر چهار YAML داده، ساختار تمام رکوردهای BibTeX، همهٔ workflowها و هر ۹ plugin خوانده شد. layout/includeهای مرتبط با خانه، افراد، CV، مقاله، منو، metadata، خبر و پروژه و entryهای Sass/JS بررسی شدند. ادعای ممیزی خط‌به‌خط همهٔ کتابخانه‌های minified یا باینری‌ها وجود ندارد.

گزارش به‌صورت فایل جدید ساخته شد. پیش از ایجاد، شمار ۴۷۰ فایل و hash همهٔ فایل‌های موجود دوباره با snapshot آغاز مقایسه شد. روش: مسیر نسبی UTF-8 و SHA-256 بایت‌های هر فایل، به‌ترتیب مرتب‌شده، وارد یک SHA-256 تجمیعی شد. baseline:

```text
470 files, excluding .git
b9708906210ba575b9b0068087566b4d58ac93166bb05edcc66f931498bac1ac
```

کنترل نهایی Git و hash پس از تکمیل گزارش انجام شد: شمار و hash هر ۴۷۰ فایل قبلی دقیقاً با آغاز برابر بود و تنها فایل افزوده‌شده همین گزارش است. پوشش هر ۶۵ فایل محتوا و محدودهٔ خطِ ۲۲۱ ارجاع صریح گزارش نیز کنترل شد. وضعیت نهایی:

```text
?? CODEX_H_SADOGHI_SITE_DISCOVERY.md
?? SITE_ARCHITECTURE_REPORT.md
```

هیچ فایل موجود یا تنظیم Git تغییر نکرده است. build/deploy، تست خودکار، crawl آنلاین و broken-link check کامل انجام نشد. بررسی محدود وجود فایل و case محلی انجام شد. مانع محلی برای گزارش نبود؛ خروجی build و وضعیت حساب/میزبانی محدودیت‌های تأیید باقی‌مانده‌اند.

## ۱۳. خلاصهٔ تحویل به ChatGPT — Hand-off summary for ChatGPT

این بخش برای همکاری بدون سابقهٔ گفتگو نوشته شده است. مخزن h-sadoghi/h-sadoghi.github.io در `J:\SadoghiSite2\h-sadoghi.github.io`، branch master و HEAD 8512a9f بررسی شد. در ابتدا فقط CODEX_H_SADOGHI_SITE_DISCOVERY.md به‌صورت untracked بود. مرحلهٔ اول فقط شناخت و گزارش بود؛ تنها فایل ساخته‌شده SITE_ARCHITECTURE_REPORT.md است. هیچ source، dependency، workflow یا asset اصلاح نشده؛ build، نصب، اینترنت، commit و push اجرا نشده است. این گزارش تأیید سلامت یا خرابی deployment زنده نیست.

سایت یک مشتق شخصی‌سازی‌شدهٔ al-folio مبتنی بر Jekyll است؛ وضعیت رسمی fork و نسخهٔ قالب معلوم نیست. هویت Hadi Sadoghi Yazdi و Pattern Recognition Laboratory است. _config.yml تنظیم نام، URL، featureها، collectionها و Scholar را دارد. title:blank در قالب sentinel نمایش نام کامل است. URL فعلی https://h-sadoghi.github.io/ و baseurl خالی است؛ دامنهٔ اختصاصی از checkout اثبات نمی‌شود. منبع HTML، Markdown/YAML/BibTeX و قالب Liquid است؛ JS رفتار مرورگر را می‌دهد و سامانهٔ backend توصیف‌شده در پروژه‌ها جزو runtime این سایت نیست.

خانه از _pages/about.md با permalink:/ و layout:about آغاز می‌شود؛ تصویر واقعی assets/img/Sadoghi.jpg است. _layouts/about.liquid متن، خبر، سه نوشتهٔ اخیر، منتخب‌های علمی و social را می‌چیند؛ default قالب مشترک head/header/footer/scriptهاست. برای bio/تصویر اول Markdown و config را تغییر دهید؛ برای رنگ _sass/_themes.scss و _variables.scss، برای فواصل و اجزا _base.scss و _layout.scss. assets/css/main.scss ورودی Sass است. header/footer ثابت و theme روشن/تاریک/system فعال‌اند؛ عرض profile در موبایل تمام و در عرض بیشتر ۳۰٪ است.

۵۱ فایل _pages برابر ۵۱ صفحهٔ مستقل HTML نیست: ۱۱ فایل front matter دارند، ۳۹ زندگی‌نامه با آرایهٔ profiles در /people/ درج می‌شوند و about_einstein.md نمونهٔ exclude است. افزودن عضو نیازمند entry در _pages/profiles.md، متن بدون front matter و تصویر با نام دقیق است؛ front matter دادن به fragment رفتار انتشار را تغییر می‌دهد. منو از nav/nav_order ساخته می‌شود، about اختصاصی است و submenus از dropdown.md می‌آید. فایل بدون permalink را با مسیر منتشرشده یکی نگیرید. /news/ عمومی اما خارج navbar است.

مقالات از _bibliography/papers.bib و Jekyll Scholar می‌آیند؛ publications.md ورودی و _layouts/bib.liquid قالب هر مدخل است. ۱۲۳ رکورد خام، ۱۱۱ کلید یکتا، ۱۲ کلید تکراری و ۱۹ selected=true خام وجود دارد. قبل از حذف/افزودن علمی، duplicateها را با DOI، نویسنده و metadata مقایسه و selected را حفظ کنید. شمار رکورد، شمار اثر علمی یکتا نیست. شکل‌های نام صاحب سایت یکدست نیست و coauthors/venues هنوز مثال‌های قالب‌اند. PDF نسبی در assets/pdf/ قرار می‌گیرد؛ PDF غیرخالی در بانک اصلی و PDF اختصاصی CV معرفی نشده. فیلتر bibliography از bibsearch.js و hash URL استفاده می‌کند و با جست‌وجوی عمومی Ninja Keys متفاوت است.

پنج پست محلی، شش پروژه و سه خبر وجود دارد. همهٔ پروژه‌ها category:work دارند ولی landing دستهٔ fun خالی را هم می‌سازد. خبر inline هنوز collection با output:true است. teaching پنج پیوند به سایت مستقل LaboratoryPatternRecognition دارد؛ تغییر فهرست، محتوای بیرونی درس را عوض نمی‌کند. CV شخصی در _data/cv.yml است؛ قالب JSON را مقدم می‌داند ولی resume.json غایب و config هنوز درخواستش می‌کند، پس رفتار build آزمایشی باید روشن شود. repositories.yml schema نامناسب URL سازمان و یک user خالی دارد که باید برای کارت‌های owner/repo اصلاح شود.

موارد مشخص برای تغییر آینده: کلیدهای تکراری BibTeX، case ارجاع WiFiLocation.jpg در _projects/2_project.md:57 نسبت به WiFiLocation.JPG، درخواست JSON غایب CV، child.url در search dropdown، debounce فوری bibsearch و hash مسیر اشتباه assets/_sass در cache-bust. تصویرهای اصلی خانه، درس‌ها و هر ۳۹ عضو موجودند؛ WebP خروجی باید جدا بررسی شود. placeholder تیم، متن CV، نویسهٔ اضافی people و هویت Haghighi/Salkhordeh تأیید انسانی می‌خواهند. هیچ‌یک در این ممیزی اصلاح نشده‌اند.

انتشار در .github/workflows/deploy.yml تعریف شده: push/PR master/main با فیلتر، Ruby 3.2.2، نصب Jupyter، Jekyll build، PurgeCSS و انتشار _site با JamesIves برای غیرPR. branch مقصد صریح نیست؛ اسناد و workflow دیگر انتظار gh-pages دارند. Pages settings و آخرین موفقیت باید از مالک/لاگ بررسی شود. gh-pages خروجی است و محل ویرایش محتوا نیست. برای preview کپی آزمایشی ترجیح دارد: bin/entry_point.sh با bind mount، Gemfile.lock tracked را حذف می‌کند؛ bin/deploy حذف فایل/شاخه و force-push دارد و برای preview مناسب نیست. external_sources هنوز Medium/Google Blog نمونه را دریافت می‌کند و Scholar tag هم شبکه می‌زند؛ شرایط بیرونی روی تعداد پست و نتیجه/زمان build اثر دارد.

همکار بعدی ابتدا هدف دقیق طراحی، اختیار حساب/دامنه و منبع مطلوب CV/محتوای بیرونی را روشن کند؛ وضعیت Git را حفظ و تغییر را از نزدیک‌ترین ورودی داده‌ای انجام دهد. قالب مشترک را فقط در صورت نیاز ساختاری دست بزند. در محیط مجاز آزمایشی، build و خروجی مسیرهای اصلی، عکس‌ها، CV، bibliography و حالت موبایل/تاریک را بررسی و نتیجهٔ واقعی را ثبت کند. فایل‌های حیاتی آغاز: _config.yml، _pages/about.md، _pages/profiles.md، _bibliography/papers.bib، _data/cv.yml، _data/repositories.yml، _layouts/about.liquid، _layouts/profiles.liquid، _layouts/bib.liquid، _includes/header.liquid، _includes/figure.liquid، assets/css/main.scss و .github/workflows/deploy.yml. از ادغام، پاک‌سازی گسترده، تغییر dependency یا انتشار پیش از روشن‌شدن خواستهٔ مرحلهٔ بعد خودداری شود.

# Codex — مرحله ۲: پشتیبان‌گیری و آزمون ساخت/انتشار از طریق GitHub Actions

## هدف
روی **همان مخزن موجود** کار کن: `J:\SadoghiSite2\h-sadoghi.github.io`، با remote مورد انتظار `https://github.com/h-sadoghi/h-sadoghi.github.io.git`، شاخه محلی `master` و HEAD گزارش‌شده `8512a9f7c870de98749a38b14c7a93ce6e6cea90`.

مالک می‌خواهد از **GitHub Actions موجود** برای Build سایت Jekyll استفاده کنیم و سراغ Docker، نصب Ruby روی ویندوز یا اجرای Jekyll محلی نرویم. هدف این مرحله بررسی *baseline فعلی* است؛ **هیچ اصلاحی در کد، داده، قالب و workflow انجام نده**. اگر نسخه فعلی روی GitHub با نسخه محلی یکسان است، برای اجرای workflow هیچ commit/Push ساختگی لازم نیست.

## محدودیت‌های الزامی
- ابتدا وضعیت Git را بخوان. تغییرات staged/unstaged یا فایل‌های untracked موجود را حذف، reset، clean، amend یا overwrite نکن؛ وضعیت اولیه و نهایی ثبت شود.
- فایل‌های اصلی، `.github/workflows/deploy.yml`، `_config.yml`، `Gemfile.lock`، BibTeX و فایل‌های سایت را تغییر نده.
- بدون پشتیبان معتبر از **شاخه منبع** و **شاخه خروجی منتشرشده (اگر وجود دارد)** هیچ deploy/dispatch اجرا نکن.
- هیچ `git push --force`، حذف branch، `bin/deploy`، `docker compose`، اصلاح اتوماتیک، build محلی، تغییر Pages settings یا ایجاد/Push tag انجام نده.
- هر خطای احراز هویت/مجوز/شبکه یا اختلاف شاخه‌ها را صادقانه گزارش کن؛ رمز/توکن را در گزارش ننویس. هیچ موفقیتی را حدس نزن.

## فاز A — شناسایی و نسخه پشتیبان (اجباری، بدون انتشار)
1. در مخزن اصلی `git status --porcelain=v1 --untracked-files=all`، نام شاخه، `git rev-parse HEAD` و `git remote -v` را بررسی و ثبت کن. مطمئن شو remote واقعاً مخزن فوق است.
2. با عملیات فقط‌خواندنی سمت سرور (`git ls-remote` و در صورت دسترسی `git fetch origin` بدون checkout/merge/reset) SHA شاخه‌های `master` و `gh-pages` و در صورت لزوم `main` را بررسی کن. اگر SHA منبع محلی و remote/master متفاوت‌اند، **برای انتشار توقف کن**؛ نه pull خودکار، نه push.
3. پوشه پشتیبان مستقلی بیرون مخزن ایجاد کن، مثلاً `J:\SadoghiSite2\h-sadoghi_site_backups\baseline_2026-10-17` (در صورت وجود، نام غیرتکراری بگذار). این پوشه باید مستقل از sandbox قبلی باشد.
4. از working tree فعلی، شامل untrackedهای موجود، یک ZIP مستقل با حفظ مسیرهای نسبی بگیر؛ `.git` را از ZIP فایل‌های کاری خارج کن. فایل‌های باینری/تصاویر/پروژه‌ها/مقالات را از قلم نینداز. SHA-256 و فهرست تعداد فایل‌ها را ثبت کن. هیچ فایل را در مخزن تغییر نده.
5. از تاریخچه/refs موجود Git، **پس از fetch موفق** یک `git bundle` معتبر (`git bundle create ... --all`) بساز و با `git bundle verify` کنترل کن؛ hash و وضعیت verify ثبت شود.
6. اگر `origin/gh-pages` موجود است، از همان commit دقیق، آرشیو جداگانهٔ خروجی سایت بساز (`git archive --format=zip ... origin/gh-pages`) و SHA آن را ثبت کن. اگر وجود ندارد صریحاً بنویس `NO_GH_PAGES_BRANCH`؛ هرگز branch جدید نساز. این branch را با شاخه سورس قاطی نکن.
7. `README_RESTORE.md` کنار backup بنویس که مسیر هر آرشیو، commit source، commit output و روش قابل‌فهم بازیابی محلی را ثبت کند. **بازیابی واقعی یا push برگشتی نکن**.

## فاز B — کنترل انتشار GitHub (پیش از اجرای آن)
1. بررسی کن `.github/workflows/deploy.yml` در نسخه remote/master وجود دارد و trigger `workflow_dispatch` را می‌پذیرد؛ اجرای آن برای event دستی ممکن است شاخه انتشار را تغییر دهد.
2. اگر دسترسی خواندن تنظیمات GitHub Pages داری، روش انتشار، branch/path فعلی و آخرین workflow موفق را ثبت کن. اگر دسترسی نداری با `UNKNOWN` علامت بزن و انتشار را تا تأیید روش مقصد Pages متوقف کن؛ به‌هیچ‌وجه تنظیمات را حدس نزن.
3. بررسی کن که آرشیو خروجی قبلی با روش انتشار واقعی سازگار است: اگر Pages از `gh-pages` می‌خواند، پشتیبان `gh-pages` الزامی است؛ اگر مسیر دیگری دارد، همان مقصد واقعی هم باید ذخیره شود.
4. اگر source remote و local متفاوت‌اند، backup ناقص/verify ناموفق است، مقصد انتشار معلوم نیست یا تنظیمات غیرمنتظره‌اند، **همین‌جا توقف کن و تنها گزارش بده**.

## فاز C — اجرای baseline روی GitHub، فقط اگر همه کنترل‌ها موفق بود
1. `deploy.yml` را **فقط یک‌بار** روی `master` از طریق GitHub Actions → Run workflow یا GitHub CLI احرازشده (`gh workflow run deploy.yml --ref master`) اجرا کن. این عمل ممکن است سایت زنده را دوباره منتشر کند و مالک آن را برای آزمون baseline خواسته است.
2. برای تحریک workflow، commit خالی/مصنوعی یا دستکاری README و Push انجام نده. در baseline بدون تغییر منبع، `workflow_dispatch` جایگزین درست Push است. اگر CLI آماده نیست، از کاربر بخواه در GitHub UI گزینه Run workflow را بزند و گزارش اجرای آن را بدهد؛ در این حالت وضعیت را `AWAITING_USER_DISPATCH` بنویس.
3. شماره run، URL آن، نتیجهٔ jobها و لاگ کامل یا لینک گزارش build/deploy را ذخیره کن. اگر شکست خورد، خطاهای Jekyll/Bundler/Scholar/PurgeCSS/Permissions را با متن شاهد از هم جدا کن؛ **هیچ کدی برای سبزشدن Build تغییر نده**.
4. در صورت موفقیت workflow، تنظیمات واقعی Pages را دوباره تأیید کن و URLهای `/`، `/publications/`، `/projects/`، `/people/`، `/cv/`، `/teaching/` و `/repositories/` را از سایت زنده درخواست بده. فقط HTTP status کافی نیست؛ وجود محتوای اصلی و نبود خطای ظاهری را تا حدی که ابزارهای موجود اجازه می‌دهند بررسی کن. نتیجه‌های غیرقابل‌بررسی را `NOT_VERIFIED` ثبت کن؛ cache/انتشار با تأخیر را از شکست قطعی تفکیک کن.
5. اگر workflow سبز بود ولی Pages هنوز نسخه قبلی را نشان می‌دهد، تأخیر انتشار و source branch را بررسی کن؛ بدون شاهد موفقیت نمایش نهایی را ادعا نکن.

## خروجی مورد انتظار
خارج از مخزن اصلی، در پوشه‌ای هم‌سطح backup، فایل‌های زیر را تولید کن:
- `PHASE02_GITHUB_BASELINE_REPORT.md`: وضعیت پشتیبان، شاخه‌ها و SHAها، وضعیت Pages، Run ID/URL و موفقیت یا شکست Build و صفحه‌ها.
- `git_before.txt` و `git_after.txt`: status/branch/HEAD؛ تغییرات موجود قبلی باید حفظ شده باشند.
- `backup_manifest.txt`: نام آرشیوها، SHA-256، اندازه و محل ذخیره، همراه وضعیت bundle verify و README_RESTORE.
- `github_actions_build.log` (اگر قابل دریافت بود) و `website_checks.csv` (تنها برای آزمون‌های اجراشده).
- `PHASE02_HANDOFF.zip` شامل **گزارش‌ها و لاگ‌ها**، نه لزوماً خود آرشیوهای حجیم backup؛ آن‌ها را روی هارد نگه دار.

در پایان به زبان فارسی مختصر گزارش بده: `BACKUP_OK` یا دلیل شکست؛ `DISPATCH_SUCCESS / BUILD_FAILED / AWAITING_USER_DISPATCH / BLOCKED_PREFLIGHT`؛ URL اجرای GitHub در صورت وجود؛ و مسیر دقیق `PHASE02_HANDOFF.zip`. سپس متوقف شو و منتظر تحلیل مرحله بعد بمان.

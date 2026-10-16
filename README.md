# پازل رنگ‌آمیزی ۴۸۰ تکه — نسخهٔ اندروید

این پوشه یک پروژهٔ کامل اندروید است که همان `index.html` بازی را داخل یک **WebView آفلاین** اجرا می‌کند:

- **هیچ مجوزی (permission) نمی‌خواهد** و به اینترنت وصل نمی‌شود (حتی مجوز `INTERNET` هم در manifest نیست).
- گالری/فایل‌منیجر گوشی برای انتخاب عکس کار می‌کند (`onShowFileChooser` در `MainActivity`).
- با دو انگشت می‌توانی زوم کنی (برای موزاییک ۴۸۰ تکه لازم است).
- کمترین نسخهٔ اندروید: **7.0 (API 24)**.

---

## راه ۱ — ساخت ابری با GitHub (پیشنهادی، بدون نصب هیچ چیز)

۱. در <https://github.com> یک حساب بساز و یک ریپازیتوری تازه بساز (خصوصی هم می‌شود).
۲. محتویات همین پوشه را در ریپازیتوری آپلود کن: دکمهٔ **Add file → Upload files** و همهٔ فایل‌ها/پوشه‌ها را رها کن.
   ⚠️ پوشهٔ مخفی **`.github`** هم باید آپلود شود (همان چیزی که ساخت APK را انجام می‌دهد). اگر با کشیدن‌ورهاکردن نرفت،
   فایل `.github/workflows/build-apk.yml` را دستی بساز و محتوایش را کپی کن (Add file → Create new file و در نام فایل
   مسیر `.github/workflows/build-apk.yml` را تایپ کن).
۳. به تب **Actions** برو؛ ورک‌فلوی **Build APK** را انتخاب کن و **Run workflow** را بزن.
۴. حدود ۳ تا ۵ دقیقه صبر کن. بعد وارد همان اجرا شو و در پایین صفحه از بخش **Artifacts** فایل
   `coloring-puzzle-apk` را دانلود کن (یک فایل zip است؛ داخلش `coloring-puzzle.apk`).
۵. APK را به گوشی منتقل کن (کابل، تلگرام، Google Drive...) و نصب کن. اندروید برای بار اول اجازهٔ
   «نصب برنامه از منابع نامعلوم» را برای مرورگر یا فایل‌منیجر می‌خواهد؛ بده و نصب کن.

> اگر ورک‌فلو خطا داد، لاگ را برایم بفرست تا درستش کنم.

---

## راه ۲ — ساخت روی کامپیوتر با Android Studio

۱. [Android Studio](https://developer.android.com/studio) را نصب کن (نسخهٔ رایگان، حدود ۱ گیگابایت دانلود).
۲. `File → Open` و همین پوشهٔ `coloring-puzzle-android` را باز کن؛ خودش Gradle wrapper را می‌سازد و
   وابستگی‌ها را دانلود می‌کند (به اینترنت نیاز دارد).
۳. برای تست روی گوشی: `Run → Run 'app'`.
۴. برای گرفتن فایل APK: `Build → Build Bundle(s) / APK(s) → Build APK(s)`؛ خروجی اینجا ساخته می‌شود:
   `app/build/outputs/apk/debug/app-debug.apk`

---

## راه ۳ — بدون APK، همین حالا

فایل `coloring-puzzle.html` را به گوشی بفرست و با **Chrome** بازش کن؛ کامل کار می‌کند (تمام منطق بازی داخل خود فایل است).
در منوی Chrome گزینهٔ **Add to Home screen** یک آیکن روی صفحهٔ اصلی می‌سازد که مثل یک اپ باز می‌شود.

---

## عوض کردن نام، آیکن و شناسهٔ اپ

| چه چیزی | کجا |
|---|---|
| نام اپ روی گوشی | `app/src/main/res/values/strings.xml` (کلید `app_name`) |
| آیکن | `app/src/main/res/mipmap-*/ic_launcher.png` (اندازه‌ها: ۴۸، ۷۲، ۹۶، ۱۴۴، ۱۹۲) |
| شناسهٔ اپ | `applicationId` و `namespace` در `app/build.gradle` |
| محتوای بازی | `app/src/main/assets/index.html` (همان فایل HTML؛ برای تغییر بازی همین را عوض کن) |
| تعداد تکه‌ها | در بالای اسکریپت همان `index.html`: `var PIECES = 480;` |

---

## انتشار در Google Play (اختیاری)

نسخهٔ `release` فعلاً با کلید debug امضا می‌شود تا نصب‌شدنی باشد. برای انتشار واقعی:

```
keytool -genkey -v -keystore my-release.jks -keyalg RSA -keysize 2048 -validity 10000 -alias mykey
```

بعد در `app/build.gradle` یک `signingConfigs.release` با مقادیر کلید بساز و در `buildTypes.release` از آن استفاده کن،
و `gradle assembleRelease` بگیر. (برای Play به فرمت `.aab` هم می‌شود فایل ساخت: `gradle bundleRelease`.)

---

## ساختار پروژه

```
coloring-puzzle-android/
├─ .github/workflows/build-apk.yml   ← ساخت ابری APK
├─ settings.gradle
├─ build.gradle
├─ gradle.properties
├─ README.md
└─ app/
   ├─ build.gradle
   └─ src/main/
      ├─ AndroidManifest.xml        ← بدون هیچ مجوزی
      ├─ assets/index.html          ← خود بازی
      ├─ java/ir/coloring/puzzle/MainActivity.java
      └─ res/
         ├─ values/{strings,colors,themes}.xml
         └─ mipmap-*/ic_launcher.png
```

# دفتر كاش المحل — نسخة التطبيق (مشروع تخرج)

## الملفات دي إيه؟
- `index.html` — التطبيق كامل: صفحة تسجيل دخول / إنشاء حساب، الرئيسية، عملية جديدة، السجل، الحسابات، نسخة احتياطية.
- `manifest.webmanifest` + `sw.js` + `icon-192.png` + `icon-512.png` — ملفات الـ PWA اللي بتخلي التطبيق يتثبّت على الموبايل ويشتغل أوفلاين.
- `capacitor.config.json` + `package.json` — ملفات Capacitor اللي بتحوّل نفس الموقع لتطبيق أندرويد و iOS أصلي.

## أهم إصلاح: الداتا مش بتتمسح
النسخة الأصلية كانت بتحفظ العمليات في ذاكرة الصفحة بس، فبتضيع مع القفل أو التحديث.
النسخة دي بتحفظ كل حاجة في `localStorage` (مفتاح `daftar_kash_v1`) عند كل عملية،
وتسجيل الدخول محفوظ في `daftar_kash_auth` + `daftar_kash_session`.
جرّب: سجّل عملية، اقفل الصفحة، افتحها تاني — هتلاقيها موجودة.

## 1) التثبيت السريع من المتصفح (من غير برامج) — أندرويد وآيفون
ارفع الملفات على أي استضافة (أو افتح `index.html` من سيرفر محلي)، وبعدين:
- **أندرويد (Chrome):** افتح التطبيق ← قائمة النقاط ⋮ ← «تثبيت التطبيق» / «إضافة إلى الشاشة الرئيسية».
- **آيفون (Safari):** افتح التطبيق ← زر المشاركة ← «إضافة إلى الشاشة الرئيسية».
هيظهر أيقونة «دفتر كاش المحل» على الشاشة ويفتح ملء الشاشة زي التطبيق.

## 2) نسخة APK للأندرويد (ملف يتثبّت)
محتاج: Node.js + Android Studio على كمبيوتر ويندوز/ماك.
```bash
cd dftr-kash-app-package
npm install
npx cap add android
npx cap sync
npx cap open android
```
في Android Studio: Build ← Build App Bundle(s) / APK(s) ← Build APK(s).
هيطلع ملف APK في `android/app/build/outputs/apk/debug/app-debug.apk` — انقله للموبايل وثبّته.
لنسخة نهائية للنشر: Build ← Generate Signed App Bundle / APK.

## 3) نسخة iOS للآيفون
محتاج: جهاز Mac + Xcode + حساب Apple Developer (للتجربة على جهازك تكفي النسخة المجانية).
```bash
cd dftr-kash-app-package
npm install
npx cap add ios
npx cap sync
npx cap open ios
```
في Xcode: اختر جهازك، فعّل «Automatically manage signing»، ودوس Run.
ملف IPA النهائي بيطلع من Product ← Archive ← Distribute App (محتاج حساب مطوّر مدفوع للنشر على App Store، أما التجربة على جهازك فمجانية).

## ملحوظة للمناقشة
- تسجيل الدخول هنا حساب محلي على الجهاز (مناسب للعرض في مشروع التخرج)، مش حساب سيرفر.
- لو عدّلت `index.html` بعد إضافة أندرويد/iOS، نفّذ `npx cap sync` تاني قبل البناء.
- نسخة الويب اللي اتبنت في Muse (قاعدة بيانات سيرفر) هي بديل أسرع للعرض، أما الملفات دي فهي اللي بتطلّع APK و IPA حقيقيين.

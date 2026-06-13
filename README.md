[README.md](https://github.com/user-attachments/files/28914184/README.md)
# تطبيق إدارة الديون (Credit Manager)

تطبيق Flutter لإدارة البيع بالآجل للعملاء في المحلات التجارية، مع الاعتماد على الأوامر الصوتية بالعربية.

## المتطلبات

- Flutter SDK (3.x أو أحدث)
- Android SDK
- جهاز Android أو محاكي (API 23+)

## التشغيل

```bash
flutter pub get
flutter run
```

## بناء APK

```bash
flutter build apk --release
```
الملف الناتج: `build/app/outputs/flutter-apk/app-release.apk`

## ملاحظة بخصوص الخطوط في ملفات PDF

لعرض النص العربي بشكل صحيح في تقارير PDF، أضف ملفات الخط التالية إلى `assets/fonts/`:
- `NotoSansArabic-Regular.ttf`
- `NotoSansArabic-Bold.ttf`

يمكن تحميلها مجانًا من: https://fonts.google.com/noto/specimen/Noto+Sans+Arabic

بدون هذه الملفات سيعمل التطبيق بشكل طبيعي، ولكن قد لا يظهر النص العربي بشكل صحيح داخل ملفات PDF فقط.

## هيكل المشروع

```
lib/
├── main.dart                  # نقطة البداية
├── core/
│   ├── database/              # SQLite (DatabaseHelper)
│   └── theme/                 # الثيمات (فاتح/ليلي)
├── models/                     # Customer, Product, Sale, Payment
├── repositories/               # طبقة الوصول لقاعدة البيانات
├── services/                   # Voice Parser, Speech, PDF, Backup
├── providers/                  # Riverpod providers
├── screens/
│   ├── home/                   # الصفحة الرئيسية + لوحة التحكم
│   ├── customer/                # قائمة العملاء، التفاصيل، النموذج
│   ├── product/                 # قائمة المنتجات والنموذج
│   ├── statement/                # كشف الحساب + PDF
│   └── settings/                 # الإعدادات والنسخ الاحتياطي
└── widgets/                     # عناصر مشتركة + واجهة الأوامر الصوتية
```

## الميزة الرئيسية: الأوامر الصوتية

اضغط على زر الميكروفون في الصفحة الرئيسية وقل مثلاً:

- "أحمد أخذ علبة سجائر"
- "أحمد أخذ علبتين سجائر"
- "محمد أخذ كولا"
- "أحمد دفع 100 جنيه"

يقوم `VoiceCommandParser` (في `lib/services/voice_command_parser.dart`) بتحليل النص المُحوّل من الصوت، واستخراج اسم العميل والمنتج والكمية أو المبلغ، ثم تظهر نافذة تأكيد قبل الحفظ النهائي.

### تحسين دقة التعرف على المنتجات

لكل منتج يمكن إضافة "مسميات أخرى" (aliases) من شاشة المنتجات، مثل: كولا، بيبسي، غازي → لمنتج "مياه غازية". هذا يحسّن من دقة التعرف الصوتي.

## قاعدة البيانات

SQLite عبر `sqflite`، الجداول:
- `customers`
- `products`
- `sales` (تدعم الحذف المؤقت `is_deleted` للاسترجاع)
- `payments` (تدعم الحذف المؤقت أيضًا)
- `settings`

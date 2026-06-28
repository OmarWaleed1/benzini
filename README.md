# ⛽ بنزيني v2.0 — تطبيق تقارير محطات الوقود

تطبيق مجتمعي عراقي يساعد المواطنين على معرفة حالة محطات الوقود قبل التوجه إليها.

---

## 🌟 المميزات الكاملة

### 🔥 البنية التحتية
| الميزة | التقنية |
|--------|---------|
| بيانات مباشرة مشتركة بين الجميع | Firebase Realtime Database |
| تسجيل دخول برقم الهاتف | Firebase Auth (OTP) |
| إشعارات فورية | Firebase Messaging + Local Notifications |

### ⭐ تجربة المستخدم
- **خريطة تفاعلية** + قائمة المحطات
- **تسجيل دخول** برقم الهاتف العراقي
- **إضافة محطات جديدة** مع تحديد الموقع على الخريطة
- **فلترة حسب المدينة** (بغداد، الفلوجة، الرمادي...)
- **تقدير وقت الانتظار** بناءً على الازدحام
- **تنبيه تقرير قديم** (أكثر من ساعتين)
- **اشتراك بمحطة** للحصول على إشعار عند تغيير حالتها
- **مشاركة حالة المحطة** عبر واتساب وغيره
- **نظام نقاط ورتب** لتحفيز المساهمة
- **لوحة المتصدرين** لأكثر المبلّغين دقة

---

## 🚀 خطوات الإعداد

### 1. تثبيت Flutter
```
https://flutter.dev/docs/get-started/install
flutter doctor
```

### 2. إعداد Firebase (مطلوب للبيانات المشتركة)

**أ. أنشئ مشروعاً جديداً:**
- اذهب إلى https://console.firebase.google.com
- أنشئ مشروعاً جديداً باسم "benzini"

**ب. فعّل الخدمات:**
- Authentication → Sign-in method → Phone → Enable
- Realtime Database → Create database → Start in test mode
- Cloud Messaging → تلقائي مع المشروع

**ج. حمّل ملفات الإعداد:**
- أندرويد: `google-services.json` → ضعه في `android/app/`
- iOS: `GoogleService-Info.plist` → ضعه في `ios/Runner/`

**د. أضف FlutterFire CLI:**
```bash
dart pub global activate flutterfire_cli
flutterfire configure
```

**ه. فعّل Firebase في main.dart:**
```dart
// أزل علامة التعليق عن هذا السطر في lib/main.dart:
await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform);
```

### 3. تثبيت الحزم والتشغيل
```bash
flutter pub get
flutter run
```

### 4. بناء APK
```bash
flutter build apk --release
# الملف: build/app/outputs/flutter-apk/app-release.apk
```

---

## 📁 هيكل المشروع

```
lib/
├── main.dart                         # نقطة الدخول
├── theme.dart                        # الألوان والتصميم
├── models/
│   └── station.dart                  # Station + FuelReport + AppUser
├── providers/
│   ├── auth_provider.dart            # حالة المصادقة
│   └── stations_provider.dart        # Firebase streams
├── services/
│   └── firebase_service.dart         # كل عمليات Firebase
├── screens/
│   ├── home_screen.dart              # الرئيسية (خريطة + قائمة)
│   ├── auth_screen.dart              # تسجيل الدخول بالهاتف
│   ├── station_detail_screen.dart    # تفاصيل + اشتراك + مشاركة
│   ├── add_station_screen.dart       # إضافة محطة جديدة
│   └── profile_screen.dart          # الملف + النقاط + المتصدرون
└── widgets/
    ├── station_card.dart             # بطاقة المحطة
    └── report_sheet.dart            # نموذج التقرير
```

---

## 🗄️ هيكل Firebase Database

```json
{
  "stations": {
    "STATION_ID": {
      "id": "...", "name": "محطة الكرادة", "address": "...",
      "city": "بغداد", "lat": 33.31, "lng": 44.36, "addedBy": "USER_ID"
    }
  },
  "reports": {
    "STATION_ID": {
      "REPORT_ID": {
        "fuelType": 0, "crowdLevel": 2, "status": 0,
        "price": 750, "reportedAt": "2024-...", "upvotes": 5
      }
    }
  },
  "users": {
    "USER_ID": {
      "displayName": "أحمد", "phone": "+9647...",
      "points": 150, "reportsCount": 15,
      "subscribedStations": { "STATION_ID": true }
    }
  }
}
```

---

## 🎨 نظام النقاط والرتب

| النقاط | الرتبة |
|--------|--------|
| 0-49   | 🌱 جديد |
| 50-199 | 🥉 نشيط |
| 200-499 | 🥈 محترف |
| 500+  | 🥇 خبير |

| الفعل | النقاط |
|-------|--------|
| إرسال تقرير | +10 |
| تصويت إيجابي | +5 |
| إضافة محطة | +20 |

---

## ⚠️ ملاحظات التطوير

- التطبيق يعمل بدون Firebase لكن البيانات لن تُشارك بين المستخدمين
- قواعد الأمان في Firebase تحتاج ضبط قبل الإطلاق العلني
- OTP يحتاج رقم هاتف حقيقي في بيئة الإنتاج

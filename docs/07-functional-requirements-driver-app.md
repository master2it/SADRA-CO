# 7. نیازمندی‌های عملکردی — اپلیکیشن راننده

[← بازگشت به فهرست](./README.md) | [لجستیک — ۱۹ فرایند](./logistics/01-shipment-loading-processes.md)

---

## 7.1 ویژگی‌های اصلی اپ

| کد | عنوان | توضیح |
|----|-------|-------|
| DRV-01 | ثبت‌نام راننده | موبایل + مدارک + تایید ادمین |
| DRV-02 | پروفایل راننده | نوع کامیون، ظرفیت، کشورهای مجاز |
| DRV-03 | بارهای موجود | لیست بارهای آماده در منطقه فعلی یا مقصد |
| DRV-04 | فیلتر بار | بر اساس: مسیر، نوع بار، وزن، قیمت |
| DRV-05 | بار برگشت (Backhaul) | پس از رسیدن به مقصد، بارهای برگشت پیشنهاد می‌شود |
| DRV-06 | قبول بار | ثبت درخواست + تایید Provider |
| DRV-07 | ارسال موقعیت GPS | هر ۳۰ ثانیه به سرور |
| DRV-08 | ارسال موقعیت در پس‌زمینه | حتی وقتی اپ بسته است |
| DRV-09 | حالت آفلاین | ذخیره موقعیت و ارسال در زمان اتصال |
| DRV-10 | ناوبری داخلی | مسیریابی به مقصد |
| DRV-11 | ثبت رویدادها | ۱۹ فرایند بارگیری (مراحل ۷–۱۹) — [L1](./logistics/01-shipment-loading-processes.md) |
| DRV-12 | آپلود مستندات | عکس بارنامه، رسید گمرک، POD |
| DRV-13 | کیف پول | مشاهده درآمد، درخواست تسویه |
| DRV-14 | چت با Provider | ارتباط درون‌برنامه‌ای |
| DRV-15 | امتیاز و نظرات | دیدن امتیاز خود و مشتری |

---

## 7.2 ویژگی‌های فنی اپ

| ویژگی | توضیح |
|-------|-------|
| **پلتفرم** | Flutter (iOS + Android با یک کد) |
| **Background Location** | WorkManager (Android) + Background Tasks (iOS) |
| **Battery Optimization** | ارسال موقعیت هوشمند (بر اساس حرکت) |
| **Offline Mode** | SQLite محلی + Sync در زمان اتصال |
| **Push Notification** | Firebase Cloud Messaging |
| **Maps** | Google Maps یا Mapbox |

---

## جزئیات پیاده‌سازی

### ساختار پروژه Flutter

```
apps/driver-app/
├── lib/
│   ├── main.dart
│   ├── app.dart
│   ├── core/
│   │   ├── api/              # Dio client + interceptors
│   │   ├── auth/             # Token storage (flutter_secure_storage)
│   │   ├── location/         # Background location service
│   │   ├── storage/          # Isar local DB
│   │   └── theme/
│   ├── features/
│   │   ├── auth/
│   │   │   ├── presentation/ # Screens, Widgets
│   │   │   ├── domain/       # Entities, UseCases
│   │   │   └── data/         # Repositories, API
│   │   ├── loads/            # DRV-03, DRV-04, DRV-05
│   │   ├── trip/             # DRV-06, DRV-11
│   │   ├── tracking/         # DRV-07, DRV-08, DRV-09
│   │   ├── navigation/       # DRV-10
│   │   ├── documents/        # DRV-12
│   │   ├── wallet/           # DRV-13
│   │   └── profile/          # DRV-02, DRV-15
│   └── shared/
│       ├── widgets/
│       └── models/
├── android/
├── ios/
└── pubspec.yaml
```

### API Endpoints (Driver)

| Method | Endpoint | توضیح | DRV |
|--------|----------|-------|-----|
| POST | `/v1/driver/register` | ثبت‌نام + آپلود مدارک | 01 |
| GET | `/v1/driver/profile` | پروفایل راننده | 02 |
| PATCH | `/v1/driver/profile` | ویرایش پروفایل | 02 |
| GET | `/v1/driver/loads` | بارهای موجود | 03 |
| GET | `/v1/driver/loads/backhaul` | بارهای برگشت | 05 |
| POST | `/v1/driver/loads/:id/accept` | قبول بار | 06 |
| POST | `/v1/driver/trips/:id/events` | ثبت رویداد | 11 |
| POST | `/v1/driver/trips/:id/documents` | آپلود مستند | 12 |
| GET | `/v1/driver/wallet` | کیف پول | 13 |
| POST | `/v1/tracking/location` | ارسال GPS | 07 |
| WS | `/tracking` | WebSocket ردیابی | 07, 08 |

### Query Parameters — فیلتر بار (DRV-04)

```
GET /v1/driver/loads?
  lat=35.6892&
  lng=51.3890&
  radius=100&              # کیلومتر
  destination_country=ایران&
  cargo_type=general&
  weight_min=10&
  weight_max=20&
  sort=price|distance|date
```

### Trip Status Flow — یکپارچه با ۱۹ فرایند (L1)

> فرایند کامل: [logistics/01-shipment-loading-processes.md](./logistics/01-shipment-loading-processes.md)

| DRV Action | Process # | Code |
|------------|-----------|------|
| حرکت به مبدا | 7 | `en_route_to_origin` |
| رسیدن به مبدا | 8 | `arrived_at_origin` |
| شروع بارگیری | 9 | `loading_started` |
| اتمام بارگیری | 10 | `loading_completed` |
| توزین | 11 | `weighbridge_recorded` |
| خروج از مبدا | 13 | `departed_origin` |
| رسیدن به مرز | 15 | `arrived_at_border` |
| ترخیص گمرک | 16 | `customs_clearance` |
| عبور از مرز | 17 | `border_crossed` |
| رسیدن به مقصد | 18 | `arrived_at_destination` |
| تحویل POD | 19 | `unloaded_delivered` |

```text
assigned → (processes 7-19 via shipment_process_events)
```

### Background Location Service

```dart
// lib/core/location/location_service.dart
class LocationService {
  // تنظیمات
  static const intervalActive = Duration(seconds: 30);   // در حال سفر
  static const intervalIdle = Duration(minutes: 5);      // بدون سفر
  static const minDistanceFilter = 50.0;                 // متر

  Future<void> startTracking(String tripId) async {
    // Android: flutter_background_geolocation
    // iOS: background_fetch + location updates
  }

  Future<void> sendLocation(LocationData data) async {
    try {
      await api.post('/tracking/location', data.toJson());
    } catch (e) {
      await localDb.savePendingLocation(data);  // DRV-09 offline
    }
  }
}
```

### Offline Sync Strategy (DRV-09)

```
┌─────────────┐     Online      ┌─────────────┐
│  GPS Event  │ ──────────────▶ │   API       │
└─────────────┘                 └─────────────┘
       │ Offline
       ▼
┌─────────────┐   On reconnect  ┌─────────────┐
│  Isar DB    │ ──────────────▶ │  Sync Queue │
│  (pending)  │                 │  (batch)    │
└─────────────┘                 └─────────────┘
```

### Local Database Schema (Isar)

```dart
@collection
class PendingLocation {
  Id id = Isar.autoIncrement;
  late String tripId;
  late double lat;
  late double lng;
  late double? speed;
  late double? heading;
  late DateTime recordedAt;
  late bool synced;
}

@collection
class CachedLoad {
  Id id = Isar.autoIncrement;
  late String loadId;
  late String jsonData;  // serialized Load entity
  late DateTime cachedAt;
}
```

### Push Notifications (FCM)

| Event | Title | Body | Action |
|-------|-------|------|--------|
| بار جدید در منطقه | بار جدید نزدیک شما | تهران → مشهد، ۱۵ تن | Open loads |
| تایید بار | بار شما تایید شد | Provider X بار را تایید کرد | Open trip |
| بار برگشت | بار برگشت موجود | مشهد → تهران، ۱۰ تن | Open backhaul |
| تسویه حساب | تسویه انجام شد | مبلغ X به حساب واریز شد | Open wallet |

### Screen List

| Screen | Route | DRV Codes |
|--------|-------|-----------|
| Splash | `/` | — |
| Login/Register | `/auth` | 01 |
| OTP Verify | `/auth/verify` | 01 |
| Document Upload | `/auth/documents` | 01 |
| Home (Available Loads) | `/home` | 03, 04 |
| Load Detail | `/loads/:id` | 03, 06 |
| Backhaul Loads | `/backhaul` | 05 |
| Active Trip | `/trip/:id` | 06, 07, 11 |
| Trip Events | `/trip/:id/events` | 11 |
| Document Upload | `/trip/:id/documents` | 12 |
| Navigation | `/trip/:id/navigate` | 10 |
| Wallet | `/wallet` | 13 |
| Profile | `/profile` | 02, 15 |
| Settings | `/settings` | — |

### Permissions Required

| Permission | Android | iOS | دلیل |
|------------|---------|-----|------|
| Location (foreground) | ACCESS_FINE_LOCATION | NSLocationWhenInUse | نقشه و بارهای نزدیک |
| Location (background) | ACCESS_BACKGROUND_LOCATION | NSLocationAlways | DRV-08 |
| Camera | CAMERA | NSCameraUsageDescription | DRV-12 |
| Storage | READ_EXTERNAL_STORAGE | NSPhotoLibraryUsageDescription | DRV-12 |
| Notifications | POST_NOTIFICATIONS | — | FCM |

### Battery Optimization

```dart
// ارسال هوشمند بر اساس وضعیت
enum TrackingMode {
  highFrequency,  // هر ۳۰ ثانیه — در حال سفر فعال
  lowFrequency,   // هر ۵ دقیقه — متوقف ولی سفر فعال
  geofence,       // فقط هنگام ورود/خروج از geofence — نزدیک مرز
  off,            // بدون سفر
}
```

---

## اولویت‌بندی پیاده‌سازی

| فاز | DRV Codes | مدت تخمینی |
|-----|-----------|-----------|
| **MVP (P0)** | DRV-01 to DRV-08 | ۶ هفته |
| **P1** | DRV-09 to DRV-13 | ۳ هفته |
| **P2** | DRV-14, DRV-15 | ۲ هفته |

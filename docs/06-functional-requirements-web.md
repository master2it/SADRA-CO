# 6. نیازمندی‌های عملکردی — وب پلتفرم

[← بازگشت به فهرست](./README.md)

---

## 6.1 ماژول احراز هویت (AUTH)

| کد | عنوان | توضیح |
|----|-------|-------|
| AUTH-01 | ثبت‌نام با موبایل/ایمیل | پشتیبانی از هر دو |
| AUTH-02 | ورود چندعاملی | OTP + پسورد اختیاری |
| AUTH-03 | انتخاب نقش | مشتری / Provider / راننده |
| AUTH-04 | احراز هویت Provider | آپلود مدارک + تایید ادمین |
| AUTH-05 | احراز هویت راننده | آپلود گواهینامه + کارت ماشین |

---

## 6.2 ماژول Provider و تابلو نرخ (PROVIDER)

| کد | عنوان | توضیح |
|----|-------|-------|
| PRV-01 | پروفایل شرکت | نام، مجوز، ناوگان، کشورهای فعال |
| PRV-02 | تابلو نرخ (Rate Board) | Provider نرخ‌های خود را به ازای مسیر/وزن/نوع بار وارد می‌کند |
| PRV-03 | تعریف قالب نرخ | بر اساس: مبدا × مقصد × نوع بار × وزن × نوع کامیون |
| PRV-04 | نرخ‌های پلکانی | مثلاً ۰-۵ تن یک قیمت، ۵-۱۰ تن قیمت دیگر |
| PRV-05 | نرخ‌های ویژه (Special Rates) | برای مسیرهای خاص یا مشتریان VIP |
| PRV-06 | اعتبار زمانی نرخ | هر نرخ تاریخ انقضا دارد |
| PRV-07 | Import/Export نرخ | آپلود اکسل برای وارد کردن دسته‌ای نرخ‌ها |
| PRV-08 | تاریخچه تغییرات نرخ | چه کسی، کی، چه تغییری داده |
| PRV-09 | داشبورد Provider | تعداد درخواست‌ها، نرخ تبدیل، درآمد |

---

## 6.3 ماژول مشتری و درخواست (SHIPPER)

| کد | عنوان | توضیح |
|----|-------|-------|
| SHP-01 | مشاهده فوری نرخ‌ها | بدون ثبت درخواست، نرخ‌ها را ببیند |
| SHP-02 | استعلام نرخ هوشمند | مبدا، مقصد، نوع بار، وزن → نمایش نرخ همه Providerها |
| SHP-03 | مقایسه Providerها | قیمت، زمان تحویل، امتیاز، سابقه |
| SHP-04 | ثبت درخواست | ارسال به Provider انتخاب‌شده یا همه |
| SHP-05 | مذاکره قیمت | امکان چت/پیام با Provider برای تخفیف |
| SHP-06 | تاریخچه درخواست‌ها | لیست + وضعیت + فاکتورها |
| SHP-07 | مشتریان مورد علاقه | ذخیره Providerهای محبوب |
| SHP-08 | درخواست‌های تکراری | ثبت درخواست از روی درخواست قبلی |

---

## 6.4 ماژول مسیر و نقشه (ROUTE)

| کد | عنوان | توضیح |
|----|-------|-------|
| RTE-01 | تعریف مسیرها | ایران ↔ چین، ایران ↔ ترکیه، ایران ↔ عراق و... |
| RTE-02 | مسیرهای چندوجهی | زمینی + دریایی (در آینده) |
| RTE-03 | نمایش مسیر روی نقشه | با جزئیات: کشورها، مرزها، شهرهای عبوری |
| RTE-04 | محاسبه فاصله و زمان | بر اساس مسیر واقعی جاده‌ای |
| RTE-05 | اطلاعات مرزی | زمان تقریبی عبور از مرز، مدارک لازم |
| RTE-06 | مسیرهای پیشنهادی | چند مسیر جایگزین با مقایسه |
| RTE-07 | محتوای SEO مسیر | صفحه اختصاصی برای هر مسیر |

---

## 6.5 ماژول محتوا و SEO (CONTENT)

| کد | عنوان | توضیح |
|----|-------|-------|
| CNT-01 | صفحات مسیر | برای هر مسیر یک صفحه با محتوای غنی |
| CNT-02 | بلاگ | مقالات تخصصی حمل‌ونقل، گمرک، قوانین |
| CNT-03 | راهنمای کشورها | قوانین گمرکی هر کشور |
| CNT-04 | سوالات متداول | FAQ ساختاریافته |
| CNT-05 | Glossary | واژه‌نامه تخصصی |
| CNT-06 | Case Studies | نمونه کارهای موفق |
| CNT-07 | Landing Pageهای کمپین | برای کمپین‌های تبلیغاتی |

---

## 6.6 ماژول ادمین (ADMIN)

| کد | عنوان | توضیح |
|----|-------|-------|
| ADM-01 | داشبورد مدیریتی | آمار کلیدی لحظه‌ای |
| ADM-02 | مدیریت کاربران | همه نقش‌ها |
| ADM-03 | تایید Providerها | بررسی مدارک |
| ADM-04 | تایید رانندگان | بررسی مدارک |
| ADM-05 | مدیریت مسیرها | افزودن/ویرایش مسیرهای بین‌المللی |
| ADM-06 | نظارت بر نرخ‌ها | تشخیص نرخ‌های غیرمنطقی |
| ADM-07 | گزارش‌های مالی | کمیسیون، درآمد، تراکنش‌ها |
| ADM-08 | مدیریت محتوا | ویرایش صفحات SEO |
| ADM-09 | لاگ فعالیت‌ها | چه کسی چه کاری انجام داده |
| ADM-10 | تنظیمات سیستم | پیامک، ایمیل، کمیسیون پیش‌فرض |

---

## جزئیات پیاده‌سازی — API Endpoints

### Auth Module

| Method | Endpoint | Request Body | Response |
|--------|----------|-------------|----------|
| POST | `/v1/auth/register` | `{ mobile, role, email? }` | `{ userId, otpSent: true }` |
| POST | `/v1/auth/verify-otp` | `{ mobile, code }` | `{ accessToken, refreshToken, user }` |
| POST | `/v1/auth/login` | `{ mobile, password? }` | `{ accessToken, refreshToken }` |
| POST | `/v1/auth/refresh` | `{ refreshToken }` | `{ accessToken }` |
| GET | `/v1/auth/me` | — | `{ user, profile }` |

### Pricing / Rate Search (SHP-01, SHP-02)

| Method | Endpoint | Query Params | Response |
|--------|----------|-------------|----------|
| GET | `/v1/rates/search` | `origin, destination, cargoType, weight, vehicleType?` | `{ rates: RateResult[] }` |
| GET | `/v1/rates/compare` | `rateIds[]` | `{ comparison: ComparisonTable }` |

```typescript
interface RateResult {
  id: string;
  provider: { id, name, rating, totalShipments };
  price: number;
  currency: string;
  estimatedDays: number;
  validUntil: string;
  isSpecialRate: boolean;
}
```

### Provider Rate Board (PRV-02 to PRV-08)

| Method | Endpoint | Body/Params | Roles |
|--------|----------|------------|-------|
| GET | `/v1/provider/rates` | `?page, limit, origin?, destination?` | provider |
| POST | `/v1/provider/rates` | `CreateRateDto` | provider |
| PATCH | `/v1/provider/rates/:id` | `UpdateRateDto` | provider |
| DELETE | `/v1/provider/rates/:id` | — | provider |
| POST | `/v1/provider/rates/import` | `multipart/form-data (xlsx)` | provider |
| GET | `/v1/provider/rates/export` | — | provider |
| GET | `/v1/provider/rates/:id/history` | — | provider |
| GET | `/v1/provider/dashboard` | — | provider |

```typescript
interface CreateRateDto {
  originCountry: string;
  originCity: string;
  destinationCountry: string;
  destinationCity: string;
  cargoType: CargoType;
  weightMin: number;
  weightMax: number;
  vehicleType: VehicleType;
  price: number;
  currency: Currency;
  estimatedDays: number;
  validFrom: string;
  validUntil: string;
}
```

### Shipment Requests (SHP-04 to SHP-08)

| Method | Endpoint | Body | Roles |
|--------|----------|------|-------|
| POST | `/v1/shipments` | `CreateShipmentDto` | shipper |
| GET | `/v1/shipments` | `?status, page` | shipper |
| GET | `/v1/shipments/:id` | — | shipper, provider |
| POST | `/v1/shipments/:id/duplicate` | — | shipper |
| POST | `/v1/shipments/:id/select-provider` | `{ providerId, rateId }` | shipper |
| GET | `/v1/shipments/:id/offers` | — | shipper |
| GET | `/v1/shipments/:id/tracking` | — | shipper |

### Route Module (RTE-01 to RTE-07)

| Method | Endpoint | Params | Public |
|--------|----------|--------|--------|
| GET | `/v1/routes` | `?country, mode` | ✅ |
| GET | `/v1/routes/:slug` | — | ✅ |
| GET | `/v1/routes/:slug/rates` | `?weight, cargoType` | ✅ |
| GET | `/v1/routes/:slug/geometry` | — | ✅ |
| POST | `/v1/admin/routes` | `CreateRouteDto` | admin |
| PATCH | `/v1/admin/routes/:id` | — | admin |

### Admin Module

| Method | Endpoint | Roles |
|--------|----------|-------|
| GET | `/v1/admin/dashboard` | admin |
| GET | `/v1/admin/providers/pending` | admin |
| PATCH | `/v1/admin/providers/:id/approve` | admin |
| PATCH | `/v1/admin/providers/:id/reject` | admin |
| GET | `/v1/admin/rates/anomalies` | admin |
| GET | `/v1/admin/activity-logs` | admin |

---

## Frontend Components (Next.js)

### صفحات عمومی
```
components/
├── search/
│   ├── RateSearchForm.tsx       # فرم جستجوی نرخ صفحه اصلی
│   └── RateResultsList.tsx      # لیست نرخ‌ها
├── routes/
│   ├── RouteMap.tsx             # نقشه Mapbox
│   ├── RouteInfo.tsx            # اطلاعات مسیر
│   └── RouteRatesWidget.tsx    # ویجت نرخ در صفحه مسیر
├── comparison/
│   └── ProviderComparisonTable.tsx
└── auth/
    ├── OtpInput.tsx
    └── RoleSelector.tsx
```

### پنل Provider
```
app/provider/
├── dashboard/page.tsx
├── rate-board/
│   ├── page.tsx                 # لیست نرخ‌ها
│   ├── new/page.tsx             # افزودن نرخ
│   └── import/page.tsx          # Import اکسل
├── requests/page.tsx
└── analytics/page.tsx
```

### State Management

```typescript
// stores/rate-search.store.ts (Zustand)
interface RateSearchStore {
  origin: Location | null;
  destination: Location | null;
  cargoType: CargoType;
  weight: number;
  results: RateResult[];
  isLoading: boolean;
  search: () => Promise<void>;
}

// hooks/useRates.ts (React Query)
export function useRateSearch(params: SearchParams) {
  return useQuery({
    queryKey: ['rates', params],
    queryFn: () => api.rates.search(params),
    staleTime: 5 * 60 * 1000,
  });
}
```

---

## Import/Export اکسل (PRV-07)

### فرمت فایل Import

| ستون | نوع | اجباری | مثال |
|------|-----|--------|------|
| origin_country | string | ✅ | ایران |
| origin_city | string | ✅ | تهران |
| destination_country | string | ✅ | چین |
| destination_city | string | ✅ | شانگهای |
| cargo_type | enum | ✅ | general |
| weight_min | number | ✅ | 0 |
| weight_max | number | ✅ | 5 |
| vehicle_type | enum | ✅ | truck_20t |
| price | number | ✅ | 45000 |
| currency | enum | ✅ | CNY |
| estimated_days | number | ✅ | 12 |
| valid_until | date | ✅ | 2026-12-31 |

### Flow Import

```
Upload Excel → Validate rows → Show preview (errors highlighted)
→ Confirm → Bulk insert → Audit log → Notification
```

---

## Notification Triggers

| Event | SMS | Email | Push | In-App |
|-------|-----|-------|------|--------|
| درخواست جدید برای Provider | ✅ | ✅ | — | ✅ |
| پیشنهاد جدید برای Shipper | ✅ | ✅ | — | ✅ |
| تایید Provider توسط ادمین | ✅ | ✅ | — | ✅ |
| تغییر وضعیت Shipment | ✅ | ✅ | — | ✅ |
| نرخ در حال انقضا (Provider) | ✅ | — | — | ✅ |

---

## 6.7 ماژول CRM — مدیریت مشتری (CRM)

| کد | عنوان | توضیح |
|----|-------|-------|
| CRM-01 | ثبت Lead از همه کانال‌ها | وب، شبکه اجتماعی، معرفی، نمایشگاه، تلفن، AI |
| CRM-02 | پروفایل ۳۶۰ درجه مشتری | تاریخچه تماس، درخواست‌ها، پیش‌فاکتورها |
| CRM-03 | Pipeline فروش | Kanban: New → Contacted → Quote → Contract → Won/Lost |
| CRM-04 | تخصیص خودکار Lead | بر اساس منطقه، نوع بار، بار کاری اپراتور |
| CRM-05 | SLA و یادآور | Lead بدون پیگیری → هشدار به اپراتور/مدیر |
| CRM-06 | صفر Lead گم‌شده | هیچ Lead بدون وضعیت و مسئول نماند |
| CRM-07 | گزارش کانال جذب | کدام کانال بیشترین تبدیل دارد |
| CRM-08 | خودخدمتی مشتری | مشتری نرخ ببیند و درخواست ثبت کند بدون اپراتور |
| CRM-09 | تاریخچه تعاملات | تماس، چت، ایمیل، یادداشت |

---

## 6.8 ماژول SRM — مدیریت تامین‌کننده (SRM)

| کد | عنوان | توضیح |
|----|-------|-------|
| SRM-01 | پروفایل تامین‌کننده | اطلاعات شرکت، مجوز، ناوگان |
| SRM-02 | تابلو نرخ خودخدمتی | Provider نرخ را مستقیم ثبت می‌کند (جایگزین تماس دستی اپراتور) |
| SRM-03 | Import/Export اکسل | ورود دسته‌ای نرخ‌ها |
| SRM-04 | اعتبار و انقضای نرخ | نرخ قدیمی خودکار غیرفعال |
| SRM-05 | Onboarding تامین‌کننده | ثبت‌نام + تایید + آموزش |
| SRM-06 | داشبورد عملکرد | درخواست‌ها، تبدیل، درآمد |
| SRM-07 | اعلان درخواست جدید | نوتیف برای Leadهای مرتبط با مسیر Provider |

---

## 6.9 ماژول دستیار AI (AI)

| کد | عنوان | توضیح |
|----|-------|-------|
| AI-01 | چت هوشمند مشتری | پاسخ به سوالات حمل، گمرک، نرخ |
| AI-02 | استعلام نرخ در چت | نمایش نرخ از SRM بدون اپراتور |
| AI-03 | تولید پیش‌فاکتور | PDF خودکار |
| AI-04 | پیش‌نویس قرارداد | قرارداد اولیه برای بررسی اپراتور |
| AI-05 | ثبت Lead از چت | هر مکالمه → Lead در CRM |
| AI-06 | Handoff به اپراتور | انتقال با خلاصه کامل مکالمه |
| AI-07 | پیگیری خودکار | follow-up اگر مشتری پاسخ نداد |

> جزئیات کامل API و دیتابیس: [داکیومنت ۱۸](./18-crm-srm-ai-assistant.md)

---

## 6.10 ماژول کاتالوگ محصولات و تولید (PRODUCT)

| کد | عنوان | توضیح |
|----|-------|-------|
| PRD-01 | کاتالوگ محصولات | مشتری اجناس را در سایت ببیند |
| PRD-02 | پیش‌فاکتور محصول موجود | نرخ + صدور پیش‌فاکتور فوری |
| PRD-03 | درخواست سفارش تولید | اگر جنس نبود → سفارش تولید در کارخانه |
| PRD-04 | پیش‌فاکتور سفارشی | نمایش: موجود نیست، X روز، Y هزینه، پیش‌پرداخت |
| PRD-05 | محصول اختصاصی ادمین | ادمین برای مشتری خاص محصول + پیش‌فاکتور |
| PRD-06 | لینک پیش‌فاکتور اختصاصی | مشتری فقط پیش‌فاکتور ببیند (بدون کاتالوگ) |
| PRD-07 | مدیریت کارخانه‌ها | قرارداد با کارخانه‌ها، ارجاع سفارش تولید |
| PRD-08 | تولید نرخ پس از تولید | نرخ جدید برای محصول سفارشی |

> جزئیات کامل: [داکیومنت ۱۹](./19-product-catalog-and-manufacturing.md)

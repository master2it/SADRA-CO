# 1. چشم‌انداز و معرفی

[← بازگشت به فهرست](./README.md)

---

## 1.1 معرفی محصول

پلتفرم **B2B زنجیره تامین صادراتی** که فروش محصولات (مانند Bitumen، Base Oil، Urea و …) را از **استعلام تا مدارک حمل** پوشش می‌دهد و پس از هر معامله، مسیر **خرید/تدارکات → حمل → تحویل** را سیستمی می‌کند.

چارچوب استاندارد: **SCOR** (Plan / Source / Make / Deliver / Return / Enable) — جزئیات در [docs/20-supply-chain-scor.md](./20-supply-chain-scor.md).

در فاز اول تمرکز روی **فروش صادراتی + تدارکات + حمل زمینی بین‌المللی** است؛ معماری برای حمل دریایی/ریلی آماده می‌ماند.

### دامنه محصول (Product Scope)

| در محدوده فاز ۱ | خارج از محدوده فاز ۱ |
|-----------------|----------------------|
| **زنجیره تامین** (فروش → خرید → حمل → تحویل) | پرداخت آنلاین کامل / Escrow |
| **Export Sales** (۱۱ مرحله Inquiry تا Shipping Docs) | Return پیشرفته (مرجوعی) |
| شرایط پرداخت محصول (۲۰/۸۰ و ۱۰۰٪ Advance، T/T) | White Label |
| CRM + SRM + دستیار AI | AI قیمت‌گذاری پیشرفته |
| کاتالوگ محصولات + تولید کارخانه‌ای | — |
| تدارکات / خرید از کارخانه (حداقلی) | — |
| لجستیک + ۱۹ فرایند بارگیری + GPS + اپ راننده | حمل دریایی/ریلی کامل |

---

## 1.2 ارزش پیشنهادی (Value Proposition)

| برای | ارزش |
|-------|------|
| **صاحب بار** | مشاهده فوری نرخ‌ها بدون انتظار برای پاسخ اپراتور، مقایسه چند Provider، شفافیت قیمت |
| **Provider (شرکت حمل)** | پنل مستقل برای مدیریت نرخ‌ها، دریافت درخواست‌های هدفمند، کاهش هزینه بازاریابی |
| **راننده** | پیدا کردن بار برگشت، کاهش سفر خالی، درآمد بیشتر |
| **پلتفرم** | حذف گلوگاه اپراتور، مقیاس‌پذیری، جمع‌آوری داده قیمتی بازار |
| **Buyer (خریدار صادراتی)** | مسیر شفاف Inquiry → Quotation → PI → Contract → پرداخت → مدارک حمل |
| **تیم بازرگانی** | CRM + Export Sales سیستمی، صفر Lead گم‌شده |
| **تیم تدارکات** | خرید از کارخانه پس از تأیید پیش‌پرداخت |
| **تیم تامین / لجستیک** | SRM + ۱۹ فرایند حمل قابل مشاهده برای همه نقش‌ها |

### ماژول‌های اصلی

| ماژول | توضیح | داکیومنت |
|-------|-------|----------|
| **زنجیره تامین SCOR** | Plan / Source / Make / Deliver | [20](./20-supply-chain-scor.md) |
| **Export Sales** | ۱۱ مرحله فروش صادراتی + شرایط پرداخت | [commerce/C3](./commerce/03-export-sales-procedure.md) |
| **CRM / SRM / AI** | Lead، تامین‌کننده، Quotation/PI | [commerce/C1](./18-crm-srm-ai-assistant.md) |
| **کاتالوگ + تولید** | محصولات، Made-to-Order | [commerce/C2](./19-product-catalog-and-manufacturing.md) |
| **لجستیک — ۱۹ فرایند** | Timeline بارگیری | [logistics/L1](./logistics/01-shipment-loading-processes.md) |

---

## 1.3 مدل کسب‌وکار

- **مدل فعلی (فاز ۱):** Lead Generation — مشتری درخواست می‌دهد، Provider پیشنهاد می‌دهد
- **مدل آینده (فاز ۲+):** Transaction-based — کمیسیون از هر تراکنش موفق

---

## جزئیات پیاده‌سازی

### نام‌گذاری در کد

| مفهوم | نام در کد | توضیح |
|-------|-----------|-------|
| صاحب بار | `Shipper` | نقش `shipper` در enum |
| شرکت حمل | `Provider` | نقش `provider` در enum |
| راننده | `Driver` | نقش `driver` در enum |
| درخواست حمل | `ShipmentRequest` | entity اصلی |
| تابلو نرخ | `RateBoard` | نرخ‌های Provider |
| سرنخ فروش | `Lead` | CRM entity |
| مکالمه AI | `AiConversation` | AI Assistant |
| پیش‌فاکتور | `ProformaInvoice` | تولید توسط AI یا اپراتور |
| محصول | `Product` | کاتالوگ — موجود / سفارشی |
| درخواست تولید | `CustomManufacturingOrder` | محصول ناموجود |
| کارخانه | `Factory` | قرارداد تولید |

### اصول طراحی معماری

1. **API-First:** همه کلاینت‌ها (وب، موبایل، ادمین) از یک API مشترک استفاده کنند
2. **Multi-tenant ready:** هر Provider داده‌های جداگانه داشته باشد
3. **Transport-mode extensible:** فیلد `transport_mode` از ابتدا `land | sea | rail | multi` باشد
4. **i18n from day one:** کلیدهای ترجمه، نه متن هاردکد
5. **Currency-aware:** همه قیمت‌ها با `amount + currency` ذخیره شوند

### Entityهای هسته (Core Domain)

```
Shipper ──creates──▶ ShipmentRequest ──receives──▶ ShipmentOffer
                          │
                          ▼
Provider ──manages──▶ RateBoard ◀──belongs── Route
                          │
                          ▼
Driver ──executes──▶ Trip ──tracks──▶ LocationTracking
```

### Environment Variables مورد نیاز

```env
# Core
APP_NAME=SADRA
APP_ENV=development|staging|production
APP_URL=https://sadra.example.com
API_URL=https://api.sadra.example.com

# Auth
JWT_SECRET=
JWT_EXPIRES_IN=7d
OTP_EXPIRES_IN=300

# Database
DATABASE_URL=postgresql://...
REDIS_URL=redis://...

# External Services
MAPBOX_TOKEN=
SMS_API_KEY=
EMAIL_API_KEY=
S3_BUCKET=
S3_ENDPOINT=
```

### چک‌لیست راه‌اندازی اولیه پروژه

- [ ] ایجاد monorepo با Turborepo یا Nx
- [ ] تنظیم ESLint + Prettier + Husky
- [ ] ایجاد `packages/shared-types` با enumهای نقش و وضعیت
- [ ] راه‌اندازی PostgreSQL با PostGIS extension
- [ ] راه‌اندازی Redis
- [ ] تنظیم i18n (fa, en, ar, zh)
- [ ] تنظیم multi-currency (IRR, USD, EUR, CNY)

# پیوست‌ها

[← بازگشت به فهرست](./README.md)

---

## پیوست الف: چک‌لیست راه‌اندازی

### زیرساخت

- [ ] دامنه ثبت شده (`sadra.ir` + `sadra.com`)
- [ ] SSL فعال (Cloudflare)
- [ ] سرورها راه‌اندازی شده (Hetzner + Arvan)
- [ ] Docker Compose / K8s configured
- [ ] CI/CD pipeline (GitHub Actions) فعال
- [ ] Staging environment آماده
- [ ] Production environment آماده
- [ ] Backup روزانه به S3 فعال
- [ ] Monitoring (Sentry + Prometheus + Grafana) فعال
- [ ] Cloudflare CDN configured

### سرویس‌های خارجی

- [ ] پنل پیامک فعال (Kavenegar / FarazSMS)
- [ ] سرویس ایمیل فعال (AWS SES / Mailgun)
- [ ] حساب Mapbox (Web) + Google Maps (Mobile)
- [ ] حساب S3 / فضای ابری (MinIO / Arvan Cloud)
- [ ] حساب Sentry برای مانیتورینگ
- [ ] Firebase project (FCM for push notifications)
- [ ] Google Search Console
- [ ] Google Analytics 4

### اپلیکیشن

- [ ] اپلیکیشن در Google Play Store
- [ ] اپلیکیشن در Apple App Store
- [ ] App signing certificates configured
- [ ] Push notification certificates (APNs)

### محتوا و قانونی

- [ ] صفحات حقوقی (قوانین، حریم خصوصی)
- [ ] محتوای حداقل ۱۰ مسیر اصلی (۲۰۰۰+ کلمه)
- [ ] FAQ با Schema markup
- [ ] About Us + Contact pages
- [ ] OG images for social sharing

### داده اولیه

- [ ] Seed data: ۲۰+ مسیر بین‌المللی
- [ ] Import نرخ‌های Providerهای موجود
- [ ] ۱۰ Provider تایید‌شده با نرخ فعال
- [ ] ۲۰ راننده تایید‌شده

---

## پیوست ب: مترادف‌ها و واژه‌نامه

| واژه | معنی |
|------|------|
| **Shipper** | صاحب بار / مشتری / Buyer |
| **Buyer** | خریدار صادراتی |
| **Seller** | فروشنده (SADRA) |
| **Provider / Carrier** | شرکت حمل / تامین‌کننده خدمات حمل |
| **Inquiry** | استعلام رسمی خریدار |
| **Quotation** | پیشنهاد قیمت فروشنده |
| **LOI** | Letter of Intent — نامه قصد خرید |
| **PI / Proforma Invoice** | پیش‌فاکتور |
| **Sales Contract** | قرارداد فروش |
| **T/T** | Telegraphic Transfer — حواله بانکی |
| **SWIFT** | تأییدیه انتقال بانکی بین‌المللی |
| **B/L** | Bill of Lading — بارنامه |
| **COO** | Certificate of Origin — گواهی مبدأ |
| **SCOR** | Supply Chain Operations Reference |
| **Plan / Source / Make / Deliver / Return / Enable** | شش فرایند اصلی SCOR |
| **Procurement** | تدارکات / خرید |
| **Rate Board** | تابلو نرخ |
| **Backhaul** | بار برگشت |
| **POD (Proof of Delivery)** | رسید تحویل |
| **CMR** | بارنامه بین‌المللی جاده‌ای |
| **ETA** | زمان تقریبی رسیدن |
| **LTL** | Less than Truckload |
| **FTL** | Full Truckload |
| **SRS** | Software Requirements Specification |
| **MVP** | Minimum Viable Product |
| **KPI** | Key Performance Indicator |
| **RACM** | Role Access Control Matrix |
| **SSR** | Server-Side Rendering |
| **PostGIS** | PostgreSQL extension for geographic data |
| **JWT** | JSON Web Token |
| **OTP** | One-Time Password |
| **FCM** | Firebase Cloud Messaging |
| **WCAG** | Web Content Accessibility Guidelines |

---

## پیوست ج: API Error Codes

| Code | HTTP Status | Message (FA) | Message (EN) |
|------|------------|-------------|-------------|
| `AUTH_001` | 401 | توکن نامعتبر | Invalid token |
| `AUTH_002` | 401 | توکن منقضی شده | Token expired |
| `AUTH_003` | 403 | دسترسی غیرمجاز | Forbidden |
| `AUTH_004` | 429 | تعداد درخواست OTP بیش از حد | OTP rate limit exceeded |
| `USER_001` | 404 | کاربر یافت نشد | User not found |
| `USER_002` | 409 | شماره موبایل قبلاً ثبت شده | Mobile already registered |
| `RATE_001` | 404 | نرخی یافت نشد | No rates found |
| `RATE_002` | 400 | نرخ منقضی شده | Rate expired |
| `SHIP_001` | 404 | درخواست یافت نشد | Shipment not found |
| `SHIP_002` | 400 | وضعیت درخواست نامعتبر | Invalid shipment status |
| `PROV_001` | 403 | Provider تایید نشده | Provider not approved |
| `DRV_001` | 403 | راننده تایید نشده | Driver not approved |
| `FILE_001` | 400 | فرمت فایل نامعتبر | Invalid file format |
| `FILE_002` | 400 | حجم فایل بیش از حد | File too large |
| `VAL_001` | 400 | داده‌های ورودی نامعتبر | Validation error |

---

## پیوست د: Notification Templates

### SMS Templates

```
# OTP
کد تایید SADRA: {code}
این کد تا ۵ دقیقه معتبر است.

# New Request (Provider)
درخواست جدید: {origin} به {destination}، {weight} تن.
جزئیات: sadra.ir/provider/requests/{id}

# Offer Accepted (Provider)
مشتری پیشنهاد شما را قبول کرد!
مسیر: {origin} به {destination}
sadra.ir/provider/trips

# Shipment Delivered (Shipper)
بار شما تحویل داده شد.
مسیر: {origin} به {destination}
امتیاز دهید: sadra.ir/dashboard/requests/{id}
```

### Email Templates

| Template | Subject | Trigger |
|----------|---------|---------|
| `welcome` | خوش آمدید به SADRA | After registration |
| `provider_approved` | حساب شما تایید شد | Admin approves provider |
| `new_request` | درخواست جدید در مسیر شما | New shipment request |
| `offer_accepted` | پیشنهاد شما قبول شد | Shipper accepts offer |
| `rate_expiring` | نرخ‌های شما در حال انقضا هستند | 7 days before expiry |
| `shipment_delivered` | بار تحویل داده شد | Trip completed |

---

## پیوست ه: Git Branching Strategy

```
main          ← Production (protected)
  └── develop ← Integration branch
       ├── feature/auth-module
       ├── feature/rate-board
       ├── feature/driver-app
       ├── fix/gps-offline-sync
       └── release/v1.0.0
```

### Commit Message Convention

```
feat: add rate search API endpoint
fix: resolve GPS offline sync issue
docs: update API documentation
test: add unit tests for pricing service
chore: update dependencies
refactor: extract notification service
```

### PR Checklist

- [ ] Code follows project conventions
- [ ] Tests added/updated
- [ ] API docs updated (if API change)
- [ ] No secrets in code
- [ ] Reviewed by at least 1 team member
- [ ] CI passes (lint, test, build)

---

## پیوست و: Useful Commands

```bash
# Development
pnpm dev                          # Start all apps
pnpm --filter api dev             # Start API only
pnpm --filter web dev             # Start web only

# Database
pnpm --filter api prisma migrate dev --name <name>
pnpm --filter api prisma db seed
pnpm --filter api prisma studio     # DB GUI

# Testing
pnpm test                           # All tests
pnpm --filter api test:unit
pnpm --filter web test:e2e

# Build & Deploy
pnpm build
docker compose -f docker-compose.prod.yml up -d

# Flutter
cd apps/driver-app
flutter run
flutter build apk --release
flutter build ios --release
```

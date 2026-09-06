# 🚛 لجستیک (Logistics)

[← بازگشت به فهرست اصلی](../README.md)

---

بخش **لجستیک** پلتفرم SADRA شامل حمل‌ونقل بین‌المللی، مدیریت سفر، ردیابی GPS، پنل Provider، مسیرها و **۱۹ فرایند بارگیری/حمل** است.

> لجستیک در مدل SCOR زیرمجموعه **Deliver** است و معمولاً پس از تأیید پیش‌پرداخت در [فروش صادراتی](../commerce/03-export-sales-procedure.md) شروع می‌شود.

---

## فهرست داکیومنت‌های لجستیک

| # | عنوان | فایل | اولویت |
|---|-------|------|--------|
| **L1** | **۱۹ فرایند بارگیری و حمل** | [01-shipment-loading-processes.md](./01-shipment-loading-processes.md) | 🔴 اصلی |
| L2 | نیازمندی‌های وب (حمل، مسیر، Provider) | [../06-functional-requirements-web.md](../06-functional-requirements-web.md) | — |
| L3 | اپلیکیشن راننده | [../07-functional-requirements-driver-app.md](../07-functional-requirements-driver-app.md) | — |
| L4 | مدل داده (Trip, Shipment, Tracking) | [../08-data-model.md](../08-data-model.md) | — |
| L5 | سفر کاربر (مشتری، Provider، راننده) | [../09-user-journey.md](../09-user-journey.md) | — |
| L6 | صفحات و UX | [../10-pages-and-ux.md](../10-pages-and-ux.md) | — |
| L7 | استراتژی SEO (صفحات مسیر) | [../11-seo-strategy.md](../11-seo-strategy.md) | — |
| L8 | قابلیت‌های MVP لجستیک | [../12-mvp-features.md](../12-mvp-features.md) | — |

---

## جایگاه در زنجیره تامین

```
فروش صادراتی (C3) → تأیید پیش‌پرداخت
        ↓
   تدارکات / خرید (Source)
        ↓
   ┌─────────────────────────────────────┐
   │  لجستیک SADRA (Deliver)             │
   │  ۱۹ فرایند بارگیری — Timeline       │
   │  [ادمین] [مشتری] [راننده] [Provider]│
   └─────────────────────────────────────┘
        ↓
   مدارک حمل (C3 مرحله ۱۱) → تحویل
```

---

## داکیومنت‌های مرتبط

| عنوان | فایل |
|-------|------|
| زنجیره تامین SCOR | [../20-supply-chain-scor.md](../20-supply-chain-scor.md) |
| فروش صادراتی | [../commerce/03-export-sales-procedure.md](../commerce/03-export-sales-procedure.md) |
| معماری سیستم | [../05-system-architecture.md](../05-system-architecture.md) |

---

## ترتیب مطالعه (تیم لجستیک)

```
20 (SCOR) → C3 (فروش) → L1 (۱۹ فرایند) → 08 → L2/L3 → 16
```

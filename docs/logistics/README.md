# 🚛 لجستیک (Logistics)

[← بازگشت به فهرست اصلی](../README.md)

---

بخش **لجستیک** پلتفرم SADRA شامل حمل‌ونقل بین‌المللی، مدیریت سفر، ردیابی GPS، پنل Provider، مسیرها و **۱۹ فرایند بارگیری/حمل** است که به‌صورت Timeline برای ادمین، مشتری و راننده نمایش داده می‌شود.

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

## داکیومنت‌های مشترک (مرتبط با لجستیک)

| عنوان | فایل |
|-------|------|
| معماری سیستم | [../05-system-architecture.md](../05-system-architecture.md) |
| ذینفعان و نقش‌ها | [../04-stakeholders-and-roles.md](../04-stakeholders-and-roles.md) |
| استک فنی | [../13-tech-stack.md](../13-tech-stack.md) |
| معیارهای پذیرش | [../16-acceptance-criteria.md](../16-acceptance-criteria.md) |

---

## خلاصه ماژول‌های لجستیک

```
┌─────────────────────────────────────────────────────────┐
│                    لجستیک SADRA                          │
├─────────────┬─────────────┬─────────────┬───────────────┤
│  Shipment   │   Route     │  Provider   │   Driver App  │
│  درخواست    │   مسیر/نقشه │  تابلو نرخ  │   GPS/بار     │
├─────────────┴─────────────┴─────────────┴───────────────┤
│         ۱۹ فرایند بارگیری — Timeline مشترک              │
│    [ادمین]  [مشتری]  [راننده]  [Provider]              │
└─────────────────────────────────────────────────────────┘
```

---

## ترتیب مطالعه (تیم لجستیک)

```
L1 (فرایندها) → 04 → 05 → 08 → L2/L3 → L5 → L6 → 16
```

# 📚 مستندات پروژه SADRA

## پلتفرم بین‌المللی حمل‌ونقل بار — فاز ۱ (MVP)

**نسخه:** 2.1 | **تاریخ:** ۳۱ مرداد ۱۴۰۵ | **وضعیت:** پیش‌نویس نهایی

---

## ساختار داکیومنت‌ها

مستندات SADRA در **دو دسته اصلی** سازمان‌دهی شده‌اند:

| دسته | توضیح | فهرست |
|------|-------|-------|
| 🚛 **لجستیک** | حمل‌ونقل، بارگیری، GPS، راننده، مسیر، ۱۹ فرایند | [logistics/README.md](./logistics/README.md) |
| 💼 **بازرگانی** | CRM، SRM، AI، کاتالوگ، پیش‌فاکتور، کارخانه | [commerce/README.md](./commerce/README.md) |

---

## 🚛 لجستیک (Logistics)

| # | عنوان | فایل |
|---|-------|------|
| **L1** | **۱۹ فرایند بارگیری و حمل** | [logistics/01-shipment-loading-processes.md](./logistics/01-shipment-loading-processes.md) |
| L2 | نیازمندی‌های وب (حمل، مسیر، Provider) | [06-functional-requirements-web.md](./06-functional-requirements-web.md) |
| L3 | اپلیکیشن راننده | [07-functional-requirements-driver-app.md](./07-functional-requirements-driver-app.md) |
| L4 | مدل داده | [08-data-model.md](./08-data-model.md) |
| L5 | سفر کاربر | [09-user-journey.md](./09-user-journey.md) |
| L6 | صفحات و UX | [10-pages-and-ux.md](./10-pages-and-ux.md) |
| L7 | استراتژی SEO | [11-seo-strategy.md](./11-seo-strategy.md) |
| L8 | قابلیت‌های MVP | [12-mvp-features.md](./12-mvp-features.md) |

---

## 💼 بازرگانی (Commerce)

| # | عنوان | فایل |
|---|-------|------|
| **C1** | CRM، SRM و دستیار AI | [18-crm-srm-ai-assistant.md](./18-crm-srm-ai-assistant.md) |
| **C2** | کاتالوگ محصولات و تولید کارخانه‌ای | [19-product-catalog-and-manufacturing.md](./19-product-catalog-and-manufacturing.md) |

---

## 📋 مشترک (Shared)

| # | عنوان | فایل |
|---|-------|------|
| 1 | چشم‌انداز و معرفی | [01-vision-and-introduction.md](./01-vision-and-introduction.md) |
| 2 | تحلیل وضعیت موجود | [02-current-state-analysis.md](./02-current-state-analysis.md) |
| 3 | اهداف فاز ۱ | [03-phase-1-goals.md](./03-phase-1-goals.md) |
| 4 | ذینفعان و نقش‌ها | [04-stakeholders-and-roles.md](./04-stakeholders-and-roles.md) |
| 5 | معماری کلان سیستم | [05-system-architecture.md](./05-system-architecture.md) |
| 13 | استک فنی | [13-tech-stack.md](./13-tech-stack.md) |
| 14 | زمان‌بندی و تیم | [14-timeline-and-team.md](./14-timeline-and-team.md) |
| 15 | ریسک‌ها و راهکارها | [15-risks-and-mitigation.md](./15-risks-and-mitigation.md) |
| 16 | معیارهای پذیرش | [16-acceptance-criteria.md](./16-acceptance-criteria.md) |
| 17 | خارج از محدوده فاز ۱ | [17-out-of-scope.md](./17-out-of-scope.md) |
| — | پیوست‌ها | [appendix.md](./appendix.md) |

---

## ترتیب مطالعه

### تیم لجستیک
```
01 → 04 → 05 → L1 (۱۹ فرایند) → 08 → L2/L3 → L5 → 16
```

### تیم بازرگانی
```
01 → 02 → C1 → C2 → 16
```

---

## ساختار پروژه

```
sadra/
├── apps/
│   ├── web/              # Next.js — وب (لجستیک + بازرگانی)
│   ├── api/              # NestJS — Backend
│   └── driver-app/       # Flutter — اپ راننده (لجستیک)
├── docs/
│   ├── logistics/        # 🚛 داکیومنت‌های لجستیک
│   ├── commerce/         # 💼 داکیومنت‌های بازرگانی
│   └── ...               # مشترک
└── infrastructure/
```

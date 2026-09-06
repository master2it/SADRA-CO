# 📚 مستندات پروژه SADRA

## پلتفرم زنجیره تامین صادراتی — فاز ۱ (MVP)

**نسخه:** 3.0 | **وضعیت:** پیش‌نویس نهایی

---

## ماهیت پروژه

SADRA یک سیستم **زنجیره تامین (Supply Chain)** است که فروش صادراتی، تدارکات/خرید، تولید و لجستیک را پوشش می‌دهد.

```
فروش (Export Sales) → خرید/تدارکات → حمل (Logistics) → تحویل
```

چارچوب استاندارد: **SCOR** — [docs/20-supply-chain-scor.md](./20-supply-chain-scor.md)

---

## ساختار داکیومنت‌ها

| دسته | توضیح | فهرست |
|------|-------|-------|
| 🔗 **زنجیره تامین** | مدل SCOR، جریان End-to-End | [20-supply-chain-scor.md](./20-supply-chain-scor.md) |
| 💼 **بازرگانی** | فروش صادراتی، CRM، کاتالوگ، پرداخت | [commerce/README.md](./commerce/README.md) |
| 🚛 **لجستیک** | حمل، بارگیری، GPS، ۱۹ فرایند | [logistics/README.md](./logistics/README.md) |

---

## 🔗 زنجیره تامین (اصلی)

| # | عنوان | فایل |
|---|-------|------|
| **SCOR** | مدل زنجیره تامین + نگاشت ماژول‌ها | [20-supply-chain-scor.md](./20-supply-chain-scor.md) |

---

## 💼 بازرگانی (Commerce)

| # | عنوان | فایل |
|---|-------|------|
| **C3** | پروسیجر فروش صادراتی (۱۱ مرحله) + شرایط پرداخت | [commerce/03-export-sales-procedure.md](./commerce/03-export-sales-procedure.md) |
| **C1** | CRM، SRM و دستیار AI | [18-crm-srm-ai-assistant.md](./18-crm-srm-ai-assistant.md) |
| **C2** | کاتالوگ محصولات و تولید کارخانه‌ای | [19-product-catalog-and-manufacturing.md](./19-product-catalog-and-manufacturing.md) |

### شرایط پرداخت (خلاصه)

| نوع | محصولات |
|-----|---------|
| ۲۰٪ Advance / ۸۰٪ Balance | Base Oil Recycled، Bitumen، Caustic Soda، Urea |
| ۱۰۰٪ Advance | Sulphur، Base Oil Virgin، LPG، Polymers، Glycols |

---

## 🚛 لجستیک (Logistics)

| # | عنوان | فایل |
|---|-------|------|
| **L1** | ۱۹ فرایند بارگیری و حمل | [logistics/01-shipment-loading-processes.md](./logistics/01-shipment-loading-processes.md) |
| L2 | نیازمندی‌های وب | [06-functional-requirements-web.md](./06-functional-requirements-web.md) |
| L3 | اپلیکیشن راننده | [07-functional-requirements-driver-app.md](./07-functional-requirements-driver-app.md) |
| L4 | مدل داده | [08-data-model.md](./08-data-model.md) |
| L5 | سفر کاربر | [09-user-journey.md](./09-user-journey.md) |
| L6 | صفحات و UX | [10-pages-and-ux.md](./10-pages-and-ux.md) |
| L7 | استراتژی SEO | [11-seo-strategy.md](./11-seo-strategy.md) |
| L8 | قابلیت‌های MVP | [12-mvp-features.md](./12-mvp-features.md) |

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

## ترتیب مطالعه پیشنهادی

```
01 → 20 (SCOR) → C3 (فروش صادراتی) → C2 → C1 → L1 (لجستیک) → 08 → 16
```

1. چشم‌انداز و مدل زنجیره تامین
2. پروسیجر فروش صادراتی (۱۱ مرحله)
3. کاتالوگ و CRM
4. ۱۹ فرایند لجستیک
5. مدل داده و معیار پذیرش

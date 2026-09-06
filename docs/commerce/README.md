# 💼 بازرگانی (Commerce)

[← بازگشت به فهرست اصلی](../README.md)

---

بخش **بازرگانی** پلتفرم SADRA شامل فروش صادراتی، CRM، SRM، دستیار AI، کاتالوگ محصولات و تولید کارخانه‌ای است.

> فرایند اصلی فروش: **Export Sales Procedure** (۱۱ مرحله)  
> پس از فروش: **خرید → حمل → تحویل** در چارچوب زنجیره تامین [SCOR](../20-supply-chain-scor.md)

---

## فهرست داکیومنت‌های بازرگانی

| # | عنوان | فایل |
|---|-------|------|
| **C1** | CRM، SRM و دستیار AI | [../18-crm-srm-ai-assistant.md](../18-crm-srm-ai-assistant.md) |
| **C2** | کاتالوگ محصولات و تولید کارخانه‌ای | [../19-product-catalog-and-manufacturing.md](../19-product-catalog-and-manufacturing.md) |
| **C3** | پروسیجر فروش صادراتی + شرایط پرداخت | [03-export-sales-procedure.md](./03-export-sales-procedure.md) |

---

## خلاصه ماژول‌های بازرگانی

```
┌─────────────────────────────────────────────────────────────┐
│                    بازرگانی SADRA                            │
├────────────────┬──────────────┬─────────────────────────────┤
│  Export Sales  │     CRM      │   کاتالوگ + کارخانه          │
│  ۱۱ مرحله فروش │ Lead/Pipeline│  محصول + شرایط پرداخت       │
├────────────────┴──────────────┴─────────────────────────────┤
│  SRM (تامین) + AI (Quotation/PI) + Finance (SWIFT)          │
│  اصل: صفر Lead گم‌شده | شفافیت Inquiry تا Shipping Docs    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ پس از تأیید پیش‌پرداخت
                    خرید → لجستیک → تحویل
```

---

## ترتیب مطالعه (تیم بازرگانی)

```
20 (SCOR) → C3 (فروش صادراتی) → C2 (کاتالوگ) → C1 (CRM/AI) → 16
```

---

## داکیومنت‌های مرتبط

| عنوان | فایل |
|-------|------|
| زنجیره تامین SCOR | [../20-supply-chain-scor.md](../20-supply-chain-scor.md) |
| ۱۹ فرایند لجستیک | [../logistics/01-shipment-loading-processes.md](../logistics/01-shipment-loading-processes.md) |
| تحلیل وضعیت | [../02-current-state-analysis.md](../02-current-state-analysis.md) |
| اهداف فاز ۱ | [../03-phase-1-goals.md](../03-phase-1-goals.md) |

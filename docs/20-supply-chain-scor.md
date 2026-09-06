# ۲۰. زنجیره تامین SADRA بر اساس مدل SCOR

[← فهرست اصلی](./README.md) | [بازرگانی](./commerce/README.md) | [لجستیک](./logistics/README.md)

---

## ۲۰.۱ چشم‌انداز

SADRA یک پلتفرم **زنجیره تامین (Supply Chain)** است — نه فقط حمل‌ونقل یا فقط فروش.

هر معامله موفق این مسیر را طی می‌کند:

```
فروش (Export Sales) → خرید/تدارکات (Procurement) → حمل (Logistics) → تحویل (Delivery)
```

چارچوب مرجع استاندارد جهانی: **SCOR** (Supply Chain Operations Reference) — مدل کلاسیک ASCM/Supply Chain Council.

---

## ۲۰.۲ مدل SCOR (کلاسیک)

SCOR فرایندهای کسب‌وکار لازم برای برآورده کردن تقاضای مشتری را در شش حوزه توصیف می‌کند:

| فرایند SCOR | معنی | نقش در SADRA |
|-------------|------|--------------|
| **Plan** | برنامه‌ریزی تقاضا و عرضه | پیش‌بینی سفارش، ظرفیت کارخانه، برنامه حمل |
| **Source** | تامین / خرید | تدارکات، خرید از کارخانه/تامین‌کننده، SRM |
| **Make** | تولید / تبدیل | تولید موجود، Made-to-Order، سفارش سفارشی |
| **Deliver** | تحویل به مشتری | فروش صادراتی، لجستیک، ۱۹ فرایند بارگیری، مدارک حمل |
| **Return** | مرجوعی / پس از تحویل | فاز ۲ (اختلاف، مرجوعی، ادعا) |
| **Enable** | توانمندساز | CRM، Auth، AI، مالی، مستندات، SLA، مانیتورینگ |

```
                    ┌──────────┐
                    │  PLAN    │
                    └────┬─────┘
         ┌───────────────┼───────────────┐
         ▼               ▼               ▼
   ┌──────────┐   ┌──────────┐   ┌──────────┐
   │  SOURCE  │──▶│   MAKE   │──▶│ DELIVER  │
   │  خرید    │   │  تولید   │   │ فروش+حمل │
   └──────────┘   └──────────┘   └────┬─────┘
                                      │
                                      ▼
                               ┌──────────┐
                               │  RETURN  │ (فاز ۲)
                               └──────────┘

         ════════════ ENABLE (CRM / SRM / AI / Finance) ════════════
```

---

## ۲۰.۳ نگاشت فرایند واقعی SADRA به SCOR

### Deliver — سمت مشتری (فروش + لجستیک)

| لایه | داکیومنت | محتوا |
|------|----------|-------|
| فروش صادراتی | [C3](./commerce/03-export-sales-procedure.md) | ۱۱ مرحله: Inquiry تا Shipping Documents |
| شرایط پرداخت | C3 | ۲۰/۸۰ یا ۱۰۰٪ Advance بر اساس محصول |
| حمل و بارگیری | [L1](./logistics/01-shipment-loading-processes.md) | ۱۹ فرایند Timeline |
| مدارک حمل | C3 مرحله ۱۱ | B/L، Invoice، Packing List، COO، Quality |

### Source — تدارکات و خرید

| فعالیت | توضیح | اولویت |
|--------|-------|--------|
| شناسایی تامین‌کننده/کارخانه | از SRM و قراردادهای کارخانه | P0 |
| درخواست خرید (PO) | پس از تأیید پیش‌پرداخت مشتری | P0 |
| پیگیری تامین | وضعیت تولید/آماده‌سازی کالا | P0 |
| کنترل کیفیت قبل از بارگیری | گواهی کیفیت | P1 |

### Make — تولید

| فعالیت | توضیح | داکیومنت |
|--------|-------|----------|
| موجود در انبار | فروش مستقیم | [C2](./19-product-catalog-and-manufacturing.md) |
| Made-to-Order | تولید پس از سفارش | C2 |
| محصول اختصاصی مشتری | ثبت توسط ادمین | C2 |

### Plan — برنامه‌ریزی

| فعالیت | توضیح | اولویت |
|--------|-------|--------|
| ظرفیت کارخانه | زمان تولید تقریبی | P1 |
| برنامه حمل | ETA، مسیر، مرز | P0 |
| موجودی محصول | موجود / ناموجود | P0 |

### Enable — زیرساخت فرایندی

| ماژول | نقش |
|-------|-----|
| CRM | Lead و جلوگیری از از دست رفتن مشتری |
| SRM | تامین‌کننده و تابلو نرخ |
| AI | Quotation / PI / پاسخ اولیه |
| Finance | تأیید SWIFT و پرداخت‌ها |
| Auth / Roles | تفکیک Buyer، Seller ops، Logistics، Finance |

---

## ۲۰.۴ جریان End-to-End یک معامله

```
[Buyer] Inquiry (محصول، مقدار، بندر، end use)
    ↓
[Seller] Quotation (+ payment terms از دسته محصول)
    ↓
[Buyer] LOI → [Seller] PI → [Buyer] Confirm PI
    ↓
[طرفین] Sales Contract
    ↓
[Buyer] Advance Payment (T/T + SWIFT)
    ↓
[Finance] Payment Confirmation  ─── قفل باز می‌شود
    ↓
┌───────────────────────────────────────────┐
│  SOURCE: خرید از کارخانه / تامین‌کننده   │
│  MAKE:   تولید در صورت نیاز               │
│  DELIVER/Logistics: ۱۹ فرایند بارگیری    │
└───────────────────────────────────────────┘
    ↓
[Seller] Loading & Shipment (C3-09 / L1)
    ↓
[Buyer] Balance Payment (اگر ۲۰/۸۰ باشد)
    ↓
[Seller] Shipping Documents (پس از پرداخت کامل)
    ↓
تحویل نهایی / تکمیل سفارش
```

---

## ۲۰.۵ بخش‌های سیستم بر اساس زنجیره تامین

| بخش سازمانی | ماژول نرم‌افزاری | SCOR |
|-------------|------------------|------|
| فروش / بازرگانی | Export Sales + CRM + کاتالوگ | Deliver (Order) + Enable |
| تدارکات / خرید | Procurement / Factory PO | Source |
| تولید | Manufacturing Orders | Make |
| لجستیک | Shipment + Driver + Tracking | Deliver (Fulfill) |
| مالی | Payments / SWIFT verify | Enable |
| پشتیبانی | Operator + AI | Enable |

---

## ۲۰.۶ نیازمندی‌های ماژول تدارکات (Procurement) — فاز ۱ حداقل

| کد | عنوان | توضیح | اولویت |
|----|-------|-------|--------|
| PRC-01 | ایجاد PO از سفارش صادراتی | پس از payment_confirmed | P0 |
| PRC-02 | اتصال به Factory | ارجاع به کارخانه قراردادشده | P0 |
| PRC-03 | وضعیت تامین | pending → confirmed → ready_for_loading | P0 |
| PRC-04 | لینک به Shipment | وقتی کالا آماده شد → شروع L1 | P0 |
| PRC-05 | اعلان به لجستیک | آماده بارگیری | P0 |

```sql
CREATE TYPE procurement_status AS ENUM (
  'draft', 'sent_to_factory', 'confirmed', 'in_production',
  'ready_for_loading', 'cancelled'
);

CREATE TABLE procurement_orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  export_order_id UUID NOT NULL REFERENCES export_sales_orders(id),
  factory_id UUID REFERENCES factories(id),
  status procurement_status DEFAULT 'draft',
  expected_ready_date DATE,
  notes TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

## ۲۰.۷ KPIهای زنجیره تامین (نمونه SCOR)

| Attribute SCOR | شاخص پیشنهادی SADRA | هدف فاز ۱ |
|----------------|---------------------|-----------|
| Reliability | Perfect Order (فروش→تحویل بدون خطا) | پایه اندازه‌گیری |
| Responsiveness | زمان Inquiry تا Quotation | < ۵ دقیقه (سیستمی) |
| Responsiveness | زمان Payment Confirm تا شروع بارگیری | قابل اندازه‌گیری |
| Cost | هزینه عملیاتی هر سفارش | کاهش دخالت دستی |
| Asset | نرخ تبدیل Lead به Contract | ≥ ۳۰٪ |

---

## ۲۰.۸ Acceptance Criteria — زنجیره تامین

- [ ] هر سفارش صادراتی مسیر کامل فروش → خرید → حمل → تحویل را در سیستم داشته باشد
- [ ] بدون تأیید پیش‌پرداخت، تدارکات/حمل شروع نشود
- [ ] Timeline فروش (۱۱ مرحله) و Timeline لجستیک (۱۹ فرایند) به هم لینک باشند
- [ ] شرایط پرداخت بر اساس دسته محصول اعمال شود
- [ ] نقش‌های Finance، Logistics، Sales در سیستم تفکیک شده باشند
- [ ] مدارک حمل فقط پس از پرداخت کامل صادر شوند

---

## ۲۰.۹ ارتباط با داکیومنت‌های دیگر

| موضوع | فایل |
|-------|------|
| فروش صادراتی | [commerce/03-export-sales-procedure.md](./commerce/03-export-sales-procedure.md) |
| CRM / SRM / AI | [18-crm-srm-ai-assistant.md](./18-crm-srm-ai-assistant.md) |
| کاتالوگ و تولید | [19-product-catalog-and-manufacturing.md](./19-product-catalog-and-manufacturing.md) |
| ۱۹ فرایند بارگیری | [logistics/01-shipment-loading-processes.md](./logistics/01-shipment-loading-processes.md) |
| مدل داده | [08-data-model.md](./08-data-model.md) |

# C3. پروسیجر فروش صادراتی (Export Sales Procedure)

[← فهرست بازرگانی](./README.md) | [← فهرست اصلی](../README.md)

---

## C3.1 هدف

این سند، **پروسیجر رسمی فروش صادراتی** توافق‌شده بین **Seller (SADRA)** و **Buyer (مشتری)** را تعریف می‌کند تا اجرای هر معامله شفاف، قابل پیگیری و سیستمی باشد.

> هر معامله‌ای که به سفارش و فروش منجر شود، پس از این پروسیجر وارد زنجیره **خرید (تدارکات) → حمل (لجستیک) → تحویل** می‌شود.  
> چارچوب کلی: [زنجیره تامین SCOR](../20-supply-chain-scor.md)

---

## C3.2 پروسیجر ۱۱ مرحله‌ای فروش صادراتی

```
01 Inquiry → 02 Quotation → 03 LOI → 04 PI → 05 Confirm PI
     → 06 Sales Contract → 07 Advance Payment → 08 Payment Confirmation
     → 09 Loading & Shipment → 10 Balance Payment → 11 Shipping Documents
```

| # | مرحله | کد سیستم | توضیح | مسئول اصلی |
|---|-------|----------|-------|------------|
| 1 | **Inquiry** | `inquiry` | خریدار استعلام رسمی می‌دهد: مشخصات محصول، مقدار، شرایط تحویل، بندر مقصد، کاربرد نهایی | Buyer |
| 2 | **Quotation** | `quotation` | فروشنده پیشنهاد رسمی صادر می‌کند: قیمت، شرایط تحویل، شرایط پرداخت، زمان تحویل، اعتبار پیشنهاد | Seller / AI / اپراتور |
| 3 | **Letter of Intent (LOI)** | `loi` | خریدار پذیرش پیشنهاد را با صدور LOI تأیید می‌کند | Buyer |
| 4 | **Proforma Invoice (PI)** | `proforma` | فروشنده پیش‌فاکتور با مشخصات توافق‌شده ارسال می‌کند | Seller |
| 5 | **Confirmation of PI** | `pi_confirmed` | خریدار PI را بررسی، امضا و برمی‌گرداند | Buyer |
| 6 | **Sales Contract** | `sales_contract` | طرفین قرارداد فروش را امضا می‌کنند | Seller + Buyer |
| 7 | **Advance Payment** | `advance_payment` | فروشنده مشخصات بانکی می‌دهد؛ خریدار پیش‌پرداخت را واریز و SWIFT می‌فرستد | Buyer |
| 8 | **Payment Confirmation** | `payment_confirmed` | پس از تأیید دریافت پیش‌پرداخت، فروشنده زمان‌بندی و شروع حمل را آغاز می‌کند | Seller / Finance |
| 9 | **Loading and Shipment** | `loading_shipment` | آماده‌سازی، بارگیری و حمل طبق برنامه — ورود به **لجستیک (۱۹ فرایند)** | Logistics |
| 10 | **Balance Payment** | `balance_payment` | خریدار مانده مبلغ را طبق قرارداد واریز می‌کند | Buyer |
| 11 | **Shipping Documents** | `shipping_documents` | پس از تأیید پرداخت کامل، مجموعه مدارک حمل صادر می‌شود | Seller |

### مدارک مرحله ۱۱ (Shipping Documents)

| مدرک | توضیح |
|------|-------|
| Bill of Lading (B/L) | بارنامه |
| Commercial Invoice | فاکتور تجاری |
| Packing List | لیست بسته‌بندی |
| Certificate of Origin | گواهی مبدأ |
| Quality Certificates | گواهی‌های کیفیت (در صورت نیاز) |

---

## C3.3 شرایط پرداخت (همه از طریق T/T)

| # | محصول | شرایط پرداخت | کد `payment_term` |
|---|--------|--------------|-------------------|
| 1 | Base Oil Recycled | ۲۰٪ پیش‌پرداخت / ۸۰٪ مانده | `advance_20_balance_80` |
| 2 | Bitumen | ۲۰٪ پیش‌پرداخت / ۸۰٪ مانده | `advance_20_balance_80` |
| 3 | Caustic Soda | ۲۰٪ پیش‌پرداخت / ۸۰٪ مانده | `advance_20_balance_80` |
| 4 | Urea | ۲۰٪ پیش‌پرداخت / ۸۰٪ مانده | `advance_20_balance_80` |
| 5 | Sulphur | ۱۰۰٪ پیش‌پرداخت | `advance_100` |
| 6 | Base Oil Virgin | ۱۰۰٪ پیش‌پرداخت | `advance_100` |
| 7 | LPG (Liquefied Petroleum Gas) | ۱۰۰٪ پیش‌پرداخت | `advance_100` |
| 8 | Polymers | ۱۰۰٪ پیش‌پرداخت | `advance_100` |
| 9 | Glycols | ۱۰۰٪ پیش‌پرداخت | `advance_100` |

**نکته پیاده‌سازی:** شرایط پرداخت باید روی **محصول / دسته محصول** در کاتالوگ تنظیم شود و در Quotation و PI به‌صورت خودکار اعمال گردد.

---

## C3.4 اتصال به زنجیره پس از فروش

پس از مرحله ۸ (تأیید پیش‌پرداخت)، معامله وارد زنجیره تامین می‌شود:

```
فروش صادراتی (C3: مراحل ۱–۸)
        ↓
   خرید / تدارکات (Source)     ← تامین از کارخانه/تامین‌کننده
        ↓
   تولید در صورت نیاز (Make)   ← محصول ناموجود / سفارشی
        ↓
   حمل و بارگیری (Deliver)     ← L1: ۱۹ فرایند لجستیک
        ↓
   C3 مرحله ۹–۱۱               ← بارگیری، مانده، مدارک حمل
        ↓
   تحویل نهایی به مشتری
```

---

## C3.5 نیازمندی‌های عملکردی

| کد | عنوان | توضیح | اولویت |
|----|-------|-------|--------|
| EXP-01 | ثبت Inquiry | فرم استعلام: محصول، مقدار، Incoterm، بندر مقصد، end use | P0 |
| EXP-02 | صدور Quotation | قیمت، شرایط پرداخت (بر اساس محصول)، زمان تحویل، اعتبار | P0 |
| EXP-03 | ثبت LOI | آپلود/ثبت Letter of Intent توسط مشتری | P0 |
| EXP-04 | تولید PI | پیش‌فاکتور رسمی از روی Quotation + LOI | P0 |
| EXP-05 | تأیید PI | امضای دیجیتال/آپلود PI امضاشده | P0 |
| EXP-06 | قرارداد فروش | تولید/آپلود Sales Contract | P0 |
| EXP-07 | پیش‌پرداخت + SWIFT | ثبت مبلغ، ارز، مرجع SWIFT | P0 |
| EXP-08 | تأیید مالی | Finance وضعیت پرداخت را تایید کند → unlock حمل | P0 |
| EXP-09 | شروع لجستیک | ایجاد Shipment از روی سفارش فروش تاییدشده | P0 |
| EXP-10 | مانده پرداخت | ردیابی balance بر اساس payment_term محصول | P0 |
| EXP-11 | صدور مدارک حمل | B/L، Invoice، Packing List، COO، Quality | P0 |
| EXP-12 | Timeline فروش | نمایش ۱۱ مرحله برای مشتری، اپراتور، ادمین | P0 |
| EXP-13 | قوانین پرداخت محصول | ماتریس ۲۰/۸۰ و ۱۰۰٪ روی دسته محصول | P0 |

---

## جزئیات پیاده‌سازی

### State Machine — سفارش فروش صادراتی

```
inquiry → quotation → loi → proforma → pi_confirmed
  → sales_contract → advance_payment → payment_confirmed
  → loading_shipment → balance_payment → shipping_documents → completed
```

قوانین:
- بدون `payment_confirmed` نمی‌توان `loading_shipment` را شروع کرد
- برای محصولات `advance_100`، پس از تأیید پیش‌پرداخت، `balance_payment` خودکار `skipped` می‌شود
- صدور مدارک حمل (مرحله ۱۱) فقط پس از تأیید **پرداخت کامل**

### Database

```sql
CREATE TYPE export_sales_status AS ENUM (
  'inquiry', 'quotation', 'loi', 'proforma', 'pi_confirmed',
  'sales_contract', 'advance_payment', 'payment_confirmed',
  'loading_shipment', 'balance_payment', 'shipping_documents', 'completed', 'cancelled'
);

CREATE TYPE payment_term_type AS ENUM (
  'advance_20_balance_80',
  'advance_100'
);

CREATE TABLE export_sales_orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  order_number VARCHAR(50) UNIQUE NOT NULL,
  shipper_id UUID NOT NULL REFERENCES shipper_profiles(id),
  lead_id UUID REFERENCES leads(id),
  status export_sales_status DEFAULT 'inquiry',
  -- محصول
  product_id UUID REFERENCES products(id),
  product_name VARCHAR(255) NOT NULL,
  product_category VARCHAR(100),
  quantity DECIMAL(15,3) NOT NULL,
  unit VARCHAR(30),
  end_use TEXT,
  -- تحویل
  delivery_terms VARCHAR(50),      -- Incoterms: FOB, CIF, ...
  destination_port VARCHAR(255),
  delivery_schedule TEXT,
  -- مالی
  currency currency NOT NULL,
  unit_price DECIMAL(15,2),
  total_amount DECIMAL(15,2),
  payment_term payment_term_type NOT NULL,
  advance_percent INT NOT NULL,    -- 20 یا 100
  advance_amount DECIMAL(15,2),
  balance_amount DECIMAL(15,2) DEFAULT 0,
  quotation_valid_until TIMESTAMPTZ,
  -- اسناد
  loi_file_url VARCHAR(500),
  pi_file_url VARCHAR(500),
  pi_signed_file_url VARCHAR(500),
  contract_file_url VARCHAR(500),
  -- اتصال به لجستیک
  shipment_request_id UUID REFERENCES shipment_requests(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE export_sales_payments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  export_order_id UUID NOT NULL REFERENCES export_sales_orders(id),
  payment_type VARCHAR(20) NOT NULL,  -- advance | balance
  amount DECIMAL(15,2) NOT NULL,
  currency currency NOT NULL,
  swift_reference VARCHAR(100),
  proof_file_url VARCHAR(500),
  status VARCHAR(20) DEFAULT 'pending', -- pending | verified | rejected
  verified_by UUID REFERENCES users(id),
  verified_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE export_sales_documents (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  export_order_id UUID NOT NULL REFERENCES export_sales_orders(id),
  doc_type VARCHAR(50) NOT NULL, -- bl | commercial_invoice | packing_list | coo | quality
  file_url VARCHAR(500),
  issued_at TIMESTAMPTZ,
  status VARCHAR(20) DEFAULT 'draft'
);

-- شرایط پرداخت روی دسته محصول
CREATE TABLE product_payment_terms (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  product_category VARCHAR(100) UNIQUE NOT NULL,
  payment_term payment_term_type NOT NULL,
  advance_percent INT NOT NULL,
  notes TEXT
);

INSERT INTO product_payment_terms (product_category, payment_term, advance_percent) VALUES
('base_oil_recycled', 'advance_20_balance_80', 20),
('bitumen', 'advance_20_balance_80', 20),
('caustic_soda', 'advance_20_balance_80', 20),
('urea', 'advance_20_balance_80', 20),
('sulphur', 'advance_100', 100),
('base_oil_virgin', 'advance_100', 100),
('lpg', 'advance_100', 100),
('polymers', 'advance_100', 100),
('glycols', 'advance_100', 100);
```

### API Endpoints

| Method | Endpoint | Roles | توضیح |
|--------|----------|-------|-------|
| POST | `/v1/export/inquiries` | shipper, guest | ثبت Inquiry |
| POST | `/v1/export/orders/:id/quotation` | operator, admin, ai | صدور Quotation |
| POST | `/v1/export/orders/:id/loi` | shipper | آپلود LOI |
| POST | `/v1/export/orders/:id/proforma` | operator, admin, ai | تولید PI |
| POST | `/v1/export/orders/:id/confirm-pi` | shipper | تأیید PI |
| POST | `/v1/export/orders/:id/contract` | operator, admin | قرارداد |
| POST | `/v1/export/orders/:id/payments` | shipper | ثبت پرداخت + SWIFT |
| PATCH | `/v1/export/orders/:id/payments/:pid/verify` | finance, admin | تأیید پرداخت |
| POST | `/v1/export/orders/:id/start-shipment` | admin, logistics | شروع لجستیک |
| POST | `/v1/export/orders/:id/shipping-documents` | admin, operator | صدور مدارک |
| GET | `/v1/export/orders/:id/timeline` | shipper, operator, admin | Timeline ۱۱ مرحله |

### Frontend Pages

```
/export/inquiry                 ثبت استعلام
/dashboard/export-orders        لیست سفارش‌های صادراتی
/dashboard/export-orders/:id    Timeline ۱۱ مرحله + پرداخت + مدارک
/operator/export/               مدیریت فروش صادراتی
/finance/payments               تأیید SWIFT و پیش‌پرداخت
/admin/export/payment-terms     ماتریس شرایط پرداخت محصولات
```

### UI — Timeline فروش صادراتی (مشتری)

```
┌─────────────────────────────────────────────────────────┐
│  سفارش صادراتی #EXP-2026-0112 — Bitumen                 │
│  شرایط پرداخت: ۲۰٪ Advance / ۸۰٪ Balance (T/T)         │
├─────────────────────────────────────────────────────────┤
│  ✅ 01 Inquiry                                              │
│  ✅ 02 Quotation                                            │
│  ✅ 03 LOI                                                  │
│  ✅ 04 Proforma Invoice                                     │
│  ✅ 05 PI Confirmed                                         │
│  ✅ 06 Sales Contract                                       │
│  ✅ 07 Advance Payment (SWIFT: XXX)                         │
│  🔵 08 Payment Confirmation — در انتظار تأیید مالی         │
│  ⬜ 09 Loading & Shipment                                   │
│  ⬜ 10 Balance Payment                                      │
│  ⬜ 11 Shipping Documents                                   │
└─────────────────────────────────────────────────────────┘
```

### Acceptance Criteria

- [ ] مشتری بتواند Inquiry ثبت کند
- [ ] Quotation شامل شرایط پرداخت مطابق دسته محصول باشد
- [ ] مسیر ۱۱ مرحله‌ای در Timeline برای مشتری/اپراتور/ادمین نمایش داده شود
- [ ] بدون تأیید پیش‌پرداخت، بارگیری شروع نشود
- [ ] برای محصولات ۱۰۰٪ Advance، مرحله Balance خودکار skip شود
- [ ] پس از تأیید پرداخت کامل، مدارک حمل قابل صدور باشند
- [ ] مرحله ۹ به ماژول لجستیک (۱۹ فرایند) متصل شود

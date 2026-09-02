# C2. کاتالوگ محصولات، تولید کارخانه‌ای و پیش‌فاکتور

[← فهرست بازرگانی](./commerce/README.md) | [← فهرست اصلی](./README.md)

---

## 19.1 چشم‌انداز

پلتفرم SADRA علاوه بر **حمل‌ونقل**، با **کارخانه‌های تولیدی** هم قرارداد دارد. مشتری باید بتواند:

1. **اجناس موجود** را در سایت ببیند، نرخ ببیند و پیش‌فاکتور بزند
2. اگر جنس موردنظر **وجود نداشت**، **درخواست سفارش تولید** بزند
3. پس از تولید در کارخانه، **نرخ جدید** برایش تولید شود
4. در پیش‌فاکتور **شفاف** ببیند: این محصول موجود نیست، X روز زمان می‌برد، Y هزینه دارد، باید پرداخت کند

همچنین **ادمین** می‌تواند برای یک مشتری خاص محصول ثبت کند و پیش‌فاکتور اختصاصی صادر کند — مشتری **فقط پیش‌فاکتور** را می‌بیند (نه کل کاتالوگ).

---

## 19.2 وضعیت فعلی vs هدف

### فعلی (دستی)

```
مشتری تماس می‌گیرد → اپراتور با کارخانه هماهنگ می‌کند
     → قیمت دستی → پیش‌فاکتور کاغذی/اکسل
     → مشتری نمی‌داند چه محصولاتی موجود است
```

### هدف (سیستمی)

```
مشتری → کاتالوگ محصولات (سایت)
     ├─ محصول موجود → نرخ فوری → پیش‌فاکتور
     └─ محصول ناموجود → درخواست سفارش تولید
              ↓
         کارخانه تولید می‌کند
              ↓
         نرخ جدید تولید → پیش‌فاکتور با جزئیات تولید
              ↓
     (ادمین) محصول اختصاصی برای مشتری → فقط پیش‌فاکتور نمایش
```

---

## 19.3 انواع محصول

| نوع | کد | توضیح | نمایش در کاتالوگ عمومی |
|-----|-----|-------|----------------------|
| **موجود (In Stock)** | `in_stock` | در انبار/کارخانه موجود، نرخ ثابت | ✅ بله |
| **قابل تولید (Made to Order)** | `made_to_order` | با سفارش تولید می‌شود، زمان و هزینه مشخص | ✅ بله (با برچسب «سفارشی») |
| **اختصاصی مشتری (Customer-Specific)** | `customer_specific` | فقط برای یک مشتری، توسط ادمین ثبت | ❌ خیر — فقط پیش‌فاکتور |

---

## 19.4 نیازمندی‌های عملکردی

### 19.4.1 کاتالوگ محصولات (مشتری)

| کد | عنوان | توضیح | اولویت |
|----|-------|-------|--------|
| PRD-01 | مشاهده کاتالوگ | لیست اجناس با تصویر، مشخصات، نرخ | P0 |
| PRD-02 | جستجو و فیلتر | بر اساس دسته، کارخانه، قیمت، موجودی | P0 |
| PRD-03 | جزئیات محصول | مشخصات فنی، نرخ، زمان تحویل | P0 |
| PRD-04 | پیش‌فاکتور محصول موجود | انتخاب تعداد → پیش‌فاکتور فوری | P0 |
| PRD-05 | درخواست سفارش تولید | اگر محصول نبود → فرم سفارش سفارشی | P0 |
| PRD-06 | پیگیری سفارش تولید | وضعیت: درخواست → تولید → آماده → ارسال | P0 |
| PRD-07 | مشاهده پیش‌فاکتور اختصاصی | مشتری فقط پیش‌فاکتورهای خودش را ببیند | P0 |
| PRD-08 | ترکیب حمل + محصول | پیش‌فاکتور شامل قیمت محصول + هزینه حمل | P1 |

### 19.4.2 سفارش تولید سفارشی (Custom Manufacturing)

| کد | عنوان | توضیح | اولویت |
|----|-------|-------|--------|
| MFG-01 | ثبت درخواست محصول ناموجود | مشخصات، تعداد، توضیحات، فایل پیوست | P0 |
| MFG-02 | ارجاع به کارخانه | سیستم درخواست را به کارخانه مرتبط ارسال | P0 |
| MFG-03 | برآورد زمان تولید | کارخانه/ادمین: X روز | P0 |
| MFG-04 | برآورد هزینه تولید | کارخانه/ادمین: Y تومان/ارز | P0 |
| MFG-05 | تولید نرخ پس از تایید | نرخ جدید برای محصول سفارشی | P0 |
| MFG-06 | پیش‌فاکتور با جزئیات تولید | نمایش: «موجود نیست»، زمان، هزینه، پیش‌پرداخت | P0 |
| MFG-07 | اعلان به مشتری | وقتی نرخ آماده شد | P0 |
| MFG-08 | پیش‌پرداخت/پرداخت | مشتری باید پرداخت کند (فاز ۲: آنلاین) | P1 |

### 19.4.3 مدیریت ادمین

| کد | عنوان | توضیح | اولویت |
|----|-------|-------|--------|
| ADM-PRD-01 | ثبت محصول برای مشتری خاص | محصول `customer_specific` فقط برای یک Shipper | P0 |
| ADM-PRD-02 | صدور پیش‌فاکتور اختصاصی | ادمین پیش‌فاکتور می‌سازد | P0 |
| ADM-PRD-03 | نمایش فقط پیش‌فاکتور به مشتری | مشتری کاتالوگ نمی‌بیند، فقط لینک پیش‌فاکتور | P0 |
| ADM-PRD-04 | مدیریت کارخانه‌ها | ثبت کارخانه، قرارداد، ظرفیت | P0 |
| ADM-PRD-05 | مدیریت درخواست‌های تولید | تایید/رد، ارجاع به کارخانه | P0 |
| ADM-PRD-06 | تعیین نرخ محصول سفارشی | پس از دریافت قیمت از کارخانه | P0 |

---

## 19.5 ساختار پیش‌فاکتور

### 19.5.1 پیش‌فاکتور محصول موجود

```json
{
  "type": "standard",
  "items": [
    {
      "productId": "uuid",
      "name": "ورق فولادی ۲mm",
      "quantity": 100,
      "unitPrice": 500000,
      "currency": "IRR",
      "availability": "in_stock",
      "estimatedDeliveryDays": 3
    }
  ],
  "subtotal": 50000000,
  "shippingCost": 2000000,
  "total": 52000000
}
```

### 19.5.2 پیش‌فاکتور محصول سفارشی (ناموجود — نیاز به تولید)

```json
{
  "type": "custom_manufacturing",
  "items": [
    {
      "productId": null,
      "customOrderId": "uuid",
      "name": "پروفیل آلومینیومی سفارشی ۵۰×۳۰",
      "quantity": 500,
      "availability": "not_available",
      "manufacturingRequired": true,
      "manufacturingDays": 21,
      "manufacturingCost": 150000000,
      "prepaymentRequired": true,
      "prepaymentAmount": 75000000,
      "prepaymentPercent": 50,
      "notes": "این محصول در انبار موجود نیست. پس از تولید در کارخانه X، ارسال خواهد شد."
    }
  ],
  "warnings": [
    "⚠️ این محصول موجود نیست و نیاز به تولید دارد.",
    "⏱ زمان تولید: ۲۱ روز کاری",
    "💰 هزینه تولید: ۱۵۰,۰۰۰,۰۰۰ تومان",
    "💳 پیش‌پرداخت ۵۰٪ (۷۵,۰۰۰,۰۰۰ تومان) الزامی است."
  ],
  "total": 150000000
}
```

### 19.5.3 پیش‌فاکتور اختصاصی ادمین (فقط نمایش به مشتری)

```json
{
  "type": "admin_private",
  "visibility": "customer_only",
  "customerId": "shipper-uuid",
  "items": [
    {
      "name": "محصول اختصاصی — قرارداد ۱۴۰۵/۰۶",
      "quantity": 1,
      "unitPrice": 250000000,
      "notes": "ثبت‌شده توسط ادمین — فقط برای شما"
    }
  ],
  "shareToken": "abc123xyz",
  "expiresAt": "2026-10-01"
}
```

مشتری از لینک `/proforma/abc123xyz` فقط این پیش‌فاکتور را می‌بیند — بدون دسترسی به کاتالوگ.

---

## 19.6 Flowها

### Flow 1: محصول موجود

```
مشتری → /products → انتخاب محصول → تعداد
     → مشاهده نرخ → [صدور پیش‌فاکتور]
     → PDF + ذخیره در CRM
```

### Flow 2: محصول ناموجود — درخواست تولید

```
مشتری → جستجو → «محصول یافت نشد»
     → [درخواست سفارش تولید]
     → فرم: نام، مشخصات، تعداد، فایل نقشه/نمونه
     → ثبت CustomManufacturingOrder
     → ادمین/کارخانه: برآورد زمان + هزینه
     → سیستم: تولید نرخ + پیش‌فاکتور با جزئیات تولید
     → اعلان به مشتری
     → مشتری: مشاهده پیش‌فاکتور (موجود نیست، X روز، Y هزینه، پیش‌پرداخت)
     → پرداخت پیش‌پرداخت → کارخانه تولید
     → پس از تولید: آماده ارسال + حمل
```

### Flow 3: ادمین — محصول اختصاصی برای مشتری

```
ادمین → /admin/products/custom
     → انتخاب مشتری (Shipper)
     → ثبت محصول (نام، مشخصات، قیمت)
     → [صدور پیش‌فاکتور]
     → تولید لینک اختصاصی
     → ارسال لینک به مشتری (SMS/ایمیل)
     → مشتری: فقط پیش‌فاکتور را می‌بیند (بدون کاتالوگ)
```

---

## 19.7 یکپارچگی با کارخانه‌ها

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   مشتری (Web)   │────▶│  Product Module │────▶│   Factory A     │
│   کاتالوگ       │     │  Manufacturing  │     │   Factory B     │
└─────────────────┘     └────────┬────────┘     └─────────────────┘
                                 │
                    ┌────────────┼────────────┐
                    ▼            ▼            ▼
              ┌──────────┐ ┌──────────┐ ┌──────────┐
              │   CRM    │ │ Proforma │ │ Shipment │
              │  Lead    │ │ Invoice  │ │  (حمل)   │
              └──────────┘ └──────────┘ └──────────┘
```

| اطلاعات کارخانه | فیلد |
|-----------------|------|
| نام کارخانه | `factory.name` |
| محصولات قابل تولید | `factory.capabilities[]` |
| زمان تولید معمول | `factory.avgProductionDays` |
| قرارداد فعال | `factory.contractStatus` |

---

## جزئیات پیاده‌سازی

### Database Schema

```sql
CREATE TYPE product_availability AS ENUM (
  'in_stock', 'made_to_order', 'customer_specific', 'out_of_stock'
);

CREATE TYPE custom_order_status AS ENUM (
  'pending', 'quoted', 'prepayment_received', 'in_production',
  'ready', 'shipped', 'cancelled'
);

-- کارخانه‌ها
CREATE TABLE factories (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  contact_person VARCHAR(255),
  phone VARCHAR(20),
  email VARCHAR(255),
  address TEXT,
  capabilities JSONB DEFAULT '[]',  -- دسته محصولات
  avg_production_days INT,
  contract_status VARCHAR(20) DEFAULT 'active',
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- محصولات
CREATE TABLE products (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  factory_id UUID REFERENCES factories(id),
  sku VARCHAR(50) UNIQUE,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  category VARCHAR(100),
  specifications JSONB DEFAULT '{}',
  unit VARCHAR(20) DEFAULT 'عدد',
  availability product_availability NOT NULL,
  -- فقط برای customer_specific
  customer_id UUID REFERENCES shipper_profiles(id),
  -- نرخ
  unit_price DECIMAL(15,2),
  currency currency DEFAULT 'IRR',
  -- تولید سفارشی
  manufacturing_days INT,           -- اگر made_to_order
  manufacturing_base_cost DECIMAL(15,2),
  -- نمایش
  is_public BOOLEAN DEFAULT true,   -- false = customer_specific
  image_urls JSONB DEFAULT '[]',
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- درخواست سفارش تولید (محصول ناموجود)
CREATE TABLE custom_manufacturing_orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  shipper_id UUID NOT NULL REFERENCES shipper_profiles(id),
  lead_id UUID REFERENCES leads(id),
  -- مشخصات درخواستی
  product_name VARCHAR(255) NOT NULL,
  description TEXT,
  specifications JSONB DEFAULT '{}',
  quantity DECIMAL(10,2) NOT NULL,
  unit VARCHAR(20),
  attachment_urls JSONB DEFAULT '[]',
  -- ارجاع
  factory_id UUID REFERENCES factories(id),
  assigned_to UUID REFERENCES users(id),
  -- برآورد (پس از تایید کارخانه/ادمین)
  estimated_production_days INT,
  estimated_cost DECIMAL(15,2),
  currency currency,
  -- وضعیت
  status custom_order_status DEFAULT 'pending',
  -- پس از تولید → محصول جدید
  resulting_product_id UUID REFERENCES products(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- پیش‌فاکتور (گسترش از doc 18)
CREATE TABLE proforma_invoices (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  invoice_number VARCHAR(50) UNIQUE,
  type VARCHAR(30) NOT NULL,  -- standard | custom_manufacturing | admin_private | shipping_only
  visibility VARCHAR(20) DEFAULT 'customer',  -- customer | customer_only (لینک اختصاصی)
  share_token VARCHAR(64) UNIQUE,  -- برای لینک اختصاصی
  -- ارتباطات
  lead_id UUID REFERENCES leads(id),
  shipper_id UUID NOT NULL REFERENCES shipper_profiles(id),
  custom_order_id UUID REFERENCES custom_manufacturing_orders(id),
  shipment_request_id UUID REFERENCES shipment_requests(id),
  -- مبالغ
  items JSONB NOT NULL,
  subtotal DECIMAL(15,2),
  shipping_cost DECIMAL(15,2) DEFAULT 0,
  total_amount DECIMAL(15,2) NOT NULL,
  currency currency NOT NULL,
  -- تولید سفارشی
  manufacturing_warnings JSONB,  -- آرایه هشدارها
  prepayment_required BOOLEAN DEFAULT false,
  prepayment_amount DECIMAL(15,2),
  prepayment_percent INT,
  -- متادیتا
  generated_by VARCHAR(20) DEFAULT 'system',  -- system | ai | admin
  created_by UUID REFERENCES users(id),
  approved_by UUID REFERENCES users(id),
  status VARCHAR(20) DEFAULT 'draft',  -- draft | sent | viewed | accepted | expired | paid
  valid_until TIMESTAMPTZ,
  pdf_url VARCHAR(500),
  viewed_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_products_public ON products (is_public, is_active) WHERE is_public = true;
CREATE INDEX idx_products_customer ON products (customer_id) WHERE customer_id IS NOT NULL;
CREATE INDEX idx_proforma_share_token ON proforma_invoices (share_token) WHERE share_token IS NOT NULL;
```

### API Endpoints

#### کاتالوگ (عمومی / مشتری)

| Method | Endpoint | Roles | توضیح |
|--------|----------|-------|-------|
| GET | `/v1/products` | public | لیست محصولات عمومی |
| GET | `/v1/products/:id` | public | جزئیات محصول |
| GET | `/v1/products/search` | public | جستجو |
| POST | `/v1/products/:id/proforma` | shipper, guest | پیش‌فاکتور محصول موجود |
| POST | `/v1/manufacturing/orders` | shipper, guest | درخواست سفارش تولید |
| GET | `/v1/manufacturing/orders` | shipper | لیست درخواست‌های من |
| GET | `/v1/manufacturing/orders/:id` | shipper | جزئیات + وضعیت |
| GET | `/v1/proforma/:token` | public (با token) | مشاهده پیش‌فاکتور اختصاصی |
| GET | `/v1/dashboard/proformas` | shipper | پیش‌فاکتورهای من |

#### ادمین

| Method | Endpoint | Roles | توضیح |
|--------|----------|-------|-------|
| POST | `/v1/admin/products` | admin | ثبت محصول |
| POST | `/v1/admin/products/custom` | admin | محصول اختصاصی برای مشتری |
| POST | `/v1/admin/proforma` | admin, operator | صدور پیش‌فاکتور |
| POST | `/v1/admin/proforma/:id/share` | admin | تولید لینک اختصاصی |
| GET | `/v1/admin/manufacturing/orders` | admin | درخواست‌های تولید |
| PATCH | `/v1/admin/manufacturing/orders/:id/quote` | admin | تعیین زمان + هزینه |
| CRUD | `/v1/admin/factories` | admin | مدیریت کارخانه‌ها |

### Frontend Pages

```
/products                      کاتالوگ عمومی
/products/:id                  جزئیات محصول
/products/:id/quote            صدور پیش‌فاکتور
/manufacturing/request         درخواست سفارش تولید (محصول ناموجود)
/manufacturing/orders          پیگیری درخواست‌های تولید
/proforma/:token               پیش‌فاکتور اختصاصی (لینک ادمین)

/dashboard/proformas           پیش‌فاکتورهای من
/dashboard/proformas/:id       جزئیات پیش‌فاکتور

/admin/products                مدیریت محصولات
/admin/products/custom         محصول اختصاصی برای مشتری
/admin/factories               مدیریت کارخانه‌ها
/admin/manufacturing           درخواست‌های تولید
/admin/proforma/new            صدور پیش‌فاکتور
```

### UI — پیش‌فاکتور محصول سفارشی

```
┌─────────────────────────────────────────────────────────┐
│  پیش‌فاکتور شماره PF-2026-0042                          │
│  تاریخ: ۱۴۰۵/۰۶/۱۲                                      │
├─────────────────────────────────────────────────────────┤
│  ⚠️ توجه: این محصول در انبار موجود نیست                 │
│                                                         │
│  ┌───────────────────────────────────────────────────┐  │
│  │ پروفیل آلومینیومی سفارشی ۵۰×۳۰                    │  │
│  │ تعداد: ۵۰۰ عدد                                    │  │
│  │                                                   │  │
│  │ ⏱ زمان تولید: ۲۱ روز کاری                       │  │
│  │ 💰 هزینه تولید: ۱۵۰,۰۰۰,۰۰۰ تومان                 │  │
│  │ 💳 پیش‌پرداخت (۵۰٪): ۷۵,۰۰۰,۰۰۰ تومان — الزامی   │  │
│  │                                                   │  │
│  │ پس از تولید در کارخانه، ارسال به آدرس شما       │  │
│  └───────────────────────────────────────────────────┘  │
│                                                         │
│  جمع کل: ۱۵۰,۰۰۰,۰۰۰ تومان                              │
│                                                         │
│  [پرداخت پیش‌پرداخت]  [دانلود PDF]  [تماس با پشتیبانی] │
└─────────────────────────────────────────────────────────┘
```

### Integration با CRM و AI

| Event | CRM | AI |
|-------|-----|-----|
| مشاهده محصول | Lead activity | — |
| درخواست سفارش تولید | Lead → qualified | AI می‌تواند فرم را پر کند |
| صدور پیش‌فاکتور | Lead → quote_sent | AI تولید پیش‌فاکتور |
| پیش‌فاکتور اختصاصی ادمین | Lead link | — |

### Acceptance Criteria

- [ ] مشتری کاتالوگ محصولات را در سایت ببیند
- [ ] مشتری برای محصول موجود پیش‌فاکتور بزند
- [ ] مشتری درخواست سفارش تولید برای محصول ناموجود بزند
- [ ] پیش‌فاکتور سفارشی: «موجود نیست»، زمان تولید، هزینه، پیش‌پرداخت نمایش داده شود
- [ ] ادمین محصول اختصاصی برای مشتری ثبت کند
- [ ] ادمین پیش‌فاکتور صادر کند و لینک اختصاصی به مشتری بدهد
- [ ] مشتری فقط پیش‌فاکتور را ببیند (بدون کاتالوگ) وقتی لینک اختصاصی دارد
- [ ] درخواست تولید به کارخانه ارجاع شود
- [ ] پس از تایید کارخانه، نرخ جدید تولید و به مشتری اعلان شود

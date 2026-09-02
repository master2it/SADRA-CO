# C1. بازرگانی: CRM، SRM و دستیار هوش مصنوعی

[← فهرست بازرگانی](./commerce/README.md) | [← فهرست اصلی](./README.md)

---

## 18.1 چشم‌انداز بخش بازرگانی

بخش **بازرگانی** پلتفرم SADRA از دو زیرسیستم اصلی تشکیل می‌شود:

| زیرسیستم | مخاطب | هدف |
|----------|-------|-----|
| **CRM** (Customer Relationship Management) | مشتری / صاحب بار | مدیریت سرنخ، پیگیری فروش، جلوگیری از از دست رفتن مشتری |
| **SRM** (Supplier Relationship Management) | تامین‌کننده / Provider | مدیریت تامین‌کنندگان، نرخ‌گذاری خودکار، همکاری سیستمی |
| **کاتالوگ محصولات** | مشتری / ادمین | مشاهده اجناس، پیش‌فاکتور، سفارش تولید | [C2](../19-product-catalog-and-manufacturing.md) |

**اصل طلایی:** هیچ مشتری نباید وقتی وارد سیستم می‌شود، از دست برود.

---

## 18.2 وضعیت فعلی (دستی و کاغذی)

در حال حاضر **تمام فرآیندهای بازرگانی** به صورت دستی و کاغذی انجام می‌شود:

```
کانال ورودی (شبکه اجتماعی / معرفی / نمایشگاه / تماس تلفنی)
     ↓
اپراتور یادداشت دستی / تماس تلفنی
     ↓
تماس یکی‌یکی با تامین‌کنندگان برای گرفتن نرخ
     ↓
ثبت دستی نرخ در سایت
     ↓
تماس مجدد با مشتری + پیش‌فاکتور دستی
     ↓
بستن قرارداد (کاغذی)
```

### مشکلات وضعیت فعلی

| # | مشکل | اثر |
|---|------|-----|
| B1 | نبود CRM — سرنخ‌ها پراکنده و گم می‌شوند | از دست رفتن مشتری |
| B2 | کانال‌های ورودی متنوع بدون ثبت سیستمی | عدم ردیابی منبع جذب |
| B3 | مشتری نمی‌تواند خودش نرخ ببیند | وابستگی ۱۰۰٪ به اپراتور |
| B4 | اپراتور دستی با هر Provider تماس می‌گیرد | تاخیر ۲۴-۷۲ ساعت |
| B5 | نرخ‌ها دستی در سایت ثبت می‌شوند | خطا، قدیمی شدن، عدم مقیاس |
| B6 | پیش‌فاکتور و قرارداد دستی | کندی، خطای انسانی |
| B7 | نبود SRM — رابطه با تامین‌کننده سیستمی نیست | عدم شفافیت عملکرد Provider |
| B8 | اپراتور گلوگاه همه فرآیندها | عدم مقیاس‌پذیری |

---

## 18.3 وضعیت هدف (سیستمی)

```
کانال ورودی (سایت / شبکه اجتماعی / معرفی / نمایشگاه / تماس)
     ↓
ثبت خودکار Lead در CRM + تخصیص به اپراتور/AI
     ↓
مشتری نرخ‌ها را فوری در سایت می‌بیند (از تابلو SRM)
     ↓
مشتری اجناس را در کاتالوگ می‌بیند / پیش‌فاکتور می‌زند
     ├─ محصول موجود → پیش‌فاکتور فوری
     └─ محصول ناموجود → درخواست تولید → نرخ جدید + پیش‌فاکتور با جزئیات تولید
     ↓
دستیار AI: استعلام، مقایسه، پیش‌فاکتور، پیش‌نویس قرارداد
     ↓
اپراتور: بررسی نهایی + تایید قرارداد + پیگیری اجرا
     ↓
هیچ Lead بدون پیگیری نمی‌ماند (SLA + یادآور)
```

---

## 18.4 CRM — مدیریت ارتباط با مشتری

### 18.4.1 کانال‌های ورودی Lead

| کانال | کد | نحوه ثبت در سیستم |
|-------|-----|------------------|
| وب‌سایت (فرم تماس / استعلام نرخ) | `web` | خودکار — API |
| شبکه‌های اجتماعی (اینستاگرام، لینکدین، تلگرام) | `social` | Webhook / اپراتور |
| معرفی دوستان / مشتریان | `referral` | کد معرف در ثبت‌نام |
| نمایشگاه / رویداد | `exhibition` | فرم سریع اپراتور |
| تماس تلفنی | `phone` | ثبت دستی اپراتور در CRM |
| چت آنلاین / دستیار AI | `ai_chat` | خودکار |
| ایمیل | `email` | Integration |

### 18.4.2 چرخه عمر Lead (Pipeline)

```
جدید (New)
  ↓
تماس اولیه (Contacted)
  ↓
واجد شرایط (Qualified)
  ↓
استعلام نرخ (Quote Sent)
  ↓
مذاکره (Negotiation)
  ↓
پیش‌فاکتور (Proforma Issued)  ← AI می‌تواند تا اینجا برساند
  ↓
قرارداد (Contract)             ← اپراتور تایید نهایی
  ↓
برنده (Won) / از دست رفته (Lost)
```

### 18.4.3 نیازمندی‌های CRM

| کد | عنوان | توضیح | اولویت |
|----|-------|-------|--------|
| CRM-01 | ثبت Lead از همه کانال‌ها | هر ورودی مشتری در سیستم ثبت شود | P0 |
| CRM-02 | پروفایل ۳۶۰ درجه مشتری | تاریخچه تماس، درخواست‌ها، پیش‌فاکتورها | P0 |
| CRM-03 | Pipeline فروش | Kanban board برای پیگیری Leadها | P0 |
| CRM-04 | تخصیص خودکار Lead | بر اساس منطقه، نوع بار، بار کاری اپراتور | P0 |
| CRM-05 | SLA و یادآور | اگر Lead در X ساعت پیگیری نشد → هشدار | P0 |
| CRM-06 | صفر Lead گم‌شده | هیچ Lead بدون وضعیت و مسئول نماند | P0 |
| CRM-07 | گزارش کانال جذب | کدام کانال بیشترین تبدیل دارد | P1 |
| CRM-08 | امتیاز Lead (Scoring) | اولویت‌بندی خودکار بر اساس رفتار | P1 |
| CRM-09 | خودخدمتی مشتری | مشتری پنل خودش را ببیند و نرخ بگیرد | P0 |
| CRM-10 | تاریخچه تعاملات | تماس، چت، ایمیل، یادداشت اپراتور | P0 |

### 18.4.4 قانون «صفر Lead گم‌شده»

```typescript
// هر Lead باید:
interface LeadSLA {
  assignedTo: string;        // اپراتور یا AI
  status: LeadStatus;        // همیشه مشخص
  nextFollowUpAt: Date;      // تاریخ پیگیری بعدی
  lastContactAt: Date;       // آخرین تماس
  source: LeadSource;        // کانال ورودی
  lostReason?: string;       // اگر Lost — دلیل الزامی
}

// Cron: هر ۱ ساعت
// اگر Lead بدون پیگیری > SLA → escalation به مدیر
// اگر Lead جدید > ۱۵ دقیقه بدون تخصیص → auto-assign
```

---

## 18.5 SRM — مدیریت ارتباط با تامین‌کننده

### 18.5.1 هدف SRM

جایگزینی فرآیند دستی «اپراتور تماس می‌گیرد و نرخ می‌گیرد» با **خودخدمتی Provider**:

```
قبل:  اپراتور → تماس با ۱۰ Provider → جمع‌آوری نرخ → ثبت دستی
بعد:  Provider → وارد پنل SRM → نرخ خودش را ثبت → مشتری فوری می‌بیند
```

### 18.5.2 نیازمندی‌های SRM

| کد | عنوان | توضیح | اولویت |
|----|-------|-------|--------|
| SRM-01 | پروفایل تامین‌کننده | اطلاعات شرکت، مجوز، ناوگان، کشورها | P0 |
| SRM-02 | تابلو نرخ خودخدمتی | Provider نرخ‌ها را مستقیم وارد می‌کند | P0 |
| SRM-03 | Import/Export اکسل | ورود دسته‌ای نرخ‌ها | P0 |
| SRM-04 | اعتبار و انقضای نرخ | نرخ قدیمی خودکار غیرفعال | P0 |
| SRM-05 | امتیازدهی Provider | کیفیت، سرعت پاسخ، نرخ تبدیل | P1 |
| SRM-06 | قرارداد همکاری | مدیریت قرارداد پلتفرم با Provider | P1 |
| SRM-07 | داشبورد عملکرد | درخواست‌ها، تبدیل، درآمد | P0 |
| SRM-08 | اعلان درخواست جدید | نوتیف برای Leadهای مرتبط با مسیر Provider | P0 |
| SRM-09 | تاریخچه همکاری | تمام تعاملات با Provider | P1 |
| SRM-10 | Onboarding تامین‌کننده | فرآیند ثبت‌نام + تایید + آموزش | P0 |

### 18.5.3 تفاوت SRM با پنل Provider موجود

پنل Provider در داکیومنت‌های قبلی = بخش اصلی SRM. SRM لایه **مدیریتی** اضافه می‌کند:

| قابلیت | پنل Provider (SRM خودخدمتی) | پنل ادمین SRM |
|--------|---------------------------|---------------|
| مدیریت نرخ | ✅ Provider | ✅ نظارت + تایید |
| تایید Provider | ❌ | ✅ ادمین |
| امتیازدهی | مشاهده امتیاز خود | ✅ مدیریت |
| قرارداد همکاری | ❌ | ✅ ادمین |
| گزارش عملکرد | ✅ داشبورد خود | ✅ همه Providerها |

---

## 18.6 دستیار هوش مصنوعی (AI Assistant)

### 18.6.1 نقش AI در فرآیند بازرگانی

دستیار AI **جایگزین بخشی از کار اپراتور** می‌شود، نه کل آن:

```
┌─────────────────────────────────────────────────────────┐
│  کارهای AI (خودکار)          │  کارهای اپراتور (انسان) │
├──────────────────────────────┼──────────────────────────┤
│  پاسخ به سوالات مشتری        │  تایید نهایی قرارداد     │
│  استعلام و نمایش نرخ         │  مذاکره پیچیده            │
│  مقایسه Providerها           │  استثناها و موارد خاص     │
│  تولید پیش‌فاکتور            │  هماهنگی اجرایی           │
│  پیش‌نویس قرارداد           │  پیگیری پس از قرارداد     │
│  جمع‌آوری اطلاعات بار        │  شکایت و اختلاف           │
│  پیگیری اولیه Lead           │  تایید Provider جدید    │
│  یادآور و follow-up          │  تصمیم‌گیری استراتژیک   │
└──────────────────────────────┴──────────────────────────┘
```

### 18.6.2 نیازمندی‌های AI Assistant

| کد | عنوان | توضیح | اولویت |
|----|-------|-------|--------|
| AI-01 | چت هوشمند مشتری | پاسخ به سوالات حمل، گمرک، نرخ | P0 |
| AI-02 | استعلام نرخ در چت | مشتری در چت بپرسد، AI نرخ نشان دهد | P0 |
| AI-03 | تولید پیش‌فاکتور | AI پیش‌فاکتور PDF تولید کند | P0 |
| AI-04 | پیش‌نویس قرارداد | AI قرارداد اولیه آماده کند | P1 |
| AI-05 | ثبت Lead از چت | هر مکالمه → Lead در CRM | P0 |
| AI-06 | Handoff به اپراتور | انتقال به انسان وقتی AI نتواند | P0 |
| AI-07 | خلاصه مکالمه برای اپراتور | اپراتور context کامل ببیند | P0 |
| AI-08 | پیگیری خودکار Lead | AI follow-up بزند اگر مشتری پاسخ نداد | P1 |
| AI-09 | چندزبانه | FA / EN / AR / ZH | P1 |
| AI-10 | یادگیری از تاریخچه | بهبود پاسخ‌ها بر اساس داده | P2 |

### 18.6.3 Flow دستیار AI

```
مشتری وارد سایت/چت می‌شود
     ↓
AI: "سلام! چطور می‌تونم کمکتون کنم؟"
     ↓
مشتری: "می‌خوام بار از چین به ایران بفرستم"
     ↓
AI: جمع‌آوری اطلاعات (وزن، نوع بار، تاریخ)
     ↓
AI: نمایش نرخ‌های فعال از SRM (بدون اپراتور)
     ↓
مشتری: "پیش‌فاکتور بده"
     ↓
AI: تولید پیش‌فاکتور PDF + ثبت Lead در CRM
     ↓
AI: "پیش‌فاکتور آماده است. برای نهایی شدن، همکار ما تماس می‌گیرد"
     ↓
اپراتور: notification + خلاصه مکالمه → تایید/مذاکره/قرارداد
```

### 18.6.4 معماری AI

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Chat Widget │────▶│  AI Service  │────▶│  LLM API     │
│  (Web/App)   │     │  (NestJS)    │     │  (OpenAI/    │
└──────────────┘     └──────┬───────┘     │   Local)     │
                            │             └──────────────┘
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │   CRM    │  │  Pricing │  │   SRM    │
        │  Service │  │  Service │  │  Service │
        └──────────┘  └──────────┘  └──────────┘
```

**Tools/Functions برای AI (Function Calling):**

```typescript
const aiTools = [
  { name: 'search_rates', description: 'جستجوی نرخ حمل' },
  { name: 'create_lead', description: 'ثبت سرنخ جدید در CRM' },
  { name: 'generate_proforma', description: 'تولید پیش‌فاکتور' },
  { name: 'search_products', description: 'جستجوی محصولات کاتالوگ' },
  { name: 'create_manufacturing_order', description: 'درخواست سفارش تولید' },
  { name: 'get_route_info', description: 'اطلاعات مسیر و گمرک' },
  { name: 'handoff_to_operator', description: 'انتقال به اپراتور' },
  { name: 'schedule_followup', description: 'تنظیم پیگیری بعدی' },
];
```

---

## 18.7 یکپارچگی CRM + SRM + AI + پلتفرم

```
                    ┌─────────────────┐
                    │   مشتری (Web)   │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │   CRM    │   │    AI    │   │ Pricing  │
        │  Leads   │◀─▶│ Assistant│──▶│  Rates   │
        └────┬─────┘   └────┬─────┘   └────┬─────┘
             │              │              │
             ▼              ▼              ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │ اپراتور  │   │ پیش‌فاکتور│   │   SRM    │
        │  Panel   │   │  قرارداد  │   │ Providers│
        └──────────┘   └──────────┘   └──────────┘
```

---

## جزئیات پیاده‌سازی

### Database — CRM Tables

```sql
CREATE TYPE lead_source AS ENUM (
  'web', 'social', 'referral', 'exhibition', 'phone', 'ai_chat', 'email'
);

CREATE TYPE lead_status AS ENUM (
  'new', 'contacted', 'qualified', 'quote_sent',
  'negotiation', 'proforma_issued', 'contract', 'won', 'lost'
);

CREATE TABLE leads (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  -- اطلاعات تماس
  full_name VARCHAR(255),
  mobile VARCHAR(15),
  email VARCHAR(255),
  company_name VARCHAR(255),
  -- منبع و وضعیت
  source lead_source NOT NULL,
  source_detail VARCHAR(255),       -- مثلاً "نمایشگاه تهران ۱۴۰۵"
  referral_code VARCHAR(50),
  status lead_status DEFAULT 'new',
  -- تخصیص
  assigned_to UUID REFERENCES users(id),  -- اپراتور
  assigned_by VARCHAR(20) DEFAULT 'auto', -- auto | manual | ai
  -- SLA
  next_follow_up_at TIMESTAMPTZ,
  last_contact_at TIMESTAMPTZ,
  sla_breached BOOLEAN DEFAULT false,
  -- ارتباط با سیستم
  shipper_id UUID REFERENCES shipper_profiles(id),  -- اگر تبدیل به مشتری شد
  shipment_request_id UUID REFERENCES shipment_requests(id),
  -- متادیتا
  lost_reason TEXT,
  notes TEXT,
  metadata JSONB DEFAULT '{}',      -- اطلاعات اضافی از AI/فرم
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE lead_activities (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  lead_id UUID NOT NULL REFERENCES leads(id) ON DELETE CASCADE,
  type VARCHAR(30) NOT NULL,  -- call, email, chat, note, status_change, ai_action
  description TEXT,
  performed_by UUID REFERENCES users(id),  -- null = AI/system
  metadata JSONB DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE ai_conversations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  lead_id UUID REFERENCES leads(id),
  user_id UUID REFERENCES users(id),
  messages JSONB NOT NULL DEFAULT '[]',
  summary TEXT,                     -- خلاصه برای اپراتور
  handoff_requested BOOLEAN DEFAULT false,
  handoff_to UUID REFERENCES users(id),
  status VARCHAR(20) DEFAULT 'active',  -- active, handed_off, closed
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE proforma_invoices (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  lead_id UUID REFERENCES leads(id),
  shipment_request_id UUID REFERENCES shipment_requests(id),
  provider_id UUID REFERENCES provider_profiles(id),
  items JSONB NOT NULL,
  total_amount DECIMAL(15,2),
  currency currency,
  valid_until TIMESTAMPTZ,
  pdf_url VARCHAR(500),
  generated_by VARCHAR(20) DEFAULT 'ai',  -- ai | operator
  approved_by UUID REFERENCES users(id),
  status VARCHAR(20) DEFAULT 'draft',  -- draft, sent, accepted, expired
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### API Endpoints — CRM

| Method | Endpoint | Roles | توضیح |
|--------|----------|-------|-------|
| POST | `/v1/crm/leads` | public, operator | ثبت Lead جدید |
| GET | `/v1/crm/leads` | operator, admin | لیست Leadها (فیلتر + pipeline) |
| GET | `/v1/crm/leads/:id` | operator, admin | جزئیات Lead |
| PATCH | `/v1/crm/leads/:id` | operator, admin | تغییر وضعیت / تخصیص |
| POST | `/v1/crm/leads/:id/activities` | operator, admin | ثبت فعالیت |
| GET | `/v1/crm/leads/:id/activities` | operator, admin | تاریخچه فعالیت‌ها |
| GET | `/v1/crm/pipeline` | operator, admin | Kanban board data |
| GET | `/v1/crm/reports/sources` | admin | گزارش کانال‌های جذب |
| GET | `/v1/crm/sla/breached` | operator, admin | Leadهای نقض SLA |

### API Endpoints — AI Assistant

| Method | Endpoint | Roles | توضیح |
|--------|----------|-------|-------|
| POST | `/v1/ai/chat` | public (guest + auth) | ارسال پیام به AI |
| GET | `/v1/ai/conversations/:id` | operator, owner | تاریخچه مکالمه |
| POST | `/v1/ai/handoff` | ai (internal) | انتقال به اپراتور |
| POST | `/v1/ai/proforma` | ai, operator | تولید پیش‌فاکتور |
| GET | `/v1/ai/conversations/:id/summary` | operator | خلاصه برای اپراتور |

### Frontend — صفحات CRM (اپراتور/ادمین)

```
/operator/
├── dashboard              # آمار Leadها، SLA، تبدیل
├── pipeline               # Kanban board
├── leads/
│   ├── page.tsx           # لیست Leadها
│   └── [id]/page.tsx      # جزئیات + تاریخچه + AI summary
├── proforma/
│   └── [id]/page.tsx      # بررسی/تایید پیش‌فاکتور AI
└── reports/
    └── sources/page.tsx   # گزارش کانال‌ها
```

### Frontend — Chat Widget (مشتری)

```tsx
// components/ai/ChatWidget.tsx
// - Floating button در همه صفحات عمومی
// - پشتیبانی از guest (بدون لاگین)
// - ثبت خودکار Lead در اولین پیام
// - دکمه "صحبت با اپراتور" برای handoff
// - نمایش پیش‌فاکتور در چت
```

### Cron Jobs — SLA

```typescript
@Cron('0 * * * *')  // هر ساعت
async checkLeadSLA() {
  // Leadهای new بدون تخصیص > ۱۵ دقیقه → auto-assign
  // Leadهای بدون follow-up > SLA → flag + notify manager
  // Leadهای quote_sent بدون پاسخ > ۴۸ ساعت → AI follow-up
}

@Cron('0 9 * * *')  // هر روز ۹ صبح
async dailyLeadReport() {
  // گزارش: Leadهای جدید، تبدیل، گم‌شده، SLA breached
}
```

### Acceptance Criteria — بازرگانی

- [ ] Lead از هر کانال (وب، تلفن، نمایشگاه، AI) در CRM ثبت می‌شود
- [ ] هیچ Lead بدون مسئول و وضعیت نمی‌ماند
- [ ] مشتری نرخ را بدون دخالت اپراتور می‌بیند (از SRM)
- [ ] Provider نرخ خودش را در SRM ثبت می‌کند (بدون اپراتور)
- [ ] AI پیش‌فاکتور تولید می‌کند
- [ ] AI مکالمه را به اپراتور handoff می‌کند با خلاصه کامل
- [ ] اپراتور پیش‌فاکتور AI را تایید/رد می‌کند
- [ ] SLA breach → notification به مدیر
- [ ] گزارش کانال جذب قابل مشاهده است

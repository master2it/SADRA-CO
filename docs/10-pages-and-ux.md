# 10. ساختار صفحات و UX

[← بازگشت به فهرست](./README.md)

---

## 10.1 اصول طراحی UX

| اصل | توضیح |
|-----|-------|
| **۳ کلیک تا هدف** | کاربر در حداکثر ۳ کلیک به نرخ برسد |
| **شفافیت** | قیمت، زمان، مسیر همه واضح باشند |
| **اعتماد** | نشان‌های تایید، امتیاز، نظرات |
| **سرعت** | لود صفحات زیر ۲ ثانیه |
| **ریسپانسیو** | اولویت با موبایل |
| **دسترسی‌پذیری** | WCAG 2.1 AA |

---

## 10.2 ساختار صفحات وب

### صفحات عمومی

```
/                              صفحه اصلی
/about                         درباره ما
/contact                       تماس با ما
/terms                         قوانین
/privacy                       حریم خصوصی
```

### صفحات مسیر (SEO-critical)

```
/routes                        لیست همه مسیرها
/routes/china-to-iran          صفحه اختصاصی مسیر (با نقشه + نرخ + محتوا)
/routes/iran-to-turkey
/routes/...
```

### صفحات مشتری

```
/dashboard                     داشبورد مشتری
/dashboard/new-request         ثبت درخواست جدید
/dashboard/quotes              مشاهده نرخ‌ها
/dashboard/requests            لیست درخواست‌ها
/dashboard/requests/:id        جزئیات درخواست
/dashboard/requests/:id/tracking   Timeline ۱۹ فرایند + نقشه
/dashboard/providers           لیست Providerها
/dashboard/profile             پروفایل
```

### صفحات Provider

```
/provider/dashboard            داشبورد Provider
/provider/rate-board           تابلو نرخ (مدیریت)
/provider/rate-board/new       افزودن نرخ جدید
/provider/rate-board/import    وارد کردن دسته‌ای
/provider/requests             درخواست‌های دریافتی
/provider/offers               پیشنهادهای ثبت‌شده
/provider/trips                سفرهای فعال
/provider/profile              پروفایل شرکت
/provider/analytics            گزارش‌ها
```

### صفحات ادمین

```
/admin/dashboard
/admin/users
/admin/providers               تایید Providerها
/admin/drivers                 تایید رانندگان
/admin/routes                  مدیریت مسیرها
/admin/content                 مدیریت محتوا
/admin/rates                   نظارت بر نرخ‌ها
/admin/reports                 گزارش‌ها
/admin/trips/:id               Timeline ۱۹ فرایند + GPS
/admin/settings                تنظیمات
```

#### صفحات اپراتور (CRM)

```
/operator/dashboard            داشبورد Leadها و SLA
/operator/pipeline             Kanban board فروش
/operator/leads                لیست Leadها
/operator/leads/:id            جزئیات + تاریخچه + خلاصه AI
/operator/proforma/:id         بررسی/تایید پیش‌فاکتور AI
/operator/reports/sources      گزارش کانال‌های جذب
```

#### صفحات محصولات

```
/products                      کاتالوگ عمومی
/products/:id                  جزئیات + پیش‌فاکتور
/manufacturing/request         درخواست سفارش تولید (محصول ناموجود)
/proforma/:token               پیش‌فاکتور اختصاصی (لینک ادمین)
/dashboard/proformas           پیش‌فاکتورهای من
```

#### صفحات ادمین — محصولات

```
/admin/products                مدیریت کاتالوگ
/admin/products/custom         محصول اختصاصی برای مشتری
/admin/factories               مدیریت کارخانه‌ها
/admin/manufacturing           درخواست‌های تولید
/admin/proforma/new            صدور پیش‌فاکتور
```

---

## 10.3 وایرفریم کلیدی (توضیحی)

### صفحه اصلی

```
┌────────────────────────────────────────────────────┐
│  Logo    [مسیرها] [Providerها] [بلاگ]   [ورود/ثبت‌نام] │
├────────────────────────────────────────────────────┤
│                                                    │
│   حمل بار بین‌المللی، سریع و شفاف                  │
│                                                    │
│   ┌────────────────────────────────────────────┐   │
│   │  مبدا: [ایران - تهران ▼]                   │   │
│   │  مقصد: [چین - شانگهای ▼]                   │   │
│   │  نوع بار: [عمومی ▼]  وزن: [20 تن ▼]       │   │
│   │                                            │   │
│   │         [مشاهده نرخ‌ها →]                   │   │
│   └────────────────────────────────────────────┘   │
│                                                    │
│   مسیرهای پرطرفدار:                               │
│   🇨🇳 چین ↔ 🇮🇷 ایران  🇹🇷 ترکیه ↔ 🇮🇷 ایران      │
│   🇮🇶 عراق ↔ 🇮🇷 ایران  🇦🇪 امارات ↔ 🇮🇷 ایران     │
│                                                    │
│   چرا ما؟                                          │
│   ✓ نرخ لحظه‌ای  ✓ ردیابی زنده  ✓ ۵۰۰+ Provider   │
│                                                    │
└────────────────────────────────────────────────────┘
```

### صفحه مسیر (SEO Page)

```
┌────────────────────────────────────────────────────┐
│  Breadcrumb: خانه > مسیر‌ها > چین به ایران          │
├────────────────────────────────────────────────────┤
│                                                    │
│  حمل بار از چین به ایران                           │
│  سریع‌ترین و مطمئن‌ترین مسیر حمل زمینی              │
│                                                    │
│  ┌─────────────────────────────────────────────┐  │
│  │         [نقشه تعاملی مسیر]                  │  │
│  │   🇨🇳 ──→ 🇰🇿 ──→ 🇺🇿 ──→ 🇹🇲 ──→ 🇮🇷       │  │
│  │   شانگهای   آلماتی   تاشکنت   عشق‌آباد   تهران │  │
│  │                                              │  │
│  │   فاصله: ۵,۸۰۰ کیلومتر                       │  │
│  │   زمان: ۱۲ تا ۱۵ روز                         │  │
│  └─────────────────────────────────────────────┘  │
│                                                    │
│  ┌─────────────────────────────────────────────┐  │
│  │  نرخ‌های فعلی (به‌روزرسانی: ۵ دقیقه پیش)     │  │
│  │                                              │  │
│  │  Provider A  ⭐ 4.8   ۱۲ روز   ۴۵,۰۰۰ ¥    │  │
│  │  Provider B  ⭐ 4.6   ۱۴ روز   ۴۲,۰۰۰ ¥    │  │
│  │  Provider C  ⭐ 4.9   ۱۱ روز   ۴۸,۰۰۰ ¥    │  │
│  │                                              │  │
│  │         [ثبت درخواست →]                     │  │
│  └─────────────────────────────────────────────┘  │
│                                                    │
│  راهنمای کامل حمل بار از چین به ایران              │
│  ─────────────────────────────────────             │
│  ## مدارک لازم                                      │
│  ## مراحل گمرکی                                     │
│  ## مرزهای اصلی                                     │
│  ## هزینه‌های جانبی                                  │
│  ## سوالات متداول                                   │
│                                                    │
└────────────────────────────────────────────────────┘
```

---

## جزئیات پیاده‌سازی

### Next.js App Router Structure

```
apps/web/src/app/
├── (public)/
│   ├── page.tsx                    # صفحه اصلی
│   ├── about/page.tsx
│   ├── contact/page.tsx
│   ├── routes/
│   │   ├── page.tsx                # لیست مسیرها
│   │   └── [slug]/page.tsx         # صفحه مسیر SEO
│   ├── blog/
│   │   ├── page.tsx
│   │   └── [slug]/page.tsx
│   ├── guides/[slug]/page.tsx
│   ├── faq/page.tsx
│   └── glossary/page.tsx
├── (auth)/
│   ├── login/page.tsx
│   └── register/page.tsx
├── dashboard/                      # Shipper (protected)
│   ├── layout.tsx
│   ├── page.tsx
│   ├── new-request/page.tsx
│   ├── requests/
│   │   ├── page.tsx
│   │   └── [id]/page.tsx
│   └── profile/page.tsx
├── provider/                       # Provider (protected)
│   ├── layout.tsx
│   ├── dashboard/page.tsx
│   ├── rate-board/
│   │   ├── page.tsx
│   │   ├── new/page.tsx
│   │   └── import/page.tsx
│   ├── requests/page.tsx
│   └── analytics/page.tsx
├── admin/                          # Admin (protected)
│   ├── layout.tsx
│   ├── dashboard/page.tsx
│   ├── providers/page.tsx
│   ├── drivers/page.tsx
│   └── ...
└── layout.tsx                      # Root layout
```

### Design System (shadcn/ui)

| Component | Usage |
|-----------|-------|
| `Button` | CTAها، اقدامات |
| `Card` | کارت نرخ، کارت Provider |
| `DataTable` | تابلو نرخ، لیست درخواست‌ها |
| `Dialog` | تایید، جزئیات |
| `Form` + `Input` + `Select` | فرم‌ها |
| `Badge` | وضعیت، نوع بار |
| `Tabs` | تب‌های داشبورد |
| `Sheet` | منوی موبایل |
| `Toast` | نوتیفیکیشن‌ها |
| `Skeleton` | Loading states |

### Color Palette

```css
:root {
  --primary: 220 70% 50%;      /* آبی — CTA اصلی */
  --secondary: 160 60% 45%;    /* سبز — موفقیت */
  --accent: 30 90% 55%;        /* نارنجی — هشدار */
  --destructive: 0 70% 50%;    /* قرمز — خطا */
  --muted: 220 15% 95%;        /* پس‌زمینه */
  --card: 0 0% 100%;
  --border: 220 15% 90%;
}
```

### Typography

```css
/* فارسی */
font-family: 'Vazirmatn', sans-serif;

/* انگلیسی/اعداد */
font-family: 'Inter', sans-serif;

/* سایزها */
--text-xs: 0.75rem;
--text-sm: 0.875rem;
--text-base: 1rem;
--text-lg: 1.125rem;
--text-xl: 1.25rem;
--text-2xl: 1.5rem;
--text-3xl: 1.875rem;
--text-4xl: 2.25rem;
```

### Responsive Breakpoints

| Breakpoint | Width | Layout |
|------------|-------|--------|
| `sm` | 640px | موبایل landscape |
| `md` | 768px | تبلت |
| `lg` | 1024px | دسکتاپ |
| `xl` | 1280px | دسکتاپ بزرگ |

### Key Components Implementation

#### RateSearchForm
```tsx
// components/search/RateSearchForm.tsx
interface RateSearchFormProps {
  defaultOrigin?: Location;
  defaultDestination?: Location;
  onSearch: (params: SearchParams) => void;
}

// Fields: Origin (autocomplete), Destination (autocomplete),
//         CargoType (select), Weight (number + unit),
//         Submit button
```

#### ProviderComparisonTable
```tsx
// components/comparison/ProviderComparisonTable.tsx
// Columns: Provider Name, Rating, Price, Currency, ETA, Actions
// Features: Sort by price/time/rating, Select button, Highlight best value
```

#### RouteMap (Mapbox)
```tsx
// components/routes/RouteMap.tsx
// - Display route geometry from API
// - Waypoint markers with country flags
// - Distance/duration overlay
// - Interactive: zoom, pan
```

### Loading & Error States

| State | Component | Behavior |
|-------|-----------|----------|
| Loading rates | `RateCardSkeleton` × 3 | Shimmer animation |
| No rates found | `EmptyState` | "نرخی یافت نشد — درخواست ثبت کنید" |
| API error | `ErrorBoundary` | Retry button |
| Offline | `OfflineBanner` | "اتصال اینترنت برقرار نیست" |

### Accessibility Checklist

- [ ] همه تصاویر `alt` دارند
- [ ] فرم‌ها `label` مرتبط دارند
- [ ] رنگ‌ها contrast ratio ≥ 4.5:1
- [ ] Keyboard navigation کامل
- [ ] Focus indicators واضح
- [ ] Screen reader friendly
- [ ] RTL support برای فارسی و عربی

### Performance Targets

| Metric | Target | Tool |
|--------|--------|------|
| LCP | < 2.5s | Lighthouse |
| FID | < 100ms | Lighthouse |
| CLS | < 0.1 | Lighthouse |
| TTFB | < 600ms | WebPageTest |
| Bundle size (initial) | < 200KB | webpack-bundle-analyzer |

### i18n Structure

```
apps/web/messages/
├── fa.json
├── en.json
├── ar.json
└── zh.json

// Usage with next-intl
import { useTranslations } from 'next-intl';
const t = useTranslations('HomePage');
t('hero.title') // "حمل بار بین‌المللی، سریع و شفاف"
```

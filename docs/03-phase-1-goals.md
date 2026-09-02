# 3. اهداف فاز ۱

[← بازگشت به فهرست](./README.md)

---

## 3.1 اهداف کلان

1. **حذف کامل دخالت اپراتور** در فرآیند قیمت‌گذاری و جمع‌آوری نرخ
2. **راه‌اندازی CRM** — هیچ Lead از دست نرود، پیگیری سیستمی از همه کانال‌ها
3. **راه‌اندازی SRM** — تامین‌کنندگان خودشان نرخ ثبت کنند
4. **دستیار AI** — پیش‌فاکتور و پاسخ اولیه خودکار، اپراتور فقط تایید نهایی
5. **خودخدمتی مشتری** — مشاهده نرخ بدون انتظار برای اپراتور
6. **راه‌اندازی پنل مستقل Provider** برای مدیریت نرخ‌ها
7. **پشتیبانی از حمل زمینی بین‌المللی** (ایران ↔ سایر کشورها)
8. **نمایش مسیر روی نقشه** با جزئیات (کشورهای عبوری، مرزها)
9. **اپلیکیشن راننده** با قابلیت بار برگشت و ردیابی GPS
10. **UX حرفه‌ای** و **SEO قوی** برای جذب ارگانیک
11. **کاتالوگ محصولات** — مشاهده اجناس، پیش‌فاکتور، سفارش تولید کارخانه‌ای
12. **معماری آماده** برای افزودن حمل دریایی و ریلی

---

## 3.2 اهداف کمی (KPI) — ۶ ماه اول

| شاخص | هدف | نحوه اندازه‌گیری |
|------|-----|-----------------|
| Providerهای فعال | حداقل ۲۰ شرکت | `ProviderProfile.status = approved` |
| درخواست‌های روزانه | ۱۰۰+ | `ShipmentRequest` per day |
| نرخ تبدیل درخواست به قرارداد | ۳۰٪+ | `accepted / total requests` |
| رانندگان فعال اپ | ۲۰۰+ | `DriverProfile` with app login in 30 days |
| رتبه گوگل (کلمات کلیدی اصلی) | صفحه اول | Google Search Console |
| زمان پاسخ به مشتری | زیر ۵ دقیقه | `time_to_first_rate` metric |
| Lead گم‌شده | ۰٪ | همه Leadها در CRM با وضعیت |
| پیش‌فاکتور توسط AI | ≥ ۵۰٪ درخواست‌ها | `proforma_invoices.generated_by = ai` |
| نرخ ثبت‌شده توسط Provider | ≥ ۸۰٪ | بدون دخالت اپراتور |

---

## جزئیات پیاده‌سازی

### Epic Breakdown (تقسیم اهداف به Epic)

| هدف | Epic | Story Points (تخمینی) |
|-----|------|----------------------|
| حذف اپراتور از نرخ‌گیری | E1: SRM + Rate Board + Instant Quote | 40 |
| CRM و Lead Management | E1b: CRM Pipeline + SLA + کانال‌ها | 30 |
| AI Assistant | E1c: چت + پیش‌فاکتور + Handoff | 25 |
| پنل Provider | E2: Provider Dashboard (SRM) | 35 |
| حمل زمینی | E3: Route Management | 25 |
| نقشه | E4: Interactive Map | 20 |
| اپ راننده | E5: Driver App MVP | 50 |
| UX/SEO | E6: SEO Pages + Landing | 30 |
| معماری | E7: Core Infrastructure | 20 |

**جمع تخمینی:** ~220 Story Points (۶ ماه با تیم ۶ نفره)

### Definition of Done (DoD) برای فاز ۱

هر Feature زمانی Done است که:

- [ ] کد Review شده و Merge شده
- [ ] Unit Test نوشته شده (حداقل ۷۰٪ coverage برای Business Logic)
- [ ] API Documentation (Swagger) به‌روز شده
- [ ] UI مطابق Figma تایید شده
- [ ] Responsive روی موبایل و دسکتاپ تست شده
- [ ] i18n برای FA و EN پیاده شده
- [ ] بدون باگ Critical/Major
- [ ] Deploy شده روی Staging

### Metrics Dashboard (داشبورد KPI)

```
┌─────────────────────────────────────────────────┐
│  SADRA Admin Dashboard — KPIs                   │
├──────────────┬──────────────┬───────────────────┤
│ Providers    │ Requests/day │ Conversion Rate   │
│ 20 / target  │ 100 / target │ 30% / target      │
├──────────────┼──────────────┼───────────────────┤
│ Active       │ Avg Response │ SEO Pages         │
│ Drivers 200  │ Time < 5min  │ 20+ indexed       │
└──────────────┴──────────────┴───────────────────┘
```

### Events برای Analytics

| Event | Properties | هدف KPI |
|-------|-----------|---------|
| `rate_search` | origin, destination, weight | درخواست‌های روزانه |
| `rate_viewed` | provider_id, price | engagement |
| `request_created` | route_id, provider_id | conversion funnel |
| `offer_accepted` | request_id, provider_id | نرخ تبدیل |
| `driver_app_opened` | driver_id | رانندگان فعال |
| `gps_location_sent` | trip_id, lat, lng | ردیابی |

### اولویت‌بندی MoSCoW

| Must Have | Should Have | Could Have | Won't Have (فاز ۱) |
|-----------|-------------|------------|-------------------|
| تابلو نرخ + SRM | Import اکسل | Rate Alert | پرداخت آنلاین |
| CRM + Pipeline | چت با Provider | Price Lock | حمل دریایی |
| AI Assistant (چت + پیش‌فاکتور) | PDF پیش‌فاکتور | محاسبه‌گر گمرکی | AI pricing |
| پنل Provider | آفلاین mode | Referral | White Label |
| اپ راننده + GPS | — | — | ERP integration |
| صفحات SEO | — | — | — |

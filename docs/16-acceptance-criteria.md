# 16. معیارهای پذیرش (Acceptance Criteria)

[← بازگشت به فهرست](./README.md)

---

پروژه فاز ۱ زمانی **تکمیل‌شده** تلقی می‌شود که:

## 17.1 معیارهای عملکردی

- ✅ مشتری بتواند بدون ثبت‌نام، نرخ‌ها را ببیند
- ✅ Provider بتواند تابلو نرخ خود را مستقل مدیریت کند
- ✅ مشتری بتواند Provider را مقایسه و انتخاب کند
- ✅ مسیر روی نقشه با جزئیات کشورها و مرزها نمایش داده شود
- ✅ اپلیکیشن راننده روی iOS و Android نصب و کار کند
- ✅ راننده بتواند بار برگشت پیدا کند
- ✅ موقعیت GPS راننده لحظه‌ای برای ادمین قابل مشاهده باشد
- ✅ همه پیامک‌ها و نوتیفیکیشن‌های کلیدی ارسال شوند
- ✅ Lead از هر کانال (وب، تلفن، نمایشگاه، AI) در CRM ثبت شود
- ✅ هیچ Lead بدون مسئول و وضعیت نماند (صفر Lead گم‌شده)
- ✅ Provider نرخ خودش را در SRM ثبت کند (بدون اپراتور)
- ✅ دستیار AI پیش‌فاکتور تولید و به اپراتور handoff کند
- ✅ مشتری کاتالوگ محصولات را ببیند و پیش‌فاکتور بزند
- ✅ مشتری درخواست سفارش تولید برای محصول ناموجود بزند
- ✅ پیش‌فاکتور سفارشی: موجود نیست، زمان تولید، هزینه، پیش‌پرداخت نمایش داده شود
- ✅ ادمین محصول اختصاصی ثبت کند و فقط پیش‌فاکتور به مشتری نشان دهد
- ✅ Timeline ۱۹ فرایند بارگیری برای ادمین، مشتری و راننده نمایش داده شود
- ✅ راننده مرحله بعد را از اپ ثبت کند (فرایندهای ۷–۱۹)

---

## 17.2 معیارهای کیفیت

- ✅ Core Web Vitals در محدوده سبز (LCP < 2.5s)
- ✅ صفحات مسیر توسط گوگل ایندکس شوند
- ✅ حداقل ۲۰ صفحه مسیر با محتوای ۲۰۰۰+ کلمه
- ✅ اپلیکیشن راننده در حالت آفلاین کار کند
- ✅ هیچ باگ Critical یا Major وجود نداشته باشد
- ✅ تست امنیتی پایه پاس شود

---

## 17.3 معیارهای کسب‌وکار

- ✅ حداقل ۱۰ Provider فعال در Beta
- ✅ حداقل ۲۰ راننده فعال در Beta
- ✅ نرخ تبدیل بازدیدکننده به درخواست ≥ ۵٪
- ✅ رضایت کاربر (NPS) ≥ ۴۰

---

## جزئیات پیاده‌سازی — Test Cases

### AC-01: مشاهده نرخ بدون ثبت‌نام

```gherkin
Feature: Public rate search
  Scenario: Guest user searches for rates
    Given I am not logged in
    And I am on the homepage
    When I search for rates from "تهران" to "شانگهای" with weight "20" tons
    Then I see a list of rates from active providers
    And I am NOT prompted to login
    And results appear within 3 seconds
```

**Test:** `e2e/rate-search.spec.ts`

---

### AC-02: مدیریت تابلو نرخ Provider

```gherkin
Feature: Provider rate board management
  Scenario: Provider creates a new rate
    Given I am logged in as an approved provider
    When I navigate to /provider/rate-board/new
    And I fill in origin "تهران", destination "مشهد", price "5000000" IRR
    And I submit the form
    Then the rate appears in my rate board
    And the rate is searchable by customers

  Scenario: Provider imports rates from Excel
    Given I am on /provider/rate-board/import
    When I upload a valid Excel file with 50 rates
    Then all 50 rates are imported
    And I see a summary of imported/failed rows
```

**Test:** `e2e/provider-rate-board.spec.ts`

---

### AC-03: مقایسه و انتخاب Provider

```gherkin
Feature: Provider comparison and selection
  Scenario: Shipper compares providers
    Given I have search results with 3+ providers
    When I view the comparison table
    Then I can sort by price, time, and rating
    And I can select a provider
    And I can proceed to create a shipment request
```

---

### AC-04: نقشه مسیر

```gherkin
Feature: Route map display
  Scenario: Route page shows interactive map
    Given I am on /routes/china-to-iran
    Then I see an interactive map with the route drawn
    And I see waypoint markers for each country
    And I see distance and estimated time
    And the map is responsive on mobile
```

---

### AC-05: اپ راننده iOS/Android

```gherkin
Feature: Driver app installation
  Scenario: App installs and runs on Android
    Given the APK is installed on Android 10+
    When I open the app
    Then I see the login/register screen
    And I can complete registration with OTP

  Scenario: App installs and runs on iOS
    Given the app is installed from App Store on iOS 15+
    When I open the app
    Then the app functions identically to Android
```

---

### AC-06: بار برگشت

```gherkin
Feature: Backhaul loads
  Scenario: Driver finds return load after delivery
    Given I have completed a delivery to "مشهد"
    When I open the backhaul section
    Then I see available loads from "مشهد" to other destinations
    And I can filter by destination and weight
```

---

### AC-07: ردیابی GPS لحظه‌ای

```gherkin
Feature: Real-time GPS tracking
  Scenario: Admin sees driver location
    Given a driver is on an active trip
    And the driver app is sending GPS every 30 seconds
    When an admin opens the trip tracking page
    Then they see the driver's current location on the map
    And the location updates within 60 seconds
```

---

### AC-08: نوتیفیکیشن‌ها

| Event | SMS | Email | Push | Test |
|-------|-----|-------|------|------|
| OTP login | ✅ | — | — | `auth/otp.spec.ts` |
| New request to provider | ✅ | ✅ | — | `notification/provider.spec.ts` |
| Offer accepted | ✅ | ✅ | — | `notification/shipper.spec.ts` |
| New load for driver | — | — | ✅ | `notification/driver.spec.ts` |

---

### Quality Metrics — Automated Tests

```bash
# Performance
npx lighthouse https://staging.sadra.ir --output=json
# Assert: LCP < 2500, FID < 100, CLS < 0.1

# Security
npm audit --audit-level=high
# OWASP ZAP scan on staging

# Coverage
pnpm test --coverage
# Assert: > 70% for business logic modules
```

### Bug Severity Classification

| Severity | Definition | Acceptable at Launch |
|----------|-----------|---------------------|
| **Critical** | System down, data loss, security breach | 0 |
| **Major** | Core feature broken, no workaround | 0 |
| **Minor** | Feature works with workaround | < 10 |
| **Trivial** | Cosmetic, typo | Unlimited |

### UAT Checklist (User Acceptance Testing)

#### مشتری (Shipper)
- [ ] جستجوی نرخ بدون لاگین
- [ ] ثبت‌نام و ورود
- [ ] ثبت درخواست حمل
- [ ] مقایسه Providerها
- [ ] انتخاب Provider
- [ ] پیگیری وضعیت درخواست
- [ ] مشاهده ردیابی راننده

#### Provider
- [ ] ثبت‌نام + آپلود مدارک
- [ ] افزودن نرخ دستی
- [ ] Import نرخ از اکسل
- [ ] دریافت نوتیفیکیشن درخواست جدید
- [ ] ثبت پیشنهاد
- [ ] تخصیص راننده
- [ ] مشاهده داشبورد آمار

#### راننده
- [ ] ثبت‌نام در اپ
- [ ] مشاهده بارهای موجود
- [ ] قبول بار
- [ ] ارسال GPS (foreground + background)
- [ ] ثبت رویدادها
- [ ] آپلود POD
- [ ] مشاهده بار برگشت

#### ادمین
- [ ] تایید Provider
- [ ] تایید راننده
- [ ] مدیریت مسیرها
- [ ] نظارت بر نرخ‌ها
- [ ] مشاهده ردیابی لحظه‌ای
- [ ] گزارش‌های مدیریتی

### Sign-off Template

```
پروژه SADRA — فاز ۱
تاریخ تست: ___________
تست‌کننده: ___________

معیارهای عملکردی:  ☐ Pass  ☐ Fail
معیارهای کیفیت:     ☐ Pass  ☐ Fail
معیارهای کسب‌وکار:  ☐ Pass  ☐ Fail

امضای کارفرما: ___________
امضای مدیر فنی: ___________
```

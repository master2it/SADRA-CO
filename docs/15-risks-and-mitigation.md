# 15. ریسک‌ها و راهکارها

[← بازگشت به فهرست](./README.md)

---

| # | ریسک | احتمال | اثر | راهکار |
|---|------|--------|-----|--------|
| R1 | Providerها از تابلو نرخ استفاده نکنند | زیاد | زیاد | آموزش + پشتیبانی + Import اکسل |
| R2 | راننده‌ها اپ نصب نکنند | متوسط | زیاد | پاداش نصب + سادگی اپ |
| R3 | GPS در مرز قطع شود | زیاد | متوسط | حالت آفلاین + Sync خودکار |
| R4 | نرخ‌ها قدیمی بمانند | متوسط | زیاد | یادآور انقضا + غیرفعال کردن خودکار |
| R5 | SEO زمان‌بر باشد | زیاد | متوسط | شروع زود + محتوا از روز اول |
| R6 | تحریم و محدودیت بین‌المللی | متوسط | زیاد | تمرکز بر کشورهای دوست + ارزهای محلی |
| R7 | رقبای موجود (باربری‌های آنلاین) | متوسط | متوسط | تمایز در UX + تمرکز B2B بین‌المللی |
| R8 | پیچیدگی گمرکی هر کشور | زیاد | متوسط | همکاری با کارشناس گمرک + محتوای دقیق |

---

## جزئیات پیاده‌سازی

### R1: Providerها از تابلو نرخ استفاده نکنند

**راهکارهای فنی:**
```typescript
// Onboarding wizard for new providers
// Step 1: Company info → Step 2: Upload rates (manual or Excel) → Step 3: Review

// Reminder cron job
@Cron('0 9 * * 1') // Every Monday 9 AM
async remindProvidersWithoutRates() {
  const providers = await this.getProvidersWithoutActiveRates();
  for (const provider of providers) {
    await this.notificationService.send(provider, {
      type: 'rate_reminder',
      message: 'تابلو نرخ شما خالی است. مشتریان منتظر نرخ‌های شما هستند.',
    });
  }
}

// Excel import to reduce friction
POST /v1/provider/rates/import
// Accept .xlsx with template download
```

**راهکارهای غیرفنی:**
- آموزش حضوری/آنلاین برای ۱۰ Provider اول
- پشتیبانی تلفنی برای Import اولین بار
- نمایش آمار: "Providerهایی با نرخ فعال، ۳ برابر بیشتر درخواست دریافت می‌کنند"

---

### R2: راننده‌ها اپ نصب نکنند

**راهکارهای فنی:**
```dart
// Minimal onboarding — 3 steps max
// 1. Phone + OTP → 2. Basic info → 3. Start browsing loads
// Document upload can be done later

// Incentive system
class ReferralBonus {
  // 50,000 Toman for first completed trip
  // Bonus for installing app
}
```

**راهکارهای غیرفنی:**
- پاداش نصب (۵۰,۰۰۰ تومان برای اولین سفر)
- Providerها رانندگان خود را به اپ دعوت کنند
- اپ سبک (< 30MB) و ساده

---

### R3: GPS در مرز قطع شود

**راهکار فنی:**
```dart
// Offline queue with Isar
class OfflineLocationQueue {
  Future<void> saveLocation(LocationData data) async {
    await isar.pendingLocations.put(PendingLocation.from(data));
  }

  Future<void> syncWhenOnline() async {
    final pending = await isar.pendingLocations
      .filter().syncedEqualTo(false).findAll();
    for (final loc in pending) {
      try {
        await api.sendLocation(loc);
        loc.synced = true;
        await isar.pendingLocations.put(loc);
      } catch (_) { break; }
    }
  }
}

// Geofence-based events at borders
// When driver enters border geofence → record event even without network
```

**Fallback UI:**
- Admin dashboard shows "Last known location: 2 hours ago"
- Manual event logging by driver when network returns

---

### R4: نرخ‌ها قدیمی بمانند

**راهکار فنی:**
```typescript
// Cron job: deactivate expired rates
@Cron('0 0 * * *') // Daily midnight
async deactivateExpiredRates() {
  await this.rateBoardRepo.updateMany({
    where: { validUntil: { lt: new Date() }, isActive: true },
    data: { isActive: false },
  });
}

// Warning 7 days before expiry
@Cron('0 9 * * *')
async warnExpiringRates() {
  const expiring = await this.getRatesExpiringIn(7);
  for (const rate of expiring) {
    await this.notifyProvider(rate.providerId, 'rate_expiring_soon', rate);
  }
}
```

---

### R5: SEO زمان‌بر باشد

**راهکار:**
- شروع تولید محتوا از **ماه ۲** (نه ماه ۵)
- ۴ صفحه مسیر اصلی آماده قبل از لانچ
- Pre-launch: submit sitemap to Google Search Console
- Internal linking structure from day one

---

### R6: تحریم و محدودیت بین‌المللی

**راهکار فنی:**
```typescript
// Payment: focus on local currencies (IRR) for MVP
// Hosting: Hetzner (EU) for international, Arvan for Iran
// SMS: Kavenegar for Iran, international SMS via Twilio
// Maps: Mapbox (less restricted than Google in some regions)

// Country whitelist for MVP
const SUPPORTED_COUNTRIES = [
  'IR', 'CN', 'TR', 'IQ', 'AE', 'UZ', 'TM', 'AZ', 'KZ'
];
```

---

### R7: رقبای موجود

**تمایز:**
- B2B focus (نه B2C مثل اسنپ‌باکس)
- بین‌المللی (نه داخلی)
- شفافیت قیمت (نرخ لحظه‌ای)
- بار برگشت (Backhaul) — unique feature

---

### R8: پیچیدگی گمرکی

**راهکار:**
- محتوای دقیق با بازبینی کارشناس گمرک
- Disclaimer: "اطلاعات راهنما هستند، برای تصمیم نهایی با کارشناس مشورت کنید"
- FAQ structured data for common customs questions

---

## Risk Monitoring Dashboard

```typescript
// Admin dashboard risk indicators
interface RiskMetrics {
  providersWithoutRates: number;      // R1 — alert if > 50%
  driverAppInstallRate: number;       // R2 — alert if < 30%
  gpsOfflineTrips: number;            // R3 — alert if > 20%
  expiredActiveRates: number;         // R4 — alert if > 0
  seoPagesIndexed: number;            // R5 — alert if < 10 at launch
}
```

## Contingency Plans

| Risk Materialized | Plan B |
|-------------------|--------|
| R1: < 5 providers with rates | Ops team manually enters rates for top 10 providers |
| R2: < 10 drivers | Partner with 2-3 large transport companies |
| R3: GPS unreliable | Manual status updates by driver + phone check-ins |
| R5: No organic traffic | Paid ads (Google Ads) for top 5 keywords |
| R6: Service blocked | Mirror site on Arvan Cloud for Iran users |

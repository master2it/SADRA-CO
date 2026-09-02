# 17. خارج از محدوده فاز ۱

[← بازگشت به فهرست](./README.md)

---

## فاز ۲ (ماه ۷ تا ۹)

- 💳 پرداخت آنلاین + کیف پول + Escrow
- 📄 بارنامه دیجیتال با امضای الکترونیکی
- 🚢 افزودن حمل دریایی
- 🚂 افزودن حمل ریلی
- ⭐ سیستم امتیازدهی دوبل
- 🌐 نسخه عربی و چینی کامل

---

## فاز ۳ (ماه ۱۰ تا ۱۲)

- 🤖 هوش مصنوعی برای قیمت‌گذاری هوشمند
- 📊 داشبورد تحلیلی پیشرفته
- 🔌 API عمومی برای شرکت‌های بزرگ
- 🛡️ بیمه بار آنلاین
- 📦 بازار لوازم جانبی حمل‌ونقل

---

## فاز ۴ (سال دوم)

- 🌍 حمل بین‌المللی چندوجهی (Multimodal)
- 🏷️ White Label برای شرکت‌های بزرگ
- 🤝 اتصال به ERP شرکت‌های بزرگ
- 📱 اپلیکیشن مشتری نیتیو

---

## جزئیات پیاده‌سازی

### چرا خارج از محدوده؟

| Feature | دلیل تعویق | پیش‌نیاز فاز ۱ |
|---------|-----------|---------------|
| پرداخت آنلاین | Lead Gen کافی برای MVP | حجم تراکنش، قرارداد با درگاه |
| حمل دریایی/ریلی | تمرکز بر حمل زمینی | `transport_mode` extensible در DB |
| دستیار AI پایه (چت + پیش‌فاکتور) | در MVP فاز ۱ پیاده می‌شود | [داکیومنت ۱۸](./18-crm-srm-ai-assistant.md) |
| AI Pricing پیشرفته | نیاز به داده تاریخی ۶+ ماهه | پلتفرم پایدار + CRM data |
| White Label | نیاز به مشتری Enterprise | پلتفرم پایدار |
| اپ مشتری نیتیو | وب PWA کافی برای MVP | — |

### آماده‌سازی معماری برای فازهای بعدی

#### فاز ۲: پرداخت
```sql
-- Tables ready but not used in MVP
CREATE TABLE wallets (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  balance DECIMAL(15,2) DEFAULT 0,
  currency currency NOT NULL
);

CREATE TABLE transactions (
  id UUID PRIMARY KEY,
  wallet_id UUID REFERENCES wallets(id),
  amount DECIMAL(15,2),
  type VARCHAR(20), -- deposit, withdrawal, commission, escrow
  status VARCHAR(20),
  reference_id UUID,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE escrows (
  id UUID PRIMARY KEY,
  shipment_id UUID REFERENCES shipment_requests(id),
  amount DECIMAL(15,2),
  status VARCHAR(20), -- held, released, refunded
  released_at TIMESTAMPTZ
);
```

#### فاز ۲: حمل دریایی/ریلی
```typescript
// Already in schema — just add UI and business logic
enum TransportMode {
  LAND = 'land',    // ✅ MVP
  SEA = 'sea',      // Phase 2
  RAIL = 'rail',    // Phase 2
  MULTI = 'multi',  // Phase 4
}

// Route waypoints support multi-modal
interface Waypoint {
  country: string;
  city: string;
  borderCrossing?: boolean;
  transportMode: TransportMode;  // can change per segment
  portName?: string;             // for sea
  stationName?: string;          // for rail
}
```

#### فاز ۳: API عمومی
```typescript
// Future: Public API with API keys
// GET /v2/public/rates?origin=...&destination=...
// Rate limit: 1000 req/day per API key
// Authentication: X-API-Key header
```

### Roadmap Timeline

```
2026 Q1-Q2: Phase 1 (MVP) ──────────────────────────▶ Launch
2026 Q3:    Phase 2 ──── Payment, Sea/Rail, Rating
2026 Q4:    Phase 3 ──── AI, Analytics, Public API
2027 Q1-Q2: Phase 4 ──── Multimodal, White Label, ERP
```

### Feature Request Process (Post-MVP)

```
User/Stakeholder Request
  → Product Backlog
  → Prioritization (RICE score)
  → Sprint Planning
  → Development
  → Release
```

### Technical Debt to Address Before Phase 2

- [ ] Refactor rate search for multi-modal routes
- [ ] Add payment gateway abstraction layer
- [ ] Implement proper audit logging
- [ ] Scale GPS tracking (partitioning, archiving)
- [ ] Add comprehensive E2E test suite
- [ ] Performance optimization for 1000+ concurrent users

### Migration Notes

When adding Phase 2 features, ensure:
1. **Backward compatibility** — existing API v1 continues working
2. **Database migrations** — additive only, no breaking changes
3. **Feature flags** — new features behind flags until stable
4. **Documentation** — update API docs and user guides

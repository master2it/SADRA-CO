# 5. معماری کلان سیستم

[← بازگشت به فهرست](./README.md)

---

## 5.1 دیاگرام معماری

```
┌─────────────────────────────────────────────────────────────┐
│                        کاربران                               │
│  [مشتری Web]  [Provider Web]  [راننده App]  [ادمین/اپراتور]  [AI Chat] │
└────────┬──────────────┬──────────────┬──────────────┬───────┘
         │              │              │              │
         ▼              ▼              ▼              ▼
┌─────────────────────────────────────────────────────────────┐
│                     API Gateway (Nginx)                     │
└────────┬────────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│                  Backend Services (NestJS)                  │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐       │
│  │   Auth   │ │ Shipment │ │ Provider │ │  Driver  │       │
│  │ Service  │ │ Service  │ │ Service  │ │ Service  │       │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘       │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐       │
│  │  Pricing │ │  Route   │ │Notification│ │ Tracking │       │
│  │ Service  │ │ Service  │ │ Service  │ │ Service  │       │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘       │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐                    │
│  │   CRM    │ │   SRM    │ │    AI    │                    │
│  │ Service  │ │ Service  │ │ Service  │                    │
│  └──────────┘ └──────────┘ └──────────┘                    │
└────────┬────────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│                     Data Layer                              │
│  [PostgreSQL]  [Redis Cache]  [S3 Storage]  [ElasticSearch]│
└─────────────────────────────────────────────────────────────┘
```

---

## 5.2 ماژول‌های سیستم

| ماژول | مسئولیت |
|-------|---------|
| **Auth** | احراز هویت چندنقشی (OTP + JWT) |
| **Shipment** | مدیریت درخواست‌های حمل |
| **Provider** | مدیریت Providerها و تابلو نرخ |
| **Pricing** | محاسبه نرخ، مقایسه، پیشنهاد بهترین |
| **Route** | مدیریت مسیرهای بین‌المللی، نقشه |
| **Driver** | اپلیکیشن راننده، بار برگشت |
| **Tracking** | ردیابی GPS لحظه‌ای |
| **Notification** | پیامک، ایمیل، پوش نوتیفیکیشن |
| **Content/SEO** | مدیریت محتوای SEO، بلاگ، راهنماها |
| **CRM** | مدیریت Lead، Pipeline فروش، SLA، کانال‌های ورودی |
| **SRM** | مدیریت تامین‌کننده، Onboarding، عملکرد Provider |
| **AI** | دستیار هوشمند، چت، پیش‌فاکتور، handoff به اپراتور |
| **Product** | کاتالوگ، سفارش تولید | [commerce](./commerce/README.md) |
| **ExportSales** | ۱۱ مرحله فروش صادراتی + پرداخت | [commerce/C3](./commerce/03-export-sales-procedure.md) |
| **Procurement** | تدارکات / خرید از کارخانه | [SCOR](./20-supply-chain-scor.md) |
| **Process** | ۱۹ فرایند بارگیری | [logistics/L1](./logistics/01-shipment-loading-processes.md) |

> جزئیات CRM/SRM/AI: [commerce/C1](./18-crm-srm-ai-assistant.md) | کاتالوگ: [commerce/C2](./19-product-catalog-and-manufacturing.md) | فروش صادراتی: [C3](./commerce/03-export-sales-procedure.md) | SCOR: [20](./20-supply-chain-scor.md) | فرایندها: [logistics/L1](./logistics/01-shipment-loading-processes.md)

---

## جزئیات پیاده‌سازی

### ساختار NestJS Modules

```
apps/api/src/
├── main.ts
├── app.module.ts
├── common/
│   ├── guards/          # JwtAuthGuard, RolesGuard
│   ├── decorators/      # @CurrentUser, @Roles
│   ├── filters/         # HttpExceptionFilter
│   ├── interceptors/  # Logging, Transform
│   └── pipes/           # ValidationPipe
├── modules/
│   ├── auth/
│   │   ├── auth.module.ts
│   │   ├── auth.controller.ts
│   │   ├── auth.service.ts
│   │   ├── strategies/  # jwt.strategy.ts, otp.strategy.ts
│   │   └── dto/
│   ├── shipment/
│   ├── provider/
│   ├── pricing/
│   ├── route/
│   ├── driver/
│   ├── tracking/
│   ├── notification/
│   ├── content/
│   ├── crm/             # Lead management, pipeline, SLA
│   ├── srm/             # Supplier onboarding, performance
│   ├── ai/              # Chat assistant, proforma, handoff
│   ├── product/         # Catalog, manufacturing orders
│   └── factory/         # Factory management
├── database/
│   ├── migrations/
│   └── seeds/
└── config/
```

### Communication Patterns

| الگو | استفاده | ابزار |
|------|---------|-------|
| Sync REST | CRUD عملیات | HTTP/JSON |
| WebSocket | ردیابی لحظه‌ای GPS | Socket.io |
| Queue | SMS, Email, Import اکسل | BullMQ |
| Cache | نرخ‌ها، Session | Redis |
| Event Bus | تغییر وضعیت Shipment | NestJS EventEmitter |

### API Versioning

```
Base URL: https://api.sadra.example.com/v1/

/v1/auth/*
/v1/rates/*
/v1/shipments/*
/v1/providers/*
/v1/routes/*
/v1/drivers/*
/v1/tracking/*
/v1/crm/*
/v1/srm/*
/v1/ai/*
/v1/products/*
/v1/manufacturing/*
/v1/admin/*
```

### WebSocket Events (Tracking)

```typescript
// Server → Client (Admin Dashboard)
'tracking:location_update' → { tripId, lat, lng, speed, timestamp }
'tracking:trip_status_change' → { tripId, status, event }

// Client → Server (Driver App)
'tracking:subscribe' → { tripId }
'tracking:location' → { lat, lng, speed, heading, accuracy }
```

### Redis Cache Strategy

| Key Pattern | TTL | توضیح |
|-------------|-----|-------|
| `rates:{origin}:{dest}:{weight}` | 5 min | نتیجه استعلام نرخ |
| `route:{slug}` | 1 hour | داده مسیر |
| `session:{userId}` | 7 days | JWT refresh |
| `otp:{mobile}` | 5 min | کد OTP |
| `provider:{id}:stats` | 15 min | آمار داشبورد |

### Docker Compose (Development)

```yaml
services:
  api:
    build: ./apps/api
    ports: ["3001:3001"]
    depends_on: [postgres, redis]

  web:
    build: ./apps/web
    ports: ["3000:3000"]
    depends_on: [api]

  postgres:
    image: postgis/postgis:16-3.4
    ports: ["5432:5432"]
    volumes: [pgdata:/var/lib/postgresql/data]

  redis:
    image: redis:7-alpine
    ports: ["6379:6379"]

  minio:
    image: minio/minio
    ports: ["9000:9000"]
    command: server /data

  elasticsearch:
    image: elasticsearch:8.11.0
    ports: ["9200:9200"]
```

### Security Layers

```
Internet → Cloudflare (DDoS, SSL, CDN)
         → Nginx (Rate Limiting, CORS)
         → NestJS (JWT, RBAC, Validation)
         → PostgreSQL (Row-level for Provider data)
```

### Rate Limiting

| Endpoint | Limit | Window |
|----------|-------|--------|
| `/auth/otp` | 3 requests | 5 min per mobile |
| `/rates/search` | 30 requests | 1 min per IP |
| `/tracking/location` | 120 requests | 1 min per driver |
| General API | 100 requests | 1 min per user |

### Monitoring Stack

```
App (Sentry) → Error tracking
API (Prometheus) → Metrics
Grafana → Dashboards
Loki → Log aggregation
Uptime Kuma → Health checks
```

### Health Check Endpoints

```
GET /health          → { status: "ok" }
GET /health/db       → PostgreSQL connection
GET /health/redis    → Redis connection
GET /health/storage  → S3/MinIO connection
```

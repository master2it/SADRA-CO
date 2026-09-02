# 13. استک فنی پیشنهادی

[← بازگشت به فهرست](./README.md)

---

## 13.1 وب پلتفرم

| لایه | تکنولوژی | دلیل |
|------|---------|------|
| **Frontend** | Next.js 14 + TypeScript + TailwindCSS | SSR برای SEO، سرعت بالا |
| **UI Components** | shadcn/ui + Radix | حرفه‌ای، قابل سفارشی‌سازی |
| **Maps** | Mapbox GL JS | زیبا، سریع، ارزان‌تر از Google Maps |
| **State Management** | Zustand + React Query | سبک و کارآمد |
| **Backend** | NestJS + TypeScript | ماژولار، مقیاس‌پذیر |
| **Database** | PostgreSQL 16 | پشتیبانی از PostGIS برای نقشه |
| **Cache** | Redis | برای نرخ‌ها و Session |
| **Search** | Elasticsearch | برای جستجوی مسیر و محتوا |
| **File Storage** | S3-compatible (MinIO / Arvan Cloud) | برای مدارک و تصاویر |
| **Queue** | BullMQ (Redis-based) | برای پردازش ناهمگام |
| **AI/LLM** | OpenAI API / Local LLM | دستیار هوشمند مشتری، پیش‌فاکتور |
| **SMS** | Kavenegar / FarazSMS | پیامک داخلی |
| **Email** | AWS SES / Mailgun | ایمیل بین‌المللی |
| **Auth** | JWT + OTP | استاندارد |
| **Monitoring** | Sentry + Prometheus + Grafana | پایش سیستم |

---

## 13.2 اپلیکیشن راننده

| لایه | تکنولوژی | دلیل |
|------|---------|------|
| **Framework** | Flutter 3.x | یک کد برای iOS + Android |
| **State** | Riverpod | مدرن و تست‌پذیر |
| **Maps** | Google Maps Flutter | دقیق و پایدار |
| **Background Location** | flutter_background_geolocation | بهینه برای باتری |
| **Local DB** | Isar / Drift | آفلاین |
| **Push** | Firebase Cloud Messaging | استاندارد |

---

## 13.3 زیرساخت

| لایه | تکنولوژی |
|------|---------|
| **Container** | Docker + Docker Compose |
| **Orchestration** | Kubernetes (در مقیاس بزرگ) / Docker Swarm (اول) |
| **CI/CD** | GitHub Actions |
| **Hosting** | Hetzner (خارج) + ابر آروان (داخل) |
| **CDN** | Cloudflare |
| **Domain & SSL** | Cloudflare |
| **Backup** | روزانه به S3 |

---

## جزئیات پیاده‌سازی

### Monorepo Setup (Turborepo)

```json
// package.json (root)
{
  "name": "sadra",
  "private": true,
  "workspaces": ["apps/*", "packages/*"],
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "test": "turbo run test",
    "lint": "turbo run lint"
  },
  "devDependencies": {
    "turbo": "^2.0.0",
    "typescript": "^5.4.0"
  }
}
```

```
sadra/
├── apps/
│   ├── web/                 # Next.js 14
│   ├── api/                 # NestJS
│   └── driver-app/          # Flutter
├── packages/
│   ├── shared-types/        # TypeScript types + enums
│   ├── ui/                  # Shared React components
│   ├── eslint-config/       # Shared ESLint
│   └── tsconfig/            # Shared TSConfig
├── infrastructure/
│   ├── docker/
│   ├── k8s/
│   └── terraform/
├── docs/
├── turbo.json
└── package.json
```

### Backend Dependencies (NestJS)

```json
{
  "dependencies": {
    "@nestjs/common": "^10.0.0",
    "@nestjs/core": "^10.0.0",
    "@nestjs/platform-express": "^10.0.0",
    "@nestjs/jwt": "^10.0.0",
    "@nestjs/passport": "^10.0.0",
    "@nestjs/swagger": "^7.0.0",
    "@nestjs/bull": "^10.0.0",
    "@nestjs/websockets": "^10.0.0",
    "@prisma/client": "^5.0.0",
    "passport-jwt": "^4.0.0",
    "bcrypt": "^5.0.0",
    "class-validator": "^0.14.0",
    "class-transformer": "^0.5.0",
    "bullmq": "^5.0.0",
    "ioredis": "^5.0.0",
    "@aws-sdk/client-s3": "^3.0.0"
  }
}
```

### Frontend Dependencies (Next.js)

```json
{
  "dependencies": {
    "next": "^14.2.0",
    "react": "^18.3.0",
    "react-dom": "^18.3.0",
    "@tanstack/react-query": "^5.0.0",
    "zustand": "^4.5.0",
    "next-intl": "^3.0.0",
    "mapbox-gl": "^3.0.0",
    "tailwindcss": "^3.4.0",
    "@radix-ui/react-dialog": "^1.0.0",
    "lucide-react": "^0.300.0",
    "zod": "^3.22.0",
    "react-hook-form": "^7.50.0"
  }
}
```

### Flutter Dependencies

```yaml
# pubspec.yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_riverpod: ^2.5.0
  dio: ^5.4.0
  isar: ^3.1.0
  google_maps_flutter: ^2.6.0
  flutter_background_geolocation: ^4.15.0
  firebase_messaging: ^14.7.0
  flutter_secure_storage: ^9.0.0
  geolocator: ^11.0.0
  image_picker: ^1.0.0
```

### Environment Setup

```bash
# Prerequisites
node >= 20
pnpm >= 8
docker >= 24
flutter >= 3.19
postgresql >= 16 (with PostGIS)
redis >= 7

# Clone & install
git clone <repo>
cd sadra
pnpm install

# Start infrastructure
docker compose up -d postgres redis minio

# Run migrations
pnpm --filter api prisma migrate dev

# Seed data
pnpm --filter api prisma db seed

# Start dev
pnpm dev
```

### CI/CD Pipeline (GitHub Actions)

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v2
      - run: pnpm install
      - run: pnpm lint
      - run: pnpm test
      - run: pnpm build

  deploy-staging:
    needs: lint-and-test
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    steps:
      - run: docker build -t sadra-api ./apps/api
      - run: docker push registry/sadra-api:staging
      - run: kubectl apply -f infrastructure/k8s/staging/
```

### Database Migration Strategy

```bash
# Development
pnpm --filter api prisma migrate dev --name add_rate_boards

# Production
pnpm --filter api prisma migrate deploy

# Generate client after schema change
pnpm --filter api prisma generate
```

### API Documentation

```typescript
// Swagger setup in main.ts
const config = new DocumentBuilder()
  .setTitle('SADRA API')
  .setDescription('International Freight Platform API')
  .setVersion('1.0')
  .addBearerAuth()
  .build();
const document = SwaggerModule.createDocument(app, config);
SwaggerModule.setup('api/docs', app, document);
// Access: http://localhost:3001/api/docs
```

### Logging & Monitoring

```typescript
// Structured logging with Pino
import { Logger } from 'nestjs-pino';

// Sentry error tracking
import * as Sentry from '@sentry/nestjs';
Sentry.init({ dsn: process.env.SENTRY_DSN });

// Prometheus metrics
import { PrometheusModule } from '@willsoto/nestjs-prometheus';
```

### Security Checklist

- [ ] HTTPS everywhere (Cloudflare)
- [ ] JWT with short expiry + refresh tokens
- [ ] Rate limiting on auth endpoints
- [ ] Input validation (class-validator)
- [ ] SQL injection prevention (Prisma parameterized queries)
- [ ] XSS prevention (React auto-escape + CSP headers)
- [ ] CORS configured properly
- [ ] Secrets in environment variables (not in code)
- [ ] File upload validation (type, size)
- [ ] RBAC on all protected endpoints

### Development URLs

| Service | URL | Port |
|---------|-----|------|
| Web (Next.js) | http://localhost:3000 | 3000 |
| API (NestJS) | http://localhost:3001 | 3001 |
| API Docs (Swagger) | http://localhost:3001/api/docs | 3001 |
| PostgreSQL | localhost:5432 | 5432 |
| Redis | localhost:6379 | 6379 |
| MinIO Console | http://localhost:9001 | 9001 |
| Elasticsearch | http://localhost:9200 | 9200 |

# 4. ذینفعان و نقش‌های کاربران

[← بازگشت به فهرست](./README.md)

---

## 4.1 ماتریس نقش‌ها

| نقش | توضیح | دسترسی کلیدی |
|-----|-------|-------------|
| **مهمان (Guest)** | بازدیدکننده سایت | مشاهده نرخ‌ها، مسیرها، محتوای SEO |
| **مشتری (Shipper)** | صاحب بار | ثبت درخواست، مشاهده نرخ‌ها، انتخاب Provider |
| **Provider (شرکت حمل)** | تامین‌کننده خدمات حمل | مدیریت تابلو نرخ، دریافت درخواست، ثبت پیشنهاد |
| **راننده (Driver)** | راننده کامیون | اپلیکیشن موبایل، مشاهده بار، ارسال موقعیت |
| **اپراتور (Operator)** | کارمند داخلی (فقط برای نظارت) | پشتیبانی، دخالت در موارد خاص |
| **ادمین (Admin)** | مدیر پلتفرم | مدیریت کل سیستم |
| **مدیر مالی (Finance)** | حسابداری پلتفرم | تأیید SWIFT، پیش‌پرداخت، مانده، گزارش مالی |

---

## 4.2 ماتریس دسترسی (RACM)

| قابلیت | مهمان | مشتری | Provider | راننده | اپراتور | ادمین |
|--------|-------|-------|----------|--------|---------|-------|
| مشاهده نرخ‌ها | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |
| ثبت درخواست | ❌ | ✅ | ❌ | ❌ | ✅ | ✅ |
| مدیریت تابلو نرخ | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ |
| ثبت پیشنهاد | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ |
| مشاهده بارهای موجود | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ |
| ارسال GPS | ❌ | ❌ | ❌ | ✅ | ❌ | ✅ |
| تایید Provider | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| مدیریت Lead (CRM) | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| Pipeline فروش | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| تایید پیش‌فاکتور AI | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |

---

## جزئیات پیاده‌سازی

### Enum نقش‌ها (TypeScript)

```typescript
// packages/shared-types/src/enums/role.enum.ts
export enum UserRole {
  GUEST = 'guest',       // بدون احراز هویت
  SHIPPER = 'shipper',
  PROVIDER = 'provider',
  DRIVER = 'driver',
  OPERATOR = 'operator',
  ADMIN = 'admin',
  FINANCE = 'finance',
}

export enum UserStatus {
  PENDING = 'pending',     // در انتظار تایید
  ACTIVE = 'active',
  BLOCKED = 'blocked',
  SUSPENDED = 'suspended',
}
```

### RBAC در NestJS

```typescript
// apps/api/src/common/decorators/roles.decorator.ts
export const ROLES_KEY = 'roles';
export const Roles = (...roles: UserRole[]) => SetMetadata(ROLES_KEY, roles);

// apps/api/src/common/guards/roles.guard.ts
@Injectable()
export class RolesGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.get<UserRole[]>(ROLES_KEY, context.getHandler());
    if (!requiredRoles) return true;
    const { user } = context.switchToHttp().getRequest();
    return requiredRoles.includes(user.role);
  }
}
```

### Permission Matrix (جزئی‌تر)

| Resource | Action | Guest | Shipper | Provider | Driver | Operator | Admin |
|----------|--------|-------|---------|----------|--------|----------|-------|
| `rate_board` | read | ✅ public | ✅ | ✅ own | ❌ | ✅ | ✅ |
| `rate_board` | create | ❌ | ❌ | ✅ own | ❌ | ❌ | ✅ |
| `rate_board` | update | ❌ | ❌ | ✅ own | ❌ | ❌ | ✅ |
| `rate_board` | delete | ❌ | ❌ | ✅ own | ❌ | ❌ | ✅ |
| `shipment_request` | create | ❌ | ✅ | ❌ | ❌ | ✅ | ✅ |
| `shipment_request` | read | ❌ | ✅ own | ✅ related | ❌ | ✅ | ✅ |
| `shipment_offer` | create | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ |
| `trip` | read | ❌ | ✅ own | ✅ related | ✅ own | ✅ | ✅ |
| `location` | create | ❌ | ❌ | ❌ | ✅ own | ❌ | ✅ |
| `provider` | approve | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| `driver` | approve | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| `lead` | read | ❌ | ✅ own | ❌ | ❌ | ✅ assigned | ✅ |
| `lead` | update | ❌ | ❌ | ❌ | ❌ | ✅ assigned | ✅ |
| `proforma` | approve | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| `ai_chat` | use | ✅ guest | ✅ | ❌ | ❌ | ✅ | ✅ |

### JWT Payload Structure

```typescript
interface JwtPayload {
  sub: string;           // user.id (UUID)
  role: UserRole;
  status: UserStatus;
  providerId?: string;   // اگر role = provider
  driverId?: string;     // اگر role = driver
  iat: number;
  exp: number;
}
```

### Route Guards در Next.js (Middleware)

```typescript
// apps/web/src/middleware.ts
const protectedRoutes: Record<string, UserRole[]> = {
  '/dashboard': ['shipper'],
  '/provider': ['provider'],
  '/admin': ['admin', 'operator', 'finance'],
};

// اپ راننده جداگانه — Flutter با JWT
```

### Flow احراز هویت Provider/Driver

```
ثبت‌نام → آپلود مدارک → status: pending
                              ↓
                    ادمین بررسی مدارک
                              ↓
              approved → status: active → دسترسی کامل
              rejected → notification + دلیل رد
```

### API Endpoints مرتبط با نقش‌ها

| Method | Endpoint | Roles | توضیح |
|--------|----------|-------|-------|
| POST | `/auth/register` | public | ثبت‌نام با انتخاب نقش |
| POST | `/auth/login` | public | ورود OTP/JWT |
| GET | `/auth/me` | authenticated | پروفایل فعلی |
| POST | `/provider/documents` | provider | آپلود مدارک |
| PATCH | `/admin/providers/:id/approve` | admin | تایید Provider |
| PATCH | `/admin/drivers/:id/approve` | admin | تایید راننده |
| GET | `/admin/users` | admin, operator | لیست کاربران |

### Database: جدول users

```sql
CREATE TYPE user_role AS ENUM (
  'shipper', 'provider', 'driver', 'operator', 'admin', 'finance'
);

CREATE TYPE user_status AS ENUM (
  'pending', 'active', 'blocked', 'suspended'
);

CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  mobile VARCHAR(15) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE,
  password_hash VARCHAR(255),
  role user_role NOT NULL,
  status user_status DEFAULT 'pending',
  language VARCHAR(5) DEFAULT 'fa',
  currency VARCHAR(3) DEFAULT 'IRR',
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

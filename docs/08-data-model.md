# 8. مدل داده

[← بازگشت به فهرست](./README.md)

---

## 8.1 موجودیت‌های اصلی

```
User
├── id (UUID)
├── mobile (unique)
├── email (unique, nullable)
├── role (enum: shipper | provider | driver | operator | admin)
├── status (enum: pending | active | blocked)
├── language (enum: fa | en | ar | zh)
├── currency (enum: IRR | USD | EUR | CNY)
├── created_at

ShipperProfile
├── user_id (FK)
├── company_name
├── contact_person
├── national_id / business_license
├── address, phone, website

ProviderProfile
├── user_id (FK)
├── company_name
├── license_number
├── fleet_size
├── fleet_types (jsonb) — [{type: "truck", capacity: 20}, ...]
├── countries_served (array)
├── verification_status (pending | approved | rejected)
├── rating (decimal)
├── total_shipments (int)

DriverProfile
├── user_id (FK)
├── full_name
├── national_id
├── license_number, license_expiry
├── vehicle_type, vehicle_plate, vehicle_capacity
├── countries_allowed (array)
├── current_location (geography Point)
├── last_location_update (timestamp)
├── status (enum: available | on_trip | offline)

RateBoard (تابلو نرخ Provider)
├── id (UUID)
├── provider_id (FK)
├── origin_country, origin_city
├── destination_country, destination_city
├── cargo_type (enum)
├── weight_min, weight_max
├── vehicle_type
├── price (decimal)
├── currency (enum)
├── estimated_days (int)
├── valid_from, valid_until
├── is_active (bool)
├── created_by, updated_at

Route (مسیر)
├── id (UUID)
├── origin_country, origin_city
├── destination_country, destination_city
├── transport_mode (enum: land | sea | rail | multi)
├── distance_km (int)
├── estimated_days (int)
├── waypoints (jsonb) — [{country, city, border_crossing}, ...]
├── geometry (LINESTRING) — برای نمایش روی نقشه
├── required_documents (jsonb)
├── seo_slug (unique)
├── meta_title, meta_description

ShipmentRequest
├── id (UUID)
├── shipper_id (FK)
├── route_id (FK)
├── origin, destination
├── cargo_type, cargo_description
├── weight, dimensions
├── pickup_date, delivery_deadline
├── status (enum: draft | quoted | accepted | in_progress | delivered | cancelled)
├── selected_provider_id (FK, nullable)
├── selected_rate_id (FK, nullable)
├── total_price, currency
├── created_at

ShipmentOffer
├── id (UUID)
├── request_id (FK)
├── provider_id (FK)
├── price, currency
├── estimated_days
├── notes
├── status (enum: pending | accepted | rejected | expired)
├── valid_until
├── created_at

Trip (سفر راننده)
├── id (UUID)
├── driver_id (FK)
├── shipment_id (FK)
├── status (enum: assigned | picked_up | at_border | crossed_border | delivered)
├── started_at, delivered_at
├── current_location (geography)
├── location_history (jsonb) — [{lat, lng, timestamp}, ...]
├── events (jsonb) — [{type, timestamp, location, photo_url}, ...]

LocationTracking
├── id (UUID)
├── driver_id (FK)
├── trip_id (FK)
├── location (geography Point)
├── speed, heading, accuracy
├── recorded_at

Content (برای SEO)
├── id (UUID)
├── type (enum: route_page | blog | guide | faq | glossary)
├── slug (unique)
├── title, meta_title, meta_description
├── content (jsonb — rich text)
├── language (enum)
├── published_at
```

---

## 8.2 دیاگرام ERD (خلاصه)

```
User (1) ──── (1) ShipperProfile / ProviderProfile / DriverProfile
ProviderProfile (1) ──── (N) RateBoard
Route (1) ──── (N) RateBoard
Route (1) ──── (N) ShipmentRequest
ShipmentRequest (1) ──── (N) ShipmentOffer
ShipmentRequest (1) ──── (0..1) Trip
Trip (1) ──── (1) DriverProfile
Trip (1) ──── (N) LocationTracking
```

---

## جزئیات پیاده‌سازی — SQL Schema

### Extensions

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "postgis";
```

### Enums

```sql
CREATE TYPE user_role AS ENUM ('shipper','provider','driver','operator','admin','finance');
CREATE TYPE user_status AS ENUM ('pending','active','blocked','suspended');
CREATE TYPE verification_status AS ENUM ('pending','approved','rejected');
CREATE TYPE cargo_type AS ENUM ('general','refrigerated','hazardous','oversized','container');
CREATE TYPE vehicle_type AS ENUM ('truck_10t','truck_20t','truck_40t','trailer','tanker');
CREATE TYPE currency AS ENUM ('IRR','USD','EUR','CNY');
CREATE TYPE transport_mode AS ENUM ('land','sea','rail','multi');
CREATE TYPE shipment_status AS ENUM ('draft','quoted','accepted','in_progress','delivered','cancelled');
CREATE TYPE offer_status AS ENUM ('pending','accepted','rejected','expired');
CREATE TYPE trip_status AS ENUM ('assigned','picked_up','en_route','at_border','crossed_border','delivered');
CREATE TYPE driver_status AS ENUM ('available','on_trip','offline');
CREATE TYPE content_type AS ENUM ('route_page','blog','guide','faq','glossary','case_study','landing');
```

### Core Tables

```sql
-- users
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  mobile VARCHAR(15) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE,
  password_hash VARCHAR(255),
  role user_role NOT NULL,
  status user_status DEFAULT 'pending',
  language VARCHAR(5) DEFAULT 'fa',
  currency currency DEFAULT 'IRR',
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- shipper_profiles
CREATE TABLE shipper_profiles (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID UNIQUE NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  company_name VARCHAR(255),
  contact_person VARCHAR(255),
  national_id VARCHAR(20),
  business_license VARCHAR(50),
  address TEXT,
  phone VARCHAR(20),
  website VARCHAR(255),
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- provider_profiles
CREATE TABLE provider_profiles (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID UNIQUE NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  company_name VARCHAR(255) NOT NULL,
  license_number VARCHAR(50),
  fleet_size INT DEFAULT 0,
  fleet_types JSONB DEFAULT '[]',
  countries_served TEXT[] DEFAULT '{}',
  verification_status verification_status DEFAULT 'pending',
  rating DECIMAL(3,2) DEFAULT 0,
  total_shipments INT DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- driver_profiles
CREATE TABLE driver_profiles (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID UNIQUE NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  full_name VARCHAR(255) NOT NULL,
  national_id VARCHAR(20),
  license_number VARCHAR(50),
  license_expiry DATE,
  vehicle_type vehicle_type,
  vehicle_plate VARCHAR(20),
  vehicle_capacity DECIMAL(10,2),
  countries_allowed TEXT[] DEFAULT '{}',
  current_location GEOGRAPHY(POINT, 4326),
  last_location_update TIMESTAMPTZ,
  status driver_status DEFAULT 'offline',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- routes
CREATE TABLE routes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  origin_country VARCHAR(100) NOT NULL,
  origin_city VARCHAR(100) NOT NULL,
  destination_country VARCHAR(100) NOT NULL,
  destination_city VARCHAR(100) NOT NULL,
  transport_mode transport_mode DEFAULT 'land',
  distance_km INT,
  estimated_days INT,
  waypoints JSONB DEFAULT '[]',
  geometry GEOGRAPHY(LINESTRING, 4326),
  required_documents JSONB DEFAULT '[]',
  seo_slug VARCHAR(255) UNIQUE NOT NULL,
  meta_title VARCHAR(255),
  meta_description TEXT,
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- rate_boards
CREATE TABLE rate_boards (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  provider_id UUID NOT NULL REFERENCES provider_profiles(id) ON DELETE CASCADE,
  route_id UUID REFERENCES routes(id),
  origin_country VARCHAR(100) NOT NULL,
  origin_city VARCHAR(100) NOT NULL,
  destination_country VARCHAR(100) NOT NULL,
  destination_city VARCHAR(100) NOT NULL,
  cargo_type cargo_type NOT NULL,
  weight_min DECIMAL(10,2) NOT NULL,
  weight_max DECIMAL(10,2) NOT NULL,
  vehicle_type vehicle_type NOT NULL,
  price DECIMAL(15,2) NOT NULL,
  currency currency NOT NULL,
  estimated_days INT NOT NULL,
  valid_from TIMESTAMPTZ DEFAULT NOW(),
  valid_until TIMESTAMPTZ NOT NULL,
  is_active BOOLEAN DEFAULT true,
  is_special_rate BOOLEAN DEFAULT false,
  created_by UUID REFERENCES users(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- shipment_requests
CREATE TABLE shipment_requests (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  shipper_id UUID NOT NULL REFERENCES shipper_profiles(id),
  route_id UUID REFERENCES routes(id),
  origin_country VARCHAR(100) NOT NULL,
  origin_city VARCHAR(100) NOT NULL,
  destination_country VARCHAR(100) NOT NULL,
  destination_city VARCHAR(100) NOT NULL,
  cargo_type cargo_type NOT NULL,
  cargo_description TEXT,
  weight DECIMAL(10,2) NOT NULL,
  dimensions JSONB,  -- {length, width, height}
  pickup_date DATE,
  delivery_deadline DATE,
  status shipment_status DEFAULT 'draft',
  selected_provider_id UUID REFERENCES provider_profiles(id),
  selected_rate_id UUID REFERENCES rate_boards(id),
  total_price DECIMAL(15,2),
  currency currency,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- shipment_offers
CREATE TABLE shipment_offers (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  request_id UUID NOT NULL REFERENCES shipment_requests(id) ON DELETE CASCADE,
  provider_id UUID NOT NULL REFERENCES provider_profiles(id),
  price DECIMAL(15,2) NOT NULL,
  currency currency NOT NULL,
  estimated_days INT NOT NULL,
  notes TEXT,
  status offer_status DEFAULT 'pending',
  valid_until TIMESTAMPTZ NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- trips
CREATE TABLE trips (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  driver_id UUID NOT NULL REFERENCES driver_profiles(id),
  shipment_id UUID NOT NULL REFERENCES shipment_requests(id),
  status trip_status DEFAULT 'assigned',
  started_at TIMESTAMPTZ,
  delivered_at TIMESTAMPTZ,
  current_location GEOGRAPHY(POINT, 4326),
  events JSONB DEFAULT '[]',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- location_tracking (partitioned by month for performance)
CREATE TABLE location_tracking (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  driver_id UUID NOT NULL REFERENCES driver_profiles(id),
  trip_id UUID NOT NULL REFERENCES trips(id),
  location GEOGRAPHY(POINT, 4326) NOT NULL,
  speed DECIMAL(6,2),
  heading DECIMAL(5,2),
  accuracy DECIMAL(8,2),
  recorded_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- rate_board_audit_log (PRV-08)
CREATE TABLE rate_board_audit_log (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  rate_board_id UUID NOT NULL REFERENCES rate_boards(id),
  changed_by UUID NOT NULL REFERENCES users(id),
  action VARCHAR(20) NOT NULL,  -- create, update, delete
  old_values JSONB,
  new_values JSONB,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- documents (مدارک Provider/Driver/Trip)
CREATE TABLE documents (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  entity_type VARCHAR(20) NOT NULL,  -- provider, driver, trip
  entity_id UUID NOT NULL,
  document_type VARCHAR(50) NOT NULL,
  file_url VARCHAR(500) NOT NULL,
  status verification_status DEFAULT 'pending',
  uploaded_at TIMESTAMPTZ DEFAULT NOW()
);

-- contents (SEO)
CREATE TABLE contents (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  type content_type NOT NULL,
  slug VARCHAR(255) NOT NULL,
  title VARCHAR(255) NOT NULL,
  meta_title VARCHAR(255),
  meta_description TEXT,
  content JSONB NOT NULL,
  language VARCHAR(5) DEFAULT 'fa',
  published_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(slug, language)
);
```

### Indexes

```sql
-- Performance indexes
CREATE INDEX idx_rate_boards_search ON rate_boards (
  origin_country, origin_city, destination_country, destination_city,
  cargo_type, weight_min, weight_max
) WHERE is_active = true AND valid_until > NOW();

CREATE INDEX idx_rate_boards_provider ON rate_boards (provider_id);
CREATE INDEX idx_shipment_requests_shipper ON shipment_requests (shipper_id, status);
CREATE INDEX idx_shipment_requests_status ON shipment_requests (status);
CREATE INDEX idx_trips_driver ON trips (driver_id, status);
CREATE INDEX idx_location_tracking_trip ON location_tracking (trip_id, recorded_at DESC);
CREATE INDEX idx_routes_slug ON routes (seo_slug);
CREATE INDEX idx_driver_location ON driver_profiles USING GIST (current_location);
CREATE INDEX idx_contents_slug ON contents (slug, language);
```

### Prisma Schema (Alternative)

```prisma
model User {
  id        String   @id @default(uuid())
  mobile    String   @unique
  email     String?  @unique
  role      UserRole
  status    UserStatus @default(PENDING)
  language  String   @default("fa")
  currency  Currency @default(IRR)
  createdAt DateTime @default(now())

  shipperProfile  ShipperProfile?
  providerProfile ProviderProfile?
  driverProfile   DriverProfile?
}

model RateBoard {
  id                  String    @id @default(uuid())
  providerId          String
  provider            ProviderProfile @relation(fields: [providerId], references: [id])
  originCountry       String
  originCity          String
  destinationCountry  String
  destinationCity     String
  cargoType           CargoType
  weightMin           Decimal
  weightMax           Decimal
  vehicleType         VehicleType
  price               Decimal
  currency            Currency
  estimatedDays       Int
  validFrom           DateTime  @default(now())
  validUntil          DateTime
  isActive            Boolean   @default(true)

  @@index([originCountry, destinationCountry, cargoType])
}
```

### Seed Data — مسیرهای اولیه

```sql
INSERT INTO routes (origin_country, origin_city, destination_country, destination_city, distance_km, estimated_days, seo_slug, meta_title) VALUES
('چین', 'شانگهای', 'ایران', 'تهران', 5800, 14, 'china-to-iran', 'حمل بار از چین به ایران'),
('ایران', 'تهران', 'ترکیه', 'استانبول', 2500, 5, 'iran-to-turkey', 'حمل بار از ایران به ترکیه'),
('ایران', 'تهران', 'عراق', 'بغداد', 800, 2, 'iran-to-iraq', 'حمل بار از ایران به عراق'),
('امارات', 'دبی', 'ایران', 'تهران', 1500, 3, 'uae-to-iran', 'حمل بار از امارات به ایران');
```

---

## 8.3 موجودیت‌های CRM/SRM/AI

```
Lead (سرنخ فروش)
├── id (UUID)
├── full_name, mobile, email, company_name
├── source (enum: web | social | referral | exhibition | phone | ai_chat | email)
├── status (enum: new | contacted | qualified | quote_sent | negotiation | proforma_issued | contract | won | lost)
├── assigned_to (FK → users/operator)
├── next_follow_up_at, last_contact_at
├── shipper_id (FK, nullable)
├── shipment_request_id (FK, nullable)
├── lost_reason, notes, metadata (jsonb)

LeadActivity
├── lead_id (FK)
├── type (call | email | chat | note | status_change | ai_action)
├── performed_by (FK, nullable = AI)
├── description, metadata

AiConversation
├── lead_id (FK)
├── messages (jsonb)
├── summary (برای اپراتور)
├── handoff_requested, handoff_to

ProformaInvoice
├── lead_id, shipment_request_id, provider_id
├── items (jsonb), total_amount, currency
├── generated_by (ai | operator)
├── pdf_url, status (draft | sent | accepted | expired)
```

### ERD — CRM/SRM

```
Lead (1) ──── (N) LeadActivity
Lead (1) ──── (0..1) AiConversation
Lead (1) ──── (0..N) ProformaInvoice
Lead (0..1) ──── (1) ShipperProfile
ProviderProfile (1) ──── (N) RateBoard  ← SRM
```

> SQL کامل CRM: [داکیومنت ۱۸](./18-crm-srm-ai-assistant.md) | محصولات/تولید: [داکیومنت ۱۹](./19-product-catalog-and-manufacturing.md)

---

## 8.4 موجودیت‌های کاتالوگ و تولید

```
Factory (کارخانه)
├── id, name, capabilities, avg_production_days, contract_status

Product (محصول)
├── id, factory_id, sku, name, availability (in_stock | made_to_order | customer_specific)
├── unit_price, currency, manufacturing_days, manufacturing_base_cost
├── customer_id (FK, nullable — فقط برای customer_specific)
├── is_public (false = فقط از طریق پیش‌فاکتور)

CustomManufacturingOrder (درخواست تولید)
├── shipper_id, product_name, specifications, quantity
├── factory_id, estimated_production_days, estimated_cost
├── status (pending → quoted → in_production → ready)
├── resulting_product_id (پس از تولید)

ProformaInvoice (گسترش)
├── type (standard | custom_manufacturing | admin_private)
├── share_token (لینک اختصاصی)
├── manufacturing_warnings, prepayment_required, prepayment_amount
```

### ERD — لجستیک: ۱۹ فرایند

```
ShipmentRequest (1) ──── (N) ShipmentProcessEvent
Trip (1) ──── (N) ShipmentProcessEvent
ShipmentProcessEvent
├── process_code (enum — 19 codes)
├── process_number (1-19)
├── recorded_by (admin | driver | provider | system | gps_auto)
├── location, attachment_urls, occurred_at
```

> جزئیات کامل: [logistics/L1](./logistics/01-shipment-loading-processes.md)

---

## 8.5 موجودیت‌های فروش صادراتی و تدارکات

```
ExportSalesOrder
├── status: inquiry → … → shipping_documents → completed
├── payment_term: advance_20_balance_80 | advance_100
├── product_category, quantity, destination_port, delivery_terms
├── shipment_request_id (لینک به لجستیک)

ExportSalesPayment
├── payment_type: advance | balance
├── swift_reference, status: pending | verified

ProcurementOrder
├── export_order_id, factory_id
├── status: draft → ready_for_loading

ProductPaymentTerms
├── product_category → advance_percent
```

> جزئیات کامل: [commerce/C3](./commerce/03-export-sales-procedure.md) | [SCOR](./20-supply-chain-scor.md)

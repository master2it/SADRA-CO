# 11. استراتژی SEO

[← بازگشت به فهرست](./README.md)

---

## 11.1 اصول فنی SEO

| مورد | اجرا |
|------|------|
| **Server-Side Rendering (SSR)** | Next.js برای ایندکس کامل توسط گوگل |
| **سرعت** | Core Web Vitals: LCP < 2.5s, FID < 100ms, CLS < 0.1 |
| **Schema Markup** | JSON-LD برای Organization, Product, FAQ, BreadcrumbList |
| **Sitemap** | XML sitemap پویا برای همه مسیرها و محتوا |
| **Robots.txt** | بهینه‌سازی شده |
| **Canonical URLs** | جلوگیری از محتوای تکراری |
| **Hreflang** | برای نسخه‌های چندزبانه |
| **Mobile-First** | طراحی اول موبایل |
| **HTTPS** | اجباری |
| **CDN** | برای تصاویر و استاتیک‌ها |

---

## 11.2 استراتژی محتوایی

### کلمات کلیدی هدف

| دسته | نمونه کلمات |
|------|------------|
| **مسیرها** | حمل بار از چین به ایران، باربری ترکیه به ایران، حمل زمینی عراق |
| **خدمات** | شرکت حمل و نقل بین المللی، ترخیص کالا از گمرک، بارنامه بین المللی |
| **اطلاعاتی** | هزینه حمل بار از چین، مدارک لازم برای واردات از ترکیه |
| **Long-tail** | بهترین شرکت حمل بار از شانگهای به تهران با قیمت مناسب |

### ساختار محتوا

```
/ (صفحه اصلی)
├── /routes (لیست مسیرها)
│   ├── /routes/china-to-iran (صفحه مسیر + ۳۰۰۰ کلمه محتوا)
│   ├── /routes/iran-to-turkey
│   └── /routes/...
├── /guides (راهنماها)
│   ├── /guides/customs-clearance-iran
│   ├── /guides/cmrs-document
│   └── ...
├── /blog (مقالات)
│   ├── /blog/how-to-ship-from-china
│   └── ...
├── /faq (سوالات متداول)
└── /glossary (واژه‌نامه)
```

---

## 11.3 SEO On-Page برای هر صفحه مسیر

- **Title Tag:** حمل بار از [مبدا] به [مقصد] | قیمت + زمان + [نام برند]
- **Meta Description:** ۱۵۰-۱۶۰ کاراکتر با CTA
- **H1:** حمل بار از [مبدا] به [مقصد] — راهنمای کامل ۱۴۰۵
- **H2ها:** مدارک لازم، هزینه‌ها، زمان، مسیر، سوالات متداول
- **تصاویر:** نقشه مسیر، اینفوگرافیک، ALT بهینه
- **Internal Linking:** لینک به مسیرهای مرتبط، راهنماها
- **External Linking:** به منابع معتبر (گمرک، راهداری)

---

## 11.4 SEO Off-Page

- بک‌لینک از سایت‌های لجستیک، گمرکی، بازرگانی
- ثبت در دایرکتوری‌های حمل‌ونقل
- فعالیت در LinkedIn و Twitter با محتوای تخصصی
- همکاری با بلاگرهای حوزه بازرگانی

---

## جزئیات پیاده‌سازی

### Next.js Metadata API

```typescript
// app/routes/[slug]/page.tsx
export async function generateMetadata({ params }): Promise<Metadata> {
  const route = await getRoute(params.slug);
  return {
    title: `حمل بار از ${route.originCity} به ${route.destinationCity} | قیمت و زمان | SADRA`,
    description: `حمل بار زمینی از ${route.originCity} به ${route.destinationCity}. فاصله ${route.distanceKm} کیلومتر، زمان ${route.estimatedDays} روز. مشاهده نرخ لحظه‌ای.`,
    alternates: {
      canonical: `https://sadra.ir/routes/${route.seoSlug}`,
      languages: {
        'fa-IR': `/fa/routes/${route.seoSlug}`,
        'en-US': `/en/routes/${route.seoSlug}`,
      },
    },
    openGraph: {
      title: route.metaTitle,
      description: route.metaDescription,
      images: [`/og/routes/${route.seoSlug}.png`],
    },
  };
}
```

### JSON-LD Schema Markup

```typescript
// components/seo/RoutePageSchema.tsx
const routeSchema = {
  "@context": "https://schema.org",
  "@type": "Service",
  "name": `حمل بار از ${origin} به ${destination}`,
  "provider": {
    "@type": "Organization",
    "name": "SADRA",
    "url": "https://sadra.ir"
  },
  "areaServed": [originCountry, destinationCountry],
  "offers": rates.map(rate => ({
    "@type": "Offer",
    "price": rate.price,
    "priceCurrency": rate.currency,
    "seller": { "@type": "Organization", "name": rate.providerName }
  }))
};

const faqSchema = {
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": faqs.map(faq => ({
    "@type": "Question",
    "name": faq.question,
    "acceptedAnswer": { "@type": "Answer", "text": faq.answer }
  }))
};

const breadcrumbSchema = {
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "خانه", "item": "https://sadra.ir" },
    { "@type": "ListItem", "position": 2, "name": "مسیرها", "item": "https://sadra.ir/routes" },
    { "@type": "ListItem", "position": 3, "name": title }
  ]
};
```

### Dynamic Sitemap

```typescript
// app/sitemap.ts
export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const routes = await getAllRoutes();
  const blogPosts = await getAllBlogPosts();
  const guides = await getAllGuides();

  return [
    { url: 'https://sadra.ir', lastModified: new Date(), priority: 1 },
    { url: 'https://sadra.ir/routes', lastModified: new Date(), priority: 0.9 },
    ...routes.map(r => ({
      url: `https://sadra.ir/routes/${r.seoSlug}`,
      lastModified: r.updatedAt,
      priority: 0.8,
      changeFrequency: 'weekly' as const,
    })),
    ...blogPosts.map(p => ({
      url: `https://sadra.ir/blog/${p.slug}`,
      lastModified: p.publishedAt,
      priority: 0.6,
    })),
  ];
}
```

### robots.txt

```
User-agent: *
Allow: /
Disallow: /dashboard/
Disallow: /provider/
Disallow: /admin/
Disallow: /api/

Sitemap: https://sadra.ir/sitemap.xml
```

### Route Page Content Template

هر صفحه مسیر باید شامل این بخش‌ها باشد (حداقل ۲۰۰۰ کلمه):

```markdown
# H1: حمل بار از [مبدا] به [مقصد]

## مقدمه (۲۰۰ کلمه)
## نقشه و مسیر (با تصویر)
## نرخ‌های فعلی (ویجت زنده)
## H2: مدارک لازم
## H2: مراحل گمرکی
## H2: مرزهای اصلی
## H2: هزینه‌ها و عوامل مؤثر
## H2: زمان تحویل
## H2: نکات مهم
## H2: سوالات متداول (FAQ Schema)
## H2: مسیرهای مرتبط (Internal Links)
```

### Performance Optimization for SEO

```typescript
// next.config.js
module.exports = {
  images: {
    formats: ['image/avif', 'image/webp'],
    deviceSizes: [640, 750, 828, 1080, 1200],
  },
  experimental: {
    optimizeCss: true,
  },
  // ISR for route pages — revalidate every hour
};

// app/routes/[slug]/page.tsx
export const revalidate = 3600; // 1 hour
```

### Hreflang Implementation

```html
<link rel="alternate" hreflang="fa" href="https://sadra.ir/fa/routes/china-to-iran" />
<link rel="alternate" hreflang="en" href="https://sadra.ir/en/routes/china-to-iran" />
<link rel="alternate" hreflang="ar" href="https://sadra.ir/ar/routes/china-to-iran" />
<link rel="alternate" hreflang="x-default" href="https://sadra.ir/routes/china-to-iran" />
```

### SEO Content Calendar (ماه ۵-۶)

| هفته | محتوا | تعداد |
|------|-------|-------|
| ۱ | صفحات مسیر اصلی (چین، ترکیه، عراق، امارات) | ۴ |
| ۲ | صفحات مسیر ثانویه (ازبکستان، ترکمنستان، آذربایجان) | ۵ |
| ۳ | راهنماهای گمرکی + FAQ | ۵ |
| ۴ | بلاگ (مقالات تخصصی) | ۳ |
| ۵ | Case Studies + Glossary | ۳ |
| ۶ | Landing Pages کمپین | ۲ |

**هدف:** حداقل ۲۰ صفحه با ۲۰۰۰+ کلمه تا لانچ

### Analytics & Tracking

```typescript
// Google Analytics 4 events
gtag('event', 'rate_search', {
  origin: 'تهران',
  destination: 'شانگهای',
  weight: 20,
});

gtag('event', 'request_created', {
  route_slug: 'china-to-iran',
  provider_id: 'xxx',
});
```

### SEO Checklist per Page

- [ ] Title tag منحصربه‌فرد (۵۰-۶۰ کاراکتر)
- [ ] Meta description با CTA (۱۵۰-۱۶۰ کاراکتر)
- [ ] H1 یکتا
- [ ] H2-H3 ساختاریافته
- [ ] تصاویر با ALT
- [ ] Internal links (حداقل ۳)
- [ ] Schema markup (Service + FAQ + Breadcrumb)
- [ ] Canonical URL
- [ ] Mobile-friendly
- [ ] Page speed < 3s
- [ ] HTTPS

# SEO Requirements Checklist for Sage Project

This document outlines all necessary SEO implementations across your Frontend, Backend, and Admin Panel.

---

## 🎯 **FRONTEND (redox-next-js-light) - SEO Requirements**

### ✅ **1. Meta Tags & Metadata (CRITICAL)**

#### Current Status:
- ✅ Basic metadata exists in `layout.tsx` (title & description)
- ❌ Missing: Open Graph tags, Twitter Cards, viewport optimization
- ❌ Missing: Dynamic metadata for individual pages
- ❌ Missing: Canonical URLs

#### Required Implementation:

**A. Root Layout (`src/app/layout.tsx`):**
```typescript
export const metadata: Metadata = {
  title: {
    default: "Sage Craft - Creative Agency and Portfolio",
    template: "%s | Sage Craft"
  },
  description: "Your comprehensive description here",
  keywords: ["creative agency", "portfolio", "web design", "..."],
  authors: [{ name: "Sage Craft" }],
  creator: "Sage Craft",
  publisher: "Sage Craft",
  metadataBase: new URL('https://yourdomain.com'),
  alternates: {
    canonical: '/',
  },
  openGraph: {
    type: 'website',
    locale: 'en_US',
    url: 'https://yourdomain.com',
    siteName: 'Sage Craft',
    title: 'Sage Craft - Creative Agency and Portfolio',
    description: 'Your description',
    images: [
      {
        url: '/assets/imgs/logo/logo.png',
        width: 1200,
        height: 630,
        alt: 'Sage Craft Logo',
      },
    ],
  },
  twitter: {
    card: 'summary_large_image',
    title: 'Sage Craft - Creative Agency',
    description: 'Your description',
    images: ['/assets/imgs/logo/logo.png'],
  },
  robots: {
    index: true,
    follow: true,
    googleBot: {
      index: true,
      follow: true,
      'max-video-preview': -1,
      'max-image-preview': 'large',
      'max-snippet': -1,
    },
  },
  verification: {
    google: 'your-google-verification-code',
    // yandex: 'your-yandex-verification-code',
    // bing: 'your-bing-verification-code',
  },
}
```

**B. Page-Level Metadata:**
- Each page needs unique metadata
- Client components need to be converted to server components OR use `generateMetadata` function
- Dynamic routes need dynamic metadata based on content

---

### ✅ **2. robots.txt (CRITICAL)**

#### Current Status: ❌ Missing

#### Required Implementation:
Create: `public/robots.txt`
```
User-agent: *
Allow: /
Disallow: /api/
Disallow: /admin/
Disallow: /_next/
Disallow: /dashboard/

Sitemap: https://yourdomain.com/sitemap.xml
```

---

### ✅ **3. sitemap.xml (CRITICAL)**

#### Current Status: ❌ Missing

#### Required Implementation:
Create: `src/app/sitemap.ts` (Next.js 13+ App Router)
```typescript
import { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  const baseUrl = 'https://yourdomain.com'
  
  // Static routes
  const staticRoutes = [
    '',
    '/about',
    '/contact',
    '/portfolio',
    '/services',
    // Add all your static routes
  ].map((route) => ({
    url: `${baseUrl}${route}`,
    lastModified: new Date(),
    changeFrequency: 'weekly' as const,
    priority: route === '' ? 1 : 0.8,
  }))

  // Dynamic routes (fetch from API)
  // You'll need to fetch portfolio items, services, etc. from your backend
  
  return [...staticRoutes]
}
```

---

### ✅ **4. Structured Data (Schema.org) (IMPORTANT)**

#### Current Status: ❌ Missing

#### Required Implementation:

**A. Organization Schema** - Add to root layout or homepage:
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Sage Craft",
  "url": "https://yourdomain.com",
  "logo": "https://yourdomain.com/assets/imgs/logo/logo.png",
  "contactPoint": {
    "@type": "ContactPoint",
    "contactType": "Customer Service"
  },
  "sameAs": [
    "https://facebook.com/yourpage",
    "https://twitter.com/yourhandle",
    "https://linkedin.com/company/yourcompany"
  ]
}
```

**B. Website Schema:**
```json
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "Sage Craft",
  "url": "https://yourdomain.com"
}
```

**C. Service Schema** - For service pages:
```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "serviceType": "Web Design",
  "provider": {
    "@type": "Organization",
    "name": "Sage Craft"
  }
}
```

**D. Portfolio/Project Schema** - For portfolio items:
```json
{
  "@context": "https://schema.org",
  "@type": "CreativeWork",
  "name": "Project Name",
  "description": "Project description"
}
```

---

### ✅ **5. Image Optimization & Alt Tags (CRITICAL)**

#### Current Status: ⚠️ Needs Verification

#### Required Implementation:
- All images must have descriptive `alt` attributes
- Use Next.js `Image` component for optimization
- Implement lazy loading
- Use proper image formats (WebP, AVIF)
- Add image dimensions
- Optimize image file sizes

---

### ✅ **6. URL Structure & Canonical URLs (IMPORTANT)**

#### Current Status: ⚠️ Needs Implementation

#### Required Implementation:
- Clean, descriptive URLs (✅ Already good)
- Canonical URLs on every page to prevent duplicate content
- Implement in metadata:
```typescript
alternates: {
  canonical: '/current-page-url',
}
```

---

### ✅ **7. Performance Optimization (CRITICAL for SEO)**

#### Current Status: ⚠️ Needs Optimization

#### Required Implementation:
- **Core Web Vitals:**
  - LCP (Largest Contentful Paint) < 2.5s
  - FID (First Input Delay) < 100ms
  - CLS (Cumulative Layout Shift) < 0.1

- **Next.js Optimizations:**
  - Enable Image Optimization
  - Implement code splitting
  - Use dynamic imports for heavy components
  - Optimize fonts (✅ Already using next/font)
  - Enable compression (gzip/brotli)

- **Add to `next.config.ts`:**
```typescript
const nextConfig: NextConfig = {
  compress: true,
  poweredByHeader: false,
  // ... existing config
}
```

---

### ✅ **8. Mobile Responsiveness (CRITICAL)**

#### Current Status: ⚠️ Needs Verification

#### Required Implementation:
- Ensure all pages are mobile-friendly
- Test with Google Mobile-Friendly Test
- Proper viewport meta tag (Next.js handles this automatically)

---

### ✅ **9. Semantic HTML (IMPORTANT)**

#### Current Status: ⚠️ Needs Review

#### Required Implementation:
- Use proper HTML5 semantic elements (`<header>`, `<main>`, `<article>`, `<section>`, `<footer>`)
- Proper heading hierarchy (H1 → H2 → H3)
- Only ONE H1 per page
- Use `<nav>` for navigation

---

### ✅ **10. Internal Linking (IMPORTANT)**

#### Current Status: ⚠️ Needs Implementation

#### Required Implementation:
- Strategic internal linking between related pages
- Breadcrumb navigation
- Related content sections
- Sitemap in footer

---

### ✅ **11. Content Quality (CRITICAL)**

#### Current Status: ⚠️ Needs Review

#### Required Implementation:
- Unique, high-quality content on every page
- Keyword optimization (natural, not keyword stuffing)
- Regular content updates
- Blog/content section for fresh content

---

### ✅ **12. HTTPS & Security (CRITICAL)**

#### Current Status: ⚠️ Needs Verification

#### Required Implementation:
- SSL certificate (HTTPS)
- Security headers
- Add to `next.config.ts`:
```typescript
async headers() {
  return [
    {
      source: '/(.*)',
      headers: [
        {
          key: 'X-Content-Type-Options',
          value: 'nosniff',
        },
        {
          key: 'X-Frame-Options',
          value: 'DENY',
        },
        {
          key: 'X-XSS-Protection',
          value: '1; mode=block',
        },
      ],
    },
  ]
}
```

---

### ✅ **13. Analytics & Search Console (IMPORTANT)**

#### Current Status: ❌ Missing

#### Required Implementation:
- Google Analytics 4 (GA4)
- Google Search Console setup
- Bing Webmaster Tools (optional)
- Track page views, user behavior

---

### ✅ **14. 404 & Error Pages (IMPORTANT)**

#### Current Status: ✅ Has `not-found.tsx`

#### Required Implementation:
- Custom 404 page with helpful navigation
- Proper error handling
- Redirect old URLs to new ones

---

### ✅ **15. Language & Internationalization (if needed)**

#### Current Status: ⚠️ Set to "en" only

#### Required Implementation:
- If multi-language: Implement `hreflang` tags
- Proper language declarations

---

## 🔧 **BACKEND - SEO Support Requirements**

### ✅ **1. API Endpoints for SEO Data**

#### Required Implementation:
- Endpoint to fetch all pages for sitemap generation
- Endpoint to fetch metadata for dynamic pages
- Structured data endpoints
- SEO-friendly API responses

**Example:**
```javascript
// routes/seoRoute.js
router.get('/sitemap-data', async (req, res) => {
  try {
    const pages = await getAllPages(); // Fetch all public pages
    res.json(pages);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

---

### ✅ **2. SEO-Friendly URLs**

#### Current Status: ✅ API routes are fine

#### Required Implementation:
- Keep API routes clean and logical
- Support for slug-based routing
- Redirect old URLs to new ones

---

### ✅ **3. Content Management for SEO**

#### Required Implementation:
- Admin panel should allow editing:
  - Page titles
  - Meta descriptions
  - Meta keywords
  - Open Graph images
  - Alt text for images
  - Canonical URLs
  - Structured data

---

### ✅ **4. Performance Optimization**

#### Required Implementation:
- API response caching
- Database query optimization
- CDN for static assets
- Compression middleware

---

## 🛠️ **ADMIN PANEL - SEO Management Features**

### ✅ **1. SEO Content Management (IMPORTANT)**

#### Required Implementation:
- **Page SEO Settings:**
  - Title field
  - Meta description field
  - Meta keywords field
  - Open Graph title
  - Open Graph description
  - Open Graph image upload
  - Canonical URL field
  - Robots meta (index/noindex, follow/nofollow)

- **Image SEO:**
  - Alt text field for all images
  - Image title field
  - Image optimization options

- **Content SEO:**
  - Keyword suggestions
  - Content readability score
  - SEO score/preview

---

### ✅ **2. Sitemap Management**

#### Required Implementation:
- Manual sitemap regeneration
- View current sitemap
- Exclude/include pages from sitemap

---

### ✅ **3. Analytics Integration**

#### Required Implementation:
- Google Analytics integration
- Search Console integration
- View SEO metrics in dashboard

---

## 📋 **PRIORITY IMPLEMENTATION ORDER**

### **Phase 1 - Critical (Do First):**
1. ✅ robots.txt
2. ✅ sitemap.xml/sitemap.ts
3. ✅ Enhanced metadata with Open Graph & Twitter Cards
4. ✅ Image alt tags
5. ✅ Canonical URLs
6. ✅ HTTPS & Security headers

### **Phase 2 - Important (Do Next):**
7. ✅ Structured Data (Schema.org)
8. ✅ Performance optimization
9. ✅ Mobile responsiveness check
10. ✅ Internal linking strategy
11. ✅ Analytics setup

### **Phase 3 - Enhancement:**
12. ✅ SEO management in admin panel
13. ✅ Content quality improvements
14. ✅ Advanced structured data
15. ✅ Internationalization (if needed)

---

## 🧪 **Testing & Validation Tools**

### Required Tools:
1. **Google Search Console** - Submit sitemap, monitor indexing
2. **Google PageSpeed Insights** - Performance testing
3. **Google Mobile-Friendly Test** - Mobile optimization
4. **Schema Markup Validator** - Test structured data
5. **Open Graph Debugger** - Test social media previews
6. **Screaming Frog SEO Spider** - Technical SEO audit
7. **Ahrefs/SEMrush** - Keyword research & competitor analysis

---

## 📝 **Notes**

- **Admin Panel**: Doesn't need SEO itself (it's internal), but should have SEO management features
- **Backend**: Doesn't need SEO directly, but should support SEO-friendly data delivery
- **Frontend**: This is where 90% of SEO work happens

---

## 🚀 **Quick Start Checklist**

- [ ] Create `robots.txt`
- [ ] Create `sitemap.ts`
- [ ] Enhance root layout metadata
- [ ] Add Open Graph tags to all pages
- [ ] Add alt tags to all images
- [ ] Implement canonical URLs
- [ ] Add structured data
- [ ] Set up Google Search Console
- [ ] Set up Google Analytics
- [ ] Test with SEO tools
- [ ] Optimize performance
- [ ] Add SEO management to admin panel

---

**Last Updated:** 2024
**Project:** Sage - Creative Agency Platform


# Technical SEO Audit Checklist

Reference guide for evaluating website SEO health and optimization opportunities.

---

## Overview

Technical SEO ensures search engines can effectively crawl, index, and rank your website. This checklist complements the **Section 5 (AI Friendliness & SEO)** of the web-expert-review audit.

---

## 1. Crawlability & Indexing

### 1.1 Robots.txt Configuration

**What to Check:**
- Does `robots.txt` exist at the root domain?
- Are critical pages allowed for crawling?
- Are unnecessary pages blocked (e.g., admin, staging, thank-you pages)?

**How to Test:**
```
Visit: https://[domain]/robots.txt
Look for:
- User-agent: * (applies to all bots)
- Allow: / (allows indexing)
- Disallow: /admin, /staging (blocks crawling)
- Sitemap: https://[domain]/sitemap.xml
```

**Common Issues:**
- ❌ Entire site blocked (`Disallow: /`)
- ❌ Important pages blocked by mistake (e.g., `/products`, `/services`)
- ❌ No sitemap reference in robots.txt
- ❌ Overly restrictive crawl delays (too slow)

**Fix Priority:** 🔴 High — If robots.txt blocks search engines, site won't be indexed

**Indonesian Market Note:**
- Indonesian sites often block `/admin` and `/staging` correctly
- Common mistake: Blocking all `/id/` pages thinking they're duplicates

---

### 1.2 XML Sitemap

**What to Check:**
- Does `sitemap.xml` exist and is valid?
- Does it include all important pages (homepage, products, blog posts)?
- Are URLs in sitemap actually indexable?
- Is sitemap submitted to Google Search Console?

**How to Test:**
```
Visit: https://[domain]/sitemap.xml

Check for:
- Valid XML format (no errors)
- <loc> elements with full URLs
- <lastmod> dates accurate
- <changefreq> and <priority> tags present
- Not exceeding 50,000 URLs per sitemap

For large sites, check:
- sitemap_index.xml (splits sitemaps)
- Separate sitemaps for images, video, news
```

**Common Issues:**
- ❌ Sitemap doesn't exist
- ❌ Sitemap has broken/invalid URLs
- ❌ Sitemap includes pages with `noindex` (conflicts)
- ❌ Sitemap not submitted to GSC
- ❌ Last-modified date frozen (suggests it's not updated)
- ❌ Sitemap includes pagination pages (should only include canonical URLs)

**Fix Priority:** 🟡 Medium-High — Helps Google discover all pages

**Indonesian Market Note:**
- Bilingual sites: Create separate sitemaps for `/id/` and `/en/` sections, or use `hreflang` annotations
- E-commerce sites: Include image sitemap for product photos

---

### 1.3 Meta Robots Tag

**What to Check:**
- Is there a `<meta name="robots" content="">` tag in each page's `<head>`?
- Does it allow or disallow indexing?
- Are JavaScript-rendered pages marked correctly?

**How to Test:**
```html
<!-- GOOD: Allows indexing and following links -->
<meta name="robots" content="index, follow">

<!-- BAD: Blocks indexing -->
<meta name="robots" content="noindex, nofollow">

<!-- Staging/test pages should use: -->
<meta name="robots" content="noindex, follow">
```

**Common Issues:**
- ❌ Test/staging pages have no robots tag (get indexed by accident)
- ❌ Production pages have `noindex` (don't rank)
- ❌ Conflicting tags: `robots.txt` allows crawling but page has `noindex`
- ❌ JavaScript-rendered pages not marked correctly

**Fix Priority:** 🔴 High — If set incorrectly, pages won't rank

---

### 1.4 Canonical Tags

**What to Check:**
- Do duplicate pages have a `<link rel="canonical">` tag?
- Do canonical tags point to the *correct* page?
- Are canonicals self-referential on unique pages?

**How to Test:**
```html
<!-- Homepage (unique) -->
<link rel="canonical" href="https://example.com/">

<!-- Product page (unique) -->
<link rel="canonical" href="https://example.com/products/item-123">

<!-- Pagination (canonical to first page) -->
<!-- Page 2: /products?page=2 -->
<link rel="canonical" href="https://example.com/products">

<!-- URL parameters (canonical to clean URL) -->
<!-- Page: /products?utm_source=email&utm_medium=promo -->
<link rel="canonical" href="https://example.com/products">
```

**Common Issues:**
- ❌ Pagination pages not canonicalized (Google crawls all page 2, 3, 4... separately)
- ❌ Canonical points to wrong page
- ❌ Missing canonicals on URL parameter pages (e.g., `?utm_` tracking links)
- ❌ Canonical chains (Page A → Page B → Page C; should be direct)
- ❌ HTTPS and HTTP versions not canonicalized

**Fix Priority:** 🟡 Medium — Prevents indexing confusion and duplicate content penalties

**Indonesian Market Note:**
- Bilingual sites: Use `hreflang` instead of `rel="canonical"` (see Section 1.9)
- E-commerce: Canonicalize `/products?filter=color-red` to `/products` (filter parameters cause duplicates)

---

### 1.5 HTTP Status Codes

**What to Check:**
- Do important pages return 200 (OK)?
- Are 404 pages returning proper 404 status (not 200)?
- Are redirects (301/302) working correctly?
- Are client errors (4xx) and server errors (5xx) minimal?

**How to Test:**
```
Use tools: Screaming Frog, Ahrefs, or check browser console (Network tab)

Expected:
- Homepage: 200
- Product pages: 200
- About, Contact: 200
- Non-existent page: 404
- Moved page: 301 (permanent redirect)
- Temporary page: 302 (temporary redirect)
```

**Common Issues:**
- ❌ 404 pages returning 200 (soft 404; Google can't tell page is gone)
- ❌ Broken redirects (A → B → C → 404)
- ❌ Too many 301 redirects on homepage (slows down crawl)
- ❌ 5xx errors (server errors prevent indexing)
- ❌ Mixed HTTP/HTTPS (one returns 200, other 404)

**Fix Priority:** 🔴 High — Wrong status codes prevent proper indexing

---

### 1.6 SSL/HTTPS Certificate

**What to Check:**
- Is the entire site served over HTTPS (not just homepage)?
- Is the SSL certificate valid and not expired?
- Are there mixed HTTP/HTTPS resources (mixed content)?

**How to Test:**
```
1. Check domain: https://[domain]
   - Does it load without warnings?
   - Certificate shows domain name?

2. Check all pages:
   - https://[domain]/about
   - https://[domain]/products
   - Do they all load over HTTPS?

3. Check for mixed content:
   - Open developer tools → Console
   - Look for warnings: "Mixed Content: The page was loaded over HTTPS..."
```

**Common Issues:**
- ❌ Some pages load over HTTP (not secure)
- ❌ Mixed content warning (HTTPS page loads HTTP images/scripts)
- ❌ SSL certificate for different domain
- ❌ Expired certificate

**Fix Priority:** 🔴 Critical — Google favors HTTPS; mixed content hurts rankings

---

## 2. Site Structure & Architecture

### 2.1 URL Structure

**What to Check:**
- Are URLs clean, descriptive, and keyword-relevant?
- Do URLs follow a logical hierarchy?
- Are parameters minimal (or canonicalized)?

**Good URL Structure:**
```
https://example.com/products/laptops/gaming-laptop-model-x
                     └─ category ─ subcategory ─ product name

vs. BAD:
https://example.com/product.php?id=12345&color=red&utm_source=email
```

**Common Issues:**
- ❌ Unclear URLs: `/p1`, `/item`, `/content?id=xyz`
- ❌ Over-parameterized: `?color=red&size=large&brand=abc&sort=price` (duplicates)
- ❌ Deep nesting: `/category/subcategory/sub-subcategory/product` (crawl depth issue)
- ❌ Inconsistent: Some pages `/products`, others `/product` (case sensitivity)

**Fix Priority:** 🟡 Medium — Better URLs improve crawlability and CTR from SERPs

---

### 2.2 Internal Linking Structure

**What to Check:**
- Are important pages linked from homepage?
- Is link anchor text descriptive (not "Click here")?
- Are there broken internal links?
- Is link hierarchy logical (no orphaned pages)?

**How to Test:**
```
Count links to key pages:
- Homepage: Should be 1 (referenced from all pages via logo)
- Product category: 3–5 links (from homepage, footer, nav)
- Individual product: 2–3 links (from category, related products)
- Blog post: 1–2 links (from blog index, related posts)

Check anchor text:
✅ Good: "Read our guide to SEO best practices"
❌ Bad: "Click here" or "Read more"
```

**Common Issues:**
- ❌ Important pages buried deep (no direct links from homepage)
- ❌ Orphaned pages (no internal links pointing to them)
- ❌ Generic anchor text (all "Click here")
- ❌ Broken internal links (404s)
- ❌ Keyword stuffing in anchor text

**Fix Priority:** 🟡 Medium — Internal links distribute authority and guide crawlers

---

### 2.3 Breadcrumb Navigation

**What to Check:**
- Do category/product pages display breadcrumb navigation?
- Is breadcrumb structured with schema markup?

**How to Test:**
```html
<!-- GOOD: Breadcrumb with schema -->
<nav aria-label="Breadcrumb">
  <ol itemscope itemtype="https://schema.org/BreadcrumbList">
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <a itemprop="item" href="https://example.com/">Home</a>
      <meta itemprop="position" content="1" />
    </li>
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <a itemprop="item" href="https://example.com/products/">Products</a>
      <meta itemprop="position" content="2" />
    </li>
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <span itemprop="name">Gaming Laptop</span>
      <meta itemprop="position" content="3" />
    </li>
  </ol>
</nav>
```

**Common Issues:**
- ❌ No breadcrumb navigation
- ❌ Breadcrumb present but no schema markup
- ❌ Broken breadcrumb links

**Fix Priority:** 🟢 Low-Medium — Nice-to-have; helps UX and SEO slightly

---

## 3. Content & On-Page SEO

### 3.1 Page Titles & Meta Descriptions

**What to Check:**
- Is each page's `<title>` unique, descriptive, and includes target keyword?
- Is meta description compelling and under 160 characters?
- Are duplicates or missing titles?

**Good Examples:**
```html
<!-- Product Page -->
<title>Gaming Laptop 15" RTX 4090 | High Performance | Brand Name</title>
<meta name="description" content="Powerful gaming laptop with RTX 4090, 16GB RAM, 1TB SSD. Perfect for AAA gaming and 3D rendering. Free shipping on orders over $500.">

<!-- Blog Post -->
<title>10 SEO Best Practices for E-Commerce Sites in 2024 | Blog</title>
<meta name="description" content="Learn proven SEO strategies for online stores. Increase organic traffic, rankings, and conversions with these actionable tips.">
```

**Common Issues:**
- ❌ Duplicate titles across pages
- ❌ Too long (>70 characters) or too short (<30 characters)
- ❌ Keyword-stuffed title
- ❌ Missing meta description
- ❌ Meta description too long (gets cut off in search results)
- ❌ Meta description with no call-to-action

**Fix Priority:** 🔴 High — Titles & descriptions directly affect CTR from search results

**Indonesian Market Note:**
- Bilingual sites: Ensure each language version has its own unique title and meta (not auto-translated)
- Common mistake: Using English keywords on Indonesian pages (target `laptop gaming`, not just "gaming laptop" in English)

---

### 3.2 Heading Tags (H1, H2, H3)

**What to Check:**
- Does each page have exactly one H1?
- Are headings hierarchical (H1 → H2 → H3)?
- Are headings descriptive and include target keywords?

**How to Test:**
```html
<!-- GOOD heading structure -->
<h1>Gaming Laptop with RTX 4090</h1>
  <h2>Specifications</h2>
    <h3>Processor</h3>
    <h3>GPU</h3>
  <h2>Performance Comparison</h2>
    <h3>Gaming Benchmarks</h3>
    <h3>3D Rendering Speed</h3>

<!-- BAD heading structure -->
<h1>Welcome</h1>
<h3>Gaming Laptop</h3>
<h2>Why Choose Us?</h2>
<h4>Specs</h4> <!-- Jumps from H2 to H4 -->
```

**Common Issues:**
- ❌ Multiple H1 tags on one page
- ❌ No H1 tag
- ❌ Non-hierarchical heading order (H1 → H3 → H2)
- ❌ Headings with no keyword relevance
- ❌ Heading text identical to previous page

**Fix Priority:** 🟡 Medium — Proper heading structure helps SEO and accessibility

---

### 3.3 Image Optimization

**What to Check:**
- Do all images have descriptive `alt` text?
- Are images in modern formats (WebP)?
- Is image file size optimized?
- Are images lazy-loaded?

**How to Test:**
```html
<!-- GOOD image optimization -->
<img 
  src="gaming-laptop-rtx-4090-side-view.webp" 
  alt="Side view of gaming laptop with RTX 4090, 15-inch display, aluminum chassis"
  loading="lazy"
  width="800"
  height="600"
>

<!-- BAD -->
<img src="image1.jpg" alt="laptop"> <!-- No detail; generic filename -->
<img src="123456.png" alt=""> <!-- Missing alt text -->
<img src="gaming-laptop.png"> <!-- Large PNG instead of optimized WebP -->
```

**Common Issues:**
- ❌ Missing alt text
- ❌ Alt text is generic ("image", "photo")
- ❌ Alt text keyword-stuffed ("gaming laptop gaming laptop gaming laptop")
- ❌ Large image files (no compression or optimization)
- ❌ All images in JPG/PNG (not WebP)
- ❌ No lazy loading on images below the fold

**Fix Priority:** 🟡 Medium-High — Images are 40–60% of page weight; impacts performance

---

### 3.4 Content Quality & Length

**What to Check:**
- Is content original and valuable (not plagiarized)?
- Is content length adequate for topic depth?
- Is content updated regularly (freshness)?
- Are there topic clusters (related content linking)?

**Benchmarks by Page Type:**

| Page Type | Recommended Length | Purpose |
|---|---|---|
| **Product page** | 500–800 words | Describe features, benefits, specs, FAQs |
| **Category page** | 300–500 words | Introduce category, link to products |
| **Blog post** | 1500–3000 words | In-depth guide, build topical authority |
| **Homepage** | 300–500 words | Company story, unique value prop |
| **Service page** | 800–1200 words | Explain service, process, case studies |

**Common Issues:**
- ❌ Thin content (< 300 words on product pages)
- ❌ Duplicate content across pages
- ❌ Copied content from competitors
- ❌ No content updates (oldest post from 5 years ago)
- ❌ No internal links between related posts (topic clusters)

**Fix Priority:** 🔴 High — Content quality is Google's #1 ranking factor

---

### 3.5 Schema Markup (Structured Data)

**What to Check:**
- Is schema markup present for key content types?
- Are schema types correct and complete?
- Do rich snippets appear in search results?

**Common Schema Types to Audit:**

| Schema Type | Used For | Priority |
|---|---|---|
| **Organization** | Company info, logo, contact | 🔴 High |
| **Product** | Product name, price, rating, image | 🔴 High |
| **Review/Rating** | Customer reviews, star ratings | 🔴 High |
| **LocalBusiness** | Address, phone, hours (for local services) | 🟡 Medium |
| **FAQ** | Question/answer pairs on page | 🟡 Medium |
| **BreadcrumbList** | Navigation hierarchy | 🟢 Low |
| **Event** | Upcoming events (for event sites) | 🔴 High (if applicable) |

**How to Test:**
```
1. Use Google's Rich Results Test: https://search.google.com/test/rich-results
2. Paste URL
3. Check for warnings/errors
4. Verify correct schema types are detected
```

**Common Issues:**
- ❌ No schema markup at all
- ❌ Incomplete schema (missing fields like `price`, `image`, `author`)
- ❌ Incorrect schema type (using `Article` for product page)
- ❌ Schema in `<script type="text/html">` (wrong; should be `application/ld+json`)
- ❌ Orphaned schema (doesn't match visible page content)

**Fix Priority:** 🔴 High — Schema enables rich snippets, improves CTR, helps AI discovery

**Indonesian Market Note:**
- Add `inLanguage: "id"` to schema for Indonesian pages
- For fintech/e-commerce: Always include Product/Review schema with price + rating

---

## 4. Performance & Technical Health

### 4.1 Page Speed & Core Web Vitals

**What to Check:**
- Largest Contentful Paint (LCP): < 2.5 seconds
- First Input Delay (FID): < 100 milliseconds
- Cumulative Layout Shift (CLS): < 0.1

**How to Test:**
```
1. Google PageSpeed Insights: https://pagespeed.web.dev/
2. Lighthouse (Chrome DevTools)
3. WebPageTest: https://www.webpagetest.org/
```

**Common Issues:**
- ❌ LCP > 3 seconds (slow image loading)
- ❌ FID > 200ms (heavy JavaScript)
- ❌ CLS > 0.2 (images/ads jumping around)

**Fix Priority:** 🔴 High — Google uses Core Web Vitals as ranking signal

---

### 4.2 Mobile Responsiveness

**What to Check:**
- Does site display correctly on mobile (320px to 768px)?
- Are touch targets at least 48x48 pixels?
- Is text readable without zooming?
- Are images responsive (don't overflow)?

**How to Test:**
```
1. Chrome DevTools → Toggle Device Toolbar (Ctrl+Shift+M)
2. Test on multiple viewport sizes
3. Check: iPhone SE (375px), iPhone 12 (390px), iPad (768px)
```

**Common Issues:**
- ❌ Text too small (< 16px) on mobile
- ❌ Touch buttons too small (< 44px)
- ❌ Horizontal scrolling required
- ❌ Images overflow on mobile
- ❌ Desktop layout stacks poorly on mobile

**Fix Priority:** 🔴 Critical — Google is mobile-first for indexing

---

## 5. Security & Trust Signals

### 5.1 SSL Certificate & HTTPS

*See Section 1.6*

---

### 5.2 Security Headers

**What to Check:**
- Is `X-Content-Type-Options: nosniff` set?
- Is `X-Frame-Options: SAMEORIGIN` set?
- Is `Strict-Transport-Security` (HSTS) configured?
- Is `Content-Security-Policy` (CSP) present?

**How to Test:**
```
1. Use online tool: https://securityheaders.com/
2. Enter domain
3. Review security headers score (aim for A+)
```

**Common Issues:**
- ❌ Missing security headers
- ❌ HSTS not configured
- ❌ CSP too permissive

**Fix Priority:** 🟡 Medium — Impacts trust signals and security

---

## 6. International & Multi-Language SEO

### 6.1 Hreflang Tags

**What to Check:**
- Do bilingual/multi-language pages have `hreflang` tags?
- Are hreflang tags reciprocal (bidirectional)?
- Do they point to correct language/region versions?

**How to Test:**
```html
<!-- Indonesian homepage -->
<link rel="alternate" hreflang="id" href="https://example.com/id/">
<link rel="alternate" hreflang="en" href="https://example.com/en/">
<link rel="alternate" hreflang="x-default" href="https://example.com/">

<!-- Indonesian product page -->
<link rel="alternate" hreflang="id" href="https://example.com/id/products/laptop/">
<link rel="alternate" hreflang="en" href="https://example.com/en/products/laptop/">
<link rel="alternate" hreflang="x-default" href="https://example.com/products/laptop/">
```

**Common Issues:**
- ❌ No hreflang tags (Google thinks content is duplicated)
- ❌ Hreflang points to wrong language
- ❌ Missing `x-default` hreflang
- ❌ Hreflang not reciprocal (ID → EN, but EN doesn't point back to ID)

**Fix Priority:** 🔴 High — Critical for bilingual sites to avoid duplicate content penalties

**Indonesian Market Note:**
- Always include `hreflang="id"` for Indonesian pages
- For regional variants: Use `hreflang="id-ID"` if targeting Indonesia specifically
- Implement in both HTML and sitemap XML

---

### 6.2 Language Meta Tags

**What to Check:**
- Is page language declared in `<html lang="">`?
- Is language consistent with page content?

**How to Test:**
```html
<!-- Indonesian page -->
<html lang="id">
  <head>
    <meta http-equiv="content-language" content="id">
  </head>
</html>

<!-- Bilingual page (specify primary) -->
<html lang="id"> <!-- Primary language -->
```

**Common Issues:**
- ❌ No `lang` attribute
- ❌ Wrong language code (e.g., `en-US` for Indonesian)
- ❌ Inconsistent language across page

**Fix Priority:** 🟡 Medium — Helps Google understand page language

---

## 7. Competitive SEO Analysis

### 7.1 Backlink Profile

**What to Check:**
- How many referring domains does client have vs. competitors?
- Are backlinks from high-authority sites (domain rating 50+)?
- Are there spammy/low-quality backlinks?

**How to Test:**
```
Tools: Ahrefs, SEMrush, Moz, Backlinko

For client + 3 competitors:
- Total referring domains (aim for 50+)
- Top referring domains (quality > quantity)
- Backlink anchor text (should be natural, not all keyword-heavy)
```

**Common Issues:**
- ❌ Few backlinks (< 20) vs. competitors (100+)
- ❌ Low-quality backlinks (spam forums, PBNs)
- ❌ All anchor text identical (over-optimized)

**Fix Priority:** 🟡 Medium — Link building is longer-term effort

---

### 7.2 Keyword Rankings

**What to Check:**
- Which keywords is client ranking for?
- Which keywords are competitors ranking for (but client isn't)?
- What's the ranking position for target keywords?

**Target Keyword Gaps (Opportunities):**

| Keyword | Client | Competitor A | Competitor B |
|---|---|---|---|
| "gaming laptop" | #15 | #3 | #5 |
| "RTX 4090 laptop" | Not ranking | #1 | #4 |
| "laptop for video editing" | Not ranking | #7 | #12 |

**Fix Priority:** 🟡 Medium — Identifies content opportunities

---

## Audit Template

Use this checklist to evaluate sites:

```markdown
## Technical SEO Audit Summary

### Crawlability & Indexing
- [ ] Robots.txt allows crawling
- [ ] XML sitemap exists and valid
- [ ] Meta robots tags correct
- [ ] Canonical tags present (where needed)
- [ ] HTTP status codes correct
- [ ] HTTPS on all pages

**Issues Found:**
- [Issue 1]
- [Issue 2]

### Site Structure
- [ ] Clean URL structure
- [ ] Internal linking logical
- [ ] Breadcrumb navigation present
- [ ] No orphaned pages

**Issues Found:**
- [Issue 1]

### On-Page SEO
- [ ] Unique titles (50–70 chars)
- [ ] Compelling meta descriptions (150–160 chars)
- [ ] H1 tag (one per page)
- [ ] Proper heading hierarchy
- [ ] Image alt text
- [ ] Modern image formats (WebP)
- [ ] Schema markup present

**Issues Found:**
- [Issue 1]

### Performance
- [ ] LCP < 2.5 seconds
- [ ] FID < 100ms
- [ ] CLS < 0.1
- [ ] Mobile responsive
- [ ] Touch targets 48px+

**Issues Found:**
- [Issue 1]

### Security
- [ ] SSL certificate valid
- [ ] Security headers present
- [ ] No mixed HTTP/HTTPS content

**Issues Found:**
- [Issue 1]

### International/Multi-Language
- [ ] Hreflang tags (if bilingual)
- [ ] Language meta tags correct
- [ ] No duplicate content across languages

**Issues Found:**
- [Issue 1]

### Competitive Analysis
- [ ] Backlink profile assessed
- [ ] Keyword gaps identified
- [ ] Competitor rankings noted

**Opportunities:**
- [Opportunity 1]
```

---

## References

- [Google Search Central SEO Guide](https://developers.google.com/search)
- [Moz SEO Beginner's Guide](https://moz.com/beginners-guide-to-seo)
- [Backlinko SEO Guide](https://backlinko.com/hub/seo)
- [Schema.org Documentation](https://schema.org/)

---

**Last Updated:** March 2026  
**Applicable To:** Antikode web-expert-review skill audits, especially Section 5 (AI Friendliness & SEO)

# Full SEO Audit Report: yvesschleich.com

**Date:** 2026-04-03
**URL:** https://www.yvesschleich.com
**Technology:** Static HTML on Vercel, 3-page site (index, impressum, datenschutz)
**Language:** German (de)
**Business Type:** Professional Service / Software Consultancy

---

## SEO Health Score: 68 / 100

| Category | Weight | Score | Weighted |
|----------|--------|-------|----------|
| Technical SEO | 25% | 72/100 | 18.0 |
| Content Quality | 25% | 72/100 | 18.0 |
| On-Page SEO | 20% | 75/100 | 15.0 |
| Schema / Structured Data | 10% | 78/100 | 7.8 |
| Performance (CWV) | 10% | 65/100 | 6.5 |
| Images | 5% | 40/100 | 2.0 |
| AI Search Readiness | 5% | 68/100 | 3.4 |
| **Total** | **100%** | | **70.7** |

---

## Executive Summary

### Top 5 Critical Issues
1. **Google Fonts loaded externally -- GDPR violation risk** (LG Munchen ruling, Jan 2022)
2. **No Content-Security-Policy header** -- XSS exposure with inline JS and live API key
3. **Sitemap includes noindex pages** -- contradictory crawl signals
4. **Avatar image is 1.03 MB for 80x80px display** -- massive over-serving
5. **Impressum may have incomplete data** -- legal compliance risk under DDG SS 5

### Top 5 Quick Wins
1. Add `width="80" height="80"` to avatar `<img>` tag (fixes CLS)
2. Remove impressum.html and datenschutz.html from sitemap.xml
3. Add `image` property to Person schema (already have the JPG)
4. Add `datePublished` to WebPage schema
5. Compress and resize avatar image to 160x160 WebP (~10 KB)

---

## 1. Technical SEO (72/100)

### Crawlability
- **robots.txt:** Correct. Allows all, references sitemap. No AI crawler blocks (GPTBot, ClaudeBot, etc.) -- intentional decision or oversight.
- **Internal linking:** Footer links to impressum and datenschutz. No skip-to-content link. Nav links to external sites (LinkedIn, WhatsApp, Calendly).
- **Crawl depth:** All pages reachable in 1 click from homepage. Good.

### Indexability
- **Canonical tags:** Correctly set on all 3 pages.
- **noindex directives:** Correctly applied to impressum.html and datenschutz.html.
- **Missing:** No explicit `<meta name="robots" content="index, follow">` on index.html (defaults are fine, but explicit is better practice).

### Security
- **HTTPS:** Yes (Vercel default).
- **Security headers (vercel.json):**
  - X-Frame-Options: DENY -- Good
  - X-Content-Type-Options: nosniff -- Good
  - Referrer-Policy: strict-origin-when-cross-origin -- Good
  - Permissions-Policy: camera=(), microphone=(), geolocation=() -- Good
- **MISSING: Content-Security-Policy** -- No CSP configured. The page runs inline JavaScript with a Supabase API key. Without CSP, XSS attacks could exfiltrate this key or inject malicious scripts.
- **Supabase anon key in client JS (line 1589):** This is the `anon` role key designed for client use, but the `/rest/v1/waitlist` endpoint MUST have INSERT-only RLS policies. Without RLS, anyone can read/update/delete waitlist entries.

### URL Structure
- Clean URLs, no query parameters on static pages.
- `.html` extensions used for subpages -- acceptable but `/impressum/` would be cleaner.
- No trailing slash inconsistencies detected.

### Mobile Optimization
- Viewport meta tag correctly set on all pages.
- Responsive CSS with breakpoint at 640px.
- Touch targets appear adequate (CTA buttons have sufficient padding).
- Nav adapts: hides justship badge on mobile, reduces CTA size.

### HTML Validation Concerns
- Newsletter `<button>` is `type="button"` instead of `type="submit"` (line 1456) -- breaks native form validation and keyboard submission expectations.
- No `aria-label` on SVG-only nav icon links (LinkedIn, WhatsApp).
- No `<main>` landmark wrapping the primary content (there is a `<main>` tag, which is correct).

---

## 2. Content Quality (72/100)

### E-E-A-T Assessment

| Factor | Score | Analysis |
|--------|-------|----------|
| **Experience** | 78/100 | Strong signals: 8 years own company, About You App, case studies (Aime, 19ELF, Health brand). Weakened by lack of narrative depth -- claims listed but not told as stories with measurable business outcomes. |
| **Expertise** | 74/100 | Tech tags, detailed Just Ship framework with 7 named agents, tech stack listed. Missing: no blog, no long-form thought leadership, no technical writing. |
| **Authoritativeness** | 62/100 | LinkedIn and GitHub linked. No third-party citations, press mentions, testimonials, client logos, or review signals. Weakest pillar. |
| **Trustworthiness** | 82/100 | Real name, photo, Impressum, Datenschutz, WhatsApp number, Calendly link. Strong for German audience. |

**Weighted E-E-A-T: 74/100**

### Content Depth
- **Total word count:** ~858 words across all visible content.
- **Homepage minimum (500 words):** Passed.
- **Service page minimum (800 words):** Barely passed at 858, but page serves as homepage + service page + about + case studies + FAQ simultaneously.
- **Thin content risk: MODERATE.** Each section is shallow: case studies average ~30 words, service descriptions are 1-2 sentences, FAQ answers are 1-3 sentences.

### Readability
- Excellent for target audience. Short, punchy sentences. Low jargon barrier.
- "du" form creates intimacy -- appropriate for founder/entrepreneur audience.
- Estimated Flesch-Reading-Ease equivalent (German): ~65-70 (easily readable).

### Missing Content Opportunities
- Blog / Insights section for long-tail keywords
- Detailed case studies with business outcomes
- Client testimonials or social proof
- "Software Consultant" keyword in visible body text (only in schema)

---

## 3. On-Page SEO (75/100)

### Title Tags
| Page | Title | Length | Assessment |
|------|-------|--------|------------|
| index.html | "Yves Schleich -- Software ohne Dev-Team" | 42 chars | Good. Brand + primary keyword. |
| impressum.html | "Impressum -- Yves Schleich" | 27 chars | Fine for noindex page. |
| datenschutz.html | "Datenschutzerklarung -- Yves Schleich" | 38 chars | Fine for noindex page. |

### Meta Descriptions
| Page | Description | Length | Assessment |
|------|-------------|--------|------------|
| index.html | "Ich baue deine Software -- in Tagen..." | 155 chars | Good. Includes CTA, keyword, USP. |
| impressum.html | None | - | OK (noindex) |
| datenschutz.html | None | - | OK (noindex) |

### Heading Structure
- **H1:** "Die Engineering-Abteilung, die du nicht einstellen musst." -- Benefit-driven but keyword-absent. The label "Software ohne Dev-Team" is a `<div>`, not a heading.
- **H2s:** 7 total -- good distribution across sections (Vorher/Nachher, Was ich tue, Zusammenarbeit, Ergebnisse, FAQ, Kontakt, Newsletter, Just Ship).
- **H3s:** Used within steps, packages, case cards, agents. Proper hierarchy.

### Internal Linking
- Footer: links to impressum.html, datenschutz.html, and external sites.
- Nav: external links only (LinkedIn, WhatsApp, Calendly).
- No internal anchor links in nav (e.g., #kontakt, #newsletter).
- Case study cards have no links (no dedicated pages to link to).

### Open Graph / Twitter Cards
- Fully configured with title, description, image, locale.
- OG image: og-image.png (present in repo).
- Twitter card: summary_large_image.
- All URLs absolute and correct.

---

## 4. Schema / Structured Data (78/100)

### Current Implementation (index.html)
6 entities in a single `@graph`:
1. **WebSite** -- Valid. Has name, url, description, inLanguage, publisher.
2. **WebPage** -- Valid. Has dateModified but missing datePublished.
3. **Person** -- Valid but missing `image` property (should reference yves-schleich.jpg).
4. **ProfessionalService** -- Valid with OfferCatalog (3 tiers). Missing image, telephone, email, address.
5. **SoftwareApplication** -- Valid with Offer (price: 0, EUR). Eligible for Software App rich result.
6. **BreadcrumbList** -- Single item only ("Startseite"). Not eligible for rich result (needs 2+ items).

### Validation Issues
| Severity | Issue |
|----------|-------|
| Medium | Person missing `image` property |
| Medium | WebPage missing `datePublished` |
| Low | ProfessionalService missing contact info (telephone, email, address) |
| Low | Service offers "Zusammen" and "Full Service" missing `url` |
| Low | BreadcrumbList with single item has no SEO value |

### Rich Result Eligibility
- **SoftwareApplication:** Eligible (has Offer with price).
- **BreadcrumbList:** Not eligible (single item).
- **FAQ:** No FAQPage schema. Note: Google restricted FAQPage rich results to government/healthcare sites in August 2023, so adding it would provide limited benefit for this site type.

### Missing Schema on Subpages
- impressum.html: No structured data (acceptable for noindex).
- datenschutz.html: No structured data (acceptable for noindex).

---

## 5. Performance / Core Web Vitals (65/100)

### LCP (Largest Contentful Paint) -- Projected: Good (<2.5s)
- **LCP element:** H1 text in hero section -- favorable since text renders as soon as CSS is parsed.
- **Inline CSS (~1,200 lines):** Eliminates external stylesheet round-trip. Correct tradeoff for this page size.
- **Google Fonts (render-blocking):** Browser must fetch CSS from fonts.googleapis.com, then WOFF2 files. Adds 200-600ms to LCP on slow connections.
- **No `<link rel="preload">` for fonts.**

### INP (Interaction to Next Paint) -- Projected: Good (<200ms)
- Only interactive JS is 45-line newsletter handler on click/Enter.
- No frameworks, no hydration, no heavy event delegation.
- Main thread essentially idle after load.

### CLS (Cumulative Layout Shift) -- Projected: Needs Attention
- **Font swap CLS:** `display=swap` causes text reflow when DM Sans loads. DM Sans has significantly different metrics than system sans-serif.
- **Avatar image CLS:** `<img>` at line 1281 has no `width`/`height` HTML attributes. Image is 1.03 MB, 2160x2161px for 80x80 display. CSS sets dimensions but browser can't reserve space before CSS parse.

### Recommendations
1. **Self-host fonts** -- eliminates 2 origins from critical path, saves 100-300ms LCP, fixes GDPR concern.
2. **Add `size-adjust` in `@font-face` override** to match fallback metrics to DM Sans (eliminates font-swap CLS).
3. **Compress avatar:** Resize to 160x160, convert to WebP/AVIF, target <10 KB. Add `width="80" height="80"`.
4. **Add `loading="lazy"`** to avatar image (below fold relative to hero).
5. **Reduce font weight variants** -- audit actual usage of 7 loaded weights.
6. **Add explicit `Cache-Control` headers** via vercel.json for static assets.

---

## 6. Images (40/100)

### Image Inventory
| File | Size | Dimensions | Usage | Issues |
|------|------|------------|-------|--------|
| yves-schleich.jpg | 1.03 MB | 2160x2161 | Avatar (80x80 display) | Massively oversized, no width/height attrs, no lazy loading, no WebP/AVIF |
| og-image.png | N/A | N/A | OG/Twitter social | Only used in meta tags -- OK |
| favicon.ico | Small | Standard | Favicon | OK |
| favicon.svg | ~200B | Scalable | Favicon | OK |
| favicon-32x32.png | Small | 32x32 | Favicon | OK |

### Issues
- **yves-schleich.jpg** is the only content image and it has every possible issue:
  - 1.03 MB for an 80x80px display (should be ~10 KB)
  - No `width`/`height` HTML attributes (CLS risk)
  - No `loading="lazy"` (below fold)
  - No `srcset` for responsive/retina
  - Not in modern format (WebP/AVIF)
  - No `fetchpriority` hint
- **No `alt` text issues** -- avatar has `alt="Yves Schleich"` (correct).
- **Missing `rel="apple-touch-icon"`** in favicon set.

---

## 7. AI Search Readiness (68/100)

### Strengths
- Rich structured data with entity relationships (Person > ProfessionalService > OfferCatalog).
- Clean heading hierarchy (H1 > H2 > H3).
- FAQ section with clear question-answer pairs.
- `sameAs` links to LinkedIn and GitHub.

### Weaknesses
- No quotable statistics with source context (e.g., "300+ Features in 4 Monaten" is stated but not contextualized for confident AI citation).
- No visible `datePublished` or "last updated" signal on page body.
- No blog content for AI systems to reference as thought leadership.
- No robots.txt rules for AI crawlers (GPTBot, ClaudeBot, CCBot) -- decide if you want to allow or block AI training.

### AI Crawler Policy (Decision Required)
Currently no AI-specific rules in robots.txt. Options:
- **Allow all:** Current state. AI systems can crawl and cite your content.
- **Block training, allow search:** Add `Disallow` for training crawlers (CCBot) but allow search crawlers (GPTBot, Google-Extended).
- **Block all AI:** Maximally restrictive.

---

## Appendix: File-by-File Summary

### index.html
- Canonical: https://www.yvesschleich.com/ (correct)
- Robots: No directive (defaults to index, follow)
- Schema: Full @graph with 6 entities
- OG/Twitter: Fully configured
- Issues: Google Fonts GDPR, no CSP, oversized avatar, thin content sections

### impressum.html
- Canonical: https://www.yvesschleich.com/impressum.html (correct)
- Robots: noindex, follow (correct)
- Schema: None
- Issues: May have incomplete placeholder data

### datenschutz.html
- Canonical: https://www.yvesschleich.com/datenschutz.html (correct)
- Robots: noindex, follow (correct)
- Schema: None
- Issues: References Google Fonts (GDPR mention needed in privacy policy)

### sitemap.xml
- 3 URLs listed (should be 1 -- remove noindex pages)
- All lastmod identical (2026-03-17)
- Priority tags present but ignored by Google

### robots.txt
- Allows all crawlers
- References sitemap correctly
- No AI crawler rules

### vercel.json
- 4 security headers configured
- Missing CSP header
- No caching headers for static assets

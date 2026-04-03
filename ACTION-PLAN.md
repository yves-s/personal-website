# SEO Action Plan: yvesschleich.com

**Generated:** 2026-04-03
**Current Score:** 68/100
**Target Score:** 85+ / 100 (achievable with Critical + High items)

---

## Critical (Fix Immediately)

### 1. Self-host Google Fonts (GDPR + Performance)
**Impact:** Legal compliance, -200ms LCP, eliminates CLS from font swap
**Files:** index.html, impressum.html, datenschutz.html
**Steps:**
- Download DM Sans (400, 500, 600, 700) and JetBrains Mono (400, 500, 600) as WOFF2
- Subset to Latin character set only
- Host in `/fonts/` directory
- Replace `<link href="https://fonts.googleapis.com/...">` with local `@font-face` declarations
- Remove `preconnect` hints to fonts.googleapis.com and fonts.gstatic.com
- Add `size-adjust` to match fallback font metrics (eliminates CLS)
- Update Datenschutz page to remove/update Google Fonts section

### 2. Add Content-Security-Policy header
**Impact:** XSS protection, security posture
**File:** vercel.json
**Steps:**
- Add CSP header allowing inline styles, inline scripts, self-hosted resources, and Supabase API domain
- Example: `default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; connect-src 'self' https://wsmnutkobalfrceavpxs.supabase.co; img-src 'self'; font-src 'self'`

### 3. Verify Supabase RLS on waitlist table
**Impact:** Data security
**Action:** Verify that the `waitlist` table has INSERT-only RLS policy for anon role. Without it, anyone can read, update, or delete entries using the exposed anon key.

---

## High (Fix Within 1 Week)

### 4. Remove noindex pages from sitemap.xml
**Impact:** Eliminates contradictory crawl signals
**File:** sitemap.xml
**Steps:**
- Remove impressum.html and datenschutz.html entries
- Remove `<priority>` tags (ignored by Google)
- Update lastmod on index.html to current date

### 5. Optimize avatar image
**Impact:** -1 MB page weight, fixes CLS
**File:** yves-schleich.jpg
**Steps:**
- Resize to 160x160px (2x retina)
- Convert to WebP with JPEG fallback
- Target: <10 KB
- Add `width="80" height="80"` to `<img>` tag
- Add `loading="lazy"` (below fold)
- Consider `<picture>` element for format negotiation

### 6. Complete Impressum data
**Impact:** Legal compliance (DDG SS 5)
**File:** impressum.html
**Steps:**
- Verify all required fields are filled (name, address, email, phone)
- Add Umsatzsteuer-Identifikationsnummer if applicable

### 7. Expand case studies
**Impact:** Strengthens Experience signals, creates citable content
**File:** index.html
**Steps:**
- Expand each case study from ~30 words to 150-300 words
- Add measurable business outcomes (timeline, cost savings, user metrics)
- Consider dedicated case study pages (adds indexable content)

---

## Medium (Fix Within 1 Month)

### 8. Add social proof
**Impact:** Closes Authoritativeness gap (weakest E-E-A-T pillar at 62/100)
**File:** index.html
**Steps:**
- Add 2-3 client testimonials (quotes with name/role/company)
- Add client logos or "trusted by" bar
- Consider linking to LinkedIn recommendations

### 9. Fix schema issues
**Impact:** Rich result eligibility, Knowledge Graph signals
**File:** index.html
**Steps:**
- Add `"image": "https://www.yvesschleich.com/yves-schleich.jpg"` to Person entity
- Add `"datePublished": "2026-01-XX"` to WebPage entity
- Add `url` to "Zusammen" and "Full Service" Offer entities
- Remove single-item BreadcrumbList (no SEO value) or expand on subpages

### 10. Add explicit caching headers
**Impact:** Faster repeat visits
**File:** vercel.json
**Steps:**
- Add `Cache-Control: public, max-age=31536000, immutable` for images, fonts
- Add `Cache-Control: public, max-age=3600` for HTML

### 11. Start a blog / Insights section
**Impact:** Massively expands keyword footprint, demonstrates Expertise
**Suggested topics:**
- "AI Agents fur Software Entwicklung" (informational)
- "Software bauen ohne Entwickler" (informational)
- "Agentic Development Erfahrungen" (thought leadership)
- "Just Ship Framework Guide" (technical/educational)

### 12. Reduce font weight loading
**Impact:** Smaller font payload (-30-50%)
**Steps:**
- Audit which weights are actually used across all 3 pages
- Remove unused weights from @font-face declarations
- Likely needed: DM Sans 400, 600, 700 + JetBrains Mono 400, 500, 600

---

## Low (Backlog)

### 13. Add apple-touch-icon
**File:** index.html (all pages)
**Steps:** Add `<link rel="apple-touch-icon" href="/apple-touch-icon.png">` and create 180x180 icon.

### 14. Add aria-labels to SVG icon links
**File:** index.html
**Steps:** Add `aria-label="LinkedIn Profil"` and `aria-label="WhatsApp Kontakt"` to nav icon links.

### 15. Fix newsletter button type
**File:** index.html
**Steps:** Change `<button type="button">` to `<button type="submit">` for semantic correctness and native form validation.

### 16. Add visible "last updated" date
**File:** index.html
**Steps:** Show "Zuletzt aktualisiert: April 2026" somewhere on the page for freshness signals.

### 17. Decide on AI crawler policy
**File:** robots.txt
**Steps:** Add rules for GPTBot, ClaudeBot, CCBot, Google-Extended based on your preference (allow, block training, or block all).

### 18. Add "Software Consultant" to visible body text
**File:** index.html
**Steps:** Currently only in schema `jobTitle`. Add to about section or subtitle so visible content matches schema claims.

### 19. Add internal anchor links in nav
**File:** index.html
**Steps:** Consider adding nav links to page sections (#kontakt, #newsletter) for better UX and internal linking signals.

---

## Score Impact Projection

| Action | Effort | Score Impact |
|--------|--------|-------------|
| Self-host fonts | Medium | +5 (performance + security) |
| Add CSP header | Low | +3 (security) |
| Fix sitemap | Low | +2 (technical) |
| Optimize avatar | Low | +3 (images + performance) |
| Expand case studies | Medium | +4 (content) |
| Add social proof | Medium | +3 (content/E-E-A-T) |
| Fix schema issues | Low | +2 (structured data) |
| Start blog | High | +8 (content + keywords) |
| **Total potential** | | **+30 -> ~98/100** |

---

## Priority Matrix

```
                    HIGH IMPACT
                        |
   Self-host fonts  [1] | [7] Expand case studies
   Add CSP          [2] | [8] Add social proof
   Fix sitemap      [4] | [11] Start blog
   Optimize image   [5] |
                        |
   LOW EFFORT ----------+---------- HIGH EFFORT
                        |
   Fix schema       [9] | [6] Complete Impressum
   Fix button      [15] |
   aria-labels     [14] |
   AI crawlers     [17] |
                        |
                    LOW IMPACT
```

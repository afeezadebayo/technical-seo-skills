---
name: lcp-optimisation
description: "Diagnose and fix Largest Contentful Paint: field data at the 75th percentile, the LCP element by template and device, the four LCP subparts (TTFB, load delay, load duration, render delay), fetchpriority, preload, no lazy-loading on the hero, AVIF and responsive images, critical CSS, fonts and WordPress fixes. Use whenever Search Console or PageSpeed Insights flags poor LCP, a page loads its main content slowly, a hero image is slow, or someone wants to improve Core Web Vitals loading performance, including on WordPress and Elementor."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/lcp-optimisation/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-13"
---

# LCP Optimisation

Largest Contentful Paint (LCP) measures how long the main content of a page takes to appear: usually the hero image, a large heading or a video poster. Google's Core Web Vitals treat an LCP of 2.5 seconds or less as good, measured at the 75th percentile of real page loads. [Confirmed, web.dev Core Web Vitals] Slow LCP frustrates users, lowers conversion and can weigh on rankings where competing pages are otherwise similar. Most fixes are well understood, but teams often apply random tips (minify CSS, add a plugin) without finding which part of LCP is actually slow.

This skill diagnoses and fixes LCP using the four LCP subparts described on web.dev: time to first byte (TTFB), resource load delay, resource load duration and element render delay. [Reference, web.dev guide to LCP] It identifies the LCP element by template, measures field and lab data, finds the slowest subpart and applies targeted fixes for servers, images, fonts, CSS, JavaScript and CMS setups such as WordPress.

## Operating principles
1. **Field data decides.** Use real-user data (Chrome UX Report, Search Console, RUM) to judge; lab tools to debug.
2. **Find the LCP element first.** Optimise the element that is actually largest, per template and device.
3. **Fix the slowest subpart.** Break LCP into its four parts and work on the biggest.
4. **Prioritise the LCP resource.** Discover it early, load it first, never lazy-load it.
5. **Mobile first.** Mobile LCP is usually worse and matters most.

## LCP subparts
| Subpart | What it is | Common causes of slowness | Typical fixes |
|---|---|---|---|
| TTFB | Time until the first byte of HTML | Slow server, no caching, redirects, far from users | Page caching, CDN, faster hosting, fewer redirects |
| Resource load delay | Time from TTFB until the LCP resource starts loading | Image discovered late (CSS background, JS-inserted, lazy-loaded) | Put image in HTML, preload, fetchpriority="high", remove lazy-loading |
| Resource load duration | Time to download the LCP resource | Large file, slow network, no CDN | Compress, modern formats (AVIF, WebP), responsive sizes, CDN |
| Element render delay | Time from download to display | Render-blocking CSS and JS, fonts, client-side rendering | Inline critical CSS, defer JS, font-display, server rendering |

web.dev guidance suggests most LCP time should be in TTFB and resource load duration, with load delay and render delay kept small. [Reference, web.dev]

## When to use, and when not to
Use it when Search Console or PageSpeed Insights flags poor or needs-improvement LCP, before and after redesigns, for key templates (home, category, product, article) and when conversion drops with speed.

Don't use it for interaction issues (`inp-optimisation`) or layout shifts (`cls-optimisation`); the parent `core-web-vitals` skill covers the full programme.

## Inputs
- URLs and templates affected (Search Console Core Web Vitals report groups).
- Field data: CrUX or RUM by template and device.
- Lab data: PageSpeed Insights, Lighthouse, WebPageTest or browser DevTools traces.
- Tech stack: CMS, theme, page builder, hosting, CDN, caching plugins.

## Workflow

### Step 1. Confirm the problem in field data
Check 75th percentile LCP by template and device. Prioritise templates with the most traffic and value.

### Step 2. Identify the LCP element
Use DevTools Performance panel or PageSpeed Insights to see which element is LCP on mobile and desktop for each template.

### Step 3. Measure the subparts
Record TTFB, load delay, load duration and render delay from a trace or the web-vitals library attribution build.

### Step 4. Fix TTFB if large
Full-page caching, CDN with edge caching for HTML where possible, upgraded hosting, database and query optimisation, remove redirect chains.

### Step 5. Remove load delay
Make the LCP image discoverable in the initial HTML (an `img` element, not a CSS background or JS-injected image); add `fetchpriority="high"`; preload it if it must come from CSS; never apply `loading="lazy"` to it.

### Step 6. Shorten load duration
Serve AVIF or WebP, compress, use `srcset` and `sizes` for the right dimensions, serve from a CDN, avoid huge hero videos on mobile.

### Step 7. Cut render delay
Inline critical CSS, defer non-critical CSS and JavaScript, reduce render-blocking third parties, use `font-display: swap` or optional and preload key fonts, avoid hiding content behind JavaScript until hydration.

### Step 8. Verify
Lab test after each change; watch field data over the following 28 days (CrUX reports a rolling window).

## Code patterns
```html
<!-- LCP hero image: discoverable, prioritised, responsive -->
<img src="/img/hero-1200.avif"
     srcset="/img/hero-600.avif 600w, /img/hero-1200.avif 1200w"
     sizes="(max-width: 600px) 100vw, 1200px"
     width="1200" height="630" alt="..."
     fetchpriority="high">

<!-- If the LCP image is a CSS background, preload it -->
<link rel="preload" as="image" href="/img/hero-1200.avif" fetchpriority="high">

<!-- Key font -->
<link rel="preload" as="font" type="font/woff2" href="/fonts/brand.woff2" crossorigin>
```

## WordPress and page builder notes
- Use full-page caching (for example LiteSpeed Cache or similar) and a CDN.
- Exclude the hero image from lazy-loading; many plugins add lazy-loading to all images by default.
- Avoid sliders and carousels above the fold; a single static hero loads faster.
- Reduce page-builder bloat: unused widgets, global fonts, animation libraries.
- Generate critical CSS per template and defer the rest; test carefully for broken layouts.
- Serve images in AVIF or WebP with correct sizes.

## Measuring in the field
Add real-user monitoring with the open-source web-vitals JavaScript library (attribution build) to capture LCP, the LCP element and subpart timings by page type, device and country. Send the data to analytics and review weekly. This shows which fixes moved real users, not just lab scores.

## Priorities by effort
| Fix | Effort | Typical impact |
|---|---|---|
| Remove lazy-loading from hero | Low | High when load delay is large |
| Add fetchpriority to LCP image | Low | Medium to high |
| Page caching and CDN | Low to medium | High when TTFB is slow |
| Modern image formats and sizes | Medium | Medium to high |
| Critical CSS and deferred JS | Medium to high | Medium |
| Server rendering for apps | High | High for client-rendered pages |
Start with the low-effort, high-impact fixes, then measure before deeper work.

## Edge cases and judgement calls
| Situation | What to do |
|---|---|
| LCP element is text | Focus on TTFB, render-blocking CSS and fonts |
| LCP differs by device | Optimise mobile first; check both |
| Client-side rendered app | Server-side rendering or static generation for key pages |
| Hero video | Use a lightweight poster image as the LCP; load video later |
| Third-party consent banner becomes LCP | Make the banner smaller or render it after main content |
| Good lab, poor field | Look at slow devices, slow networks and geography; add RUM |

## Handoffs to related skills
- Full Core Web Vitals programme: `core-web-vitals`
- Interaction delays: `inp-optimisation`
- Layout shifts: `cls-optimisation`
- JavaScript rendering: `javascript-seo`
- Overall audit: `technical-seo-audit`

## Worked example
An ecommerce category template had a mobile 75th percentile LCP of 4.1 seconds. The LCP element was a hero image set as a CSS background and lazy-loaded by a plugin. Subparts showed TTFB 0.9s, load delay 1.6s, load duration 1.1s and render delay 0.5s. The fix moved the hero into an `img` element with `fetchpriority="high"`, excluded it from lazy-loading, served AVIF at the right sizes from the CDN and enabled full-page caching. Lab LCP fell to 1.8 seconds, and over the next 28 days field LCP for the template moved into the good range at 2.2 seconds.

## Common mistakes
- Optimising the wrong element.
- Lazy-loading the hero image.
- LCP images hidden in CSS backgrounds or injected by JavaScript.
- Chasing lab scores while field data stays poor.
- Ignoring TTFB.

## Output format
```
1. Field LCP by template and device
2. LCP element per template
3. Subpart breakdown
4. Prioritised fixes with owner and effort
5. Code or configuration changes
6. Verification plan (lab now, field over 28 days)
```

## Self-check before you answer
- Was the problem confirmed in field data?
- Is the LCP element identified for mobile and desktop?
- Are fixes targeted at the slowest subpart?
- Is the LCP resource discoverable, prioritised and not lazy-loaded?

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/lcp-optimisation/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

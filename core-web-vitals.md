---
name: core-web-vitals
description: "Diagnose and improve Core Web Vitals (Largest Contentful Paint, Interaction to Next Paint and Cumulative Layout Shift) from CrUX and Search Console field data, with lab debugging in Lighthouse, PageSpeed Insights and Chrome DevTools, template-level fixes for WordPress, Elementor, Shopify, Webflow, React and Next.js, and a release check so gains don't slip back. Use whenever someone mentions page speed, slow pages, Core Web Vitals, LCP, INP, CLS, a failing PageSpeed score, 'Poor URLs' in Search Console, layout jumps, laggy clicks, or asks whether speed affects rankings."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/core-web-vitals/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Core Web Vitals and Page Experience

Core Web Vitals measure three things real visitors feel: how fast the main content appears (LCP), how quickly the page responds to a click or tap (INP) and how much the layout jumps around (CLS). Google uses them as part of its page experience signals. They rarely outrank relevance, but slow, jumpy pages lose users and conversions whatever their rankings.

This skill works from field data first, finds the template and the element responsible, and fixes causes rather than chasing a lab score.

## Operating principles
1. **Field data decides.** Pass or fail comes from real-user data (CrUX), at the 75th percentile, for mobile and desktop separately. Lighthouse scores are for debugging.
2. **Fix templates, not URLs.** One product template fix can move thousands of URLs.
3. **Find the element, then the cause.** "LCP is slow" isn't a diagnosis; "the hero image starts loading at 2.1 seconds because it's lazy-loaded" is.
4. **Measure after release.** Field data is a 28-day rolling window, so results show gradually.
5. **Speed is a business metric too.** Tie fixes to conversion or engagement where you can, not only to Search.

## Thresholds (75th percentile of page loads)
| Metric | Good | Needs improvement | Poor | What it measures |
|---|---|---|---|---|
| LCP | 2.5 s or less | up to 4.0 s | over 4.0 s | Time until the largest image or text block in the viewport renders |
| INP | 200 ms or less | up to 500 ms | over 500 ms | Delay between an interaction and the next frame, across the visit |
| CLS | 0.1 or less | up to 0.25 | over 0.25 | Unexpected layout shifts during the visit |

INP replaced First Input Delay as a Core Web Vital in March 2024. A URL group passes only if all three metrics are good.

## When to use, and when not to
Use it for failing or borderline Core Web Vitals, slow templates, laggy interactions, layout jumps, speed work before a launch, and checking whether a new theme or app made things worse.

Don't use it for crawling or rendering problems where content is missing from HTML (`javascript-seo`), or for server errors and timeouts (`redirects-status-codes` and `log-file-analysis`). Don't promise ranking gains from speed work alone.

## Inputs to ask for, in one message
1. Search Console Core Web Vitals report: mobile and desktop, with example URL groups.
2. PageSpeed Insights results for two or three URLs per key template (field and lab sections).
3. The stack: CMS or framework, theme or page builder, hosting, CDN, image service, tag manager.
4. Third-party scripts: analytics, chat, A/B testing, ads, consent, reviews, heatmaps.
5. Any real-user monitoring data (for example from a RUM tool or the `web-vitals` library in analytics).
6. Recent releases with dates.

## Workflow

### Step 1. Read the field data by template
Group failing URLs by template (home, category, product, article, landing). Note which metric fails, on which device, and how far from the threshold. If a page has no CrUX data, use origin-level data and lab tests, and say so.

### Step 2. Reproduce in the lab
Run Lighthouse or PageSpeed Insights with mobile throttling, and a DevTools Performance recording. Identify the LCP element, the long tasks around interactions, and the elements that shift.

### Step 3. Diagnose LCP in four parts
| Part | Ask | Typical fixes |
|---|---|---|
| Time to first byte | Is the HTML slow to arrive? | Page caching, CDN, faster hosting, fewer redirects, edge caching for logged-out users |
| Resource load delay | Does the LCP image start late? | Remove lazy-loading from the hero, add `fetchpriority="high"`, preload it, put it in the HTML rather than CSS or JavaScript |
| Resource load time | Is the file too big? | Correct dimensions, AVIF or WebP, responsive `srcset`, image CDN |
| Render delay | Is rendering blocked after download? | Less render-blocking CSS and JavaScript, inline critical CSS, avoid client-side rendering of the hero |

### Step 4. Diagnose INP
Find the interactions that are slow (menu, filters, add to basket, accordions, consent banner). Causes are usually long JavaScript tasks on the main thread. Fixes: remove or defer unused scripts, break up long tasks and yield to the main thread, reduce DOM size, debounce input handlers, move work off the main thread, and load third-party tags after interaction or consent.

### Step 5. Diagnose CLS
Use the DevTools Layout Shift regions or the Performance panel. Causes: images and embeds without dimensions, ads or banners injected above content, late web fonts, cookie bars that push content, and content inserted after load. Fixes: set `width` and `height` or `aspect-ratio`, reserve space for ads and embeds, use `font-display` with matched fallback fonts, overlay rather than push for banners.

### Step 6. Prioritise and ship
Rank fixes by URLs affected, distance from the threshold and effort. Ship template fixes one at a time where possible so you can see which one worked.

### Step 7. Verify
Check lab results straight after release, then field data weekly. Use Search Console's Validate fix once the change is live. Field data takes up to 28 days to fully reflect a change.

## Platform notes
- **WordPress and Elementor:** page caching plus a CDN fixes most TTFB problems. Turn on Elementor's performance features (optimised DOM output, improved asset loading), remove unused widgets and plugins, avoid sliders and animated heroes above the fold, and don't lazy-load the hero image. Check that optimisation plugins don't defer scripts the header menu needs, which hurts INP.
- **Shopify:** most problems are apps injecting scripts on every page. Remove unused apps (and their leftover code), use the theme's responsive image helpers and limit above-the-fold sections.
- **Webflow:** compress and size images before upload, limit interactions and Lottie animations above the fold, and move heavy embeds below the fold.
- **React and Next.js:** render the hero on the server, use the framework's image component with priority on the LCP image, split bundles by route, avoid hydrating large static sections, and audit client components that run on every interaction.

## Third-party scripts
List every tag with its owner, purpose and load time. For each one decide: remove, delay until interaction or consent, load after the page is interactive, or keep. Tag managers make it easy to add scripts and hard to notice their cost; review them quarterly.

## Worked example
A SaaS homepage failed LCP on mobile at 3.9 seconds (CrUX, 75th percentile). The LCP element was a hero screenshot set as a CSS background, so the browser found it late, and a slider plugin added 180 KB of JavaScript before it. The fix: replace the slider with a single `<img>` with `fetchpriority="high"` and explicit dimensions, serve it as AVIF at the displayed size, and remove the slider script. Lab LCP dropped from 4.6 to 2.1 seconds; four weeks later the field value was 2.3 seconds and the URL group passed.

## Release guardrails
- Add a performance budget per template (for example LCP under 2.5 s and total JavaScript under a set size in lab tests).
- Run Lighthouse in CI or on staging before release for key templates.
- Record each new third-party script with an owner and an expiry date.

## Output format
```
# Core Web Vitals report: <site>, <date>
## Field status by template and device
| Template | Device | LCP | INP | CLS | Status | URLs affected |
## Diagnosis per failing template (element, cause, evidence)
## Fix list
| # | Fix | Template | Metric | Expected effect | Effort | Owner |
## Verification plan and re-check dates
```

## Common mistakes
- Chasing a Lighthouse score of 100 instead of passing field thresholds.
- Lazy-loading the hero image.
- Testing only desktop or only the homepage.
- Adding more optimisation plugins that conflict with each other.
- Ignoring third-party tags because "marketing needs them".
- Claiming speed fixes will lift rankings by a set amount.

## Questions to ask before you recommend changes
- Which templates bring the most organic traffic and revenue?
- Who owns each third-party script, and can any be removed?
- How quickly can theme or front-end changes ship?
- Is there real-user monitoring beyond CrUX?

## Self-check before you answer
- Did I base pass or fail on field data at the 75th percentile, per device?
- Did I name the element and the cause for each failing metric?
- Are fixes grouped by template and ranked by URLs affected?
- Did I avoid promising ranking changes?

## Official documentation
- Web Vitals overview: https://web.dev/articles/vitals
- Largest Contentful Paint: https://web.dev/articles/lcp
- Optimise LCP: https://web.dev/articles/optimize-lcp
- Interaction to Next Paint: https://web.dev/articles/inp
- Cumulative Layout Shift: https://web.dev/articles/cls
- Core Web Vitals and Google Search: https://developers.google.com/search/docs/appearance/core-web-vitals
- Page experience in Google Search: https://developers.google.com/search/docs/appearance/page-experience
- Core Web Vitals report: https://support.google.com/webmasters/answer/9205520

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/core-web-vitals/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

---
name: cls-optimisation
description: "Diagnose and fix Cumulative Layout Shift: field data at the 75th percentile, shift attribution, reserved space for images, embeds and ads, overlay banners, size-adjusted fallback fonts, transform-based animations, back/forward cache eligibility, code patterns and WordPress fixes. Use whenever Search Console flags poor CLS, pages jump while loading, ads or banners push content, fonts cause text to reflow, or someone wants to improve Core Web Vitals visual stability."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/cls-optimisation/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-13"
---

# CLS Optimisation

Cumulative Layout Shift (CLS) measures how much visible content moves unexpectedly while a page is used: text jumping as an image loads, buttons moving as an ad appears, a banner pushing the page down. A CLS of 0.1 or less is good, measured at the 75th percentile of real page loads. [Confirmed, web.dev CLS] Layout shifts cause misclicks, lost reading position and distrust, and they are usually easy to fix once the shifting elements are found.

This skill diagnoses and fixes CLS: finding which elements shift and why, then reserving space for images, video, embeds and ads, handling web fonts, avoiding content inserted above existing content, using CSS transforms for animations, and keeping pages eligible for the back/forward cache so return visits do not shift.

## Operating principles
1. **Reserve space for everything that loads late.** Images, embeds, ads and dynamic blocks get fixed dimensions or aspect ratios.
2. **Never insert above existing content** unless the user asked for it.
3. **Fonts should not reflow the page.** Match fallback fonts and load key fonts early.
4. **Animate with transforms.** Move things without changing layout.
5. **Measure in the field.** Shifts often happen later in the visit, not only at load.

## Common causes and fixes
| Cause | Fix |
|---|---|
| Images without width and height | Add width and height attributes or CSS `aspect-ratio` |
| Embeds and iframes (video, maps, social) | Wrap in a container with fixed aspect ratio |
| Ads and promo slots | Reserve the largest likely slot size; avoid collapsing empty slots near the top |
| Cookie and promo banners pushing content | Overlay them (fixed position) instead of inserting at the top |
| Web fonts swapping | Preload key fonts; use `font-display: optional` or swap with size-adjusted fallbacks |
| Content injected by JavaScript | Reserve space with skeletons or min-height |
| Animations of top, left, height or width | Use `transform` and `opacity` instead |
| Late-loading CSS | Load critical CSS early; avoid styles that change layout after render |

## When to use, and when not to
Use it when Search Console flags CLS, users report jumping pages, after adding ads or banners, after design changes and for templates with embeds.

Don't use it for loading speed (`lcp-optimisation`) or responsiveness (`inp-optimisation`).

## Inputs
- Field CLS by template and device.
- Layout shift attribution (which elements moved).
- The template's components: images, ads, embeds, banners, fonts, dynamic content.
- CMS and plugins in use.

## Workflow

### Step 1. Confirm in field data
Find templates with poor CLS; note whether shifts happen at load or during the visit.

### Step 2. Find shifting elements
Use DevTools (Rendering panel, Layout Shift Regions; Performance panel layout shift records) and the web-vitals attribution build to identify the largest shift sources.

### Step 3. Reserve space for media
Add dimensions or aspect ratios to every image, video and iframe, including in page builders and CMS editors.

### Step 4. Handle ads and dynamic slots
Set fixed or minimum heights for ad slots and dynamic modules; place them below the first screen when possible.

### Step 5. Fix banners
Use overlays or reserve space from the first render for consent, promo and app banners.

### Step 6. Fix fonts
Preload key fonts, use size-adjusted fallback fonts so text keeps its size, or `font-display: optional` for non-critical fonts.

### Step 7. Fix animations and late styles
Transform-based animations; no layout-changing transitions; avoid late CSS that restyles above-the-fold content.

### Step 8. Keep the back/forward cache working
Avoid `unload` handlers and other blockers so returning visitors see an instant, stable page.

### Step 9. Verify
Lab checks with layout shift regions; field CLS over 28 days.

## Code patterns
```html
<img src="/img/card.webp" width="640" height="360" alt="...">

<div style="aspect-ratio: 16 / 9;">
  <iframe src="https://www.youtube.com/embed/..." loading="lazy"
          style="width:100%;height:100%;border:0" title="..."></iframe>
</div>

<style>
.ad-slot-top { min-height: 250px; }
@font-face { font-family: Brand; src: url(/fonts/brand.woff2) format('woff2'); font-display: swap; }
@font-face { font-family: Brand-fallback; src: local('Arial'); size-adjust: 104%; }
body { font-family: Brand, Brand-fallback, sans-serif; }
</style>
```

## WordPress and page builder notes
- Make sure images inserted through the editor keep width and height attributes.
- Set fixed heights for sliders, testimonial carousels and embedded forms.
- Use overlay-style cookie banners.
- Check that lazy-loading plugins add placeholders with correct dimensions.
- Watch for icon fonts and Google Fonts swapping late; host fonts locally and preload.

## How CLS is calculated
Each unexpected shift scores impact fraction (how much of the viewport moved) multiplied by distance fraction (how far it moved). Shifts are grouped into session windows (bursts up to five seconds, with gaps under one second), and CLS is the largest window during the visit. This is why a single large shift near the top of the page can fail a template, and why late shifts during scrolling still count.

## Finding shifts in DevTools
1. Open DevTools, then Rendering, and enable Layout Shift Regions; shifted areas flash blue.
2. Record a Performance trace while loading and scrolling; layout shift entries show which nodes moved.
3. Throttle the network so late-loading elements reveal themselves.
4. Repeat on a mobile viewport.

## Measuring in the field
The web-vitals attribution build reports the largest shift target and the time it happened. Send it to analytics by page type to see which component causes most shifts across real visits.

## Prevention checklist for new templates
- [ ] Every image, video and iframe has dimensions or an aspect ratio.
- [ ] Ad and dynamic slots have reserved heights.
- [ ] Banners are overlays.
- [ ] Fonts are preloaded with fallbacks sized to match.
- [ ] Animations use transform and opacity.
- [ ] Pages are eligible for the back/forward cache.

## Quick wins
Add dimensions to hero and card images, switch the cookie banner to an overlay and reserve height for the top ad or promo slot. These three changes fix most CLS problems on content and ecommerce templates.

## Edge cases and judgement calls
| Situation | What to do |
|---|---|
| Ad sizes vary | Reserve the most common or largest size; accept some white space |
| Personalised content above the fold | Reserve space with a skeleton of the right size |
| Infinite scroll | Load new items below the viewport; keep footer out of the way |
| Shifts after user clicks | User-initiated shifts within 500 ms are excluded from CLS, but keep them smooth |
| Good lab, poor field | Shifts may happen later in visits; use field attribution |
| Single-page apps | Measure on route changes; reserve space for loaded views |

## Handoffs to related skills
- Full programme: `core-web-vitals`
- Loading: `lcp-optimisation`
- Responsiveness: `inp-optimisation`
- Rendering: `javascript-seo`

## Worked example
A news site's article template had a mobile CLS of 0.28. Attribution showed three sources: a top ad slot that collapsed and expanded, a consent banner inserted above the header and a web font swap that changed line lengths. The fix reserved a minimum height for the ad slot, changed the banner to a fixed overlay and added a size-adjusted fallback font with preloading. Lab layout shift regions disappeared, and field CLS fell to 0.04 within a month.

## Common mistakes
- Images and embeds without dimensions.
- Banners inserted at the top of the page.
- Collapsing empty ad slots.
- Animating layout properties.
- Ignoring shifts that happen later in the visit.

## Output format
```
1. Field CLS by template and device
2. Shift sources with attribution
3. Fixes per source
4. Code or configuration changes
5. Verification plan
```

## Self-check before you answer
- Were shifting elements identified from attribution?
- Do all images, embeds and ads have reserved space?
- Are banners overlays and fonts handled?
- Is field CLS tracked after changes?

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/cls-optimisation/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

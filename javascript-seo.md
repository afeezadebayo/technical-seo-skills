---
name: javascript-seo
description: "Make JavaScript-heavy sites (React, Next.js, Vue, Nuxt, Angular, Svelte, headless CMS builds and single-page apps) crawlable and indexable: choosing SSR, SSG, ISR or CSR per template, comparing raw and rendered HTML, crawlable links, lazy-loaded content, client-side routing, status codes and meta tags in apps, hydration problems, and visibility to AI crawlers that do not run JavaScript. Use whenever someone mentions React, Next.js, Vue, Angular or SPA SEO, pages not indexed after a rebuild, content missing from view-source, 'rendered HTML', dynamic rendering, prerendering, hydration, or asks whether Google or ChatGPT can see JavaScript content."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/javascript-seo/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# JavaScript SEO and Rendering

Googlebot can run JavaScript, but it does so in a second step, after the first crawl, using a recent version of Chromium. Anything that only appears after scripts run is seen later, less reliably, and sometimes not at all. Many other crawlers, including most AI answer-engine crawlers, read only the raw HTML. If your main content, links or metadata live only in the browser, a large part of the web's crawlers never see them.

This skill checks what each crawler receives, picks the right rendering strategy per template and turns the findings into fixes developers can test.

## Operating principles
1. **The raw HTML is the contract.** Main content, title, meta description, canonical, meta robots, internal links and structured data should be in the server response.
2. **Links must be links.** `<a href="/path/">` elements that resolve to real URLs; not buttons, `onclick` handlers or `#` fragments.
3. **Every route returns the right status.** A missing product returns 404 from the server, not a "not found" component with 200.
4. **Don't rely on Google to fix rendering.** Rendering is deferred and resource-limited; other crawlers may skip it.
5. **Test like a crawler.** Compare view-source, rendered DOM and Google's URL Inspection output for every template.

## How Google processes JavaScript pages
| Stage | What happens | Risk |
|---|---|---|
| Crawl | Googlebot fetches the URL and reads the raw HTML and HTTP headers | If robots.txt blocks JavaScript or API files, rendering breaks later |
| Render queue | The page waits for rendering resources | Delays indexing of content that needs JavaScript |
| Render | Chromium runs scripts and builds the DOM | Errors, timeouts, blocked API calls or user-triggered content leave gaps |
| Index | Google indexes the rendered content and links found | Links added only after a click or scroll are often missed |

If the raw HTML contains `noindex`, Google may skip rendering, so JavaScript that removes `noindex` later doesn't work.

## Rendering strategies
| Strategy | How it works | Best for | SEO risk |
|---|---|---|---|
| SSG (static generation) | HTML built at deploy time | Docs, blogs, marketing pages | Low; stale content if builds are rare |
| ISR or on-demand revalidation | Static HTML refreshed in the background | Large catalogues, listings | Low; check revalidation actually runs |
| SSR (server-side rendering) | HTML built per request | Personalised or fast-changing pages | Low; watch server response time |
| Streaming SSR | HTML sent in chunks | Large React or Next.js apps | Low if main content is in the stream, not deferred to the client |
| CSR (client-side rendering) | Empty shell, content built in the browser | Logged-in dashboards, tools | High for public pages |
| Dynamic rendering | Bots get prerendered HTML, users get CSR | Legacy apps as a stopgap | Google calls it a workaround, not a long-term fix; costly to maintain |

## When to use, and when not to
Use it for any site built with a JavaScript framework, a headless CMS front end or heavy client-side widgets; for pages that are crawled but not indexed after a rebuild; and for checking what AI crawlers can read.

Don't use it for speed tuning that doesn't affect content visibility (`core-web-vitals`) or for duplicate URL problems unrelated to rendering (`canonicalization`).

## Inputs to ask for, in one message
1. Framework and version, rendering mode per route, hosting platform and CDN.
2. Two or three example URLs per template (home, category, product or article, search, landing).
3. Search Console access, or URL Inspection screenshots of the rendered HTML and screenshot for those URLs.
4. A crawl run twice: once without JavaScript rendering and once with it.
5. robots.txt and any bot rules at the CDN or WAF.
6. How routing, 404s, redirects and metadata are handled in the app.

## Workflow

### Step 1. Compare raw and rendered HTML per template
For each sample URL, record what's present in view-source versus the rendered DOM:

| Element | Raw HTML | Rendered | Notes |
|---|---|---|---|
| Title and meta description | | | |
| Canonical and meta robots | | | Must match in both |
| H1 and main body copy | | | |
| Internal links (count) | | | |
| Structured data | | | |
| Images and alt text | | | |

A quick command-line check:
```bash
curl -s https://example.com/product/blue-widget/ | grep -ciE "<a [^>]*href="
curl -s https://example.com/product/blue-widget/ | grep -iE "<title>|rel=\"canonical\"|name=\"robots\""
```

### Step 2. Check Google's view
Use URL Inspection's live test: look at the rendered HTML, the screenshot and "More info" for blocked resources and console errors. Confirm the main content and links appear.

### Step 3. Check links and routing
Every navigational element should be an `<a>` with an `href`. Client-side routing must use the History API with clean paths, not `#!` or `#/` fragments. Pagination, filters you want indexed and "load more" content need real URLs that work when loaded directly.

### Step 4. Check status codes and metadata per route
Request a non-existent route and confirm the server returns 404. Check redirects are server-side. Confirm titles, canonicals and robots tags are generated on the server for each route, not set once for the whole app.

### Step 5. Check lazy loading and user-triggered content
Content behind tabs and accordions is fine if it's in the HTML. Content fetched only on scroll, click or hover is often missed. Use native `loading="lazy"` for below-the-fold images and IntersectionObserver for components, and keep the main content out of lazy loading.

### Step 6. Check resource access
robots.txt must not block JavaScript, CSS or API endpoints needed to render public pages. WAF and bot rules must not challenge Googlebot or block the rendering service.

### Step 7. Recommend a rendering strategy per template
Move public, search-facing templates to SSG, ISR or SSR. Keep CSR for logged-in areas. Use dynamic rendering only as a temporary bridge with a removal date.

## AI crawler visibility
Practitioner tests indicate that most AI crawlers (for example GPTBot, ClaudeBot and PerplexityBot) fetch HTML but don't run JavaScript. Treat that as consensus rather than documented fact, and re-check it for the crawlers you care about. The practical rule is the same as for Google: if it matters, put it in the server HTML. Pair this with `robots-directives` to decide which AI crawlers you allow.

## Framework notes
- **Next.js (App Router):** server components render on the server by default; keep main content out of components marked `"use client"` that fetch data in the browser. Use the Metadata API for titles and canonicals, `notFound()` for real 404s, and `redirect()` or `next.config.js` redirects for moves.
- **Nuxt:** use SSR or hybrid rendering with route rules; set head tags with `useHead` or `useSeoMeta` on the server.
- **Angular:** use server-side rendering and hydration rather than serving a bare client app.
- **React SPA (Vite or Create React App):** consider moving public pages to a framework with SSR or prerendering; a pure SPA struggles to return correct status codes.
- **Headless WordPress or other CMS:** confirm the front end renders all CMS fields (body, FAQs, schema) on the server, not only the hero.

## Worked example
An online marketplace rebuilt its listing pages as a React SPA. Within a month, indexed listing URLs fell from 42,000 to 9,000 (Search Console Page indexing). View-source showed an empty `<div id="root">`, links between listings were buttons with click handlers, and unknown listings returned 200 with a "not found" message. The fix moved listings to SSR with real `<a href>` links and server 404s. Indexed listings recovered to 38,000 over ten weeks, and soft 404 reports stopped growing.

## Ticket template for developers
```
Title: [SEO] Server-render listing pages and return real 404s
Why: listing body and links are missing from raw HTML (view-source, 5 of 5 samples); unknown IDs return 200.
Change: render listing content, title, canonical and links on the server; replace button navigation with <a href>; return 404 for unknown IDs.
Acceptance tests:
- curl of 5 listing URLs shows H1, description and 20+ internal <a href> links.
- /listing/does-not-exist/ returns HTTP 404.
- URL Inspection live test shows full content in rendered HTML and screenshot.
Metric: indexed listing URLs and soft 404 count, re-check weekly for 8 weeks.
```

## Output format
```
# JavaScript SEO review: <site>, <date>
## Summary
## Raw versus rendered comparison by template
## Findings
| # | Issue | Template | Evidence | Impact | Effort | Fix |
## Recommended rendering strategy per template
## Acceptance tests and re-check dates
```

## Common mistakes
- Testing only in a browser, where everything looks fine.
- `noindex` in raw HTML that JavaScript later removes.
- Canonicals or titles that differ between raw and rendered HTML.
- Navigation built from buttons or `onclick` handlers.
- Blocking `/api/` or script folders in robots.txt.
- Infinite scroll with no paginated URLs.
- Relying on dynamic rendering for years.

## Questions to ask before you recommend changes
- Which routes are public and need search traffic, and which are logged-in?
- Can the team move public templates to server or static rendering, and how long would it take?
- Which crawlers matter beyond Google: Bing, AI answer engines, social previews?

## Self-check before you answer
- Did I compare raw HTML, rendered DOM and Google's rendered view for each template?
- Are links, status codes and metadata correct without JavaScript?
- Did I label AI-crawler behaviour as consensus, not documented fact?
- Does every fix have a test a developer can run?

## Official documentation
- Understand JavaScript SEO basics: https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics
- Fix Search-related JavaScript problems: https://developers.google.com/search/docs/crawling-indexing/javascript/fix-search-javascript
- Dynamic rendering as a workaround: https://developers.google.com/search/docs/crawling-indexing/javascript/dynamic-rendering
- Lazy-loaded content: https://developers.google.com/search/docs/crawling-indexing/javascript/lazy-loading
- Make links crawlable: https://developers.google.com/search/docs/crawling-indexing/links-crawlable
- URL Inspection tool: https://support.google.com/webmasters/answer/9012289

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/javascript-seo/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

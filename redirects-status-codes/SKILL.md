---
name: redirects-status-codes
description: "Audit and fix redirects (301, 302, 307, 308, meta refresh and JavaScript), redirect chains and loops, 404 and 410 handling, soft 404s, 5xx errors and 503 maintenance responses, build and test redirect maps, and write rules for Apache, Nginx, Cloudflare, Vercel, Netlify and WordPress. Use whenever someone mentions broken links, 404 errors, 'Page with redirect', 'Soft 404', 'Server error (5xx)' or 'Redirect error' in Search Console, redirect chains, changing or deleting URLs, expired products, maintenance windows, or asks which redirect type to use."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/redirects-status-codes/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Redirects and HTTP Status Codes

Every URL answers a crawler with a status code before it shows any content. That number decides whether the page is indexed, dropped, merged into another URL or retried later. Get it wrong and you lose link value to chains, keep dead pages in the index, or teach Googlebot to crawl your site less.

This skill picks the right code for each situation, collapses chains to one hop, turns old URLs into a tested redirect map and writes the rules for the server or platform that serves them.

## Operating principles
1. **One hop to a 200.** Every redirect lands on a live, indexable page in a single step.
2. **Relevance over volume.** Redirect to the closest equivalent page or not at all. A redirect to an unrelated page is treated as a soft 404.
3. **Your own links never redirect.** Navigation, canonicals, sitemaps and hreflang point at final URLs; redirects exist for the rest of the web.
4. **The status code must match the content.** An empty page that returns 200 is a soft 404; a real page that returns 404 to bots is an outage.
5. **Keep a record.** Each rule has a reason and a date, so nobody deletes a redirect that still carries backlinks.

## Which code to use
| Situation | Code | What Google does |
|---|---|---|
| Page moved for good | 301 or 308 | Strong signal that the target is canonical. 308 keeps the request method (matters for forms and APIs, not for SEO) |
| Temporary move: a test, a short promotion, one page under repair | 302 or 307 | Keeps the source URL as canonical for a while; a long-lived 302 is eventually treated like a 301 |
| Page gone, no replacement | 404 or 410 | Both drop the URL from the index; 410 says "gone on purpose" and may be processed slightly faster |
| Page gone, close replacement exists | 301 to that replacement | Passes signals when the target satisfies the same need |
| Whole site or section briefly down | 503 with `Retry-After` | Googlebot comes back later; days of 5xx lead to reduced crawling and dropped URLs |
| Too many requests from one client | 429 | Treated like a server error for crawling; Googlebot slows down |
| Content behind login | 401 or 403 | Not indexed. Don't use 403 to slow down Googlebot; use 503 or 429 for short periods |
| Thin or empty page returning 200 | Fix the soft 404 | Return 404 or 410, or give the page real content |

Meta refresh and JavaScript redirects are followed by Google, but they are slower, depend on rendering and are often ignored by other crawlers. Use server-side redirects unless the platform gives you no other choice. A zero-second meta refresh is treated as permanent; a delayed one as temporary.

## When to use, and when not to
Use it for 404 clean-ups, redirect chains found in a crawl, URL changes on single pages or sections, expired products and listings, soft 404 and 5xx reports in Search Console, maintenance planning, and writing or reviewing redirect rules.

Don't use it for a full domain, CMS or URL-structure move (`site-migration`), for choosing between duplicate URLs that both stay live (`canonicalization`), or for parameter and filter URLs (`faceted-navigation-parameters`).

## Inputs to ask for, in one message
1. A recent crawl with status codes, redirect chains, final URLs and inlinks.
2. Search Console Page indexing examples for "Not found (404)", "Soft 404", "Server error (5xx)", "Redirect error" and "Page with redirect".
3. Backlink data for the affected URLs (Ahrefs, Semrush, Majestic or Search Console Links).
4. Clicks and conversions per URL for the last 12 months from Search Console and analytics.
5. Where redirects live today: server config, CDN, hosting platform, CMS plugin, application code.
6. Server or CDN logs if 5xx or timeout problems are suspected.

## Workflow

### Step 1. Inventory every redirect source
List each layer that can issue a redirect: CDN, load balancer, web server, framework, CMS, plugin. Chains usually come from two layers doing the same job, such as the CDN forcing HTTPS and the CMS adding a trailing slash.

### Step 2. Map chains and loops
From the crawl, export every redirect with its hop count and final status. Flag:
- chains of two or more hops;
- loops (A to B to A);
- redirects that end on 404, 5xx, noindex or a canonicalised URL;
- temporary codes used for permanent moves.
Collapse each chain so every source points straight at the final URL. Google follows up to 10 hops, but each hop costs crawl time, and users on slow mobile connections drop off.

### Step 3. Fix internal references
Update navigation, body links, canonicals, sitemaps, hreflang and structured data so none of them point at a redirecting URL. This is usually the biggest win, because it removes the chain at its source.

### Step 4. Triage 404s by value
A 404 deserves a redirect only if it has backlinks, recent traffic, conversions or internal links you can't remove. Everything else can stay 404. Thousands of 404s for URLs that never existed (spam links, old scrapers) are normal and don't hurt the site.

### Step 5. Fix soft 404s
Common causes: empty category or search pages, "product unavailable" templates with no content, error messages served with 200, and redirects to the homepage. Return a real 404 or 410, or add useful content and links.

### Step 6. Investigate 5xx and timeouts
Line up Search Console Crawl stats (host status, response codes by day) with server or CDN logs. Patterns to look for: errors at the same hour every day (backups, cron jobs), errors only for Googlebot (WAF or bot rules), errors on uncached pages (origin overload). Fix the cause; don't block Googlebot to hide it.

### Step 7. Write, test and record the rules
Put rules in one layer, as high up as possible (CDN or server rather than plugin). Test every source URL after deploy. Save the map with reasons and dates. Keep permanent redirects for at least a year, and for as long as the source URL still earns links or visits.

## Building a redirect map
One row per source URL with these columns: source, target, code, reason, backlinks, clicks (12 months), owner, date added. Fill it in this order:

1. **Pattern rules.** Paths that follow a rule, such as `/blog/2023/05/post-name/` to `/blog/post-name/`. One regex rule covers thousands of rows; test it against a sample first.
2. **High-value URLs by hand.** The top few hundred URLs by backlinks, clicks or revenue get a one-to-one target chosen and checked by a person.
3. **Matching for the long tail.** Match old and new crawls on title, H1, product ID or SKU, then review a random sample.
4. **No equivalent.** Mark for 410 or 404 with a reason. Never send these to the homepage.

## E-commerce and listings decisions
| Situation | Treatment |
|---|---|
| Temporarily out of stock | Keep the page live with 200, show availability and alternatives |
| Discontinued, successor product exists | 301 to the successor |
| Discontinued, no successor, page has links or traffic | 301 to the closest category, or keep the page with alternatives |
| Discontinued, no links or traffic | 410 |
| Expired listing, job or event | 404 or 410, or keep an archive page if people still search for it |
| Seasonal page | Keep the same URL every year with 200; update the content |

## Rule snippets
```nginx
# Nginx: one permanent redirect
location = /old-page/ { return 301 https://www.example.com/new-page/; }

# Nginx: many redirects from a map file ("/old/ /new/;" per line)
map $uri $redirect_to { include /etc/nginx/redirects.map; }
server {
  if ($redirect_to) { return 301 $redirect_to; }
}
```
```apache
# Apache .htaccess
Redirect 301 /old-page/ https://www.example.com/new-page/
RewriteEngine On
RewriteCond %{HTTP_HOST} ^example\.com$ [NC]
RewriteRule ^(.*)$ https://www.example.com/$1 [R=301,L]
# Gone on purpose
Redirect gone /discontinued-range/
```
```json
// vercel.json
{ "redirects": [ { "source": "/old-page", "destination": "/new-page", "permanent": true } ] }
```
```text
# Netlify _redirects
/old-page   /new-page   301
/blog/*     /articles/:splat   301
```
Cloudflare: use Bulk Redirects for long lists of exact URLs and Single Redirects for patterns. Next.js: `redirects()` in `next.config.js` with `permanent: true` returns 308.

## Testing redirects
Run the source column through a crawler in list mode. Every row should return one 301 or 308 to a URL that returns 200. For spot checks:

```bash
curl -sIL https://example.com/old-page/ | grep -iE "^HTTP|^location"
# Test as Googlebot too, in case the CDN treats bots differently
curl -sIL -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" https://example.com/old-page/ | grep -iE "^HTTP|^location"
```

More than one redirect line means a chain. Re-test after every deploy that touches routing, the CDN or the CMS, and after any WAF or bot-management change.

## Platform notes
- **WordPress:** keep redirects in one place: the server, a redirect plugin or the SEO plugin's manager, not all three. WordPress guesses redirects for old slugs and mistyped URLs; don't rely on that for planned changes. Changing a slug doesn't always create a redirect, so check after every change.
- **Shopify:** URL redirects live under Navigation; deleted products don't redirect by default. Shopify can't serve 410, so redirect valuable URLs and let the rest return 404.
- **Webflow:** 301 redirects are set in hosting settings and support wildcard patterns with capture groups; test after each publish.
- **Headless and JavaScript frameworks:** make sure unknown routes return a real 404 status from the server, not a client-rendered "not found" message with 200.

## Worked example
An agency site removed 120 service pages in a redesign. Search Console listed 340 "Not found" URLs; backlink data showed 28 of them had links from other sites. The plan: redirect those 28 to the closest current service pages, move 61 old blog URLs to the new folder with one pattern rule, update 214 internal links that still pointed at old URLs, and leave the low-value pages as 404. The CDN and the CMS were both adding trailing slashes, which created a two-hop chain on every old URL; removing the CMS rule fixed all of them at once. The next crawl showed no chains and no internal links to non-200 URLs.

## Ticket template for developers
```
Title: [SEO] Collapse redirect chains and fix internal links to redirects
Why: 1,840 internal links point to redirecting URLs; 312 redirects have 2+ hops (crawl, 9 Oct 2026).
Change: remove the CMS trailing-slash redirect (CDN rule already handles it); update listed sources to final URLs; import redirect map rows 1-412 at the CDN.
Acceptance tests:
- Each source in redirect-map.csv returns one 301 to a 200 URL (list-mode crawl attached).
- Crawl shows 0 internal links to 3xx, 4xx or 5xx URLs.
- /missing-test-url/ returns 404, not 200.
Metric: Search Console "Page with redirect" and "Not found (404)" counts, re-check in 4 weeks.
```

## Output format
```
# Redirect and status code audit: <site>, <date>
## Summary (three sentences: biggest risk, biggest win, effort)
## Findings
| # | Issue | Count | Example URL | Evidence | Impact | Effort | Fix |
## Redirect map (CSV): source, target, code, reason, backlinks, clicks, owner, date added
## Rules to deploy (by layer) and test plan
## Re-check date and metrics to watch
```

## Common mistakes
- Redirecting every removed page to the homepage.
- Leaving 302s in place for permanent moves.
- Custom 404 pages that return a 200 status.
- Redirect rules that drop or duplicate query strings.
- Removing redirects after a few months while old URLs still have backlinks.
- Stacking plugin, server and CDN rules that create chains.
- Blocking or rate-limiting Googlebot with 403 during a traffic spike.

## Questions to ask before you recommend changes
- Which layer should own redirects from now on, and who can change it?
- Which removed URLs still earn links, traffic or revenue?
- Is any 5xx pattern tied to deployments, backups or bot rules?
- Are there legal or business reasons to keep a page live rather than redirect it?

## Self-check before you answer
- Does every redirect in my plan reach a 200 in one hop?
- Did I choose targets by relevance, not convenience?
- Did I fix internal links and other references, not only the redirects?
- Does every change have an acceptance test, an owner and a re-check date?

## Official documentation
- Redirects and Google Search: https://developers.google.com/search/docs/crawling-indexing/301-redirects
- How HTTP status codes and network errors affect Google Search: https://developers.google.com/search/docs/crawling-indexing/http-network-errors
- Soft 404 errors: https://developers.google.com/search/docs/crawling-indexing/http-network-errors#soft-404-errors
- Crawl Stats report: https://support.google.com/webmasters/answer/9679690
- MDN HTTP response status codes: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/redirects-status-codes/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

---
name: crawlability-indexation-audit
description: "Diagnose why pages are not being crawled, rendered or indexed by Google and Bing, and fix the earliest broken step first. Covers every Search Console Page indexing status ('Crawled, currently not indexed', 'Discovered, currently not indexed', soft 404, duplicate without user-selected canonical, Google chose different canonical, blocked by robots.txt, excluded by noindex, redirect, 5xx), crawl stats and host status, bot blocking at the CDN or WAF, index bloat and new-page discovery. Use whenever someone says pages aren't in Google, indexed pages dropped, new pages take weeks to appear, Search Console shows indexing errors, a sitemap is 'submitted but not indexed', or they ask how to get pages indexed faster, even if they only paste a Search Console screenshot."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/crawlability-indexation-audit/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Crawlability and Indexation Audit

A page earns traffic only after it passes five gates: Google discovers the URL, crawls it, renders it, indexes it and chooses to show it. Most indexing advice jumps straight to "request indexing" or "add more content". This skill finds the exact gate where each group of URLs fails, proves it with evidence, and fixes the earliest failure first, because a fix at gate four is wasted if gate two is broken.

Work like a senior technical SEO who has to defend every recommendation to an engineering lead: specific, sourced, ranked, and honest about what the data can't show.

## Operating principles
1. **Group before you diagnose.** Never reason from one URL. Group by Search Console reason and by template, then sample.
2. **Earliest gate wins.** Discovery problems come before crawl problems, crawl before render, render before index.
3. **Separate healthy exclusions from real problems.** Redirects, intentional noindex pages and correctly canonicalised alternates are supposed to be excluded. Counting them as errors scares stakeholders for nothing.
4. **"Not indexed" is often a decision, not a bug.** Google chooses not to index pages it sees as duplicate, thin or low value. Technical fixes don't overrule that; better pages and better linking do.
5. **Label your evidence.** Every finding is Confirmed (official documentation), Observed (this site's data) or Inferred (likely, not proven).

## When to use, and when not to
Use it for indexing drops, slow discovery of new pages, growing exclusion counts, sitemaps with a big submitted-versus-indexed gap, post-launch and post-migration checks, and any time Search Console's Page indexing report looks wrong.

Don't use it for ranking drops on pages that are still indexed (start with `technical-seo-audit`), for copy problems on one page, or for link building.

## Inputs to ask for, in one message
1. The domain, the Search Console property type (domain or URL-prefix) and the templates that make money.
2. The Page indexing report: the "Why pages aren't indexed" table, with example URLs exported for each reason.
3. Crawl stats (Settings, Crawl stats): requests per day, response codes, file types, purpose (discovery versus refresh), host status and average response time.
4. A crawl export with status code, indexability, canonical, meta robots, X-Robots-Tag, inlinks and crawl depth.
5. XML sitemaps, and the date of any release, migration, CMS plugin change, CDN or security change in the last 90 days.
6. Server or CDN logs if you have them (route deep crawl questions to `log-file-analysis`).

If you get none of this, audit what is public (robots.txt, sitemaps, live HTTP responses, rendered HTML through the Rich Results Test) and say clearly which conclusions would change with Search Console data.

## The workflow

### Step 1. Build the reason by template matrix
Put every excluded URL into a table: reason, template, count, share of that template. One reason concentrated on one template almost always means one root cause and one ticket.

| Reason | Template | URLs | Share of template | Healthy or problem? |
|---|---|---|---|---|
| Crawled, currently not indexed | /blog/tag/ | 3,120 | 96% | Problem, but likely intended to be excluded |
| Discovered, currently not indexed | /listings/city/category/ | 41,800 | 74% | Problem |
| Page with redirect | old /product/ URLs | 2,040 | n/a | Healthy if links and sitemaps are clean |

### Step 2. Check host health before anything else
Open Crawl stats, then Host status. Three things matter:
- **robots.txt fetch:** if Google can't fetch robots.txt because of 5xx errors or timeouts, it may slow or stop crawling the whole host.
- **DNS resolution and server connectivity:** spikes here mean Googlebot couldn't reach you at all.
- **Average response time:** HTML responses consistently above roughly one second reduce how much Google is willing to crawl. Look at caching, hosting and database load.

If host status shows problems in the last 90 days, fix them first and note the dates. Indexing trends often follow them by days or weeks.

### Step 3. Work each status with the right test

| Status | What it usually means | How to confirm | Typical fix |
|---|---|---|---|
| Blocked by robots.txt | A Disallow rule matches | Test the URL against the live file in the robots.txt report | Narrow the rule, or keep it if the URLs shouldn't be crawled (`robots-directives`) |
| Excluded by noindex | Meta robots or X-Robots-Tag noindex | View raw HTML and response headers | Remove from pages that should rank; check CMS defaults and staging leftovers |
| Not found (404) | URL missing | Status check | Redirect only if it has links or traffic (`redirects-status-codes`) |
| Soft 404 | 200 status with empty or error-like content | Look at the rendered page | Return 404 or 410, or add real content |
| Server error (5xx) | Origin or CDN failing for Googlebot | Logs at crawl times, uptime monitor | Fix capacity, rate limits, timeouts |
| Duplicate without user-selected canonical | Near-identical pages, no canonical | Compare main content | Self-canonicals and consolidation (`canonicalization`) |
| Duplicate, Google chose different canonical than user | Your signals disagree | URL Inspection shows both canonicals | Align links, sitemaps, redirects, hreflang |
| Alternate page with proper canonical tag | Working as intended | None needed | Report as healthy |
| Page with redirect | URL redirects | Status check | Remove from sitemaps and internal links |
| Discovered, currently not indexed | Known but not crawled yet | Logs show few or no Googlebot hits | Crawl demand and capacity: links, sitemap hygiene, URL bloat, speed |
| Crawled, currently not indexed | Crawled and declined | Compare with indexed siblings | Quality, uniqueness, duplication, internal links |

### Step 4. Inspect a sample per group
For three to five URLs in each problem group, run URL Inspection with a live test and record: crawl allowed, indexing allowed, user-declared canonical, Google-selected canonical, last crawl date, and whether the rendered HTML contains the main content and links. For more than a handful of URLs, use the URL Inspection API, which has a daily quota per property.

### Step 5. Measure index bloat
Compare three numbers: URLs you want indexed (your list or clean sitemaps), URLs Google has indexed, and URLs Google knows about. If indexed or known URLs far exceed what you want indexed, the surplus (filters, tags, internal search, session IDs, printer pages, staging hosts) competes for crawl and drags down Google's view of site quality. Route faceted URL spaces to `faceted-navigation-parameters`.

### Step 6. Check bot access at the edge
CDNs and security tools sometimes challenge, rate-limit or block Googlebot and Bingbot, especially after a new rule or a traffic spike. Check WAF events and bot-management logs for crawler user agents, and verify real Googlebot by reverse DNS (the hostname ends in googlebot.com, google.com or googleusercontent.com and resolves back to the same IP) or against Google's published IP ranges. Never trust the user agent string alone.

### Step 7. Diagnose discovery for new pages
For recently published URLs, measure time to first crawl and time to index. Slow discovery usually comes from weak internal links (new pages reachable only through deep pagination), sitemaps that update late or carry inaccurate `lastmod`, or crawl being spent on low-value URLs elsewhere.

### Step 8. Write the fix list, earliest gate first
Each fix gets an owner, an effort estimate, the evidence, and the metric and date to re-check.

## Decision rules
- **Never block and noindex the same URL.** If robots.txt blocks a URL, Google can't see its noindex, and the URL can stay indexed without a snippet.
- **To remove a page from search, use noindex (crawlable) or a 404 or 410.** robots.txt controls crawling, not indexing.
- **Don't bulk-request indexing.** Request indexing in URL Inspection is for a few important URLs after a real fix. It has a daily limit and doesn't override a quality decision.
- **The Indexing API is not a shortcut.** Google supports it only for job posting and livestream pages. Using it for anything else is against its documented purpose.
- **New pages that matter** get a link from an already-crawled, high-traffic page on day one, an accurate `lastmod` in the sitemap, and a place in the hub for their topic.
- **For Bing and other engines that support it,** IndexNow notifies them of new and changed URLs within minutes. Google doesn't use IndexNow.
- **"Crawled, currently not indexed" on unique, useful pages:** strengthen internal links and the page itself, then wait for recrawl. On thin or duplicate pages: noindex, consolidate or remove.

## Worked example
A directory site shows 42,000 URLs as "Discovered, currently not indexed". The matrix puts 39,000 of them on one template: city and category combinations. A crawl shows 81% of those pages list fewer than three businesses, and the only links to them come from paginated archives eight or more clicks deep. Logs confirm Googlebot hit fewer than 2% of them in 30 days.

The diagnosis is Observed, not guessed: low crawl demand caused by thin pages and weak links. The plan: noindex and remove from sitemaps every combination with fewer than three listings; link the strongest 2,000 combinations from city hub pages; write a short, specific introduction for each; segment sitemaps by city so indexing can be watched per batch. The success metric is the share of indexed combination pages that receive impressions, checked at 4 and 12 weeks.

## Platform notes
- **WordPress:** check Settings, Reading, "Discourage search engines" after every launch. Check your SEO plugin's noindex defaults for tags, author archives, attachment pages and custom post types.
- **Shopify:** duplicate product paths under collections and filtered collection URLs inflate "Duplicate" and "Discovered" counts. Make internal links point to `/products/` URLs.
- **Headless and JavaScript sites:** if the raw HTML is an empty shell, gate three (render) is the likely failure. Route to `javascript-seo`.
- **Large sites:** segment sitemaps by template so Search Console reports indexing per section (`xml-sitemaps`).

## Ticket template for developers
```
Title: [Indexing] Return 404 for empty city/category pages
Why: 39,000 thin URLs are crawled rarely and dilute site quality (Search Console Page indexing, export 9 Oct 2026).
Change: Pages with 0 listings return HTTP 404. Pages with 1-2 listings output <meta name="robots" content="noindex,follow"> and are removed from sitemap-cities-*.xml.
Acceptance tests:
- /listings/lagos/plumbers/ with 0 listings returns 404.
- A page with 2 listings contains the noindex tag in raw HTML and is absent from sitemaps.
- Pages with 3+ listings are unchanged and self-canonical.
Metric to watch: indexed share of combination pages with impressions, re-check 6 Nov 2026.
```

## Output format
```
# Indexation diagnosis: <site>, <date>
## Summary (three sentences: biggest blocker, how many URLs, the first fix)
## Pipeline view
| Template | Known | Crawled (30 days) | Indexed | Main blocker | Gate |
## Findings
| # | Reason | Template | URLs | Evidence | Label | Root cause | Fix | Owner | Effort | Re-check date |
## Healthy exclusions (no action)
## Validation plan (sample URLs to re-inspect, when to click Validate fix)
```

## Common mistakes
- Treating every excluded URL as an error.
- Blocking URLs in robots.txt to "deindex" them.
- Using the Removals tool as a permanent fix; it hides URLs for about six months.
- Clicking Validate fix before the change is live on every affected URL.
- Calling "Crawled, currently not indexed" a technical bug without comparing the pages with indexed siblings.

## Questions to ask before you recommend anything
- Which URLs must be indexed for the business to work, and which must never be?
- What changed in the last 90 days, with dates?
- Who can change templates, robots rules, sitemaps and CDN settings?

## Self-check before you answer
- Did I group by reason and template before diagnosing?
- Is every finding tied to a gate, evidence and a label?
- Did I separate healthy exclusions?
- Does every fix have an owner, a metric and a re-check date?
- Did I avoid promising that pages will be indexed or rank?

## Official documentation
- Page indexing report: https://support.google.com/webmasters/answer/7440203
- Crawl stats report: https://support.google.com/webmasters/answer/9679690
- URL Inspection tool: https://support.google.com/webmasters/answer/9012289
- How Google Search works: https://developers.google.com/search/docs/fundamentals/how-search-works
- Verifying Googlebot and other Google crawlers: https://developers.google.com/search/docs/crawling-indexing/verifying-googlebot
- Indexing API (job posting and livestream only): https://developers.google.com/search/apis/indexing-api/v3/quickstart
- IndexNow: https://www.indexnow.org/documentation

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/crawlability-indexation-audit/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

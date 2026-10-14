---
name: site-migration
description: "Plan, run and monitor SEO-safe website migrations: domain changes, HTTP to HTTPS, CMS re-platforming (for example WordPress to headless, Webflow to Strapi), URL restructures, redesigns, site mergers, subdomain to subfolder moves and international restructures, with benchmarks, redirect maps, pre-launch checks, launch-day tests and a 90-day recovery watch. Use whenever someone mentions a migration, relaunch, new website, rebrand, domain change, moving CMS, merging sites, a traffic drop after launch, or asks how to move a site without losing rankings."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/site-migration/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Site Migration SEO

A migration changes something search engines rely on to understand a site: the domain, the URLs, the templates, the rendering or the content. Done well, rankings dip briefly or not at all. Done badly, a site can lose half its organic traffic for months, and the cause is usually something a checklist would have caught: a missing redirect map, a staging noindex left in place, or content that never made it across.

This skill runs a migration as a project with four phases, each with gates that must pass before the next one starts.

## Operating principles
1. **Benchmark before you touch anything.** You can't prove what changed without a baseline.
2. **Change as little as possible at once.** A domain move plus a redesign plus a URL restructure multiplies risk; split them if the business allows.
3. **Every old URL has a planned outcome:** redirect to an equivalent, keep, or retire on purpose.
4. **Test on staging as a crawler would,** then test again on launch day in production.
5. **Watch for 90 days.** Most problems show up in the first two weeks; some take longer.

## Migration types and their risk
| Type | Main risk | Extra steps |
|---|---|---|
| HTTP to HTTPS | Mixed content, chains | Force HTTPS in one hop, update canonicals and HSTS when stable |
| Domain change or rebrand | Lost link equity, slow re-crawl | Change of Address tool, keep old domain registered and redirecting for years |
| CMS re-platform | URL pattern changes, missing metadata, rendering | Field-by-field metadata mapping, raw HTML checks |
| URL restructure | Unmapped URLs, broken internal links | Pattern rules plus manual mapping for top URLs |
| Redesign on same URLs | Lost content, changed internal links, slower templates | Content parity checks, Core Web Vitals comparison |
| Site merger | Cannibalisation, mismatched intent | Decide which page survives for each topic before mapping |
| Subdomain to subfolder | Server routing, analytics breaks | Reverse proxy testing, cross-domain tracking review |
| International restructure | Broken hreflang, wrong country targeting | New hreflang sets, sitemap per locale |

## When to use, and when not to
Use it for any change of domain, platform, URL structure or template set that affects more than a handful of pages, and for diagnosing a traffic drop after a launch.

Don't use it for a few individual URL changes (`redirects-status-codes`) or for a design refresh that keeps URLs, content and rendering identical, which only needs a speed and content check.

## Inputs to ask for, in one message
1. What is changing (domain, CMS, URLs, templates, content, hosting) and the planned launch date.
2. Access to Search Console for old and new properties, and analytics.
3. A full crawl of the current site, plus the list of URLs from sitemaps, analytics landing pages, Search Console Performance and backlink tools (crawls alone miss orphan pages).
4. Staging URL and how it is protected (password, IP allow list or noindex).
5. The development team's release process: who deploys redirects, how fast fixes can ship after launch.
6. Business priorities: the pages and sections that make the most money.

## Phase 1. Benchmark (four or more weeks before launch)
- Export 16 months of Search Console clicks and impressions by page and query, and analytics organic sessions and conversions by landing page.
- Record rankings for a tracked keyword set covering every key template.
- Save a full crawl: status, title, meta description, H1, canonical, meta robots, hreflang, structured data types, word count, internal inlinks.
- Record Core Web Vitals field data per template.
- Export backlinks by target URL.
- Take raw HTML copies of five sample URLs per template.

## Phase 2. Plan and build (staging)

### Redirect map
Combine every known old URL from crawls, sitemaps, analytics, Search Console and backlink tools, then de-duplicate. Map in this order: pattern rules, hand-checked top URLs by value, matched long tail, and deliberate 404 or 410 for pages with no equivalent. See `redirects-status-codes` for the map format and testing.

### Parity checks, old versus new
| Element | Check |
|---|---|
| Content | Main copy, headings and FAQs carried across; no template drops body text |
| Metadata | Titles, descriptions, canonicals, meta robots and hreflang mapped field by field |
| Structured data | Same types and properties, still valid |
| Internal links | Navigation, footer and in-content links point to new final URLs; key pages keep similar click depth |
| Rendering | Main content and links present in raw HTML (see `javascript-seo`) |
| Speed | Core Web Vitals in lab tests equal or better per template |
| Media | Image URLs and alt text carried across; image redirects for images with traffic |
| Analytics | Tags, conversion events and consent behaviour work on every template |

### Staging protection
Password or IP restriction is safest. If staging uses noindex or a robots.txt disallow, write down exactly where it is set, because removing it is a launch-day step that is often forgotten.

### Go or no-go gate
Launch only when the redirect map is complete for all valuable URLs, parity checks pass on every template, and the team can deploy fixes within a day of launch.

## Phase 3. Launch day
1. Remove staging protection; confirm production robots.txt allows crawling and no template outputs noindex.
2. Deploy redirects; test the full source list in a crawler's list mode.
3. Crawl the new site: status codes, canonicals, hreflang, internal links to redirects.
4. For a domain change: verify the new property and submit the Change of Address in Search Console for the old property.
5. Submit new XML sitemaps. Keep a sitemap of old URLs live for a few weeks so Google re-crawls them and finds the redirects faster.
6. Inspect five URLs per template with URL Inspection and request indexing for the most important ones.
7. Check analytics is recording traffic and conversions.

## Phase 4. Monitor (90 days)
| When | Check |
|---|---|
| Daily, week 1 | 404 and 5xx in logs and Search Console, indexing of key pages, redirect errors |
| Weekly, weeks 2 to 6 | Clicks and impressions by template versus benchmark, Page indexing report, ranking set |
| Weeks 6 to 12 | Old URLs dropping from the index, new URLs gaining impressions, backlinks updated for top referrers |

A dip in the first two to four weeks is common while Google processes redirects. A drop that keeps growing after week three, or that hits one template only, points to a specific fault: missing redirects, lost content, noindex, or rendering.

## Diagnosing a post-launch drop
1. Compare clicks by template and by page between benchmark and now; find where the loss sits.
2. For the worst pages, check status, redirect target, canonical, meta robots and raw HTML content.
3. Check whether old URLs still rank (redirects not found yet) or have vanished (redirects missing or wrong).
4. Compare titles, headings and body copy with the benchmark crawl.
5. Check internal links and click depth for the affected section.
6. Rule out unrelated causes: a Google update on the Search Status Dashboard, seasonality, tracking changes.

## Worked example
A fintech blog moved from WordPress to a headless CMS with a Next.js front end, keeping the same URLs. Two weeks after launch, blog clicks were down 35% while product pages were stable. The benchmark crawl showed 1,400 to 2,000 words per post; the new raw HTML contained only the first paragraph, because the article body was fetched in the browser. Moving the body into server-side rendering restored the content in raw HTML, and blog clicks returned to the benchmark within five weeks. The lesson: a template that looks right in a browser can still be empty to crawlers.

## Communication plan
- Tell stakeholders to expect a short dip and agree what level of loss triggers a rollback discussion.
- Agree a freeze on other big changes for two weeks either side of launch.
- Share a daily status note in week one: what was checked, what broke, what was fixed.

## Output format
```
# Migration plan: <site>, <type>, launch <date>
## Scope and risk summary
## Benchmark snapshot (where it is saved)
## Redirect map status (rows mapped, rows retired, rows pending)
## Parity check results by template
## Launch-day runbook with owners and times
## Monitoring schedule and rollback triggers
```

## Common mistakes
- No benchmark, so nobody can prove where traffic went.
- Redirect maps built only from a crawl, missing orphan and backlinked URLs.
- Staging noindex or robots.txt disallow copied to production.
- Redirecting everything to the homepage or to one category.
- New templates that drop body copy, FAQs or internal links.
- Letting the old domain expire or removing redirects after a few months.
- Launching on a Friday or just before a peak sales period.

## Questions to ask before you recommend changes
- Can the domain, platform and URL changes be split into separate releases?
- Who owns redirects after launch, and how fast can they ship a fix?
- Which pages make the most money, and have they been checked by hand?
- What drop would make the business roll back, and is rollback possible?

## Self-check before you answer
- Is there a saved benchmark for traffic, rankings, crawl data and speed?
- Does every valuable old URL have a tested destination?
- Did I check content and metadata parity per template, not only redirects?
- Is there a launch-day runbook and a 90-day monitoring plan with owners?

## Official documentation
- Site moves with URL changes: https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes
- Site moves without URL changes (hosting changes): https://developers.google.com/search/docs/crawling-indexing/site-move-no-url-changes
- Change of Address tool: https://support.google.com/webmasters/answer/9370220
- Redirects and Google Search: https://developers.google.com/search/docs/crawling-indexing/301-redirects
- Google Search Status Dashboard: https://status.search.google.com/

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/site-migration/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

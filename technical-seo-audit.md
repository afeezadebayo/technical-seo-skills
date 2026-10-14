---
name: technical-seo-audit
description: "Run a complete, prioritised technical SEO audit of any website, from crawl and indexation through rendering, speed, structured data, internal links and AI crawler access, and return a scored action plan. Use this whenever someone asks for an SEO audit, site health check, technical review, 'why isn't my site ranking', 'why did traffic drop', a pre-launch SEO check, or a due-diligence review of a domain, even if they only paste a URL and say 'check this'. It routes to the specialist technical SEO skills for depth."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/technical-seo-audit/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Technical SEO Audit

You are a senior technical SEO running an audit that a CTO, a content lead and a founder can all act on. The aim is not a long list of warnings; it is the shortest list of changes that will move crawling, indexing and rankings, ranked by impact and effort.

## When to use
- A full or partial technical review of a site, a section, or a staging build.
- Diagnosing a traffic or indexation drop.
- Pre-launch, pre-migration or post-migration checks.

Not for: a single page's copy (use an on-page skill), keyword research, or link building.

## Inputs to ask for (ask once, in one message)
1. The domain and the sections that make money (sign-up pages, product pages, money pages).
2. Platform and rendering: CMS, framework, CDN, whether pages render server-side.
3. Access: Google Search Console (Pages, Sitemaps, Crawl stats, Core Web Vitals reports), Bing Webmaster Tools, a crawl export (Screaming Frog, Sitebulb or similar), and server logs if available.
4. Recent changes: releases, migrations, redesigns, CMS plugin changes, with dates.
5. Business goal for the next 90 days.

If data is missing, audit what is public (robots.txt, sitemaps, rendered HTML, headers, PageSpeed Insights field data) and state clearly which findings would change with Search Console or logs.

## Workflow
Work through the layers in this order, because each one depends on the one before it. A page that cannot be crawled cannot be indexed; a page that is not indexed cannot rank.

1. **Baseline.** Record indexed page count (Search Console Pages report), organic clicks for the last 16 months, the top 20 landing pages, and dates of any drops. Compare drops with Google's Search Status Dashboard before blaming the site.
2. **Crawl access.** robots.txt, server response codes, crawl stats (host status, response times), WAF or bot-protection rules blocking Googlebot or Bingbot. Route depth to `crawlability-indexation-audit` and `robots-directives`.
3. **Indexation.** "Crawled, currently not indexed", "Discovered, currently not indexed", "Duplicate without user-selected canonical", soft 404s, `noindex` on pages that should rank. Route to `canonicalization`.
4. **Discovery.** XML sitemaps (only canonical, indexable, 200 URLs), orphan pages, click depth. Route to `xml-sitemaps` and `internal-linking`.
5. **URL hygiene.** Redirect chains and loops, 404s with links pointing at them, parameter and faceted URL explosion. Route to `redirects-status-codes` and `faceted-navigation-parameters`.
6. **Rendering.** Compare raw HTML with rendered HTML for the main templates (URL Inspection, Rich Results Test). Check that titles, canonicals, links and main content exist without user interaction. Route to `javascript-seo`.
7. **Page experience.** Field data (CrUX) for LCP, INP and CLS by template; mobile parity; HTTPS; intrusive interstitials. Route to `core-web-vitals`.
8. **Structured data.** Validity, eligibility for features Google still supports, consistency with visible content. Route to `structured-data`.
9. **AI and alternative crawlers.** Whether OAI-SearchBot, PerplexityBot, Bingbot and Google-Extended are allowed or blocked on purpose, and whether key content is in server-rendered HTML.
10. **Logs (if available).** Where Googlebot actually spends its time. Route to `log-file-analysis`.

Audit by template, not by URL. Sample 3 to 5 URLs per template (home, category, product or service, article, location, programmatic page), because a fix to one template fixes thousands of pages.

## Prioritisation
Score every finding:

| Score | Impact | Effort |
|---|---|---|
| High | Blocks indexing or ranking of money pages, or affects more than 20% of indexable URLs | Needs engineering sprint or vendor change |
| Medium | Wastes crawl, weakens signals, or affects a whole secondary template | A day or two of dev or CMS work |
| Low | Cosmetic, isolated URLs, or best practice with no measurable effect | Minutes in the CMS |

Order the plan: High impact and Low effort first, then High impact and High effort (scheduled), then Medium. Drop Low-impact items into an appendix rather than the main plan, so the team isn't distracted.

## Evidence rules
- Label each finding **Confirmed** (Google or Bing documentation says so), **Observed** (seen in this site's data) or **Inferred** (likely, not proven).
- Never say a fix "will" increase rankings. Say what it removes or enables, and the metric to watch.
- Quote numbers with their source and date range, for example "Search Console Pages report, 1 to 28 September".

## Output template
```
# Technical SEO audit: <domain>, <date>

## Summary
Three sentences: the state of the site, the biggest risk, the biggest opportunity.

## Scorecard
| Layer | Status (Good / Needs work / Critical) | Key finding |

## Action plan
| # | Finding | Evidence | Impact | Effort | Owner | Fix | Metric to watch |

## Template notes
One short section per template with sample URLs.

## Appendix
Low-priority items, raw counts, crawl settings used.
```

## The ten layers in detail
| Layer | What to check | Fastest evidence | Typical high-impact finding |
|---|---|---|---|
| 1. Crawl access | robots.txt, CDN and WAF rules, server errors to bots | robots.txt, Crawl stats host status, logs | A bot rule blocking Googlebot or AI search crawlers |
| 2. Indexation | Page indexing statuses, index bloat, noindex | Search Console Page indexing, crawl | Thousands of filter or tag URLs indexed |
| 3. Canonicalisation | Declared versus Google-selected canonical, duplicates | URL Inspection, crawl | Parameter versions chosen as canonical |
| 4. Status codes and redirects | Chains, loops, soft 404s, 5xx | Crawl, Search Console | Internal links pointing to redirects sitewide |
| 5. Rendering | Raw versus rendered HTML, JavaScript dependencies | View-source, URL Inspection live test | Main content or links missing from raw HTML |
| 6. Site architecture | Click depth, orphans, internal links to money pages | Crawl, sitemap, analytics | Key pages five or more clicks deep |
| 7. Sitemaps | Coverage, non-canonical entries, lastmod honesty | Sitemaps report, crawl | Sitemaps full of redirects and noindexed URLs |
| 8. Page experience | Core Web Vitals field data by template | Search Console CWV, PageSpeed Insights | LCP failing on the product template |
| 9. Structured data | Validity, policy compliance, duplicates | Rich Results Test, crawl extraction | Conflicting Organization or Product blocks |
| 10. AI crawler access | Search and user-triggered AI crawlers allowed and reaching content | robots.txt, logs, raw HTML | OAI-SearchBot or PerplexityBot challenged at the CDN |

Sample three to five URLs per template for every layer. Template-level findings matter more than single-URL findings because one fix reaches every page.

## Edge cases and judgement calls
| Situation | What to do |
|---|---|
| No Search Console access | Audit public signals (robots.txt, sitemaps, raw HTML, headers, PageSpeed field data) and label every indexing finding as inference |
| Very large site (over 1 million URLs) | Segment by template and section; sample, then confirm patterns with logs before recommending crawl-budget work |
| Traffic drop coincides with a Google update | Check the Search Status Dashboard dates, but rule out technical causes first; don't blame an update without evidence |
| Staging site audit | Expect noindex or authentication; audit rendering, templates and redirects, and list launch-day checks separately |
| Headless or JavaScript-heavy build | Treat rendering as the first suspect for indexing and AI visibility gaps |
| Conflicting data between tools | Prefer Search Console and server logs over third-party crawlers and estimates; note the conflict |
| Client wants a score | Give the layer scorecard, but lead with the three findings that matter most and why |

## Handoffs to specialist skills
The audit goes wide; specialists go deep. Route each finding with its evidence:
- Indexing statuses and bot blocking: `crawlability-indexation-audit`
- robots.txt, meta robots and AI crawler rules: `robots-directives`
- Sitemap coverage and segmentation: `xml-sitemaps`
- Duplicate URLs and canonical conflicts: `canonicalization`
- Chains, soft 404s and server errors: `redirects-status-codes`
- Upcoming relaunch or domain move: `site-migration`
- Failing Core Web Vitals: `core-web-vitals`
- Content missing from raw HTML: `javascript-seo`
- Markup errors and policy issues: `structured-data`
- Orphans and click depth: `internal-linking`
- Crawl waste and bot verification: `log-file-analysis`
- Filter and parameter bloat: `faceted-navigation-parameters`

## Quality bar: what an excellent audit includes
- A three-sentence summary a non-specialist can act on.
- Findings grouped by template, each with evidence, impact, effort and a named fix.
- A clear first week: the two or three changes that remove the biggest risks.
- Acceptance tests for every fix, so developers know when it's done.
- Metrics to watch and a re-check date for each fix.
- Honest limits: what the audit couldn't check and which data would change the conclusions.

## Common mistakes to avoid
- Reporting tool "errors" without checking whether they matter (a missing meta description is not a technical emergency).
- Treating "Crawled, currently not indexed" as a technical bug when it is often a quality or duplication signal.
- Recommending `noindex` and a robots.txt block on the same URL: Google can't see the `noindex` if it can't crawl the page.
- Auditing a staging site behind a login and assuming the live site matches.

## Presenting the audit to leadership
Lead with three numbers: indexed pages that should rank against those that do, the share of organic clicks on templates with critical issues, and the estimated effort of the top five fixes. Keep the full action plan as an appendix for the people who will do the work. Close with what you will measure and when you will report back.

## Quality check before you hand over
- Every finding has evidence, a label (Confirmed, Observed or Inferred), an owner and a metric.
- The plan starts with high-impact, low-effort fixes.
- Volatile facts (supported rich results, crawler names, thresholds) were checked against the official page on the date of the audit.
- Nothing in the report promises a ranking change.

## Official documentation to check (re-verify anything volatile)
- Google Search Essentials: https://developers.google.com/search/docs/essentials
- Search Console Page indexing report: https://support.google.com/webmasters/answer/7440203
- Crawl budget for large sites: https://developers.google.com/search/docs/crawling-indexing/large-site-managing-crawl-budget
- Google Search Status Dashboard: https://status.search.google.com/
- Bing Webmaster Guidelines: https://www.bing.com/webmasters/help/webmaster-guidelines-30fba23a

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/technical-seo-audit/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

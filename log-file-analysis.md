---
name: log-file-analysis
description: "Analyse server, CDN or hosting access logs to see how Googlebot, Bingbot and AI crawlers actually crawl a site: verifying real bots, crawl volume and frequency by template, crawl waste on parameters and redirects, status codes and response times served to bots, orphan URLs bots still request, new-page discovery speed, and AI crawler activity, with ready-to-run command-line and SQL queries. Use whenever someone mentions server logs, access logs, crawl budget, Googlebot activity, how often Google crawls, crawl waste, fake Googlebot, AI crawler traffic (GPTBot, ClaudeBot, PerplexityBot), Crawl Stats, or pages that are slow to be found."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/log-file-analysis/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Log File Analysis for SEO

Crawl tools show what a crawler could find; Search Console shows a sample of what Google reports. Server logs show what bots actually requested, when, and what your server answered. That makes them the most direct evidence for crawl waste, slow discovery, bot-only errors and how much attention AI crawlers pay to a site.

This skill collects the right logs, verifies which bots are real, segments requests by template and answers specific questions with numbers.

## Operating principles
1. **Start with a question.** "Why are new listings indexed slowly?" leads somewhere; "analyse the logs" rarely does.
2. **Verify bots before you count them.** User-agent strings are easy to fake.
3. **Segment by template and status.** Totals hide the story; product pages versus parameter URLs tells it.
4. **Join logs with crawl and sitemap data.** The value is in comparing what bots request with what you want crawled.
5. **Handle logs as personal data.** IP addresses can identify people; strip or hash user IPs that aren't bots, and follow your data policy.

## Questions logs can answer
| Question | Measure |
|---|---|
| Is Googlebot wasting crawls? | Share of verified Googlebot requests to parameters, redirects, 404s and non-indexable URLs |
| Are important pages crawled often enough? | Days since last crawl per template; share of sitemap URLs crawled in 30 days |
| How fast are new pages found? | Hours or days from publish time to first Googlebot request |
| Do bots get errors users don't? | 5xx, 429 and 403 rates for bots versus all traffic, by hour |
| Is the server slow for bots? | Response time percentiles for bot requests by template |
| Which old URLs do bots still hit? | Requests to URLs not in the current crawl or sitemap |
| How active are AI crawlers? | Requests by AI user agent, by section, over time |

## When to use, and when not to
Use it for large or fast-changing sites, crawl budget questions, slow indexing, unexplained Crawl Stats changes, bot-specific errors, migrations (to confirm old URLs are re-crawled) and AI crawler policy decisions.

Don't use it for small sites of a few hundred pages with no indexing problems, where Search Console and a crawl answer most questions faster.

## Inputs to ask for, in one message
1. Raw access logs for at least 30 days (90 is better), from the layer that sees bot requests: CDN, load balancer or origin server. If a CDN caches pages, origin logs alone miss requests.
2. The log format, with fields for timestamp, client IP, method, URL, status, bytes, response time, user agent and host.
3. A current crawl and XML sitemap URL list.
4. Release and publishing dates for the period.
5. Search Console Crawl stats for the same period, to cross-check.

## Workflow

### Step 1. Collect and normalise
Combine files, parse into columns, normalise URLs (lowercase host, strip tracking parameters into a separate column), and convert timestamps to one time zone.

### Step 2. Verify bots
- **Googlebot:** reverse DNS lookup of the IP must end in `googlebot.com`, `google.com` or `googleusercontent.com`, and a forward lookup of that hostname must return the same IP. Or match IPs against Google's published crawler IP range files.
- **Bingbot:** reverse DNS must end in `search.msn.com`, confirmed by forward lookup.
- **AI crawlers:** several AI companies publish IP ranges for their crawlers; match against those lists where they exist, and treat unverified requests as unverified.
Label each request verified, unverified or spoofed. Report only verified bots in crawl analysis.

### Step 3. Segment URLs
Classify every URL by template (home, category, product, article, search, filter, asset, API) and by status (indexable 200, redirect, 4xx, 5xx, non-indexable 200). Add flags: in sitemap, in crawl, has parameters.

### Step 4. Measure crawl distribution
For verified Googlebot, calculate requests by template and status, unique URLs crawled, and the share of requests to URLs you don't want crawled.

### Step 5. Measure freshness and discovery
For each template, calculate the share of sitemap URLs crawled in the last 7 and 30 days, and the median time from publish to first crawl for new URLs.

### Step 6. Check errors and speed
Chart bot 5xx, 429 and 403 by hour; check response time percentiles. Spikes at fixed hours suggest backups or cron jobs; errors only for bots suggest WAF or rate-limit rules.

### Step 7. Turn findings into fixes
Link each finding to an action and an owner, then re-run the same queries after the fix ships.

## Query examples
```bash
# Requests per user agent containing Googlebot, by status (combined log format)
grep "Googlebot" access.log | awk '{print $9}' | sort | uniq -c | sort -rn

# Top 20 URLs requested by Googlebot
grep "Googlebot" access.log | awk '{print $7}' | sort | uniq -c | sort -rn | head -20

# Verify one IP
host 66.249.66.1        # should return a googlebot.com hostname
host crawl-66-249-66-1.googlebot.com   # should return 66.249.66.1
```
```sql
-- Share of verified Googlebot requests by template and status (BigQuery-style SQL)
SELECT template, status_group, COUNT(*) AS hits,
       ROUND(100 * COUNT(*) / SUM(COUNT(*)) OVER (), 1) AS pct
FROM logs
WHERE bot = 'Googlebot' AND verified = TRUE
GROUP BY template, status_group
ORDER BY hits DESC;

-- Sitemap URLs not crawled by Googlebot in 30 days
SELECT s.url FROM sitemap_urls s
LEFT JOIN (SELECT DISTINCT url FROM logs
           WHERE bot = 'Googlebot' AND verified = TRUE
             AND ts >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 30 DAY)) l
ON s.url = l.url WHERE l.url IS NULL;
```

## Reading the results
| Finding | Likely cause | Action |
|---|---|---|
| Large share of hits to parameter URLs | Faceted navigation or tracking links crawlable | `faceted-navigation-parameters`, fix internal links |
| Many hits to redirects | Internal links or sitemaps point at old URLs | `redirects-status-codes` |
| Key templates crawled rarely | Weak internal links, deep pages, low perceived value | `internal-linking`, sitemap segmentation |
| Bot-only 5xx or 429 | WAF, rate limiting, origin overload on uncached pages | Adjust bot rules, cache, scale origin |
| Hits to URLs that no longer exist | Old links or sitemaps; normal after migrations | Confirm correct redirects or 404 or 410 |
| Heavy AI crawler load on low-value sections | No AI crawler policy | `robots-directives` |

## Worked example
A property listings site with 400,000 URLs saw new listings take over a week to appear in Google. Thirty days of CDN logs showed 61% of verified Googlebot requests went to filter URLs with sort and price parameters, while only 38% of new listing URLs were requested within seven days of publishing. Blocking sort parameters in robots.txt, removing internal links to price filters and adding a sitemap of listings from the last 7 days changed the picture within a month: filter hits fell to 18% of Googlebot requests and 81% of new listings were crawled within two days.

## Tools
- Spreadsheet or Python with pandas for small files; BigQuery, Athena or a log management tool for large ones.
- Dedicated SEO log analysers can verify bots and join crawl data, but check how they verify bots and handle IPs.
- CDN dashboards often show bot traffic, but usually without enough URL detail on their own.

## Output format
```
# Log file analysis: <site>, <period>
## Questions answered
## Data (sources, period, rows, verification method)
## Findings
| # | Finding | Evidence (numbers, period) | Impact | Fix | Owner |
## Charts: verified Googlebot hits by template and status; freshness by template; bot errors by hour
## Re-run plan and dates
```

## Common mistakes
- Counting every user agent containing "Googlebot" without verifying it.
- Using origin logs only when a CDN serves most bot requests.
- Analysing a few days, which hides weekly patterns.
- Reporting totals without segmenting by template and status.
- Keeping raw user IP addresses longer than needed.

## Questions to ask before you recommend changes
- Which layer sees all bot requests, and for how long are logs kept?
- What specific crawling or indexing problem prompted this?
- Are there WAF, bot-management or rate-limit rules that treat crawlers differently?
- What is the policy for AI crawlers?

## Self-check before you answer
- Did I verify bots by DNS or published IP ranges?
- Is every finding backed by a number, a period and a segment?
- Did I connect each finding to a fix and a re-run date?
- Did I handle personal data in line with the site's policy?

## Official documentation
- Verifying Googlebot and other Google crawlers: https://developers.google.com/search/docs/crawling-indexing/verifying-googlebot
- Overview of Google crawlers and fetchers: https://developers.google.com/search/docs/crawling-indexing/overview-google-crawlers
- Optimise your crawl budget: https://developers.google.com/crawling/docs/crawl-budget
- Crawl Stats report: https://support.google.com/webmasters/answer/9679690
- Verify Bingbot: https://www.bing.com/toolbox/verify-bingbot

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/log-file-analysis/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

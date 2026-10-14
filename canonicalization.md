---
name: canonicalization
description: "Find duplicate and near-duplicate URLs and make Google and Bing choose the canonical you want, by aligning rel=canonical, redirects, internal links, sitemaps, hreflang and parameter handling. Covers www and non-www, HTTP and HTTPS, trailing slashes, case, tracking and sort parameters, product paths inside collections, pagination, syndication, near-duplicate location or currency pages, and canonicals with hreflang. Use whenever someone mentions duplicate content, 'Google chose different canonical than user', 'Duplicate without user-selected canonical', 'Alternate page with proper canonical tag', wrong URLs ranking, pages competing with each other, or asks how canonical tags work."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/canonicalization/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Canonicalization and Duplicate Content

Google groups duplicate and near-duplicate URLs together and picks one as the canonical: the version it indexes, ranks and credits with signals. `rel="canonical"` is a strong hint in that choice, not a command. When your redirects, links, sitemaps and hreflang point one way and your canonical tag points another, Google decides for itself, and it often picks the URL you least want, such as a tracking-parameter version.

This skill makes every signal agree, so the URL you choose is the URL that ranks.

## Operating principles
1. **One preferred URL per piece of content,** decided once and written down (host, protocol, trailing slash, case, parameters).
2. **Signals must agree.** Redirects, canonicals, internal links, sitemaps and hreflang all point to the preferred URL.
3. **Use the strongest tool that fits.** If a duplicate has no reason to exist, redirect it. If people need it, canonicalise it.
4. **Different content isn't duplicate.** If two pages serve different needs, make them clearly different and let both stand.
5. **Check Google's choice, not only your tag.** URL Inspection shows the Google-selected canonical; that is the truth that matters.

## Signals, strongest first
| Signal | Strength | Notes |
|---|---|---|
| 301 or 308 redirect | Strongest | The duplicate stops existing for users and crawlers |
| `rel="canonical"` (HTML head or HTTP `Link` header) | Strong hint | Use the header for PDFs and other non-HTML files |
| Sitemap inclusion | Weaker hint | List canonicals only |
| Internal links | Weaker hint, but adds up | Thousands of links to a parameter version can outweigh a tag |
| hreflang references | Supporting | Must reference canonical URLs |
| HTTPS and clean URLs | Tie-breakers | Google prefers HTTPS and simpler URLs when other signals are equal |

## When to use, and when not to
Use it for duplicate-content warnings, wrong URL versions ranking, parameter and path variants, near-duplicate programmatic pages, syndication, and canonical setup on any new template.

Don't use it for faceted navigation strategy as a whole (`faceted-navigation-parameters`), redirect implementation detail (`redirects-status-codes`) or keyword cannibalisation between different pages, which is a content decision.

## Inputs to ask for, in one message
1. Search Console examples for "Duplicate, Google chose different canonical than user", "Duplicate without user-selected canonical" and "Alternate page with proper canonical tag".
2. A crawl with status, canonical (HTML and header), meta robots, inlinks, and a content hash or near-duplicate similarity score.
3. The CMS and its URL rules: trailing slashes, case, parameters, how categories and filters build URLs.
4. The preferred URL format, if the business has one.
5. Systems that link to the site with parameters: email, ads, widgets, partners.

## Workflow

### Step 1. Fix host and protocol
Choose HTTPS and either www or non-www. The other three combinations redirect to the preferred one in a single hop. Test all four from the command line:

```bash
for u in http://example.com http://www.example.com https://example.com https://www.example.com; do
  echo "$u"; curl -sIL "$u/" | grep -iE "^HTTP|^location"; done
```

### Step 2. Fix path variants
Trailing slash versus none, uppercase versus lowercase, `/index.html` and `/index.php`, double slashes, and the same page reachable under two folders. Redirect each to one form. Set the rule in one place (server or framework configuration), not per page.

### Step 3. Handle parameters by type
| Parameter type | Example | Treatment |
|---|---|---|
| Tracking | `utm_source`, `gclid`, `fbclid` | Canonical to the clean URL; never link internally with them |
| Sort and view | `sort=price`, `view=grid` | Canonical to the clean URL; consider blocking crawl if volume is high |
| Session | `sessionid=` | Remove from URLs; canonical to clean URL |
| Pagination | `page=2` | Self-canonical on each page |
| Filters that change content | `colour=black` | Decide with `faceted-navigation-parameters` |

### Step 4. Self-referencing canonicals everywhere
Every indexable page outputs exactly one absolute canonical pointing to itself, in the server-rendered HTML. JavaScript-injected canonicals that differ from the server version create conflicts.

### Step 5. Find near-duplicates
Use your crawler's near-duplicate report or compare main-content similarity. Pages above roughly 90% shared main content (location or currency pages that swap one word, product variants with identical copy) are at risk. Either make each page clearly different with real local or specific information, consolidate them into one page with options, or keep one and redirect the rest.

### Step 6. Check Google's choice
For each duplicate group, inspect a sample URL. Where the user-declared and Google-selected canonicals differ, list every signal that points to Google's choice. Fix those signals, not just the tag.

## Diagnosing a disagreement, one URL at a time
1. Note the user-declared and Google-selected canonical in URL Inspection.
2. Fetch both URLs and compare status codes, canonical tags (HTML and header) and main content.
3. Count internal links to each version in your crawl.
4. Check which version the sitemap and hreflang list.
5. Check for a redirect from your preferred URL to the other one; that overrides everything.
6. Fix what disagrees, request indexing for a sample and validate the fix. Large sites take weeks.

## Decision table
| Situation | Use |
|---|---|
| Old URL with no reason to exist | 301 redirect |
| Variant users need (print view, sort order, tracking) | Canonical to the main URL |
| Country or language versions with real differences | Self-canonical on each, plus hreflang |
| Thin duplicate with no user value | Remove (410), or merge and redirect |
| Paginated series | Self-canonical on each page, crawlable links between pages |
| Syndicated copy on a partner site | Cross-domain canonical to the original if the partner agrees; otherwise a link back |
| PDF that duplicates an HTML page | `Link: <https://example.com/page/>; rel="canonical"` header on the PDF |

## Canonicals and hreflang together
Each language or country version is its own canonical. Never point the French page's canonical at the English page; that tells Google the French page is a duplicate and it may drop it from French results. Each version carries a self-referencing canonical and hreflang tags listing every alternate, including itself, all pointing at canonical URLs.

## Worked example
A payments site serves `/send-money/uk-to-nigeria/` and `/send-money/uk-to-nigeria?currency=GBP`. The rate widget and marketing emails link to the parameter version, so after a few months Google selects it as canonical even though the clean URL declares itself canonical. The fix: change widget and email links to the clean URL, keep the self-canonical, keep the parameter version out of sitemaps, and canonicalise all parameter versions to the clean URL. Four weeks later URL Inspection shows the clean URL selected, and the parameter version stops appearing in results.

## Platform notes
- **WordPress:** SEO plugins output canonicals automatically. Check the theme or a second plugin doesn't add another; two canonicals can make Google ignore both. Paginated archives should be self-canonical.
- **Shopify:** products are reachable at `/products/handle` and inside collections at `/collections/name/products/handle`. Themes canonicalise to `/products/handle`; make collection templates link there too.
- **Next.js:** set `trailingSlash` once in configuration, generate canonicals from the final URL with the Metadata API, and render them on the server.
- **Webflow and hosted builders:** set the global canonical URL in site settings and check CMS collection pages output self-canonicals.

## Ticket template for developers
```
Title: [SEO] Enforce one URL format and self-canonicals
Why: 12,400 URLs show "Google chose different canonical" (Search Console, 9 Oct 2026); internal links mix trailing-slash and non-slash forms.
Change: 301 non-slash to slash URLs in one hop; output one absolute self-canonical in server HTML on all indexable templates; strip utm_* from internal links.
Acceptance tests:
- /pricing redirects once to /pricing/ (301).
- View-source of 5 templates shows exactly one canonical, absolute, matching the URL.
- Crawl shows 0 internal links to redirecting or parameter URLs.
Metric: count of "Google chose different canonical" URLs, re-check after 4 weeks.
```

## Output format
```
# Canonical audit: <site>, <date>
## Summary
## Preferred URL rules (host, protocol, slash, case, parameters)
## Duplicate groups
| Group | URLs | Declared canonical | Google canonical | Conflicting signals | Fix |
## Near-duplicate templates and decision (differentiate, consolidate, remove)
## Fix list with owners, acceptance tests and re-check dates
```

## Common mistakes
- Canonicals pointing to redirected, noindex or 404 URLs.
- Two canonical tags on one page from a theme and a plugin.
- Canonicalising every page to the homepage.
- Canonicalising page 2 of a series to page 1.
- Using noindex or robots.txt to fix duplication; both stop Google from seeing your canonical signals.
- Pointing hreflang alternates' canonicals at one language.

## Questions to ask before you recommend changes
- Which URL format does the business want long term?
- Which systems create links with parameters, and can they change?
- Are some near-duplicates intentional, such as country versions with different prices or rules?

## Self-check before you answer
- Do all signals in my plan point to the same URL?
- Did I check Google's selected canonical, not just the tag?
- Did I separate true duplicates from pages that only look similar?
- Does every fix have acceptance tests and a re-check date?

## Official documentation
- How to specify a canonical URL: https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls
- What is canonicalization: https://developers.google.com/search/docs/crawling-indexing/canonicalization
- Troubleshoot canonicalization: https://developers.google.com/search/docs/crawling-indexing/canonicalization-troubleshooting
- URL structure best practices: https://developers.google.com/search/docs/crawling-indexing/url-structure
- Localised versions (hreflang): https://developers.google.com/search/docs/specialty/international/localized-versions

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/canonicalization/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

---
name: faceted-navigation-parameters
description: "Control crawling and indexing of faceted navigation, filters, sorting, internal search, pagination, infinite scroll and URL parameters on e-commerce, marketplace, listings, travel and programmatic sites: decide which filter combinations deserve indexable landing pages from search demand, block or consolidate the rest with robots.txt, canonicals, fragments and internal-link rules, and keep URL order, separators and empty results under control. Use whenever someone mentions filters, facets, faceted search, sort or price parameters, millions of URLs, crawl traps, index bloat, internal search pages in Google, pagination, load more, infinite scroll, or asks which filter pages should be indexed."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/faceted-navigation-parameters/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Faceted Navigation and URL Parameters

Filters help shoppers narrow thousands of items to a few. They also multiply URLs: ten colours, eight sizes, six price bands and four sort orders can turn one category into thousands of crawlable addresses, most of them near-duplicates. Google's documentation names faceted navigation as one of the most common causes of overcrawling, and it slows the discovery of the pages you actually want indexed.

This skill sorts every facet into one of three groups (index, crawl but don't index, don't crawl), picks landing pages from real search demand and writes the rules that enforce the decision.

## Operating principles
1. **Demand decides what gets indexed.** A filter combination earns an indexable page only if people search for it and the result set is useful.
2. **Prevent crawling of the rest.** Blocking crawl saves more resources than relying on canonicals or nofollow alone.
3. **One URL per combination.** Same filters, same order, same separator, every time.
4. **Empty results aren't pages.** A combination with no items returns 404 or isn't linked at all.
5. **Indexable filter pages are real landing pages,** with their own title, H1, copy and internal links.

## Three groups for every facet
| Group | Example facets | URL treatment | Crawl | Index |
|---|---|---|---|---|
| A. Landing pages | Brand, main type, colour where searched ("black running shoes") | Clean path or stable parameter, self-canonical, in sitemap, linked | Yes | Yes |
| B. Useful, not search-worthy | Size, rating, minor attributes | Parameter URL, canonical to the parent category, not in sitemap | Limited | No |
| C. Crawl traps | Sort, view, price sliders, session IDs, combinations of 3+ filters, internal search | Blocked in robots.txt, or applied with fragments or POST | No | No |

Canonicals and `nofollow` on filter links can reduce crawling of group B over time, but Google describes them as less effective in the long term than preventing crawling. Use robots.txt for group C.

## When to use, and when not to
Use it for e-commerce and marketplace category design, listings and travel sites, programmatic pages built from filters, index bloat with parameter URLs, "Crawled, currently not indexed" spikes on filter URLs, internal search pages appearing in Google, and pagination or infinite scroll decisions.

Don't use it for duplicates unrelated to filters, such as protocol or trailing-slash variants (`canonicalization`), or for general crawl diagnostics (`crawlability-indexation-audit`).

## Inputs to ask for, in one message
1. A list of every filter and sort parameter, with example URLs and how they are generated.
2. A crawl with parameter URLs, status, canonical, meta robots, inlinks and product count per page.
3. Search Console Page indexing examples and Performance data for parameter URLs.
4. Keyword demand for filter combinations (for example "womens black running shoes size 6"), with search volumes labelled as estimates.
5. Server or CDN logs if available, to measure how much Googlebot crawls filter URLs.
6. The platform and how much control the team has over URL formats, robots.txt and link markup.

## Workflow

### Step 1. Inventory parameters
List every parameter with its purpose (filter, sort, view, pagination, tracking, session, search) and the number of URLs it produces in the crawl or logs.

### Step 2. Measure the cost
From logs or Crawl stats, calculate the share of Googlebot requests going to filter and sort URLs. From Search Console, count indexed parameter URLs and how many earn clicks.

### Step 3. Score demand for combinations
For single facets and pairs (category plus colour, category plus brand), check search demand and the number of items returned. Promote to group A only combinations with demand and enough items to be useful; everything else goes to B or C.

### Step 4. Define URL rules
- Use standard `?key=value&key2=value2` parameters or clean paths for group A; don't invent separators.
- Fix the order of filters in the URL, regardless of click order.
- Lowercase values; no duplicate parameters.
- Return 404 for combinations with no results, and don't link to them.
- Keep tracking parameters out of internal links.

### Step 5. Enforce the groups
- **Group A:** self-canonical, in the XML sitemap, linked from the parent category with plain `<a href>` links, unique title, H1 and a short intro.
- **Group B:** canonical to the parent category, excluded from sitemaps, links optionally marked `nofollow`.
- **Group C:** disallowed in robots.txt by parameter pattern, or applied without creating crawlable URLs (fragments, form POST, JavaScript state that doesn't change the URL).

```text
# robots.txt example for group C
User-agent: *
Disallow: /*?*sort=
Disallow: /*?*view=
Disallow: /*?*price=
Disallow: /search
```
Check that rules don't block group A URLs. A blocked URL can't pass its canonical or noindex signals, so let already-indexed URLs drop out before you block them.

### Step 6. Handle pagination
Give every page in a series its own URL (`?page=2`) with a self-referencing canonical and plain links to the next and previous pages. Google no longer uses `rel="next"` and `rel="prev"` as signals, though they do no harm. Don't canonicalise page 2 to page 1. For infinite scroll or "load more", make sure each chunk is reachable at a paginated URL that works when loaded directly.

### Step 7. Monitor
Re-check crawl share to filter URLs in logs or Crawl stats, indexed parameter URL counts, and clicks to group A pages over the next 4 to 12 weeks.

## Decision table for common parameters
| Parameter | Typical group | Treatment |
|---|---|---|
| `brand=` | A if searched | Landing page, often a clean path |
| `colour=` or `color=` | A or B | A for top colours with demand, B otherwise |
| `size=` | B | Canonical to parent, no sitemap |
| `sort=` | C | robots.txt disallow |
| `view=grid` | C | robots.txt disallow or fragment |
| `price=10-50` | C | robots.txt disallow |
| `page=` | Crawlable, self-canonical | Plain links, not in robots.txt |
| `q=` (internal search) | C | robots.txt disallow; noindex the template as a backup |
| `utm_*`, `gclid` | Not linked | Canonical to clean URL; strip from internal links |

## Platform notes
- **Shopify:** filter URLs use `filter.` parameters and collection tags can create extra paths. Shopify's default robots.txt already blocks some patterns; extend it with `robots.txt.liquid` for sort and filter combinations you don't want crawled.
- **WooCommerce and WordPress filter plugins:** many create crawlable links for every option; check plugin settings for AJAX filtering without URL changes, and add parameter rules to robots.txt.
- **Magento and Adobe Commerce:** layered navigation creates large parameter sets; configure which attributes are filterable and use robots.txt for the rest.
- **Headless and custom builds:** decide the URL rules in the routing layer, generate group A pages from an approved list, and keep group C state client-side.

## Worked example
A fashion retailer had 2,100 products and 1.4 million crawlable filter URLs. Logs showed 72% of verified Googlebot requests went to sort and multi-filter combinations, and Search Console reported 310,000 filter URLs as "Crawled, currently not indexed". Keyword research found demand for 180 category-plus-colour and category-plus-brand combinations. Those became clean landing pages with copy and sitemap entries; size filters were canonicalised to parents; sort, price and three-filter combinations were blocked in robots.txt. Three months later, filter requests fell to 15% of Googlebot crawling, and the 180 landing pages drew new impressions for long-tail colour and brand queries.

## Output format
```
# Faceted navigation plan: <site>, <date>
## Summary (crawl share to filters, indexed parameter URLs, opportunity)
## Parameter inventory
| Parameter | Purpose | URLs | Group | Treatment |
## Landing pages to build (group A) with demand evidence
## URL rules, robots.txt rules and canonical rules
## Pagination approach
## Monitoring plan and re-check dates
```

## Common mistakes
- Letting every filter combination be crawlable and indexable.
- Using noindex on millions of filter URLs instead of preventing crawl.
- Blocking in robots.txt URLs that need to drop out of the index first.
- Filter order and case producing different URLs for the same result.
- Indexable filter pages with no unique copy or title.
- Canonicalising paginated pages to page 1.

## Questions to ask before you recommend changes
- Which filter combinations do customers actually search for?
- Can the platform control URL order, separators and empty results?
- Who owns robots.txt and filter link markup?
- Are any filter URLs already earning clicks that must be kept?

## Self-check before you answer
- Is every parameter assigned to a group with a clear treatment?
- Are group A pages backed by demand and enough items?
- Do robots.txt rules avoid blocking pages that should rank?
- Does the plan include measurement from logs or Crawl stats?

## Official documentation
- Managing crawling of faceted navigation URLs: https://developers.google.com/crawling/docs/faceted-navigation
- Pagination and incremental page loading: https://developers.google.com/search/docs/specialty/ecommerce/pagination-and-incremental-page-loading
- Optimise your crawl budget: https://developers.google.com/crawling/docs/crawl-budget
- Consolidate duplicate URLs: https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls
- robots.txt introduction: https://developers.google.com/search/docs/crawling-indexing/robots/intro

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/faceted-navigation-parameters/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

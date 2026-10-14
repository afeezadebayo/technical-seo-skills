---
name: internal-linking
description: "Audit and design internal linking and site architecture: URL and folder structure, click depth, orphan pages, hub and spoke topic clusters, anchor text, navigation and footer links, breadcrumbs, pagination, related-content modules, links to redirects or noindexed pages, and routing link equity to the pages that make money. Use whenever someone mentions internal links, site structure, information architecture, orphan pages, click depth, topic clusters, pillar pages, anchor text, breadcrumbs, mega menus, pages Google finds slowly, or asks how to make important pages rank better without new backlinks."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/internal-linking/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Internal Linking and Site Architecture

Internal links do three jobs at once: they help crawlers discover pages, they tell search engines which pages matter most and what they are about, and they guide people to the next useful page. A good structure means important pages sit a few clicks from the homepage, related pages link to each other with descriptive anchors, and no valuable page is left without links.

This skill maps the current link graph, finds where it works against the business, and designs a structure with a specific list of links to add, change or remove.

## Operating principles
1. **Important pages are close and well linked.** Money pages should be within three clicks of the homepage and receive links from relevant pages, not only from navigation.
2. **Anchors describe the destination.** Use the words a searcher would use, varied naturally, not "click here" and not the same exact phrase hundreds of times.
3. **Links point at final, indexable URLs.** No internal links to redirects, 404s, noindexed or canonicalised pages.
4. **Structure follows topics and intent.** Group pages so a hub covers a topic and every supporting page links back to it.
5. **Every link earns its place.** Navigation with hundreds of links dilutes the signal and the user's attention.

## Architecture patterns
| Pattern | How it works | Good for |
|---|---|---|
| Hub and spoke (topic cluster) | A hub page covers the topic and links to every supporting page; each supporting page links back and to close siblings | Content sites, SaaS resources, service businesses |
| Category tree | Home to category to subcategory to item, with breadcrumbs | E-commerce, marketplaces, directories |
| Programmatic grid | Template pages linked by dimension (location, route, product type) through index pages | Large templated sites |
| Flat with strong navigation | Few levels, most pages linked from menus | Small sites under a few hundred pages |

## When to use, and when not to
Use it for orphan pages, important pages that rank poorly, slow discovery of new pages, new sections or clusters, navigation redesigns, and after migrations or content pruning.

Don't use it for external link building, for canonical decisions between duplicates (`canonicalization`) or for filter URL control (`faceted-navigation-parameters`).

## Inputs to ask for, in one message
1. A full crawl with inlinks, outlinks, anchor text, click depth, status code and indexability per URL.
2. Search Console Performance by page (clicks, impressions, average position) for 3 to 12 months.
3. The XML sitemap URL list (to find pages not reached by links).
4. Revenue or conversion value by page or page type.
5. The navigation, footer and any related-content or breadcrumb components, and how they are generated.
6. Topic or keyword groups if a content plan exists.

## Workflow

### Step 1. Map depth and orphans
From the crawl, chart URLs by click depth. Compare crawled URLs against sitemap and analytics landing pages: URLs in the sitemap or analytics but not found by the crawler are orphans. Decide for each orphan: link it, merge it, or retire it.

### Step 2. Compare links with value
List the top pages by revenue or conversions. For each, record unique inlinks, the share from in-content links, click depth and anchor variety. Pages that make money but receive few contextual links are the first targets.

### Step 3. Find wasted links
Flag internal links to redirects, 4xx, 5xx, noindexed and canonicalised URLs; parameter URLs linked from navigation; and very large menus or footers repeated on every page.

### Step 4. Find striking-distance pages
From Search Console, list pages with high impressions and average positions roughly between 5 and 20. More relevant internal links with descriptive anchors are often the cheapest way to move them.

### Step 5. Design the target structure
For each topic or category: name the hub, list the supporting pages, and define the links each page must have (up to hub, across to siblings, down to children). Keep URL folders aligned with the structure where possible, but don't change URLs only for neatness.

### Step 6. Write the link plan
Produce a list of specific changes: source page, target page, suggested anchor, placement (body, related module, navigation, breadcrumb). Fix template-level links first, because one change reaches every page.

### Step 7. Re-crawl and measure
After changes, re-crawl to confirm depth, orphan count and broken links. Track clicks and impressions for target pages over the following 4 to 12 weeks.

## Link sources and their weight
| Source | Notes |
|---|---|
| In-content links | Strongest contextual signal; place them where the topic is discussed |
| Breadcrumbs | Show hierarchy to users and search engines; pair with BreadcrumbList markup |
| Navigation | Defines the top of the structure; keep it to the pages that matter most |
| Related-content modules | Useful at scale if they choose by topic, not at random or by date only |
| Footer | Weakest; fine for policies and key hubs, not for long keyword lists |
| Pagination | Needed so crawlers reach deep items; use plain links between pages |

## Anchor text rules
- Describe the target page in natural words: "redirect chain audit" rather than "read more".
- Vary anchors across pages; repeating one exact-match phrase sitewide looks manufactured and helps no reader.
- Make image links count with descriptive `alt` text.
- Keep internal links followed; `nofollow` on internal links wastes the signal and doesn't save crawl budget.

## Crawlable link requirements
Search engines follow `<a>` elements with an `href` that resolves to a URL. Links built with buttons, `onclick` handlers or `#` fragments, or inserted only after a user action, are often not followed. See `javascript-seo` if the site relies on JavaScript navigation.

## Worked example
A B2B SaaS site had 240 blog posts and 12 product feature pages. Feature pages were linked only from the main menu, while blog posts linked to each other by date. The crawl showed feature pages had 6 to 9 unique inlinks each, and 38 posts were orphans. The plan created four topic hubs, linked each post to its hub and to the most relevant feature page with descriptive anchors, added breadcrumbs, and merged 11 thin orphans into stronger posts. Feature pages rose to 40 to 70 unique inlinks each. Over the next three months, impressions for feature-page queries grew, and two feature pages moved from page two to page one (Search Console, comparing the 28 days before and after).

## Platform notes
- **WordPress:** use the block editor's link search for in-content links, add breadcrumbs through the SEO plugin, and replace random "related posts" plugins with topic-based ones. Check that tag and author archives don't create thousands of thin, linked pages.
- **Shopify:** collections are the hubs; link from collection descriptions to sub-collections and from product descriptions to related collections. Make product links in collections point at `/products/handle`.
- **Headless and JavaScript sites:** generate links on the server as real `<a href>` elements; check that menus are in the raw HTML.
- **Large templated sites:** build index pages (by city, category, letter) so every template page is reachable within a few clicks, and include "nearby" or "related" links chosen by data.

## Output format
```
# Internal linking and architecture review: <site>, <date>
## Summary
## Current state: depth chart, orphan count, broken internal links, top pages by inlinks
## Target structure (hubs, clusters, categories)
## Link plan
| Source URL | Target URL | Anchor | Placement | Reason | Priority |
## Template changes (navigation, breadcrumbs, related modules)
## Measurement plan and re-check dates
```

## Common mistakes
- Mega menus linking to hundreds of pages from every URL.
- Linking internally to redirects and parameter URLs.
- Orphaned landing pages built for campaigns and never linked.
- Using the same exact-match anchor everywhere.
- "Related posts" chosen by date rather than topic.
- Restructuring URLs for tidiness and creating a migration nobody planned.

## Questions to ask before you recommend changes
- Which pages make the most money, and which should rank that don't?
- Who controls navigation, templates and in-content links?
- Can related-content modules be driven by topic or taxonomy data?
- Are there sections being retired that still receive links?

## Self-check before you answer
- Did I compare link distribution against business value?
- Does every proposed link point at a final, indexable URL?
- Are anchors descriptive and varied?
- Did I prioritise template-level changes over one-off edits?

## Official documentation
- Link best practices for Google (crawlable links and anchor text): https://developers.google.com/search/docs/crawling-indexing/links-crawlable
- SEO Starter Guide (site hierarchy and navigation): https://developers.google.com/search/docs/fundamentals/seo-starter-guide
- Breadcrumb structured data: https://developers.google.com/search/docs/appearance/structured-data/breadcrumb
- URL structure best practices: https://developers.google.com/search/docs/crawling-indexing/url-structure

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/internal-linking/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

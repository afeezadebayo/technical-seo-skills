---
name: xml-sitemaps
description: "Plan, generate, audit, segment and monitor XML sitemaps and sitemap indexes, including image, video, news and hreflang sitemaps, honest lastmod dates, and batch segmentation for large, programmatic and multilingual sites. Use whenever someone asks about sitemaps, 'submit sitemap', 'Couldn't fetch', 'Sitemap could not be read', submitted versus indexed gaps, pages missing from sitemaps, how to split or segment sitemaps, sitemap settings in Yoast, Rank Math, Shopify, Webflow or Next.js, or how to get new pages discovered faster."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/xml-sitemaps/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# XML Sitemaps

A sitemap is a promise: "these are the URLs I want in your index, and here is when each one last changed." Keep the promise and Google uses the file to discover and refresh pages. Break it with redirects, noindex pages, duplicates and fake dates, and the file becomes noise Google learns to discount.

This skill builds sitemaps that are clean enough to trust and segmented enough to diagnose. Done well, the Search Console Sitemaps report stops being a box you tick and becomes your best indexing dashboard.

## Operating principles
1. **Only list URLs you want indexed:** status 200, indexable, self-canonical, not blocked by robots.txt.
2. **`lastmod` must be true or absent.** A date that changes on every deploy is worse than no date.
3. **Segment by template and batch,** so every sitemap answers a question ("how indexed are our converter pages in Spanish?").
4. **Generate from the source of truth,** the same data that builds the pages, after publishing, never from a crawl of yesterday's site.
5. **A sitemap helps discovery, not quality.** It won't make Google index weak pages.

## When to use, and when not to
Use it for building or fixing sitemaps, Search Console sitemap errors, big submitted-versus-indexed gaps, new site sections, programmatic launches, migrations and multilingual setups.

Don't use it to fix pages that are crawled but not indexed because of quality (use `crawlability-indexation-audit`), or as a substitute for internal links (use `internal-linking`).

## Inputs to ask for, in one message
1. Every sitemap URL (from robots.txt, the index and Search Console), and which tool generates each.
2. The Search Console Sitemaps report: submitted and indexed counts per file, errors, last read date.
3. A crawl of the site, ideally including a list-mode crawl of every sitemap URL.
4. The number of indexable URLs per template, and which templates make money.
5. Languages and countries served, and how alternates are declared today.

## The rules (protocol and Google)
| Rule | Detail |
|---|---|
| Size | Up to 50,000 URLs and 50 MB uncompressed per file. Gzip is fine; the limit applies before compression |
| Index files | A sitemap index can list up to 50,000 sitemaps. Indexes can't nest inside indexes |
| URLs | Absolute, canonical, same host and protocol as the sitemap's location unless the other host is verified in Search Console |
| Encoding | UTF-8, entity-escaped (`&` becomes `&amp;`) |
| `lastmod` | W3C date format (`2026-10-09` or `2026-10-09T08:00:00+00:00`). Google uses it only when it is consistently accurate |
| `priority`, `changefreq` | Ignored by Google. Leave them out |
| Ping | Google retired the sitemap ping endpoint in 2023. Submit in Search Console and reference in robots.txt |
| Image sitemaps | Use `image:loc`. Google deprecated the other image tags in 2022 |
| News sitemaps | Articles from the last two days only, up to 1,000 URLs per news sitemap |

## Workflow: audit
1. **Inventory every file.** Fetch the index and each child; record status code, URL count, size, generator and last modified date.
2. **Validate every listed URL** with a list-mode crawl: status, indexability, canonical, robots. Calculate the clean rate per file. Aim for 98% or more.

| Sitemap | URLs | Clean % | Non-200 | Noindex | Not self-canonical | Blocked | Indexed (GSC) |
|---|---|---|---|---|---|---|---|

3. **Find the gaps:** indexable URLs in the crawl but missing from sitemaps, and sitemap URLs with no internal links (orphans that only the sitemap knows about).
4. **Test `lastmod` honesty:** compare `lastmod` with the real content change date on ten URLs per template. If every URL shows today's date, the generator is stamping build time.
5. **Check duplicates across files:** the same URL in two sitemaps, or two generators (an SEO plugin and a custom script) listing overlapping sets.
6. **Read the Search Console numbers per file** and note which segments lag.

## Workflow: design the structure
Use a sitemap index with child files split by what you want to monitor.

```
sitemap_index.xml
  sitemap-pages.xml
  sitemap-blog.xml
  sitemap-products-1.xml ... sitemap-products-4.xml
  sitemap-converters-en.xml
  sitemap-converters-es.xml
  sitemap-converters-batch-05.xml   (programmatic launch batch)
```

Split rules of thumb:
- One file per template that matters commercially.
- One file per language or country version for international sites.
- One file per launch batch for programmatic pages, so you can compare indexing between batches.
- Keep files well under the limits (for example 10,000 to 25,000 URLs), which also makes failures easier to spot.

## Setting lastmod correctly
Use the date the main content last changed in a way a reader would notice: new text, prices, availability, specifications or structured data. Ignore changes to navigation, footers, related-post widgets, view counters and build timestamps. Store this as a field on the content (for example `content_updated_at`) and read it when generating the sitemap; never use file modification time, which changes on every deploy.

## Extensions
- **hreflang in sitemaps:** suits large multilingual sites. Each `<url>` lists every alternate with `xhtml:link`, including itself, and every alternate's entry lists the same set back. Missing return links make Google ignore the pairing.

```xml
<url>
  <loc>https://example.com/en/send-money/</loc>
  <xhtml:link rel="alternate" hreflang="en" href="https://example.com/en/send-money/"/>
  <xhtml:link rel="alternate" hreflang="es" href="https://example.com/es/enviar-dinero/"/>
  <xhtml:link rel="alternate" hreflang="x-default" href="https://example.com/en/send-money/"/>
</url>
```

- **Image sitemaps:** useful when important images load in ways crawlers may miss, such as JavaScript galleries.
- **Video sitemaps:** for pages where a video is the main content, with title, description, thumbnail and content or player location.
- **News sitemaps:** for publishers eligible for Google News; rotate articles out after two days.

## Generating sitemaps by platform
- **WordPress (Yoast, Rank Math):** a sitemap index lives at `/sitemap_index.xml`. Exclude post types and taxonomies that shouldn't rank (tags, attachments, author archives, thin custom post types). Check that noindexed pages drop out automatically.
- **Shopify:** `/sitemap.xml` is generated for you and split by type. Control it by keeping thin pages unpublished or hidden from search.
- **Webflow:** auto-generated; exclude pages with the page setting. Custom code is needed for anything more granular.
- **Next.js (App Router):** build sitemaps from data with `app/sitemap.ts`, split large sets with `generateSitemaps`, and take `lastModified` from the content's real update date.
- **Headless and custom builds:** generate after publish from the same database or CMS that renders the pages, so a sitemap never lists a page that doesn't exist yet.

## Diagnosis by Search Console numbers
| What you see | Likely cause | Next step |
|---|---|---|
| Submitted far above indexed for one segment | Quality, duplication or weak links on that template | Compare with a well-indexed segment; `crawlability-indexation-audit` |
| "Couldn't fetch" | Wrong URL, robots block, authentication, 5xx, or redirect on the sitemap URL | Fetch it as Googlebot; fix status |
| "Sitemap could not be read" or parsing errors | Invalid XML, wrong encoding, HTML error page served | Validate the XML; check the content type |
| Indexed URLs that are in no sitemap | Discovery through links | Confirm those pages are wanted; add them if so |
| Last read date is weeks old | Google sees little value or can't fetch | Check fetch errors and clean rate |

## Worked example: a programmatic site
A site publishes currency converter pages in batches of 2,000. Instead of one big file, it creates one sitemap per batch and per language. After eight weeks, Search Console shows batch 01 at 92% indexed and batch 04 at 31%. Comparing the two shows batch 04 pages have half the unique content and aren't linked from the currency hub. Both are fixed before batch 05 ships, and batch 05 reaches 80% indexed in six weeks. The segmentation turned a vague "programmatic pages aren't indexing" into a specific, fixable cause.

## Monitoring routine
- **Weekly:** Sitemaps report errors and indexed counts per file; flag any segment that drops.
- **Monthly:** list-mode crawl of every sitemap URL; keep the clean rate at 98% or more.
- **Every release:** new templates appear in the right file, removed pages leave, `lastmod` behaves.
- **Migrations:** keep the old-URL sitemap live for a short period so Google re-crawls the redirects, then remove it.

## Ticket template for developers
```
Title: [SEO] Segment product sitemaps and fix lastmod
Why: products.xml lists 118,000 URLs with lastmod = build time; 14% are redirects (crawl, 9 Oct 2026).
Change: Split into sitemap-products-N.xml (max 25,000 URLs each). Exclude non-200, noindex and non-canonical URLs. Set lastmod from content_updated_at.
Acceptance tests:
- Every child file returns 200, valid XML, under 25,000 URLs.
- A list crawl of all sitemap URLs shows 98%+ clean.
- lastmod on 10 sampled products matches their last content edit.
Metric: indexed/submitted per file in Search Console, re-check after 4 weeks.
```

## Output format
```
# Sitemap audit: <site>, <date>
## Summary (three sentences)
## Inventory and health
| Sitemap | URLs | Clean % | Indexed (GSC) | Issues |
## Missing indexable URLs (count and patterns)
## Recommended structure (index and children, with what each one monitors)
## lastmod findings
## Fix list with owners and re-check dates
```

## Common mistakes
- Listing redirects, 404s, noindex or parameter URLs.
- Stamping every URL with today's date.
- Two generators producing overlapping sitemaps.
- One giant file that hides which template is failing.
- Expecting a sitemap to force indexing of thin pages.
- Forgetting to remove old sitemaps after a migration.

## Questions to ask before you recommend changes
- Which templates make money, and are all their indexable URLs listed today?
- What generates the sitemaps, and when do they refresh after content changes?
- Are there URLs that must stay out of search for legal or commercial reasons?
- How are language and country alternates declared today?

## Self-check before you answer
- Is every recommended file clean (200, indexable, self-canonical)?
- Does each segment answer a monitoring question?
- Is `lastmod` tied to real content changes?
- Did I give owners, acceptance tests and a re-check date?

## Official documentation
- Sitemaps overview: https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview
- Build and submit a sitemap: https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap
- Large sitemaps and index files: https://developers.google.com/search/docs/crawling-indexing/sitemaps/large-sitemaps
- Image sitemaps: https://developers.google.com/search/docs/crawling-indexing/sitemaps/image-sitemaps
- News sitemaps: https://developers.google.com/search/docs/crawling-indexing/sitemaps/news-sitemap
- Localised versions (hreflang): https://developers.google.com/search/docs/specialty/international/localized-versions
- Sitemaps protocol: https://www.sitemaps.org/protocol.html

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/xml-sitemaps/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

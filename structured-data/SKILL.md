---
name: structured-data
description: "Plan, write, validate and debug structured data (JSON-LD and schema.org) for rich results and entity understanding: Organization, Person, WebSite, WebPage, Article, Product, Offer, merchant listings, Review and AggregateRating, BreadcrumbList, LocalBusiness, Event, JobPosting, VideoObject, SoftwareApplication and connected @graph markup with stable @id values, plus Search Console enhancement reports and policy checks. Use whenever someone mentions schema, JSON-LD, rich results, rich snippets, stars in search results, FAQ or HowTo markup, Product or Review schema errors, the Rich Results Test, knowledge panels, entity SEO, or asks which schema a page needs."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/structured-data/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Structured Data and Schema Markup

Structured data describes a page's content in a format machines can read: this page is a product, this is its price, this person wrote the article, this organisation owns the site. Google uses it to make pages eligible for rich results and to understand entities. It never guarantees a rich result, and markup that doesn't match the visible page can lead to a manual action.

This skill decides which types a page actually needs, writes connected JSON-LD, validates it and checks it against Google's policies.

## Operating principles
1. **Mark up what is visible.** Every property describes content a user can see on that page.
2. **Purpose first.** Add a type because it earns a supported rich result or clarifies an important entity, not because it exists in schema.org.
3. **One connected graph per page.** Use `@graph` with stable `@id` values so Organization, WebSite, WebPage, Person and the main entity link to each other.
4. **Generate from data, not by hand.** Prices, ratings and dates come from the same source as the page, so they never drift apart.
5. **Validate twice.** Syntax and schema.org vocabulary in the Schema Markup Validator; Google eligibility in the Rich Results Test and Search Console reports.

## Types by page, with what they can earn
| Page | Main type | Supporting types | Possible Google feature |
|---|---|---|---|
| Homepage | WebSite, Organization | WebPage, logo, sameAs | Site name, logo in knowledge panel |
| About or author page | Person or Organization (ProfilePage) | sameAs, worksFor, alumniOf | Entity understanding, profile results |
| Article or blog post | Article or BlogPosting | Person (author), Organization (publisher), BreadcrumbList | Article features, breadcrumbs |
| Product page | Product with Offer | AggregateRating, Review, BreadcrumbList, shipping and return policies | Product snippets, merchant listings |
| Category page | CollectionPage | BreadcrumbList, ItemList | Breadcrumbs |
| Local business page | LocalBusiness subtype | PostalAddress, OpeningHoursSpecification, geo | Local and knowledge panel details |
| Event page | Event | Place, Offer, Organization | Event results |
| Job page | JobPosting | Organization, Place | Job search experience |
| Video page | VideoObject | Clip or SeekToAction | Video results, key moments |
| Software or app page | SoftwareApplication | Offer, AggregateRating | Software app rich result |

FAQ rich results now show only for well-known, authoritative government and health sites, and HowTo rich results were removed in 2023. In 2025 Google retired several more types. FAQPage markup is still valid schema.org and can help machines read Q and A content, but don't promise a rich result. Check the current search gallery before recommending any type.

## When to use, and when not to
Use it for new templates, schema audits, Search Console enhancement errors, lost rich results, entity and knowledge panel work, and checking markup that a plugin generates.

Don't use it to fix content or ranking problems that markup can't solve, or to add markup for content that isn't on the page (reviews you don't show, prices you don't display, FAQs hidden from users).

## Inputs to ask for, in one message
1. Page types and two example URLs each.
2. The CMS and how markup is produced today (theme, plugin, tag manager, custom code).
3. Search Console enhancement and Merchant listings reports, with example errors.
4. Business facts for Organization or Person: legal name, logo URL, social and directory profiles, founding date, contact points.
5. Where product, price, stock, rating and review data come from.

## Workflow

### Step 1. Audit what exists
Crawl with structured data extraction turned on, or test sample URLs. List types, errors, warnings and duplicates per template. Look for two plugins outputting the same type, which often creates conflicting Organization or Product blocks.

### Step 2. Decide the target graph per template
For each template, name the main entity, the supporting types and the properties each one needs. Mark which properties are required and recommended for the Google feature you want.

### Step 3. Set the @id plan
Use URL-based identifiers that never change:
- `https://example.com/#organization`
- `https://example.com/#website`
- `https://example.com/about/#person`
- `https://example.com/page/#webpage`
- `https://example.com/page/#article` or `#product`
Reference these with `{"@id": "..."}` instead of repeating full objects on every page.

### Step 4. Write the JSON-LD
Place it in the server-rendered HTML. JavaScript-injected markup works for Google but is slower to be picked up and may be missed by other crawlers.

### Step 5. Validate
Run the Schema Markup Validator for vocabulary errors and the Rich Results Test for Google eligibility. Fix errors first; fix warnings when the property is available and true.

### Step 6. Monitor
After release, watch Search Console enhancement reports and the Merchant listings report. Use Validate fix once errors are resolved. Re-test templates after theme, plugin or CMS updates.

## Example: connected graph for an article
```json
{
  "@context": "https://schema.org",
  "@graph": [
    {"@type": "Organization", "@id": "https://example.com/#organization", "name": "Example Ltd", "url": "https://example.com/", "logo": "https://example.com/logo.png", "sameAs": ["https://www.linkedin.com/company/example"]},
    {"@type": "WebSite", "@id": "https://example.com/#website", "url": "https://example.com/", "name": "Example", "publisher": {"@id": "https://example.com/#organization"}},
    {"@type": "Person", "@id": "https://example.com/about/#person", "name": "Jane Doe", "url": "https://example.com/about/", "jobTitle": "Head of SEO", "worksFor": {"@id": "https://example.com/#organization"}},
    {"@type": "WebPage", "@id": "https://example.com/guide/#webpage", "url": "https://example.com/guide/", "isPartOf": {"@id": "https://example.com/#website"}, "breadcrumb": {"@id": "https://example.com/guide/#breadcrumb"}},
    {"@type": "BlogPosting", "@id": "https://example.com/guide/#article", "headline": "How to audit redirects", "datePublished": "2026-09-14", "dateModified": "2026-10-01", "author": {"@id": "https://example.com/about/#person"}, "publisher": {"@id": "https://example.com/#organization"}, "mainEntityOfPage": {"@id": "https://example.com/guide/#webpage"}, "image": "https://example.com/guide/cover.png"},
    {"@type": "BreadcrumbList", "@id": "https://example.com/guide/#breadcrumb", "itemListElement": [
      {"@type": "ListItem", "position": 1, "name": "Home", "item": "https://example.com/"},
      {"@type": "ListItem", "position": 2, "name": "Guides", "item": "https://example.com/guides/"},
      {"@type": "ListItem", "position": 3, "name": "How to audit redirects"}
    ]}
  ]
}
```

## Product and merchant markup checklist
- `name`, `image`, `description`, and an identifier such as `sku` or `gtin` where you have one.
- `offers` with `price`, `priceCurrency`, `availability` and `url`, matching the visible price and stock.
- Variants modelled with `ProductGroup` and `hasVariant` where the platform supports it.
- `aggregateRating` and `review` only when real reviews of that product are visible on the page.
- Shipping details and return policy properties (or the equivalent Merchant Center settings) for merchant listing eligibility.
- Price and availability updated whenever the page changes, from the same data source.

## Policy checks
| Rule | Why |
|---|---|
| Markup matches visible content | Hidden or misleading markup can cause a manual action |
| No self-serving reviews on LocalBusiness or Organization | Google doesn't show review stars for reviews a business hosts about itself |
| Ratings come from real users on that page | Copied or aggregated third-party ratings aren't eligible |
| Dates are honest | `dateModified` changes only when content meaningfully changes |
| One main entity per page | Many unrelated Product blocks on a category page confuse eligibility |

## Platform notes
- **WordPress with Yoast or Rank Math:** both output a connected graph; extend it with their filters rather than adding a second, separate block. Turn off theme schema that duplicates it.
- **Shopify:** themes output Product markup; check that apps for reviews don't add a second Product block with different values.
- **Next.js and headless builds:** render JSON-LD in a `<script type="application/ld+json">` on the server, built from the same data as the page.
- **Tag managers:** avoid for core markup; it depends on JavaScript and drifts from page content.

## Worked example
A software company's pricing and product pages showed no rich results. The audit found three Organization blocks with different names (theme, SEO plugin and a reviews widget), a SoftwareApplication block with a rating from a third-party review site that wasn't shown on the page, and Article markup on the pricing page. The fix: one connected graph from the SEO plugin, the theme and widget markup removed, SoftwareApplication with the visible price and no rating until on-page reviews launched, and WebPage instead of Article on pricing. The Rich Results Test passed on all templates and the Organization name became consistent across the site.

## Output format
```
# Structured data plan: <site>, <date>
## Current state by template (types, errors, duplicates)
## Target graph per template
| Template | Main type | Supporting types | Required properties | Google feature | Data source |
## JSON-LD for each template (ready to paste)
## Validation results and monitoring plan
```

## Common mistakes
- Marking up content users can't see.
- Two plugins outputting conflicting Organization or Product markup.
- Promising FAQ or HowTo rich results.
- Hard-coding prices and ratings that go out of date.
- Using a different `@id` for the same entity on different pages.
- Treating warnings as more urgent than errors.

## Questions to ask before you recommend changes
- Which Google feature, if any, is each type meant to earn?
- Which system owns product, price and review data?
- Which plugin or code should be the single source of markup?
- Are the business's official names and profiles consistent everywhere?

## Self-check before you answer
- Does every property describe visible content?
- Is the graph connected with stable `@id` values?
- Did I check the current Google search gallery rather than memory?
- Did I validate in both the Schema Markup Validator and the Rich Results Test?

## Official documentation
- Introduction to structured data: https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data
- Structured data general guidelines: https://developers.google.com/search/docs/appearance/structured-data/sd-policies
- Search gallery of supported features: https://developers.google.com/search/docs/appearance/structured-data/search-gallery
- Rich Results Test: https://search.google.com/test/rich-results
- Schema Markup Validator: https://validator.schema.org/
- Schema.org vocabulary: https://schema.org/

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/structured-data/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

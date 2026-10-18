---
name: robots-directives
description: "Write, audit, test and debug robots.txt files, meta robots tags and X-Robots-Tag headers for Google, Bing and AI crawlers (GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-SearchBot, PerplexityBot, Google-Extended, Applebot-Extended, CCBot and others), including separate policies for search, AI answers and AI training. Use whenever someone asks to create or fix robots.txt, block or allow a crawler, stop pages being indexed, noindex a section, protect a staging site, control AI training versus AI search access, or debug 'Blocked by robots.txt', 'Indexed, though blocked by robots.txt' or 'Excluded by noindex', even if they only paste their robots.txt and ask 'is this right?'"
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/robots-directives/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-10"
---

# Robots.txt and Robots Directives

One wrong line in robots.txt can take a whole site out of search overnight, and the opposite mistake can leave thousands of junk URLs eating your crawl. This skill writes and audits crawler rules the way a careful technical SEO does: one goal per rule, tested against real URLs, with every crawler decision dated and sourced.

The most important idea first. There are two different controls, and most mistakes come from mixing them up.

| Control | Stops | Does not stop |
|---|---|---|
| robots.txt `Disallow` | Crawling | Indexing. A blocked URL can still be indexed from links, shown without a snippet |
| `noindex` (meta robots or `X-Robots-Tag`) | Indexing | Crawling. The page must be crawlable or the noindex is never seen |

Pick the control from the goal. Save crawl on infinite or worthless URL spaces: robots.txt. Keep a page out of results: noindex. Keep something private: authentication. Never use more than one of these on the same URL unless you know exactly why.

## When to use, and when not to
Use it for any robots.txt change, AI crawler policy, noindex or X-Robots-Tag decision, staging protection, and for Search Console robots and noindex errors.

Don't use it to fix duplicate content (that is `canonicalization`), to manage faceted navigation strategy on its own (use `faceted-navigation-parameters`, which calls this skill for the rules), or to hide confidential files (robots.txt is public).

## Inputs to ask for, in one message
1. The current robots.txt for every host and protocol that serves the site (www, non-www, subdomains, staging).
2. The URL patterns of the site: templates, parameters, internal search, account and cart paths.
3. The business's crawler policy: which search engines, AI search products and AI training crawlers it wants to allow.
4. Pages or sections that must stay out of search, and why (legal, thin, private, staging).
5. The Search Console robots.txt report and any "Blocked by robots.txt", "Indexed, though blocked by robots.txt" or "Excluded by noindex" examples.

## How Google reads robots.txt (the rules that trip people up)
- **Location:** one file per host and protocol, at the root: `https://www.example.com/robots.txt` doesn't cover `https://example.com/` or `https://shop.example.com/`.
- **Size:** Google reads the first 500 KiB and ignores the rest.
- **Status codes:** a 2xx file is obeyed. A 4xx (except 429) is treated as "no restrictions", so a missing file allows everything. A 5xx or 429 is treated as a temporary full block; if the file stays unreachable for a long period, Google falls back to a cached copy or to no restrictions. Fix server errors on this file urgently.
- **Caching:** Google usually caches robots.txt for up to 24 hours, so changes aren't instant.
- **Groups:** a crawler obeys only the single most specific `User-agent` group that matches it. If a `Googlebot` group exists, Googlebot ignores the `*` group entirely. Copy shared rules into every named group.
- **Matching:** paths are case-sensitive; user-agent names aren't. `*` matches any sequence of characters, `$` anchors the end of the URL. The longest matching rule wins; on a tie, `Allow` wins.
- **Supported fields for Google:** `user-agent`, `allow`, `disallow`, `sitemap`. Google ignores `crawl-delay`, `noindex` and `nofollow` lines. Bing supports `crawl-delay`.
- **Special case:** some Google crawlers, such as AdsBot, ignore the `*` group and only obey rules that name them.

## Pattern matching, with examples
| Rule | Matches | Does not match |
|---|---|---|
| `Disallow: /search` | `/search`, `/search/shoes`, `/searching` | `/blog/search-tips` |
| `Disallow: /search/` | `/search/shoes` | `/search`, `/searching` |
| `Disallow: /*?sort=` | `/shoes?sort=price` | `/shoes?colour=red&sort=price` |
| `Disallow: /*sort=` | both of the above | `/shoes` |
| `Disallow: /*.pdf$` | `/guide.pdf` | `/guide.pdf?v=2` |
| `Disallow: /search` plus `Allow: /search/help` | `/search/help` is allowed (longer rule) | |
| `Disallow: /` plus `Allow: /$` | only the homepage is allowed | |

Always test the exact URLs you care about, including ones with parameters in a different order and with trailing slashes.

## Workflow: auditing robots.txt
1. **Fetch every host's file** and record status code, size, last modified date and redirects.
2. **Parse the groups** and list, per crawler you care about, which group it will obey.
3. **Test a URL matrix:** five to ten real URLs per template, plus parameter, search, account and asset URLs, against Googlebot, Bingbot and each AI crawler in the policy. Record allowed or blocked and the line responsible.
4. **Check render resources:** CSS, JavaScript, fonts and images used for layout must be crawlable, or Google may render a broken page.
5. **Check sitemaps:** absolute URLs, reachable, on the same host or a verified one.
6. **Remove dead lines:** unsupported directives, rules for paths that no longer exist, duplicate groups.
7. **Check traffic impact:** for every pattern that changes from allowed to blocked, pull its organic clicks for the last 90 days. Blocking anything with clicks needs a business decision.

## Workflow: AI crawler policy
Separate three decisions, because each uses different user agents and has different effects.

| Decision | Typical user agents or tokens | Effect of blocking |
|---|---|---|
| Classic search | Googlebot, Bingbot | You drop out of search, and out of AI answers built on those indexes, including Google AI Overviews and Microsoft Copilot |
| AI search and answer retrieval | OAI-SearchBot, Claude-SearchBot, PerplexityBot | You lose the chance to be cited in those products' search answers |
| AI training | GPTBot, ClaudeBot, CCBot, `Google-Extended`, `Applebot-Extended` | Future model training stops using your content where the vendor honours robots.txt |

Three things to get right:
- **`Google-Extended` and `Applebot-Extended` are control tokens, not separate crawlers.** Blocking them doesn't affect Google Search, AI Overviews or Apple's search features.
- **User-triggered fetchers** (for example ChatGPT-User or Claude-User, which fetch a page because a person asked) are treated differently by each vendor. Read the vendor's current page before relying on robots.txt to control them.
- **Names and purposes change.** Check each vendor's crawler documentation on the day you write the rules, and put that date in a comment in the file.

CDN and security tools can also block AI crawlers at the edge, separately from robots.txt. If a crawler reports errors your file doesn't explain, check those settings.

## Workflow: noindex and X-Robots-Tag
- **HTML pages:** `<meta name="robots" content="noindex">` in the `<head>` of the server response. Avoid adding it only with JavaScript.
- **Files** (PDFs, images, feeds): send `X-Robots-Tag: noindex` as an HTTP header.
- **Crawler-specific tags:** `<meta name="googlebot" content="noindex">` targets Google only.
- **Useful values:** `nofollow`, `noarchive`, `nosnippet`, `max-snippet:[n]`, `max-image-preview:large`, `max-video-preview:[n]`, `unavailable_after:[date]`, and the `data-nosnippet` attribute for specific elements.
- **Removing a section from search:** leave it crawlable with noindex until it drops out, then decide whether to block crawling. Blocking first freezes the old indexed state.
- **Staging sites:** protect with authentication (HTTP basic auth or an IP allowlist). robots.txt and noindex on staging leak into production at launch far too often.

## Templates
A typical content or SaaS site:
```
# robots.txt for https://www.example.com
# Reviewed 9 Oct 2026. AI crawler names checked against vendor docs on that date.

User-agent: *
Disallow: /search
Disallow: /*?*sort=
Disallow: /*?*sessionid=
Disallow: /cart
Disallow: /account/

# AI training opt-out; search and AI answer crawlers stay allowed via *
User-agent: GPTBot
User-agent: ClaudeBot
User-agent: CCBot
User-agent: Google-Extended
User-agent: Applebot-Extended
Disallow: /

Sitemap: https://www.example.com/sitemap_index.xml
```
Several `User-agent` lines can share one group of rules, as above.

A protected staging host returns 401 to everyone without credentials. No robots.txt trick is needed or trusted.

## Debugging "Blocked by robots.txt" and "Indexed, though blocked"
1. Open the robots.txt report in Search Console and confirm which version Google fetched, and when.
2. Test three affected URLs and note the line that blocks each.
3. Decide whether the block is intended.
   - Should rank: narrow or remove the rule, then request indexing for a sample.
   - Shouldn't be in search: unblock temporarily and add noindex until the URLs drop out, or return 404 or 410 if they shouldn't exist.
4. Watch the report for the following weeks; large sites take time to recrawl.

## Platform notes
- **WordPress:** WordPress serves a virtual robots.txt unless a physical file exists in the web root, which overrides it. SEO plugins provide an editor. The "Discourage search engines" setting is a classic launch-day disaster.
- **Shopify:** edit `robots.txt.liquid` in the theme to add rules on top of Shopify's defaults; don't remove the defaults without a reason.
- **Webflow and other hosted builders:** use the platform's robots.txt setting and its per-page "exclude from search" options.
- **Next.js:** generate the file with `app/robots.ts` and make sure preview or staging deployments block everything or require authentication.

## Ticket template for developers
```
Title: [SEO] Block sort and session parameters in robots.txt
Why: 38% of Googlebot requests in 30 days hit sort/session URLs (log analysis, 9 Sep to 8 Oct 2026).
Change: Add "Disallow: /*?*sort=" and "Disallow: /*?*sessionid=" to the * group.
Acceptance tests:
- /shoes?sort=price and /shoes?colour=red&sort=price are blocked for Googlebot.
- /shoes and /shoes?colour=red are allowed.
- robots.txt returns 200 and stays under 500 KiB.
Metric: share of Googlebot hits on parameter URLs, re-check after 30 days.
```

## Output format
```
# Robots review: <host>, <date>
## Verdict (one paragraph)
## Crawler policy (search / AI search / AI training, with vendor docs checked on <date>)
## URL test matrix
| URL | Googlebot | Bingbot | AI crawlers | Responsible line |
## Changes
| # | Change | Reason | Traffic at risk (90 days) | Owner |
## Proposed robots.txt (full file)
```

## Common mistakes
- `Disallow: /` copied from staging to production. Monitor the live file and alert on changes.
- Disallowing a URL and expecting its noindex to work.
- Blocking CSS or JavaScript and breaking rendering.
- A `Googlebot` group that forgets the rules in the `*` group.
- Using robots.txt to hide private files; it advertises them instead.
- Blocking Googlebot to stay out of AI Overviews. That removes you from Google Search.

## Questions to ask before you recommend anything
- Does the business want to appear in AI answers, and does it object to AI training?
- Which URL patterns get organic clicks today?
- Who can deploy robots.txt changes, and is the file monitored?

## Self-check before you answer
- Did I use the right control for each goal (crawl, index, privacy)?
- Did I test real URLs for every crawler in the policy?
- Did I date and source every AI crawler name?
- Did I check that no render resources are blocked?
- Did I flag any blocked pattern that currently earns clicks?

## Official documentation
- Introduction to robots.txt: https://developers.google.com/search/docs/crawling-indexing/robots/intro
- How Google interprets robots.txt: https://developers.google.com/search/docs/crawling-indexing/robots/robots_txt
- Robots meta tag and X-Robots-Tag: https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag
- Google crawlers and user agents: https://developers.google.com/search/docs/crawling-indexing/overview-google-crawlers
- RFC 9309, Robots Exclusion Protocol: https://www.rfc-editor.org/rfc/rfc9309
- OpenAI crawlers: https://platform.openai.com/docs/bots
- Bing Webmaster Guidelines: https://www.bing.com/webmasters/help/webmaster-guidelines-30fba23a

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/robots-directives/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

# Technical SEO — SKILL.md files for AI agents

[![16 skills](https://img.shields.io/badge/skills-16-0b7285)](#whats-inside)
[![Format: Agent Skills](https://img.shields.io/badge/format-SKILL.md-5f3dc4)](https://agentskills.io/specification)
[![Works with Claude Code & Codex](https://img.shields.io/badge/works%20with-Claude%20Code%20%7C%20Codex%20%7C%20Claude.ai-1c7ed6)](#install)
[![Licence: CC BY 4.0](https://img.shields.io/badge/licence-CC%20BY%204.0-2b8a3e)](https://creativecommons.org/licenses/by/4.0/)

Free, ready-to-run **technical SEO** skills for **Claude, Claude Code, OpenAI Codex** and any agent that reads the
[Agent Skills](https://agentskills.io/specification) format. Each skill is a single `SKILL.md`: a senior
practitioner's workflow — when to use it, the inputs to ask for, a numbered process, decision rules, an output
template and sources — loaded only when a task needs it.

Part of the [Marketing Skills library](https://github.com/afeezadebayo/marketing-skills) by
[Afeez Adebayo](https://afeezadebayo.com) · [Browse online](https://afeezadebayo.com/skills-md/marketing/technical-seo/)

## Install

Each skill is its own folder with a `SKILL.md`, so you can drop any folder straight into an agent.

**Claude Code**
```bash
git clone https://github.com/afeezadebayo/technical-seo-skills.git
cp -r technical-seo-skills/xml-sitemaps ~/.claude/skills/        # all projects
# or  .claude/skills/   inside one project
```

**OpenAI Codex**
```bash
cp -r technical-seo-skills/xml-sitemaps ~/.agents/skills/         # all repos
```

**Claude.ai / Desktop** — zip a skill folder, then **Settings → Capabilities → Skills → Upload skill**
(code execution must be enabled). To bulk-load the whole category into a Claude Project, custom GPT or
knowledge base, grab the single-folder `.zip` from the
[library page](https://afeezadebayo.com/skills-md/marketing/technical-seo/).

Then start a new session and ask your agent which skills it can use.

## What's inside

| Skill | What it does |
|---|---|
| [`technical-seo-audit`](technical-seo-audit/SKILL.md) | Run a complete, prioritised technical SEO audit of any website, from crawl and indexation to rendering, speed, schema and AI crawler access. |
| [`crawlability-indexation-audit`](crawlability-indexation-audit/SKILL.md) | Diagnose why pages aren't crawled, rendered or indexed, and fix the earliest broken step first. |
| [`robots-directives`](robots-directives/SKILL.md) | Write, audit, test and debug robots.txt, meta robots and X-Robots-Tag for search and AI crawlers. |
| [`xml-sitemaps`](xml-sitemaps/SKILL.md) | Plan, generate, audit, segment and monitor XML sitemaps and sitemap indexes. |
| [`canonicalization`](canonicalization/SKILL.md) | Find duplicate and near-duplicate URLs and make Google choose the canonical you want. |
| [`redirects-status-codes`](redirects-status-codes/SKILL.md) | Audit and fix redirects, chains, 404/410, soft 404s and 5xx, and build tested redirect maps. |
| [`site-migration`](site-migration/SKILL.md) | Plan, run and monitor SEO-safe migrations with benchmarks, redirect maps and a 90-day recovery watch. |
| [`internal-linking`](internal-linking/SKILL.md) | Audit and design internal linking and site architecture to route equity to the pages that matter. |
| [`faceted-navigation-parameters`](faceted-navigation-parameters/SKILL.md) | Control crawling and indexing of filters, facets, sorting, pagination and URL parameters. |
| [`javascript-seo`](javascript-seo/SKILL.md) | Make JavaScript-heavy sites (React, Next.js, Vue, SPAs) crawlable and indexable. |
| [`structured-data`](structured-data/SKILL.md) | Plan, write, validate and debug structured data (JSON-LD and schema.org) for rich results. |
| [`log-file-analysis`](log-file-analysis/SKILL.md) | Analyse server, CDN or hosting logs to see how Googlebot and AI crawlers really crawl a site. |
| [`core-web-vitals`](core-web-vitals/SKILL.md) | Diagnose and improve Core Web Vitals (LCP, INP, CLS) from field data, with template-level fixes. |
| [`lcp-optimisation`](lcp-optimisation/SKILL.md) | Diagnose and fix Largest Contentful Paint through its four subparts. |
| [`inp-optimisation`](inp-optimisation/SKILL.md) | Diagnose and fix Interaction to Next Paint across its three phases. |
| [`cls-optimisation`](cls-optimisation/SKILL.md) | Diagnose and fix Cumulative Layout Shift with reserved space, overlay banners and font fixes. |

## How these skills work

Agents use *progressive disclosure*: they read each skill's `name` and `description` (~100 tokens) at the start
of a session and load the full `SKILL.md` only when your request matches — so you can install many without
filling the context window.

## Author & licence

By [**Afeez Adebayo**](https://afeezadebayo.com) — Marketing, SEO & Growth (SEO & Organic Growth at Grey, YC W22).
[LinkedIn](https://www.linkedin.com/in/afeez-adebayo/) · [X](https://x.com/za_icon) · [Newsletter](https://afeezadebayo.com/newsletter/)

Released under [**CC BY 4.0**](https://creativecommons.org/licenses/by/4.0/) — use, share and adapt, including
commercially, with credit to Afeez Adebayo and a link to the source page in each file.

<sub>Keywords: technical SEO skills, SKILL.md, Agent Skills, Claude skills, Claude Code skills, Codex skills,
AI agent skills, SEO audit, crawlability, indexation, robots.txt, XML sitemaps, canonicalization, redirects,
JavaScript SEO, Core Web Vitals, structured data, log file analysis, site migration.</sub>

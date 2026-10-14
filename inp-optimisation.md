---
name: inp-optimisation
description: "Diagnose and fix Interaction to Next Paint: field data at the 75th percentile, slow interactions from attribution data, the three phases (input delay, processing, presentation), long tasks, yielding to the main thread, debouncing, web workers, third-party scripts, DOM size, layout thrashing, instant feedback and WordPress fixes. Use whenever Search Console flags poor INP, pages feel slow to respond, filters, menus or forms lag, after adding heavy scripts, or when someone wants to improve Core Web Vitals responsiveness."
license: CC-BY-4.0
metadata:
  author: Afeez Adebayo
  author_url: https://afeezadebayo.com
  author_linkedin: https://www.linkedin.com/in/afeez-adebayo/
  author_x: https://x.com/za_icon
  source: https://afeezadebayo.com/skills-md/marketing/technical-seo/inp-optimisation/
  category: marketing
  subcategory: technical-seo
  version: "1.1.0"
  created: "2026-10-09"
  updated: "2026-10-13"
---

# INP Optimisation

Interaction to Next Paint (INP) measures how quickly a page responds to user interactions such as clicks, taps and key presses, across the whole visit. It replaced First Input Delay as a Core Web Vital in March 2024; an INP of 200 milliseconds or less is good, measured at the 75th percentile of real page loads. [Confirmed, web.dev INP] Poor INP feels like a sluggish page: menus that hesitate, buttons that do not react, filters that freeze. The usual cause is too much JavaScript running on the main thread.

This skill diagnoses and fixes INP: finding slow interactions in field data, breaking each into its three phases (input delay, processing duration and presentation delay), and applying fixes such as breaking up long tasks, yielding to the main thread, reducing JavaScript and third-party scripts, simplifying the DOM, avoiding layout thrashing and giving immediate visual feedback.

## Operating principles
1. **Find real slow interactions.** Use field data to see which interactions and pages are slow.
2. **Split the interaction into phases.** Fix the phase that takes longest.
3. **Keep the main thread free.** Long tasks over 50 milliseconds block responses.
4. **Respond first, work later.** Show visual feedback immediately; do heavy work after.
5. **Less JavaScript wins.** Remove, defer or move work off the main thread.

## INP phases
| Phase | What happens | Common causes | Fixes |
|---|---|---|---|
| Input delay | Wait before event handlers run | Other long tasks running (scripts loading, timers, third parties) | Break up long tasks, defer non-critical scripts, reduce third parties |
| Processing duration | Event handlers run | Heavy handlers, synchronous work, large state updates | Do less in handlers, yield, debounce, move work to web workers |
| Presentation delay | Time to render the next frame | Large DOM, expensive style and layout, forced reflows | Smaller DOM, `content-visibility`, avoid layout thrashing |

## When to use, and when not to
Use it when Search Console flags INP issues, pages feel slow to respond, interactive features (filters, menus, forms, carousels) lag, or after adding heavy scripts.

Don't use it for loading speed (`lcp-optimisation`) or visual stability (`cls-optimisation`).

## Inputs
- Field INP by template and device (Search Console, CrUX, RUM).
- Which interactions are slow (from RUM attribution).
- Performance traces from DevTools.
- Tech stack, frameworks, tag manager and third-party scripts.

## Workflow

### Step 1. Confirm in field data
Find templates with poor INP on mobile; mobile devices have less processing power.

### Step 2. Find the slow interactions
Use the web-vitals library attribution build or a RUM tool to record the interaction target, type and phase timings.

### Step 3. Reproduce in the lab
Record the interaction in DevTools Performance with CPU throttling; look for long tasks around the interaction.

### Step 4. Reduce input delay
Defer or remove scripts that run during interactions; break up long tasks at load; delay third-party scripts until after the page is interactive or user consent.

### Step 5. Shorten processing
Move non-urgent work out of the handler; yield to the main thread between steps (for example with `scheduler.yield()` where supported, or `setTimeout`); debounce input handlers; use web workers for heavy computation.

### Step 6. Speed up presentation
Reduce DOM size, avoid reading layout values right after writing styles (layout thrashing), use CSS `content-visibility` for off-screen content, simplify animations.

### Step 7. Give instant feedback
Update the visual state (pressed button, spinner, opened menu) immediately, then do the heavier work.

### Step 8. Verify
Lab test the interaction; watch field INP over the following 28 days.

## Code patterns
```js
// Respond first, then do heavy work after yielding
button.addEventListener('click', async () => {
  button.classList.add('is-loading');   // immediate feedback
  await yieldToMain();                  // let the browser paint
  await applyFilters();                 // heavy work
});

function yieldToMain() {
  if ('scheduler' in window && 'yield' in scheduler) return scheduler.yield();
  return new Promise(resolve => setTimeout(resolve, 0));
}

// Debounce typing in a search box
let t; input.addEventListener('input', () => { clearTimeout(t); t = setTimeout(search, 200); });
```

## Common INP problems and fixes
| Problem | Fix |
|---|---|
| Tag manager loads many tags on every page | Audit tags; remove unused; delay non-essential |
| Chat widgets and heatmaps | Load after interaction or idle; remove if unused |
| Large framework hydration | Partial or progressive hydration; server components |
| Mega menus with huge DOM | Render on demand; simplify markup |
| Filters re-rendering whole lists | Virtualise long lists; update only changed items |
| Consent banners | Lightweight implementation; avoid heavy scripts on accept |

## WordPress and page builder notes
- Remove unused plugins and their front-end scripts.
- Delay JavaScript execution for non-critical scripts until user interaction, testing carefully.
- Avoid heavy sliders, animation libraries and multiple analytics tools.
- Reduce DOM size from nested page-builder sections.
- Load third-party embeds (maps, video, social feeds) on click.

## Finding long tasks
In DevTools Performance, record the interaction with 4x CPU slowdown. Long tasks show with red corners. Expand them to see which script and function ran. Group findings by source: your own code, frameworks, tag manager, and each third party. Fix or delay the biggest source first.

## Measuring in the field
Use the web-vitals library attribution build to send INP value, the interaction target selector, interaction type and the three phase timings to analytics. Group by page type and device, and review the worst interactions weekly. Without attribution, teams guess and often optimise the wrong feature.

## Third-party script policy
Keep a register of every third-party script with owner, purpose, pages, load timing and its measured effect on INP. New scripts need approval and a performance check. Review quarterly and remove scripts nobody uses.

Repeat field reviews monthly, since new features and scripts often bring INP problems back.

## Quick wins
Remove unused tags and plugins, delay chat and heatmap scripts, add instant visual feedback to the slowest buttons and debounce search and filter inputs. These often fix moderate INP problems before deeper code work is needed.

## Edge cases and judgement calls
| Situation | What to do |
|---|---|
| INP poor only on low-end phones | Test with CPU throttling; reduce JavaScript further |
| Third-party script is business-critical | Load later, isolate or ask the vendor for a lighter version |
| Single-page app | Optimise route changes and state updates; split bundles |
| No RUM data | Add web-vitals attribution; start with likely heavy interactions |
| Delay-JS plugin breaks features | Exclude essential scripts; test key journeys |
| Good lab, poor field | Real interactions differ; rely on attribution data |

## Handoffs to related skills
- Full programme: `core-web-vitals`
- Loading: `lcp-optimisation`
- Layout shifts: `cls-optimisation`
- JavaScript rendering for SEO: `javascript-seo`

## Worked example
A marketplace's search page had a mobile INP of 480 milliseconds. Attribution showed filter clicks were slowest, with a 210 ms input delay from a tag manager firing several tags and a 220 ms processing time re-rendering a 600-item list. The fix delayed non-essential tags, added immediate button feedback, yielded before filtering, virtualised the list and debounced the price slider. Lab interaction time fell under 120 ms, and field INP reached 170 ms within a month.

## Common mistakes
- Looking only at load-time metrics.
- Optimising interactions that are not the slow ones.
- Doing all work inside the event handler before any visual feedback.
- Ignoring third-party scripts.
- Huge DOMs from page builders.

## Output format
```
1. Field INP by template and device
2. Slowest interactions with phase breakdown
3. Prioritised fixes
4. Code or configuration changes
5. Verification plan
```

## Self-check before you answer
- Were slow interactions found in field data?
- Is each broken into input delay, processing and presentation?
- Do fixes target the longest phase?
- Is immediate feedback given before heavy work?

---
Created by Afeez Adebayo, Marketing, SEO & Growth Expert
Website: https://afeezadebayo.com
LinkedIn: https://www.linkedin.com/in/afeez-adebayo/
X: https://x.com/za_icon
Source: https://afeezadebayo.com/skills-md/marketing/technical-seo/inp-optimisation/

Licence: CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). You may copy, share and adapt this skill, including for commercial use, as long as you credit Afeez Adebayo and link to the source URL above. Keep this notice in any copy or adapted version.

---
name: ai-news
description: >-
  Produces a daily AI-only news briefing. Opens with a witty, clever-and-funny
  "resume" teaser, then five ranked top-3 categories: General & Trending AI
  News, Business & Enterprise AI News, AI Research News, Agentic AI News, and
  Quantum AI News. Personalizable via the settings block at the top of this
  file (role, focus areas). Requires an agent with live web search (e.g. a
  web_search tool) to pull current stories -- not bundled in this skill
  package. Use whenever the user asks for their "AI morning briefing," "AI
  briefing," "AI news roundup," "daily AI update," or similar -- even
  phrased casually ("what's up in AI today", "AI rundown please") or in
  another language. Always use this skill instead of improvising an ad hoc
  news summary -- the structure is tuned and should stay consistent every
  run.
---

# AI News Briefing

## Configuration

Edit the values below for yourself, then save the skill again. Both are optional.

```
role: ""              # e.g. "Backend Engineer", "Product Manager", "Founder", "Researcher" -- leave blank for a general-audience briefing
focus_areas: ""        # e.g. "agentic AI, MCP, multi-agent orchestration" or "AI in healthcare" -- comma-separated topics to prioritize
```

The briefing itself is always written in English, no matter what language the request comes in -- this is not user-configurable.

If `role` and `focus_areas` are blank, keep every category broad and general-audience -- no filtering bias. If they're filled in, use them to bias relevance and depth **within whichever category they naturally fit** (e.g. "agentic AI" focus areas mostly land in category 4; "AI in healthcare" might land in categories 1 or 2 depending on the story). Don't force a connection to a category a story doesn't genuinely fit, and don't let personalization crowd out the other four categories -- they still need their own top 3 regardless of the reader's role.

## Why the research step matters

AI news moves on an hourly cycle -- model releases, funding rounds, and papers from yesterday are stale by tomorrow. Never fill in any of the five categories from memory or training data. Always search fresh (web_search / web_search_fast, with web_fetch on any article whose snippet is too thin to summarize responsibly) before writing a single bullet. A briefing built on stale or misremembered stories defeats the purpose of a *daily* briefing.

If this skill is running as part of an unattended scheduled task, don't pause to ask clarifying questions -- gather everything and deliver the finished briefing as the final output.

## What to research

Run one search sweep per category (in parallel where your tools support it), restricted to roughly the last 24-48 hours. For each category, gather 5-8 candidate stories, then keep only the **top 3**, ranked by genuine significance -- not just recency. A quiet day with 3 solid stories beats padding with filler.

1. **General & Trending AI News** -- the broad "everyone's talking about this" pulse: major model or product launches, viral AI moments, notable partnerships, things trending across tech media generally. Lean toward stories that carry business or industry weight over pure novelty. This category is intentionally wide-aperture -- it's the "front page," not a niche cut.
2. **Business & Enterprise AI News** -- the money and strategy layer specifically: funding rounds, M&A, IPOs, earnings tied to AI, enterprise adoption/deployment case studies, competitive moves between major AI vendors, AI-related regulation or antitrust action with market consequences.
3. **AI Research News** -- papers, benchmarks, and breakthroughs out of research labs and academia: new architectures, evals, safety/alignment research, notable arXiv papers picked up by the community, open-weight model releases framed as research contributions.
4. **Agentic AI News** -- tools, frameworks, protocols, and applications specific to agentic AI: agent orchestration frameworks, agent-interop standards (e.g. MCP, A2A), agent skills/tooling, agent evaluation and alignment work, notable agentic platform or product launches, enterprise agentic deployments. Search for this separately from category 1 rather than treating it as leftovers -- it's a fast-moving domain in its own right. If `focus_areas` names specific agentic tools or standards, prioritize genuinely new, specific developments in those over generic "AI agents are the future" commentary.
5. **Quantum AI News** -- the intersection of quantum computing and AI/ML specifically: quantum machine learning research, quantum hardware milestones framed around AI workloads, hybrid quantum-classical AI approaches, enterprise quantum-AI initiatives. This is a thinner beat than the other four -- if a genuine quantum-AI story isn't there on a given day, say so plainly rather than stretching a generic quantum-computing story to fit.

**Avoid duplicate stories across categories.** If a story could plausibly fit two sections (e.g., a big funding round for an agentic-AI startup touches both category 2 and category 4), place it in the single best-fit category and don't repeat it elsewhere.

For every story used, keep the source name, URL, and publish date so a full markdown link can be cited -- a bare "(Site Name)" with no URL is not acceptable. Summaries must be written in your own words (1-2 sentences); never lift more than a short attributed phrase from a source.

## Output format

Write the finished briefing as markdown, following this structure exactly (the structure is tuned to stay consistent every day so the user knows where to look):

```
# AI Morning Briefing — [today's date, e.g. September 19, 2026]

## ☕ Today's Resume

[3-5 sentences, clever and genuinely funny (dry humor, puns, wry observations,
light irony are all fair game), that teases the day's actual stories below by
name. This is flavor and a hook, not a summary -- it should make the reader
want to jump into the sections. Vary the joke style and angle every day; don't
reuse the same gag structure. Never force humor onto a genuinely grim story (a
layoff, a safety incident, a lawsuit) -- reference it straight and put the
jokes elsewhere in the resume.]

---

## 1. General & Trending AI News

- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)

---

## 2. Business & Enterprise AI News

- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)

---

## 3. AI Research News

- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)

---

## 4. Agentic AI News

- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)

*[Optional, only when a story genuinely connects to the reader's `role` /
`focus_areas` -- one short italic line noting the practical angle for them.
Skip it entirely rather than force a connection, and skip it always if
Configuration is blank.]*

---

## 5. Quantum AI News

- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)
- **[Bold short headline]** *([date])*: [1-2 sentence summary]. [Site Name](https://full-url)

[If no genuine quantum-AI story exists in the window, say so explicitly here
(e.g. "Quiet day on the quantum-AI front -- nothing new enough to report.")
rather than padding with an unrelated quantum-computing story.]

---

**Sources:** [Site Name](https://full-url) · [Site Name](https://full-url) · ...
```

Notes on the format:

- Every heading is `## N. Category Name`, numbered 1-5 in this exact order, plus the unnumbered "Today's Resume" section at the top. Don't reorder, skip, or merge sections -- if a category genuinely has nothing new, say so inside that section (see category 5's guidance, which applies to any category on a slow day) rather than omitting it or forcing in a weak story.
- Use `---` horizontal rules between sections, as shown.
- Each category has **exactly 3 bullets** -- top 3, ranked by significance. If fewer than 3 genuine stories exist for a category, include what's real and note that it was a quiet day rather than inventing a third.
- **All** source citations -- inline under each bullet and in the final Sources line -- must be real, full, clickable markdown links (`[Site Name](https://...)`), never bare text like `(Site Name, date)` with no URL. If a URL genuinely can't be found for a fact, say so rather than inventing one.
- Keep bullets tight (1-2 sentences) -- this is a scannable morning briefing, not a deep-dive report.
- The resume section is the one place to have fun with tone; the five category sections stay factual and clean.
- The whole briefing is always written in English, regardless of what language the request came in.

## Delivering the briefing

- If this is a one-off request in a live conversation, reply with the finished markdown briefing as your message.
- If this is running unattended (e.g. fired by a scheduled task), produce the finished briefing as the final response so it reaches the user directly -- don't wait for confirmation or ask clarifying questions first.

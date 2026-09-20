---
name: skill-finder
description: >-
  Searches popular agentic skill indexes and recommends the best matching
  skills for a task described in the conversation. Fetches live results from
  https://agenticskills.io/skills/find-skills,
  https://mcpservers.org/agent-skills/vercel/find-skills, and
  https://mcpmarket.com/tools/skills, scores each result against the
  user-provided task description, and returns a ranked shortlist with
  rationale, source links, and a ready-to-use activation note for each match.
  The task description comes from the user's prompt — it is never hard-coded
  in this skill. Requires web fetch capability. Use whenever the user says
  "find a skill for", "which skill should I use", "what skill handles",
  "is there a skill that", "search skills for", "skill for this task",
  "recommend a skill", "look up a skill", or any similar phrasing in any
  language. Always use this skill rather than guessing or listing skills from
  memory — fetch the live indexes so recommendations are grounded and current.
---

# Skill Finder

Searches popular agentic skill indexes and returns a ranked list of skills that best match a task the user described in their prompt.

---

## Configuration

```
indexes: ""       # Comma-separated list of index URLs to query.
                  # Default (leave blank): all three indexes below are used.
                  #   https://agenticskills.io/skills/find-skills
                  #   https://mcpservers.org/agent-skills/vercel/find-skills
                  #   https://mcpmarket.com/tools/skills
                  # Override only if the user explicitly wants to restrict the search.
max_results: ""   # Maximum number of skills to include in the final shortlist. Default: 5.
                  # Applies across all sources combined — deduplicate before counting.
show_all: ""      # "true" | "false" (default).
                  # If "true", show every discovered skill with a relevance label
                  # instead of a ranked shortlist. Use only when explicitly requested.
```

If `indexes` is blank, query all three default indexes.
If `max_results` is blank, default to `5`.
If `show_all` is blank or `"false"`, return only the top-ranked matches.

---

## Step 1 — Capture the task description

The task description is whatever the user wrote in their prompt. Do **not** ask a clarifying question before proceeding. If the prompt is short (e.g. "find a skill for LinkedIn"), that is enough — infer the intent and proceed. Only ask for clarification if the input is genuinely ambiguous between two completely different domains, and only ask once.

---

## Step 2 — Query the skill indexes

For each index URL in the `indexes` config (or all three defaults), fetch the page content using the available web fetch tool. Treat each URL as a browsable skill directory or search endpoint.

### Fetch strategy per index

| Index URL | Fetch approach |
|---|---|
| `https://agenticskills.io/skills/find-skills` | Fetch the page and extract all listed skill entries (name, description, tags, link). If the page supports query parameters (e.g. `?q=<task>`), append the task description as the search query. |
| `https://mcpservers.org/agent-skills/vercel/find-skills` | Fetch the page and extract skill entries. If a search/filter parameter is supported, pass the task description. |
| `https://mcpmarket.com/tools/skills` | Fetch the page and extract all tool/skill entries (name, description, category, link). |

If a URL is unreachable or returns an error, log a one-line note ("Index `<url>` unavailable — skipped") and continue with the remaining indexes. Do not abort the full search because one source fails.

**Do not answer from memory or training data.** All skill candidates must come from content actually fetched in this session. If all indexes fail to load, report that clearly and do not fabricate recommendations.

---

## Step 3 — Deduplicate and normalize

After collecting raw entries from all sources:

1. **Deduplicate** — if the same skill (by name or clearly identical description) appears in more than one index, merge the entries and note all source URLs.
2. **Normalize** — for each unique skill, capture:
   - `name`: skill display name.
   - `description`: the description as listed on the index page.
   - `source`: the index URL where it was found (or both/all if deduplicated).
   - `link`: the direct skill page or installation URL if available.
   - `tags` / `category`: if provided by the index.

---

## Step 4 — Score and rank

For each normalized skill, evaluate relevance to the user's task description across four dimensions:

| Dimension | Weight | What to assess |
|---|---|---|
| **Trigger match** | High | Does the task description contain or closely paraphrase any trigger phrase, tag, or keyword in the skill's name or description? |
| **Domain overlap** | High | Does the skill's stated output or purpose directly address the user's task domain? |
| **Capability fit** | Medium | Are the skill's stated requirements (web search, MCP server, API key, etc.) likely available in a standard agent context? |
| **Scope alignment** | Low | Is the granularity a good match — not too broad, not too narrow — for the described task? |

Assign each skill a relevance tier:
- **Strong match** — directly solves or substantially accelerates the described task.
- **Partial match** — relevant to part of the task or a closely related use case.
- **Weak/no match** — not relevant; exclude from the shortlist unless `show_all` is `"true"`.

Sort strong matches first, then partial matches. Within each tier, order by domain overlap score.

---

## Step 5 — Produce the output

Open with a one-line header:

```
## Skill recommendations for: "<user's task description>"
```

---

### Shortlist (default, `show_all: false`)

Return the top `max_results` skills. For each, output a block in this exact format:

```
### [Skill Name](direct link if available, otherwise index source URL)

**Source:** [Index name](index URL) — and any additional sources if deduplicated.

**Relevance:** Strong match | Partial match

**Why this skill:** One or two sentences explaining precisely why this skill addresses the described task. Name the specific feature or output that makes it relevant — not a generic statement.

**Key capabilities:** Bullet list of 2-4 things this skill does that are useful for the described task, drawn from the fetched description.

**To activate:** Copy and paste the following into a new chat message or task prompt:

> Use the `<skill-name>` skill. [Restate the user's task in one line as a clean activation prompt.]
```

Separate each block with `---`.

After the last block, add:

- A **Coverage note** (one line) stating whether any part of the described task falls outside all discovered skills, and if so, what kind of skill would fill the gap.
- A **Sources checked** line listing each index URL and whether it was successfully fetched or skipped.

---

### Full index view (`show_all: true`)

When `show_all` is `"true"`, produce a markdown table instead:

```
| Skill | Source | Relevance | One-line reason |
|---|---|---|---|
| [Skill Name](link) | [Index](url) | Strong / Partial / Weak | ... |
```

Followed by the same Coverage note and Sources checked line.

---

## Execution rules

- **Never answer from memory.** All skill candidates must come from content fetched live in this session.
- **No hallucinated skills.** If a skill name comes to mind from training data but was not found on any fetched page, do not include it.
- **Do not ask unnecessary questions.** If the task description is clear enough to fetch and evaluate, proceed immediately.
- **Unattended execution:** If triggered by a scheduled task or automation, proceed with the task description in the prompt and produce the full output directly — do not wait for confirmation.
- **If no skills match:** Return a short message explaining that no skills in the fetched indexes address the described task and describe in one sentence what kind of skill would be needed.
- **If all indexes fail:** Report each failure clearly and suggest the user check connectivity or try the URLs directly. Do not fabricate results.

---

## Example output shape

*(Illustrative — actual content must come from live-fetched index pages.)*

```
## Skill recommendations for: "draft a post about my product launch for LinkedIn"

---

### LinkedIn Post Writer

**Source:** [agenticskills.io](https://agenticskills.io/skills/find-skills) · also on [mcpservers.org](https://mcpservers.org/agent-skills/vercel/find-skills)

**Relevance:** Strong match

**Why this skill:** Handles LinkedIn post drafting for announcements including product launches, with configurable voice and post length.

**Key capabilities:**
- Detects post type (announcement, insight, share) from the input
- Produces hook, body, and CTA following LinkedIn platform rules
- Returns craft notes explaining structural decisions

**To activate:**

> Use the `linkedin-post` skill. Draft a LinkedIn announcement post about my product launch.

---

**Coverage note:** All aspects of the described task are addressed by the skills above.

**Sources checked:** agenticskills.io ✓ · mcpservers.org ✓ · mcpmarket.com ✓
```

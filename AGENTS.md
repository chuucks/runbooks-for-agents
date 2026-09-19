# AGENTS.md

Guidance and standards for AI coding agents operating on the `runbooks-for-agents` repository.

---

## 1. Repository Overview

`runbooks-for-agents` is an open-source library of structured, versioned **Agent Skills** (runbooks). Each skill turns open-ended, one-off prompts into deterministic, high-quality, and repeatable procedures that agents can execute interactively or unattended (e.g., via scheduled cron tasks).

### Repository Structure

```text
runbooks-for-agents/
├── <skill-name>/
│   └── SKILL.md       # Skill definition, frontmatter, configuration & instructions
├── LICENSE            # MIT License
├── README.md          # Public documentation & skills directory table
└── AGENTS.md          # Agent contributor and maintenance instructions
```

---

## 2. Skill Design & Authoring Standards

When adding or modifying skills in this repository, always follow these core standards:

### A. Frontmatter Schema
Every skill must start with valid YAML frontmatter containing:
- `name`: Lowercase kebab-case identifier (matches folder name, e.g. `ai-news`).
- `description`: A comprehensive description defining:
  1. What the skill produces.
  2. The exact trigger phrases (both formal and casual/multilingual).
  3. Tool/capability dependencies (e.g., web search tools, visualization tools).
  4. Instruction to invoke the skill instead of improvising ad-hoc responses.

```yaml
---
name: skill-name
description: >-
  Produces [output description]. Includes [key sections or features]. Requires
  [dependencies, e.g., web search tool]. Use whenever the user asks for
  ["trigger 1", "trigger 2", or similar phrasing in any language]. Always use
  this skill rather than improvising an ad hoc response.
---
```

### B. In-File Configuration Block
Skills designed with customizable parameters should include an easy-to-edit configuration block near the top of `SKILL.md`:
- Use commented YAML-like key-value pairs.
- Provide clear defaults and fallback behaviors when keys are left blank.
- Specify expected data types or comma-separated formats.

### C. Zero-Hallucination & Grounding Principles
- For volatile or real-time data (news, stock prices, weather, exchange rates), skills **must explicitly prohibit answering from training memory**.
- Mandate live web search and deep fetch steps before generation.
- Require fully qualified, clickable markdown links (`[Source](https://...)`) for all citations — never bare text or placeholder domains.
- Provide explicit fallback guidance when no fresh information is found (e.g., explicitly reporting a quiet news cycle rather than inventing stories or using stale data).

### D. Strict Output Formatting
- Structure outputs with explicit markdown headings, delimiters (`---`), and tables where appropriate.
- Include a template example in the skill instructions demonstrating the expected shape and tone of the response.
- Keep tone rules scoped (e.g., humorous intro hooks vs. factual body sections).

### E. Unattended Execution Friendly
- Skills must work seamlessly when triggered by background schedules or autonomous routines.
- Explicitly instruct the agent **not to ask clarifying questions** when running unattended — gather available data and produce the finalized output directly.

---

## 3. Workflows for Agents Working on This Repo

### Adding a New Skill
1. Create a directory named `<skill-name>/`.
2. Author `<skill-name>/SKILL.md` following the standards above.
3. Update [`README.md`](README.md:1) by adding the new skill to the **📦 Skills Directory** table (Skill name, Category, Description, and Requirements).

### Modifying an Existing Skill
1. Preserve structural consistency and existing configuration keys unless deliberately refactoring.
2. Ensure triggers remain robust across phrasing variations and languages.
3. If dependencies or major capabilities change, update the corresponding entry in [`README.md`](README.md:1).

---

## 4. Quality Checklist

Before finalizing any new or updated skill:
- [ ] Directory name matches the frontmatter `name`.
- [ ] YAML frontmatter is valid and contains a detailed `description` with trigger phrases.
- [ ] Markdown syntax renders cleanly.
- [ ] Any required tool capabilities (e.g., web search, MCP) are clearly noted.
- [ ] [`README.md`](README.md:1) skills index table is updated and accurate.

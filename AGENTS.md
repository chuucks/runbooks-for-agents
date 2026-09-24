# AGENTS.md

This file provides guidance to agents when working with code in this repository.

`runbooks-for-agents` is a library of structured, versioned **Bob Agent Skills**. Each skill is a `SKILL.md` file designed to be loaded by [IBM Bob](https://www.ibm.com/bob) via the `use_skill` tool, turning open-ended prompts into deterministic, repeatable procedures.

---

## Repository Structure

```text
runbooks-for-agents/
├── <skill-name>/
│   └── SKILL.md       # The skill file — always named SKILL.md, never <skill-name>.md
├── README.md          # Skills directory table — must be updated for every new/changed skill
└── AGENTS.md
```

---

## Bob-Specific Skill Authoring Standards

### Frontmatter (required)
Every `SKILL.md` must open with YAML frontmatter using the `>-` block scalar for `description`:

```yaml
---
name: skill-name          # lowercase kebab-case, must match directory name
description: >-
  Produces [output]. Requires [tool deps]. Use whenever the user asks for
  ["trigger 1", "trigger 2", similar phrasing in any language]. Always use
  this skill rather than improvising an ad hoc response.
---
```

The `description` field is what Bob reads to decide whether to activate the skill — it must cover:
- What the skill produces
- Exact trigger phrases (formal + casual + multilingual)
- Tool/capability dependencies (e.g., web search, web fetch)
- An explicit directive: *"Always use this skill rather than improvising…"*

### Configuration Block (when parameters exist)
Place an easy-to-edit config block immediately after the frontmatter, before any headings. Two accepted styles:

**Fenced code block** (simple key/value, as in `morning-briefing`, `paper-summary`):
```
stock_ticker: ""   # e.g. "IBM" — leave blank to skip this section
```

**HTML comment block** (richer inline docs, as in `troubleshoot-agent-issues`):
```
<!-- ====================================================
  key: default   # explanation
  ==================================================== -->
```

All keys must have documented defaults and fallback behavior. Never leave a key undocumented.

### Zero-Hallucination Grounding
- Skills using volatile data (news, prices, weather) **must prohibit answering from training memory** and mandate a live web-search step first.
- All citations must be fully qualified clickable markdown links — never bare URLs or placeholder domains.
- Provide explicit fallback messaging when no fresh data is found.

### Output Formatting
- Use explicit markdown headings, `---` delimiters, and tables where appropriate.
- Include an output template example inside the skill to anchor tone and shape.

### Unattended Execution
- Skills must never stall waiting for user input when triggered by a schedule or automation.
- Add an explicit **Unattended mode** callout to any step that would pause for clarification: infer from context, apply a documented default, and continue.
- The `description` frontmatter must state the unattended behavior so Bob knows before loading the body.

---

## Workflows for Agents

### Adding a New Skill
1. Create `<skill-name>/SKILL.md` — the file **must** be named `SKILL.md`.
2. Follow all authoring standards above.
3. Add a row to the **📦 Skills Directory** table in [`README.md`](README.md) (columns: Skill, Category, Description, Requirements).

### Modifying an Existing Skill
1. Preserve existing config keys unless deliberately refactoring.
2. Keep triggers robust across phrasing variations and languages.
3. Update the [`README.md`](README.md) table row if category, description, or requirements change.

---

## Quality Checklist

- [ ] Directory name matches frontmatter `name`.
- [ ] File is named `SKILL.md` (not `<skill-name>.md` or anything else).
- [ ] `description` uses `>-` block scalar and includes trigger phrases + unattended behavior note.
- [ ] Config block present (if skill has parameters), with all keys documented.
- [ ] Markdown renders cleanly.
- [ ] Tool/capability dependencies noted in frontmatter `description`.
- [ ] [`README.md`](README.md) Skills Directory table updated.

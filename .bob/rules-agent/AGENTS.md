# Project Coding Rules (Agent Mode)

This is a **Bob Skills library** — a collection of plain-text `SKILL.md` files, not application code. There are no build steps, no test runners, and no linters.

## Non-Obvious Rules

- **File must be named `SKILL.md`** — never `<skill-name>.md`. Bob's `use_skill` tool locates skills by directory name, not filename. A wrong filename silently breaks loading.
- **`description` uses `>-` block scalar** — not `|`, not a plain string. Bob's skill registry parses this field; wrong scalar type causes the description to be truncated or malformed.
- **Directory name must exactly match frontmatter `name`** — this is how Bob resolves `use_skill("skill-name")` to a file path. A mismatch causes a silent load failure.
- **Config block goes between frontmatter and first heading** — Bob users edit this block to personalize the skill. If placed after a heading, users miss it.
- **`README.md` table must be updated for every new or renamed skill** — it is the human-facing directory AND the source of truth for what skills exist.
- **No code, dependencies, or executables** — skills are pure markdown. If a skill needs a tool (web search, file read), it declares it in `description` as a requirement; it does not bundle or install it.
- **Volatile-data skills must forbid training-memory answers** — add an explicit prohibition step (e.g., "Never answer from memory — always run a live search first") or the skill will produce stale/hallucinated results.

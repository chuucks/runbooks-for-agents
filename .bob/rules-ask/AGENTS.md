# Project Documentation Rules (Ask Mode)

This repo is a **Bob-native skills library** — the skills here are loaded by Bob via `use_skill`, not by other agent frameworks.

## Non-Obvious Context

- **Skills are loaded by Bob, not executed as scripts** — when a user asks "what does this skill do", point them at the `description` frontmatter field; that's the exact text Bob uses to decide activation.
- **`SKILL.md` is the canonical reference** — `README.md` is a human directory. If there's a discrepancy, the `SKILL.md` body wins for behavioral questions.
- **Two config styles coexist intentionally** — fenced code block (simple params) vs. HTML comment block (richly documented params). Both are valid; don't suggest "fixing" one to match the other.
- **Unattended execution is a first-class requirement** — every skill must work when triggered by Bob's scheduled tasks with no human in the loop. This is documented behavior, not aspirational.
- **"Requirements" column in README means Bob tool capabilities** — e.g., "Web search capability" means the Bob session must have a `web_search` tool available, not that users need to install anything.

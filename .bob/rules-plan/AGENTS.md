# Project Architecture Rules (Plan Mode)

This repo is a **flat-file Bob Skills library** — no application architecture, no services, no APIs.

## Non-Obvious Architectural Constraints

- **One directory = one skill, one file** — the structure is intentionally flat. Do not introduce subdirectories inside a skill folder (e.g., no `skill-name/docs/`, no `skill-name/examples/`). Everything lives in `SKILL.md`.
- **Skills are stateless markdown** — they hold no runtime state. All configuration is stored in the skill file itself (the config block). If a skill requires persistent state, it must document how the *user* manages it externally.
- **The `description` frontmatter is a contract with Bob's skill registry** — changing trigger phrases or removing the `>-` scalar changes how Bob routes user requests. Treat it as a public API.
- **No cross-skill dependencies** — skills must be fully self-contained. One skill must never reference or depend on another skill's content or output format.
- **README.md is the only index** — there is no generated manifest or registry file. The `README.md` table is what Bob's skill-finder and human contributors use. It must stay in sync with the filesystem.
- **Volatile-data pattern is mandatory, not optional** — any skill using real-time data (prices, weather, news) must include an explicit "fetch before answering" gate. This prevents a class of hallucination bugs that would be invisible in dry-run testing.

# runbooks-for-agents

> Bob Agent Skills — versioned, reusable skills for IBM Bob.

A curated, growing library of **Bob Agent Skills** — structured `SKILL.md` files loaded by [IBM Bob](https://www.ibm.com/bob) via the `use_skill` tool. Each skill turns a one-off prompt into a repeatable, deterministic procedure that runs reliably in both interactive chat and unattended scheduled workflows.

> **This library is designed for IBM Bob.** Skills are activated inside a Bob session with `use_skill("skill-name")`. They are not compatible with other agent frameworks out of the box.

---

## 📦 Skills Directory

| Skill | Category | Description | Requirements |
|---|---|---|---|
| [`ai-news`](./ai-news/SKILL.md) | Productivity | Daily 5-category curated AI news briefing with dry-humor summary and verified markdown citations. | Web search capability |
| [`morning-briefing`](./morning-briefing/SKILL.md) | Productivity | Personalized daily dashboard for stock price, FX rate, and weather with market move rationale. | Web search capability |
| [`paper-summary`](./paper-summary/SKILL.md) | Research & Analysis | Structured lay-language summary of any research paper: problem, method, key results, limitations, and why it matters. Configurable audience level and optional focus lens. | Web fetch capability |
| [`linkedin-post`](./linkedin-post/SKILL.md) | Content & Communication | Drafts ready-to-publish LinkedIn posts (insight, announcement, or share) in a configurable voice and length, with craft notes explaining key decisions. | None |
| [`skill-finder`](./skill-finder/SKILL.md) | Productivity | Searches popular agentic skill indexes (agenticskills.io, mcpservers.org, mcpmarket.com) and recommends ranked matching skills for any described task, with rationale and ready-to-use activation prompts. | Web fetch capability |
| [`troubleshoot-agent-issues`](./troubleshoot-agent-issues/SKILL.md) | Engineering | Platform-agnostic agent debugger — gathers the issue description, scans trace logs, identifies root causes (tool schema mismatches, routing failures, context loss, auth errors, etc.), and proposes or applies targeted fixes across LangChain, CrewAI, AutoGen, OpenAI Assistants, IBM watsonx Orchestrate, Vertex AI, Azure AI Foundry, and custom pipelines. | None |

---

## 🛠️ Repository Structure

Each skill lives in its own directory with a standard `SKILL.md` file containing YAML frontmatter and clear operational instructions:

```text
runbooks-for-agents/
├── ai-news/
│   └── SKILL.md
├── morning-briefing/
│   └── SKILL.md
├── .bob/                   # Bob-specific mode rules (not skills)
│   ├── rules-agent/
│   │   └── AGENTS.md
│   ├── rules-ask/
│   │   └── AGENTS.md
│   └── rules-plan/
│       └── AGENTS.md
├── .bobignore              # Files Bob should not index
├── AGENTS.md               # Agent authoring guidance
├── LICENSE
└── README.md
```

---

## 🤝 Contributing & Standards

Every skill in this repo adheres to:
- **Bob-native format**: YAML frontmatter with `>-` description scalar, loaded via Bob's `use_skill` tool.
- **Zero Hallucination Grounding**: Volatile-data skills always mandate fresh web lookups with full source citations — never answered from training memory.
- **Strict Output Consistency**: Predictable schemas, tables, and fallback messaging for every run.
- **Unattended Execution Friendly**: Self-contained instructions that never stall asking interactive questions when run via Bob's scheduled tasks.

See [`AGENTS.md`](AGENTS.md) for the full skill authoring standard.

---

## 📄 License

Distributed under the [MIT License](LICENSE).

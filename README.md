 # runbooks-for-agents

> Versioned agent skills for everyday productivity and engineering work.

A curated, growing library of agent skills that turn one-off prompts into repeatable, structured procedures. Built to run reliably both in interactive chat and in unattended scheduled workflows.

---

## 📦 Skills Directory

| Skill | Category | Description | Requirements |
|---|---|---|---|
| [`ai-news`](./ai-news/SKILL.md) | Productivity | Daily 5-category curated AI news briefing with dry-humor summary and verified markdown citations. | Web search capability |
| [`morning-briefing`](./morning-briefing/SKILL.md) | Productivity | Personalized daily dashboard for stock price, FX rate, and weather with market move rationale. | Web search capability |
| [`paper-summary`](./paper-summary/SKILL.md) | Research & Analysis | Structured lay-language summary of any research paper: problem, method, key results, limitations, and why it matters. Configurable audience level and optional focus lens. | Web fetch capability |

---

## 🛠️ Repository Structure

Each skill lives in its own directory with a standard `SKILL.md` file containing YAML frontmatter and clear operational instructions:

```text
runbooks-for-agents/
├── ai-news/
│   └── SKILL.md
├── morning-briefing/
│   └── SKILL.md
├── AGENTS.md
├── LICENSE
└── README.md
```

---

## 🤝 Contributing & Standards

Every skill in this repo adheres to:
- **Zero Hallucination Grounding**: Always mandates fresh web lookups for volatile data (news, prices, weather) with full source citations.
- **Strict Output Consistency**: Predictable schemas, tables, and fallback messaging for slow news days.
- **Unattended Execution Friendly**: Self-contained instructions that never stall asking interactive questions when run via automated schedules.

---

## 📄 License

Distributed under the [MIT License](LICENSE).

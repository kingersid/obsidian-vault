## 🧠 Karpathy LLM Wiki Schema

> This wiki follows Andrej Karpathy's approach to persistent, compounding knowledge bases maintained by AI agents.

**Source:** [karpathy/llm-wiki Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

---

### Core Principles

1. **Persistent Knowledge** — Unlike RAG which re-derives context per query, this wiki compounds over time. Every session builds on previous work.
2. **Human curates, LLM maintains** — Humans source and direct; the LLM handles summarizing, cross-referencing, filing, and bookkeeping.
3. **Append-only logs** — Nothing deleted, only superseded. The activity-log records every change chronologically.
4. **Transparent format** — Plain markdown — no vendor lock-in, always human-readable.

---

### Wiki Structure

```
wiki/
├── VPS Architecture Overview.md    ← index.md (the hub)
├── VPS Change Chronology.md         ← timeline + history
├── wiki-activity-log.md             ← log.md (append-only)
├── Karpathy LLM Wiki Schema.md      ← this file (schema/config)
├── raw/                             ← source configs/references
├── WhatsApp System.md               ← entity page
├── Meta Ads System.md                ← entity page
├── Services & Projects.md            ← entity page
├── Custom AI Skills.md               ← entity page
└── Docker & Infrastructure.md        ← entity page
```

### File Roles

| File | Role | Maintained By |
|------|------|---------------|
| VPS Architecture Overview.md | **Index** — content catalog | LLM updates on structural changes |
| VPS Change Chronology.md | **Timeline** — dated history | LLM adds entries when changes happen |
| wiki-activity-log.md | **Log** — append-only stream | LLM appends after each change session |
| *.md entity pages | **Knowledge** — deep dives | LLM updates when systems evolve |
| raw/ | **Sources** — immutable refs | Human adds; LLM reads only |

---

### Workflow

**When new information arrives:**
1. Add raw source to `raw/` directory
2. LLM reads the source and discusses key takeaways
3. LLM updates relevant entity pages with new info
4. LLM cross-references with existing pages (`[[wiki links]]`)
5. LLM appends to `wiki-activity-log.md` with timestamp

**Periodic health checks ("linting"):**
- Search for orphan pages (no inbound links)
- Find missing cross-references
- Identify contradictory claims
- Flag stale or outdated information

---

### Querying the Wiki

Rather than vector search, the LLM navigates the wiki structure:
1. Start at **VPS Architecture Overview** (the index)
2. Follow `[[wiki links]]` to relevant entity pages
3. Check **VPS Change Chronology** for timeline context
4. Reference `raw/` sources for exact configs

---

### 📌 Related
- [[VPS Architecture Overview]]
- [[wiki-activity-log]]
- [[VPS Change Chronology]]

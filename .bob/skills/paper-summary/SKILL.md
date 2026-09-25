---
name: paper-summary
description: >-
  Produces a structured lay-language summary of a research paper given an arXiv
  URL, DOI, or pasted abstract/full text. Covers: problem & motivation, method
  & approach, key results, limitations & caveats, and a plain-English "why it
  matters" takeaway. Configurable target audience (general / technical /
  domain-expert) and optional focus lens (e.g. "practical applications",
  "safety implications"). Requires an agent with web fetch capability to
  retrieve full paper content from URLs -- not bundled in this skill package.
  Use whenever the user shares a paper link or abstract and asks to "summarize
  this paper", "explain this research", "break down this arXiv paper", "what
  does this paper say", "TL;DR this paper", or similar -- in any language or
  phrasing. Always use this skill rather than improvising an ad hoc summary --
  the structure is tuned to be consistent, grounded, and never plagiaristic.
---

# Paper Summary

## Configuration

Edit the values below for yourself, then save the skill again. Both are optional.

```
audience: ""     # "general" (default) | "technical" | "domain-expert"
                 # general    = plain English, minimal jargon, analogies welcome
                 # technical  = assumes ML/CS literacy, can use standard notation
                 # domain-expert = peer-level depth, full terminology, no hand-holding
focus_lens: ""   # e.g. "practical applications", "safety implications",
                 # "reproducibility", "comparison to prior work"
                 # leave blank for a balanced, unbiased summary
```

If `audience` is blank, default to `general`.
If `focus_lens` is set, add a short focused paragraph at the end of the relevant section where it naturally fits — do not force it into every section.

---

## Why the fetch step matters

Summaries written from a title or abstract alone routinely miss the actual findings, misstate the method, or inflate the claims. Always fetch the full paper (or at minimum the full abstract + conclusions) before writing any section. A summary that misrepresents the paper's actual results is worse than no summary at all, because the user may share or act on it.

If the input is already a full pasted text (not a URL), skip the fetch step and work from what was provided.

If this skill is running as part of an unattended scheduled task, do not pause to ask clarifying questions — fetch the content and produce the finished summary as the final output.

---

## What to fetch

1. **If given an arXiv URL or arXiv ID** (e.g. `https://arxiv.org/abs/2305.12345` or `2305.12345`):
   - Fetch the abstract page first (`https://arxiv.org/abs/<id>`).
   - Then fetch the HTML full-text version (`https://arxiv.org/html/<id>`) if available — it renders more cleanly than the PDF for extraction purposes.
   - Fall back to the PDF abstract + conclusions section if the HTML version is unavailable.

2. **If given a DOI** (e.g. `10.1145/...`):
   - Resolve it via `https://doi.org/<doi>` and fetch the landing page.
   - If the publisher page is paywalled, fall back to a Semantic Scholar or OpenReview page for the abstract and any available full text.

3. **If given a direct PDF or HTML URL**: fetch it directly.

4. **If given only a title**: search for the paper on arXiv (`site:arxiv.org <title>`) or Semantic Scholar and resolve to the canonical URL before fetching.

For every source used, record the paper title, authors, venue/journal, publication date, and canonical URL so they can be cited in the output.

---

## Output format

Write the finished summary as markdown, following this structure exactly:

```
# Paper Summary: [Full Paper Title]

**Authors:** [Author 1, Author 2, ... (et al. if > 4)]
**Venue / Date:** [Conference, journal, or "arXiv preprint" + date]
**Link:** [arXiv / DOI / canonical URL as a clickable markdown link]

---

## 🧩 Problem & Motivation

[2-4 sentences. What gap or challenge does this paper address? Why does it
matter? What does the field look like before this work? Write for the
configured `audience` level -- use an analogy if audience is "general".]

---

## ⚙️ Method & Approach

[3-6 sentences (more for technical/domain-expert audiences). How do the
authors tackle the problem? What is the core idea, architecture, algorithm,
or experimental design? Avoid lifting phrases directly from the paper --
paraphrase in your own words. If the method has a named component or acronym,
introduce it plainly before using it.]

---

## 📊 Key Results

[3-5 bullets. What did they find? Lead with the most significant or
surprising result. Include numbers where they are meaningful (accuracy, speedup,
benchmark score) -- but contextualize them ("X% better than the previous
best on benchmark Y") rather than floating them without reference. Do not
cherry-pick only positive results; if the paper reports a negative or mixed
finding, include it.]

- **[Short result label]:** [1-2 sentence explanation]
- **[Short result label]:** [1-2 sentence explanation]
- ...

---

## ⚠️ Limitations & Caveats

[2-4 bullets. What are the honest constraints of this work? Include both
limitations the authors name themselves and any obvious gaps (e.g. narrow
evaluation set, compute requirements, untested assumptions). Do not fabricate
limitations -- only report what the paper acknowledges or what is plainly
evident from the method/evaluation description.]

- [Limitation 1]
- [Limitation 2]
- ...

---

## 💡 Why It Matters

[2-4 sentences. Plain-English takeaway: who should care about this work and
why? What could it enable, change, or challenge? Avoid hype -- ground the
significance in what the paper actually demonstrates, not in what it claims
or aspires to. If the contribution is incremental, say so honestly.]

[If `focus_lens` is configured, add one short paragraph here (or in the most
relevant section above) that addresses the paper specifically through that
lens. Label it clearly, e.g.: **Practical applications lens:** ...]

---

*Summary generated from: [clickable link to the source fetched]*
```

---

## Output notes

- **Never plagiarize.** Do not lift sentences or phrases verbatim from the paper. Every sentence in the summary must be written in your own words. A short attributed direct quote (max one, in quotation marks with a section reference) is acceptable only when the authors' exact phrasing is the clearest possible expression of a key claim.
- **No hallucinated citations.** All links must be real, fetchable URLs. Do not invent DOIs, arXiv IDs, or conference names. If the venue is unclear, write "venue unknown" rather than guessing.
- **No inflation.** Do not upgrade the paper's actual claims. If the paper shows a correlation, do not write "proves." If the evaluation is narrow, do not write "state-of-the-art across tasks."
- **Audience calibration.** Adjust vocabulary and sentence complexity to match the configured `audience` value throughout — not just in one section. For "general": spell out all acronyms, use everyday analogies. For "domain-expert": assume peer-level background, skip basic definitions.
- **Length discipline.** The summary should be comprehensive but not exhaustive. The goal is that someone who reads it can accurately represent the paper in a meeting — not that they could reproduce it. If a section has nothing meaningful to say (e.g. a methods paper with no empirical limitations section), keep it short and note the gap rather than padding.

---

## Delivering the summary

- If this is a one-off request in a live conversation, reply with the finished markdown summary as your message.
- If this is running unattended (e.g. fired by a scheduled task with a paper URL as input), produce the finished summary as the final response — do not wait for confirmation or ask clarifying questions first.

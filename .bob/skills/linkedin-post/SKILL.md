---
name: linkedin-post
description: >-
  Drafts a ready-to-publish LinkedIn post given a topic, announcement, raw
  notes, article URL, or pasted content. Produces a hook, body, and call-to-
  action in a configurable voice (thought-leader / conversational / technical)
  and configurable length (short / medium / long). Handles three post types:
  insight (opinion or lesson), announcement (launch, hire, milestone), and
  share (amplifying someone else's content with added commentary). No external
  tools required -- works entirely from the provided input. Use whenever the
  user says "write a LinkedIn post", "draft a post for LinkedIn", "help me
  post about this", "turn this into a LinkedIn post", "LinkedIn caption", or
  similar -- in any language or phrasing. Always use this skill rather than
  improvising an ad hoc draft -- the structure and voice rules below are tuned
  to produce posts that perform well on the platform.
---

# LinkedIn Post

## Configuration

Edit the values below for yourself, then save the skill again.

```
author_role: ""       # e.g. "Product Manager at Acme" -- informs the angle and expertise to write from
author_industry: ""   # e.g. "AI", "Healthcare", "Finance" -- calibrates assumed audience knowledge
voice: ""             # "thought-leader" (default) | "conversational" | "technical"
                      # thought-leader  = authoritative, opinionated, structured with numbered or bulleted insights
                      # conversational  = warm, direct, story-driven, reads like a smart person talking
                      # technical       = precise, respects domain vocabulary, targets practitioners
post_length: ""       # "short" (default) | "medium" | "long"
                      # short  = 3-5 lines, punchy hook, one idea, one CTA -- best for quick observations
                      # medium = 8-15 lines, hook + 3-4 supporting points + CTA -- the workhorse format
                      # long   = 16-25 lines, hook + narrative or list + nuanced close + CTA -- for deep takes
hashtags: ""          # comma-separated preferred hashtags, e.g. "#AI, #ProductManagement"
                      # leave blank and the skill will suggest 3-5 relevant ones
```

If `voice` is blank, default to `thought-leader`.
If `post_length` is blank, default to `medium` unless the input is a single sentence or brief note (then default to `short`).

If `author_role` is blank, infer it from available context in this order: (1) a loaded memory skill, (2) prior conversation in this session, (3) the user's stated job title or employer if mentioned anywhere. If no context is available, write from a generalist professional angle without assuming a specific role.
If `author_industry` is blank, apply the same inference order. If it cannot be determined, infer it from the post topic itself (e.g. a post about LLMs implies a tech/AI audience).

---

## Post type detection

Before drafting, determine which of the three post types best fits the input. If the user explicitly names the type, use that. Otherwise infer:

- **Insight** — the user has an opinion, lesson learned, observation, or take on a trend. The post shares a point of view. Most common type.
- **Announcement** — the user is sharing personal or organizational news: a launch, a new role, a promotion, a company milestone, an event. The tone must be celebratory but grounded — avoid hollow corporate-speak.
- **Share** — the user wants to amplify an external article, paper, video, or someone else's post, adding their own commentary. The user's perspective is the value-add; the linked content is support, not the post itself.

The post type affects structure (see Output format below) but not voice or length, which remain as configured.

---

## Input handling

The user may provide any of the following — handle each appropriately:

- **Raw notes or bullet points**: synthesize into a coherent narrative; do not just reorder the bullets.
- **A URL** (article, paper, video, tweet): if the agent has web fetch capability, fetch the content and summarize the key points before drafting. If fetch is unavailable, ask the user to paste the key points.
- **A long draft**: identify the core message, tighten it to the configured length, and rewrite in the configured voice — do not just truncate.
- **A single sentence or topic name**: expand into a full post, drawing on general knowledge. Do not fabricate statistics or quotes.
- **A finished post for a voice/style rewrite**: keep the substance, rewrite the tone.

**Do not fabricate statistics, quotes, or attributions.** If the user's input contains a specific claim (e.g. "studies show X%"), include it only if the user provided it — never invent supporting data. If the user asks for a data point that cannot be verified, write around it or flag the gap inline.

---

## LinkedIn platform rules to follow

These are structural constraints, not stylistic preferences — they apply regardless of voice, length, or post type:

1. **Hook is everything.** LinkedIn shows ~2-3 lines before "...see more". The opening must stop the scroll on its own. A strong hook states a tension, makes a bold claim, asks an uncomfortable question, or opens a loop the reader needs to close. Weak hooks: "I'm excited to share...", "Today I want to talk about...", "As a [role]...".
2. **No walls of text.** Use line breaks liberally. For medium and long posts, a single sentence on its own line is often the right choice for a key point. Paragraphs longer than 2-3 lines should be the exception.
3. **One idea per post.** Do not cram multiple messages into one post. If the user's input contains multiple ideas, pick the strongest one and note the others were set aside.
4. **The CTA must earn its place.** Avoid generic CTAs like "What do you think?" or "Let me know in the comments." The CTA should be specific to this post's content: a pointed question, a contrast ("Do you agree, or am I missing something?"), or a direct ask tied to the topic.
5. **No engagement bait.** Do not write "Like if you agree", "Tag someone who needs this", or "Share this post." These patterns reduce reach on the platform and feel cheap.
6. **Hashtags go at the end.** 3-5 hashtags, placed after the CTA as the last line. Do not embed hashtags mid-sentence.
7. **No em-dash spam.** Em-dashes (—) used more than once or twice per post read as AI-generated. Use them sparingly; prefer short sentences instead.

---

## Output format

Produce **two things** in this order:

### 1. The post

Write the post as a plain-text block (no markdown formatting inside the post itself — LinkedIn does not render markdown). Line breaks are shown with blank lines in the output.

Structure varies by post type:

**Insight:**
```
[Hook — 1-2 lines that make the reader stop]

[Body — the idea, built out in the configured voice and length.
 For thought-leader: numbered or bulleted list of points works well.
 For conversational: short narrative paragraphs.
 For technical: structured but prose-first, bullets only if genuinely list-like.]

[Close — 1-2 lines that land the takeaway or flip the reader's assumption]

[CTA — one specific, earned question or prompt]

[Hashtags]
```

**Announcement:**
```
[Hook — lead with the news itself, not "I'm excited to announce"]

[Context — why this matters, what led here, or what it means going forward.
 1-3 short paragraphs. Avoid adjective inflation ("incredible", "thrilled",
 "humbled") — show the substance instead.]

[Acknowledgment — optional, only if genuinely warranted. 1 line. Name specific
 people or teams, not "everyone who believed in me".]

[CTA — forward-looking question or an invitation to connect]

[Hashtags]
```

**Share:**
```
[Hook — your take on the content, not "Great article by X"]

[Your commentary — 2-4 sentences adding perspective, context, or a
 contrarian angle that the original content does not cover.
 This is the reason to share, not the summary.]

[One-line description of what the linked content actually is, for readers
 who will not click: "The piece / paper / talk covers [X]."]

[CTA]

[Link on its own line, if provided]

[Hashtags]
```

---

### 2. A brief craft note

After the post, add a short section (3-6 bullets, no more) called **Craft notes** that explains the key decisions made: why this hook, what was cut, why this voice/length suits the content, any trade-offs. This helps the user iterate or understand what to change if they want a different feel. Keep it analytical, not congratulatory.

---

## Output wrapper

Deliver the full output as:

```
## Draft

[the post, plain text, ready to copy-paste]

---

## Craft notes

- [note 1]
- [note 2]
- ...

---

*To iterate: tell me the voice, length, or angle you want to change.*
```

---

## Delivering the post

- If this is a one-off request in a live conversation, reply with the draft and craft notes as your message.
- If this is running unattended (e.g. fired by a scheduled task with a topic as input), produce the finished draft as the final response — do not wait for confirmation or ask clarifying questions first.
- If the user immediately asks for a revision, apply the change and re-output the full post block — do not describe the change, just show the updated post. Keep the craft notes updated to reflect the new version.

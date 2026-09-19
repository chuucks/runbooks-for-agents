---
name: morning-briefing
description: >-
  Produces a personal daily briefing covering a stock price, a currency
  exchange rate, and local weather -- fully configurable via the settings
  block at the top of this file, so it works for anyone after a 30-second
  edit. Requires an agent with live web search capability (e.g. a web_search
  tool) to look up current prices, rates, and weather -- that capability is
  not bundled in this skill package, so it will not produce accurate results
  without it. Use this skill whenever the user asks for their "morning
  briefing," "daily briefing," "daily update," or similar -- even phrased
  casually ("what's up today", "morning update please") or in another
  language. Always use this skill rather than improvising the format from
  scratch, so the structure stays consistent every day.
---

# Morning Briefing

## Configuration

Edit the values below for yourself, then save the skill again. Leave any
value blank to skip that whole section in the output -- the skill still
works fine with only one or two of the three configured.

```
stock_ticker: ""        # e.g. "AAPL", "IBM", "NASDAQ: MSFT" -- leave blank to skip the stock section
fx_pair: ""              # e.g. "USD/MXN", "USD/EUR" -- leave blank to skip the exchange-rate section
weather_location: ""     # e.g. "Austin, TX" or "Tokyo, Japan" -- leave blank to skip the weather section
```

The briefing itself is always written in English, no matter what language the request comes in or which values are configured above -- this is not user-configurable.

If every value is blank, this skill has nothing to report -- tell the user it needs at least one value configured before it can run, and point them at this section.

## Why the research step matters

Stock prices, exchange rates, and weather all change daily (often hourly). Never answer any configured section from memory or training data -- always use WebSearch (and WebFetch if a page needs more detail than the search snippet gives) to pull genuinely current numbers before writing anything. A stale or hallucinated number here is worse than no briefing at all, since the user may act on it (checking a stock's move against the day's market news, or deciding whether to carry an umbrella).

If this skill is running as part of an unattended scheduled task, don't pause to ask clarifying questions -- gather the data and produce the finished briefing as the final output.

## What to research

Run a search for each configured value (in parallel where your tools support it). Skip the research step entirely for anything left blank in Configuration.

1. **Stock price** (if `stock_ticker` is set) -- latest/current price in the relevant currency, the day's change (amount and %), and ideally after-hours price and how it compares to its sector. Search something like "`{stock_ticker}` stock price today".
2. **Exchange rate** (if `fx_pair` is set) -- current rate, and the day's range/change if available.
3. **Weather** (if `weather_location` is set) -- today's high/low temperature, general conditions, chance of rain, and wind if available. Use the units conventional for that location (°F for the US, °C elsewhere) unless the user has said otherwise.
4. **What moved the stock today** (if `stock_ticker` is set) -- a quick, separate search (e.g. "why `{stock_ticker}` stock moved today", "`{stock_ticker}` stock news today") for whatever market commentary or company news is actually driving today's price move. This is market context for the stock section specifically, not a general news roundup.

For every fact used, keep the source's name, URL, and publish date so the full markdown link can be cited later -- a bare "(Site Name)" with no URL is not acceptable.

## Output format

Write the finished briefing as markdown, following this structure exactly (keep it consistent every day so the user knows where to look). Only include the numbered sections for values that are configured, and number them sequentially starting from 1 in whatever order stock / exchange rate / weather appear above -- e.g. if only `weather_location` is set, it becomes section "1. Weather," not "3. Weather."

```
# Morning Briefing — [today's date, e.g. September 19, 2026]

*[one short, creative 1-2 sentence note right after the title — playfully tie
together something from today's actual configured data (the stock move, the
weather, whichever sections are active). Keep it light, specific to today's
numbers, and brief — this is flavor, not a summary.]*

## [N]. Stock Price ([stock_ticker])

| Metric | Value |
|---|---|
| Close | **$X.XX** |
| Change | **+/-$X.XX (+/-X.XX%)** |
| Prior close | $X.XX |
| After-hours | $X.XX (if available) |
| vs. sector | (if available) |

**Why it moved:**

[2-4 bullets explaining today's move, grounded in the "what moved the stock today"
search above (e.g. an earnings beat, an analyst note, a sector-wide move). If no
clear news explains it, say so plainly — e.g. "no single headline triggered the
move; it reads as a sector-wide/thematic rally" — rather than forcing a connection
that isn't supported by the sources.]

*Sources: [Site Name](https://full-url) · [Site Name](https://full-url)*

---

## [N]. Exchange Rate ([fx_pair])

| Metric | Value |
|---|---|
| Rate | **~X.XX** |
| Range | X.XX – X.XX |
| Change | +/-X.XX% |

*Sources: [Site Name](https://full-url)*

---

[If the visualize MCP tool is available (`mcp__visualize__read_me` / `mcp__visualize__show_widget`), call it here before writing this section's text: call `read_me` once with `modules: ["mockup"]`, then `show_widget` to render the forecast as a small weather card — location + date, a Tabler outline icon matching conditions (e.g. `ti-cloud-rain`, `ti-sun`, `ti-cloud`), high/low temperature, a short conditions line, and a 2-column stat row for rain chance and wind. Use the single-card HTML pattern from the mockup module (surface-2 card, 12px radius, 0.5px border). Still include the markdown table below either way, so the data is captured in the text version too — the widget is a visual bonus, not a replacement.]

## [N]. Weather — [weather_location]

| Metric | Value |
|---|---|
| High | XX° |
| Low | XX° |
| Conditions | [short description] |
| Rain chance | **XX%** |
| Wind | [if available] |

*Source: [Site Name](https://full-url)*

---

**Sources:** [Site Name](https://full-url) · [Site Name](https://full-url) · ...
```

Notes on the format:

- Only emit the sections for configured values; renumber sequentially so there are no gaps (e.g. no section jumping from "1." to "3.").
- Use `---` horizontal rules between sections, as shown.
- **All** source citations — inline, under each table, and in the final Sources line — must be real, full, clickable markdown links (`[Site Name](https://...)`), never bare text like `(Site Name, date)` with no URL. If you genuinely cannot find a URL for a fact, say so rather than inventing one.
- This skill covers only the three data sections above — it does not cover general or AI news.
- The whole briefing is always written in English, regardless of what language the request came in.

## Delivering the briefing

- If this is a one-off request in a live conversation, reply with the finished markdown briefing as your message.
- If this is running unattended (e.g. fired by a scheduled task), produce the finished briefing as the final response so it reaches the user directly — don't wait for confirmation or ask clarifying questions first.
- The weather widget (see the weather section above) renders inline in whatever session produces the briefing. In a live chat the user sees it immediately; on an unattended scheduled run it's still there when they open that day's session, but note that any push notification summarizing the run stays plain text — the widget is a bonus for whoever opens the session, not something the notification itself can carry.

# Spiral Research

> Most research agents use linear pipelines. This one thinks in spirals — iteratively narrowing hypotheses with multi-tool signal routing. Unproven but opinionated.

A structured research skill for AI coding agents (Claude Code, OpenCode, Codex). Instead of plan-then-search-then-report, it searches while thinking — each round updates hypotheses, scores new leads, and self-corrects direction.

## How It's Different

| | Linear Pipeline (most tools) | Spiral Convergence (this) |
|---|---|---|
| **Flow** | Plan → Search → Synthesize → Report | Question → Hypothesize → Search → Reflect → Narrow → Repeat |
| **Adaptation** | Fixed plan, execute once | Hypotheses update every round based on findings |
| **Tool selection** | One search tool for everything | Signal routing (S1-S6) picks tool combos per sub-question |
| **Quality check** | Post-hoc critique | Every-round Critic + Reflection, mid-research direction changes |
| **Learning** | None | Run-log + auto-review every 10 runs |
| **Human involvement** | Approve outline, then hands-off | Confirm direction before searching, consulted on pivots |

## Core Concepts

### The Spiral

```
Round 1: Vague question → 3 sub-questions → search → discover you were wrong
Round 2: Updated hypothesis → refined queries → deeper sources → new leads
Round 3: Converging → filling gaps → confidence rising → done
```

### Research Map (Working Memory)

Maintained throughout, updated every round:
- Current hypothesis (what you think the answer is)
- Sub-questions with status (unknown → partial → known)
- Scored clue pool (0-3, auto-follow high-value leads)
- Evidence with source URLs (no unsourced facts allowed)

### Signal Routing

Instead of always using the same search tool, the system matches question characteristics to tool combinations:

| Signal | Trigger | Tools |
|--------|---------|-------|
| S1 Community sentiment | "what do developers think" | Grok x_search + Reddit |
| S2 Recency | "latest", "2026", news | Grok web_search |
| S3 Deep comparison | A vs B, tech selection | Tavily Research + realtime tool |
| S4 How-to | implementation, tutorials | Tavily + YouTube |
| S5 Market/trends | adoption, market share | DataForSEO + Grok x_search |
| S6 Single source | known URL to extract | Firecrawl / Tavily extract |

### Self-Evolution

Every research run logs metrics (tools used, contribution rate, gaps, cost). Every 10 runs, auto-reviews:
- Which tools are over/under-used vs their actual contribution?
- Which signals trigger too broadly or too narrowly?
- Are quality gaps trending up or down?

## Installation

### Claude Code

```bash
git clone https://github.com/mingiress/spiral-research.git ~/.claude/skills/research-pro
```

### OpenCode

```bash
git clone https://github.com/mingiress/spiral-research.git ~/.config/opencode/skills/research-pro
```

### Codex

```bash
git clone https://github.com/mingiress/spiral-research.git ~/.codex/skills/research-pro
```

## Setup

### Required

None — the skill works with zero API keys using only built-in web fetch. But it's weak without tools.

### Recommended

| Key | What it enables | Get it |
|-----|----------------|--------|
| `TAVILY_API_KEY` | Fast search, extract, deep research | https://tavily.com |
| `XAI_API_KEY` | Grok web + X/Twitter search | https://console.x.ai |
| `REDDIT_SESSION` | Full Reddit post/comment reading | Browser DevTools → Cookies |

### Optional

| Key | What it enables | Get it |
|-----|----------------|--------|
| `FIRECRAWL_API_KEY` | JS-rendered page scraping | https://firecrawl.dev |
| `YOUTUBE_API_KEY` | Video search + transcripts | Google Cloud Console |
| `OPENROUTER_API_KEY` | Perplexity sonar fallback | https://openrouter.ai |
| `DATAFORSEO_LOGIN` | Keyword volume / SERP data | https://dataforseo.com |

### CLI Tools

```bash
# Tavily CLI (primary search)
curl -fsSL https://cli.tavily.com/install.sh | bash

# YouTube transcripts
pip install youtube-transcript-api

# Firecrawl (optional)
npm install -g firecrawl-cli
```

## Usage

Just ask a research question:

```
帮我研究 [topic]
Research [topic] in depth
```

The skill auto-classifies depth (Quick 2 rounds / Standard 5 / Deep 10) and confirms direction before searching.

### Depth Override

```
深度研究 [topic]          # Forces Deep mode
quick: [topic]            # Forces Quick mode
```

### Direct Tool Aliases

```
reddit 上怎么看 X         # Direct Reddit search
用 grok 搜 X             # Direct Grok web search
tavily extract [URL]     # Direct page extraction
```

## Output Format

Reports include YAML frontmatter (machine-parseable by other agents):

```yaml
---
type: research-report
question: "..."
confidence: high
rounds: 3
tools: [tavily, grok_web, reddit]
date: 2026-05-05
---
```

## Methodology

Detailed methodology is in [`SKILL.md`](SKILL.md) — the actual skill file that agents read.

The five phases:
1. **Understand** — Local context check → ambiguity check → decompose → confirm with user
2. **Prepare** — Signal routing → keyword construction → tool coverage check
3. **Search + Update** — Parallel search → clue scoring → Critic → Reflection → map update
4. **Converge** — Direction change check → coverage check → round limits
5. **Log + Evolve** — JSONL metrics → periodic self-review

## Benchmarks

Not yet. We plan to run side-by-side comparisons against [LangChain Open Deep Research](https://github.com/langchain-ai/open_deep_research) on the same question set. If you'd like to help, open an issue.

## Philosophy

- **"边搜边想" over "想完再搜"** — Searching changes what you think. Don't pretend you know the plan upfront.
- **Tools are ingredients, not combos** — Signal routing picks the right tools for each sub-question, not one tool for everything.
- **Self-correction is built in** — Every round has Critic + Reflection. Direction pivots require user consent.
- **Track what works** — If you can't measure tool contribution, you can't improve tool selection.

## License

MIT

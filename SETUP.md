# Setup Guide

## Install

```bash
# Claude Code
git clone https://github.com/mingiress/spiral-research.git ~/.claude/skills/research-pro

# OpenCode
git clone https://github.com/mingiress/spiral-research.git ~/.config/opencode/skills/research-pro

# Codex
git clone https://github.com/mingiress/spiral-research.git ~/.codex/skills/research-pro
```

---

## API Keys

Set these as environment variables in your shell profile (`~/.zshrc`, `~/.bashrc`) or your agent's env config:

| Key | Used for | Required? | Get it at |
|-----|---------|-----------|-----------|
| `TAVILY_API_KEY` | Tavily search/extract/research | Recommended | https://tavily.com |
| `XAI_API_KEY` | Grok web search + X/Twitter | Recommended | https://console.x.ai |
| `REDDIT_SESSION` | Reddit full post reading | Optional | Browser DevTools → Cookies |
| `FIRECRAWL_API_KEY` | Firecrawl scrape/crawl | Optional | https://firecrawl.dev |
| `YOUTUBE_API_KEY` | YouTube search + metadata | Optional | https://console.cloud.google.com |
| `OPENROUTER_API_KEY` | Perplexity sonar fallback | Optional | https://openrouter.ai |
| `DATAFORSEO_LOGIN` | Keyword volume / SERP | Optional | https://dataforseo.com |
| `DATAFORSEO_PASSWORD` | (paired with login) | Optional | https://dataforseo.com |

**Minimum setup:** Just `TAVILY_API_KEY` gets you working. Add `XAI_API_KEY` for realtime + Twitter.

```bash
# Example: add to ~/.zshrc
export TAVILY_API_KEY="tvly-your-key"
export XAI_API_KEY="xai-your-key"
export REDDIT_SESSION="your-reddit-session-cookie"
```

---

## CLI Tools

| Tool | Install | What it adds |
|------|---------|-------------|
| `tvly` | `curl -fsSL https://cli.tavily.com/install.sh \| bash` | Tavily search/extract/research |
| `firecrawl` | `npm install -g firecrawl-cli` | Web scraping with JS rendering |
| `youtube_transcript_api` | `pip install youtube-transcript-api` | YouTube transcript extraction |
| `yt-dlp` | `brew install yt-dlp` | YouTube search fallback (no API key needed) |

The skill works with whatever tools you have. Missing tools are skipped — more tools = better coverage.

---

## Verify

Ask your agent:
```
帮我研究一下 Next.js 15 有什么新功能
```

You should see it confirm direction (Quick mode), then return results with citations.

---

## How It Works

research-pro is a single skill file (`SKILL.md`) that agents read and follow. It:
1. Classifies your question into depth (Quick / Standard / Deep)
2. Confirms direction with you before spending tokens
3. Routes to the right tools via signal matching
4. Returns a structured report with citations

For the full methodology, see `SKILL.md`.

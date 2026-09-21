# Klyf

[![Klyf MCP connector, tool definition quality and endpoint health on Glama](https://glama.ai/mcp/connectors/ai.klyf/klyf/badges/score.svg)](https://glama.ai/mcp/connectors/ai.klyf/klyf)
[![M8ven Score](https://m8ven.ai/badge/mcp/klyf-ai-youtube-mcp-mvnl5f?v=7a8bd30717b0e60b54d69d50dbad760b)](https://m8ven.ai/mcp/klyf-ai-youtube-mcp-mvnl5f)

**Klyf is an AI YouTube analyst that runs inside Claude as a hosted MCP connector. It reads a creator's own channel and answers questions about it in plain language.**

Connect your YouTube channel once, then ask Claude things like "why did my last video flop" or "what should I make next", and Klyf reads your real analytics, audience and comments and answers with a decision rather than another dashboard.

- Website: <https://klyf.ai>
- Connector URL: `https://klyf.ai/api/mcp`
- MCP registry: [`ai.klyf/klyf`](https://registry.modelcontextprotocol.io/v0/servers?search=klyf)
- Transport: Streamable HTTP, remote and hosted. Nothing to install or self-host.

This repository is the public documentation for the connector. The product source is not open. The MIT
licence here covers this documentation; the Klyf service itself is proprietary.

## Connect it

### Claude (web, desktop, mobile)

1. Open Settings, then Connectors.
2. Add a custom connector with the URL `https://klyf.ai/api/mcp`.
3. Sign in when prompted, then connect your YouTube channel through Google.

Klyf is also listed in Claude's connector directory, where it can be added without pasting a URL.

### Any other MCP client

Klyf is a remote Streamable HTTP server with OAuth. Point your client at the connector URL:

```json
{
  "mcpServers": {
    "klyf": {
      "type": "http",
      "url": "https://klyf.ai/api/mcp"
    }
  }
}
```

Client registration uses [Client ID Metadata Documents](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization/client-registration), which the MCP specification recommends. Dynamic Client Registration is deliberately not offered: the spec deprecates it, and open registration is a consent-phishing surface. Clients that only implement DCR will not be able to register.

## What you can ask

Plain language, in the same conversation you already work in:

- "Why did my last video flop?"
- "What should I make next?"
- "How is my channel doing?"
- "Who is actually watching my videos?"
- "Give me title options for this video."
- "Is this idea worth making?"
- "What do my comments say this week?"
- "Review this video before I publish it."

Klyf answers in the language you write in.

## Tools

21 tools. Read-only unless marked otherwise. Most work on the free plan; the ones marked Pro are the ongoing work Klyf does for you rather than deeper numbers.

| Tool | What it does | Access | Plan |
|---|---|---|---|
| `audit_channel` | Full audit and overview of the channel in one answer | Read | Free |
| `diagnose_channel` | What is holding the channel back right now | Read | Free |
| `check_growth` | Whether the channel is growing, sliding or flat, and when it turned | Read | Free |
| `check_discoverability` | How the channel gets found, and whether new people are finding it | Read | Free, per-video is Pro |
| `analyze_video` | Deep dive on one video: how it did and why | Read | Free |
| `analyze_audience` | Who the audience is, from the channel's own data | Read | Free |
| `find_best_videos` | Best-performing videos, ranked several ways | Read | Free |
| `find_best_thumbnails` | What the best-performing thumbnails have in common | Read | Free |
| `suggest_ideas` | Specific ideas to make next, grounded in what already works | Read | Free |
| `evaluate_idea` | Whether a specific idea or title is worth making | Read | Free |
| `package_video` | Title options and thumbnail text for a video | Read | Free |
| `draft_description` | A description in the creator's own voice | Read | Free |
| `draft_script` | A script in the creator's own voice, built from what converts | Read | **Pro** |
| `review_upcoming_video` | Review a video before it publishes | Read | Free |
| `read_comments` | Themes and questions across the audience's comments | Read | Free |
| `read_comment_thread` | One comment thread in full | Read | **Pro** |
| `get_weekly_digest` | A weekly game plan | Read | **Pro** |
| `enable_auto_reply` | Turn on approved comment replies | Write | **Pro** |
| `reply_to_comment` | Post one approved reply | Write | **Pro** |
| `upgrade_to_pro` | Link to upgrade | Write | Free |
| `submit_feedback` | Send feedback or a bug report to the team | Write | Free |

## Permissions

Connecting asks Google for two read-only scopes and nothing else:

```
https://www.googleapis.com/auth/youtube.readonly
https://www.googleapis.com/auth/yt-analytics.readonly
```

Klyf cannot post, edit or delete anything with these. `youtube.force-ssl`, the scope that allows writes, is deliberately avoided in the base connection because it is far broader than a read feature needs.

The one exception is auto-reply. If a Pro user turns it on, Klyf asks separately for `youtube.force-ssl` on its own consent screen, and it gates only comment replies the creator has approved. It is never part of the initial connection.

## Pricing

Free to start. Klyf Pro is $19/mo billed annually, or $29/mo monthly, with a 30-day money-back guarantee on annual plans. Pro adds the always-on work: watching every upload, drafting comment replies, and sending a weekly game plan.

## `server.json`

```json
{
  "$schema": "https://static.modelcontextprotocol.io/schemas/2025-12-11/server.schema.json",
  "name": "ai.klyf/klyf",
  "title": "Klyf",
  "description": "AI YouTube analyst in Claude for creators: audit, fix, decide what to make next, grow subs.",
  "websiteUrl": "https://klyf.ai",
  "version": "0.1.4",
  "remotes": [
    { "type": "streamable-http", "url": "https://klyf.ai/api/mcp" }
  ]
}
```

## Links

- [Connect YouTube to Claude](https://klyf.ai/connect-youtube-to-claude)
- [YouTube MCP for Claude](https://klyf.ai/youtube-mcp)
- [Free channel audit](https://klyf.ai/youtube-channel-audit)
- [Help](https://klyf.ai/help)
- [Privacy](https://klyf.ai/privacy) and [Terms](https://klyf.ai/terms)

Support: support@klyf.ai

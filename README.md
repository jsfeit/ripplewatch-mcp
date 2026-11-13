# Ripplewatch MCP server

Ripplewatch is a competitive intelligence analyst that works inside Claude, ChatGPT and other AI assistants over [MCP](https://modelcontextprotocol.io). It watches your competitors' pricing, hiring, launches and news, gives each competitor a Momentum score, and weighs what it finds against your own positioning, buyers and won and lost deals.

You ask in the assistant you already use: "Is Northlane heating up?", "What changed this week?", "Why are we losing to Brightwell?" There is no separate dashboard to open.

This repository holds the public documentation and manifest for the hosted server. The service itself is closed source and runs at `https://www.ripplewatch.ai/api/mcp`.

- Website: https://www.ripplewatch.ai
- MCP Registry name: `ai.ripplewatch/ripplewatch`
- Transport: streamable HTTP
- Authentication: OAuth 2.0 with dynamic client registration (an API key also works for developer tools)

## Connect it

You need a Ripplewatch account. Pricing is $29 a month plus usage you prepay for, published at https://www.ripplewatch.ai/pricing. You can try a free read on one competitor with no signup at https://www.ripplewatch.ai/onboarding?path=connect.

**Claude.** Open Settings, then Connectors, then add a custom connector with this URL:

```
https://www.ripplewatch.ai/api/mcp
```

Sign in to Ripplewatch when prompted and approve the connection.

**ChatGPT.** Add the same URL as a custom MCP connector where your plan supports it, then sign in and approve.

**Cursor.** [![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](cursor://anysphere.cursor-deeplink/mcp/install?name=ripplewatch&config=eyJ1cmwiOiJodHRwczovL3d3dy5yaXBwbGV3YXRjaC5haS9hcGkvbWNwIn0=)

Click the button, confirm the install in Cursor, then choose Authenticate and sign in to Ripplewatch. Or add this to your `mcp.json` by hand:

```json
{
  "mcpServers": {
    "ripplewatch": {
      "url": "https://www.ripplewatch.ai/api/mcp"
    }
  }
}
```

**Developer tools (for example Claude Code).** Create an API key under Settings, then Developer, in your Ripplewatch account, and send it as a bearer token:

```bash
claude mcp add --transport http ripplewatch https://www.ripplewatch.ai/api/mcp \
  --header "Authorization: Bearer rw_live_..."
```

Step-by-step guides with screenshots: https://www.ripplewatch.ai/connect/claude-and-chatgpt

## Tools

Read-only tools never change anything. Write tools record only what you tell the assistant.

| Tool | What it does | Type |
|---|---|---|
| `start_here` | First-run guide: your company, early findings, prompts tailored to your competitors, the most useful next step | read |
| `how_to_use` | Explains win/loss data, Momentum, the emails you get, and what to ask | read |
| `get_briefing` | What changed across your competitors, the weekly verdict, who is heating up or cooling, top recent signals | read |
| `ask` | A free-form question answered against your positioning, buyers and the last 90 days of scored signals | read |
| `list_competitors` | The competitors you track, with domain and category | read |
| `get_competitor` | One competitor in detail: Momentum and the drivers behind it, recent signals, deals logged against them | read |
| `get_momentum` | Momentum (Heating up, Steady, Cooling, Gone quiet) for every tracked competitor, plus your own | read |
| `get_trends` | Recurring themes across your logged win/loss reasons and customer feedback | read |
| `get_next_step` | The one thing you could share that would make answers more specific | read |
| `get_slack_setup_link` | A link to connect an optional weekly Slack digest | read |
| `add_competitor` | Start tracking a competitor (checks the domain first) | write |
| `log_win_loss` | Record a won or lost deal against a competitor, with the reason | write |
| `import_win_loss` | Bring in deal history in bulk from a spreadsheet or CRM your assistant can read | write |
| `log_call_mentions` | Record short snippets of a competitor coming up on sales calls | write |
| `log_customer_feedback` | Record a piece of customer feedback, with an optional 0 to 10 NPS score | write |
| `set_context` | Replace your positioning and buyer profile (overwrites the saved text, so it asks first) | write, overwrites |

## What Momentum means

Each competitor gets a score from -100 to +100, built from up to ten inputs: hiring, pricing activity, product changes, press and funding sentiment, relevance trend, win rate against you, review sentiment, Reddit and Hacker News buzz, plus opt-in GitHub and ad activity. Above +15 is Heating up, below -15 is Cooling, and in between is Steady. Inputs with little data are flagged rather than guessed. The method is written up at https://www.ripplewatch.ai/blog/momentum-score-methodology.

## Example prompts

- "Give me my competitive briefing for this week."
- "Is Notion heating up? What's driving their Momentum score?"
- "We lost a deal to Linear because engineering wanted docs inside their issue tracker. Log it."
- "What should I tell you next so your answers get more specific?"

## Privacy and security

- Ripplewatch reads public information about the competitors you choose, plus what you tell it about your company and deals.
- Text scraped from public sources is returned to the assistant labelled as data, not instructions.
- Requests are limited to 60 per minute per key or connection.
- You can disconnect at any time from your assistant's connector settings, or revoke API keys in Ripplewatch.
- Privacy policy: https://www.ripplewatch.ai/privacy. Terms: https://www.ripplewatch.ai/terms.
- To report a security problem, use https://www.ripplewatch.ai/contact.

## More

- Connector docs: https://www.ripplewatch.ai/docs/api
- Guide to getting the most out of it: https://www.ripplewatch.ai/connect/guide
- What the MCP servers that get used have in common: https://www.ripplewatch.ai/blog/what-mcp-servers-that-get-used-have-in-common

## License

The documentation and manifest in this repository are available under the MIT License (see `LICENSE`). The hosted Ripplewatch service is governed by its [Terms](https://www.ripplewatch.ai/terms).

# mild.run — Human Approval Gate for AI Agents

**Before your AI agent does anything irreversible — your tap required.**

mild.run is an MCP server that intercepts sensitive AI agent actions (Stripe refunds, email sends, data deletes) and sends you a Telegram notification to approve or block before anything happens. Your agent has no credentials — the gate is enforced at the infrastructure level.

---

## The Problem

You're running AI agents that touch real money, real emails, and real data. When the agent misunderstands your intent or hallucinates a parameter, the damage is immediate and irreversible.

**"My agent issued a $1,500 refund. I said $150."**  
**"My agent emailed my entire CRM with a test message."**  
**"My agent cancelled a subscription I didn't mean to cancel."**

Telling the agent to "ask before acting" doesn't work — you're asking a non-deterministic system to police itself. mild.run enforces the gate at the infrastructure level. No approval → no action. The agent cannot bypass it.

---

## How It Works

1. **Sign up** at [mild.run](https://mild.run) — free
2. **Connect Stripe** (paste your restricted API key — `charges:Read` + `refunds:Write` only)
3. **Connect Telegram** (click a bot link → send one message → verified)
4. **Paste your MCP URL** into your agent platform:

```
https://mcp.mild.run/sse?mcp_token=YOUR_TOKEN
```

That's it. Every sensitive action now requires your tap on Telegram before it fires.

---

## What It Gates

| Action | Stakes | Status |
|--------|--------|--------|
| Stripe refund | Money | ✅ Live |
| Stripe subscription cancel | Revenue | ✅ Live |
| Stripe customer delete | Data | ✅ Live |
| Stripe payout | Cash | ✅ Live |
| Gmail send email | Reputation | ✅ Live |
| Slack message | Communication | ✅ Live |

---

## Compatible Platforms

Works with any MCP-compatible agent platform:

- **base44** — paste the URL in one chat message
- **Claude Desktop** — add to `claude_desktop_config.json`
- **n8n** — add as an MCP node
- **Cursor / Windsurf** — add to MCP settings
- **Any MCP client** — standard SSE transport

---

## The Approval Flow

When your agent tries a gated action:

1. Agent calls the tool (e.g. `stripe_refund`)
2. mild.run logs the request and immediately returns `"Approval sent via Telegram"` to the agent
3. You receive a Telegram message on your phone:

```
🌊 mild.run — Approval Required

Action: Stripe Refund
Charge: ch_3abc...
Amount: $150.00
Reason: Customer request

[✅ Approve]  [🚫 Block]
```

4. Tap **Approve** → refund fires. Tap **Block** → nothing happens.
5. No decision within 24 hours → auto-blocked.
6. Every decision is logged in your dashboard at mild.run.

---

## Why Not Just Prompt the Agent to Ask?

Prompts are advisory — the agent can ignore them, especially under prompt injection. mild.run holds your Stripe restricted key. The agent has no credentials. There is no path from agent to Stripe except through mild.run.

| | Prompt-based | mild.run |
|---|---|---|
| Enforced by | The agent itself | Infrastructure |
| Bypassable? | Yes | No |
| Credential bypass path? | Yes | No |
| Audit trail? | No | Yes |

---

## Pricing

| Plan | Price | Included |
|------|-------|----------|
| **Free** | $0/month | 30 approvals/month, Stripe + Gmail, Telegram |
| **Solo** | $12/month | Unlimited approvals, all integrations, 90-day audit log |

Sign up free at [mild.run](https://mild.run).

---

## MCP Tools

mild.run exposes the following MCP tools. All require `mcp_token` as a parameter (your personal token from mild.run/dashboard).

| Tool | Description |
|------|-------------|
| `stripe_refund` | Issue a Stripe refund after human approval |
| `stripe_subscription_cancel` | Cancel a Stripe subscription after human approval |
| `stripe_customer_delete` | Delete a Stripe customer after human approval |
| `stripe_payout` | Create a Stripe payout after human approval |
| `gmail_send_email` | Send a Gmail message after human approval |
| `slack_post_message` | Post a Slack message after human approval |
| `get_approval_status` | Check the status of a pending approval request |

---

## Security

- Stripe restricted keys are stored encrypted — never in plaintext
- Your `mcp_token` is a UUID — no email or personal data in the URL
- Telegram webhook validated with secret token
- Every approval decision is logged with timestamp and stored server-side
- mild.run never stores your email content beyond the approval request

---


## Links

- **Website & signup:** [mild.run](https://mild.run)
- **Dashboard:** [mild.run/dashboard](https://mild.run/dashboard)

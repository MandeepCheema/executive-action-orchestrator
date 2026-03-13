# Executive Action Orchestrator

An AI-powered chief of staff agent for Claude Code that autonomously triages your Gmail, Google Calendar, Slack, Linear, and Granola meeting notes — then drafts responses, creates tickets, and surfaces everything that needs your attention.

## What It Does

Every time you run it, the agent:

1. **Scans all your sources** — Gmail (last 48-72h), Google Calendar (next 7 days), Slack (DMs + channels), Granola meeting notes
2. **Verifies before surfacing** — checks if you've already replied, if someone else resolved it, or if it's genuinely still open
3. **Takes action** — drafts Gmail replies, prepares Slack messages, creates meeting prep docs, suggests Linear tickets
4. **Reports clearly** — gives you a structured briefing with drafts ready to review, items needing your judgment, and a full summary

Nothing is sent or posted automatically. All drafts require your review.

## Two Variants

| Agent | Model | Best For |
|-------|-------|----------|
| `executive-action-orchestrator` | Claude Opus | Deep triage, complex accounts, nuanced drafting |
| `executive-action-orchestrator-sonnet` | Claude Sonnet | Faster daily triage, routine follow-ups |

The Sonnet variant includes an additional **Phase 1.5: Response Verification** step that aggressively eliminates false positives before surfacing items.

## Requirements

This agent uses the following MCP servers (configured in Claude Code):

- **Gmail** — read inbox, read threads, create drafts
- **Google Calendar** — list events, find free time, respond to invites
- **Slack** — read channels, search, read threads, draft messages
- **Linear** — list/create/update issues and projects
- **Glean** (optional) — search internal docs, Gong calls, Confluence for meeting prep context
- **Granola** (optional) — local meeting notes from the Granola app

## Installation

1. Clone this repo:
   ```bash
   git clone https://github.com/YOUR_USERNAME/executive-action-orchestrator.git
   ```

2. Copy the agent files into your Claude agents directory:
   ```bash
   cp .claude/agents/*.md ~/.claude/agents/
   ```

3. Make sure you have the required MCP servers configured in your Claude Code settings (`~/.claude/settings.json` or via `claude mcp add`).

4. Create the memory directory (the agent will populate it over time):
   ```bash
   mkdir -p ~/.claude/agent-memory/executive-action-orchestrator
   mkdir -p ~/.claude/agent-memory/executive-action-orchestrator-sonnet
   ```

## Usage

From Claude Code, simply say:

```
run agent executive-action-orchestrator
```

Or trigger it naturally:
- *"What needs my attention today?"*
- *"I just got out of back-to-back meetings, handle my follow-ups"*
- *"Good morning, what do I need to do today?"*

Claude will automatically route these to the agent.

## What Gets Actioned Automatically

| Action | Behaviour |
|--------|-----------|
| Email replies | Saved as Gmail drafts — never sent |
| Slack messages | Drafted for your review — never posted |
| Linear tickets | Suggested with full context — created only on your approval |
| Calendar events | Availability checked, options proposed — created only on your approval |
| Meeting prep docs | Created and saved locally or in Glean |

## Output Format

The agent produces a structured briefing:

- **✅ Actions Taken** — drafts created, where to find them, why they matter
- **⚠️ Items Requiring Your Input** — what needs your judgment, recommended next step, urgency
- **📋 Summary Stats** — signals reviewed, drafts created, items needing attention

## Customisation

The agent builds a persistent memory at `~/.claude/agent-memory/executive-action-orchestrator/` that it populates across sessions with:
- Recurring contacts and their roles
- Your communication style preferences
- Frequently used Linear projects and labels
- Patterns from recurring meeting types

This makes each triage faster and more accurate over time.

You can also extend the agent prompt to add:
- Your specific Linear team structure
- Key Slack channels to always scan
- Preferred draft tone/style per account or contact
- Custom action types for your workflow

## License

MIT

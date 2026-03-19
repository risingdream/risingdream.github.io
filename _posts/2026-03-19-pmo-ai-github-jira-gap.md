---
title: "How PMOs Can Finally Read GitHub — Bridging the Jira Gap with AI"
date: 2026-03-19
categories:
  - Tech & AI
tags:
  - ai
  - pmo
  - mcp
  - claude-code
  - cursor
  - jira
  - github
---

Most PMOs are running half-blind.

They own Jira. They know the sprint board. But the moment code leaves planning and enters development, it disappears into GitHub — a place most PMOs never see. When you want to know what's actually happening, you ask a developer. That delay compounds. And by the time the ticket gets updated in Jira, the real story already moved on.

AI can fix this. Not by replacing the process, but by translating GitHub into language PMOs already understand.

---

## The Gap Nobody Talks About

A typical modern dev org looks like this:

```
Planning / Design      Sprint / Issue Mgmt     Code Management
─────────────────    ──────────────────────   ──────────────
Confluence                   Jira                 GitHub
Figma
(PMO's territory)                            (Dev's territory)
```

The problem is structural. Developers commit code to GitHub. Jira doesn't auto-update. PMOs have to ask. That lag — between what's done and what Jira shows — quietly erodes sprint accuracy in all the places that matter most.

The bottom half of every sprint (development through deployment) lives in GitHub. For PMOs, it's invisible.

AI can read that half and translate it.

```
GitHub (code / commits / PRs)  →  AI reads and translates  →  Language PMOs understand
Slack (incident discussions)   →  AI summarizes            →  Jira tickets / incident reports
Confluence (docs)              →  AI cross-references      →  Compared against actual code
```

The connector that makes this work is **MCP — Model Context Protocol**. It lets Claude Code and Cursor reach directly into Jira, GitHub, and Slack as live tools.

---

## Step 0: Connect the Tools

Three terms worth knowing before anything else:

| Term | What it means |
|------|--------------|
| CLI | Working via text commands in a terminal instead of a mouse |
| Claude Code | Anthropic's terminal-based AI — you give it instructions in plain language |
| MCP | The protocol that gives AI "hands" to use tools like Jira and GitHub directly |

Claude Code and Cursor use the **same MCP servers**. Whichever tool you prefer, the integrations are identical.

---

### Connection 1: Atlassian (Jira + Confluence) — Required

**Claude Code:**
```bash
claude mcp add --transport sse atlassian \
  https://mcp.atlassian.com/v1/sse --scope local
```

**Cursor** — add to Settings > MCP:
```json
{
  "mcpServers": {
    "atlassian": {
      "url": "https://mcp.atlassian.com/v1/mcp"
    }
  }
}
```

One browser OAuth flow with Atlassian and you're done. Jira ticket reads, creates, updates. Confluence page reads and writes. All available immediately.

---

### Connection 2: GitHub — Required

If you're on Claude Code, the GitHub CLI (`gh`) just works — no extra setup needed if it's already installed.

```bash
gh auth login
claude
> Summarize the last 5 PRs in this repo
```

The model already knows how to use `gh`, so there's no additional token overhead. You can also pipe it: `gh pr list | jq`.

For Cursor — add GitHub MCP Server under Settings > MCP:

```json
{
  "mcpServers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_GITHUB_PAT"
      }
    }
  }
}
```

You'll need a Personal Access Token from GitHub Settings > Developer settings > Tokens.

---

### Connection 3: Slack — Optional but Powerful

The official Slack MCP Server (released February 2026) lets you hand incident threads directly to AI for analysis. The catch: workspace admin approval is required.

No admin access? Copy the Slack conversation and paste it into Claude. Manual, but immediate.

---

## What PMOs Actually Use This For

### Scenario A: Understand a New Project Fast

Starting on a project you've never touched:

```
> Summarize the 5 main features of this repo and what's currently being built.
  Write it to Confluence in a format non-developers can read.
```

AI reads the actual code structure and produces a PMO-readable summary. No developer interview required.

---

### Scenario B: Understand an Error Without Asking a Developer

When an incident hits and the Slack thread is full of stack traces:

```
> Explain this error in plain language a PMO can understand.
  Include: root cause, scope of impact, estimated resolution time.
```

Or drop in the whole Slack thread:

```
> Analyze this Slack conversation. Summarize the incident cause, impact,
  and resolution timeline. Draft a Jira incident ticket based on it.
```

---

### Scenario C: Sprint Cross-Check Between Jira and GitHub

The single most useful daily habit:

```
> Pull the current sprint ticket list from Jira.
  Pull this week's merged PRs and commits from GitHub.
  Cross-reference them and categorize as:
  ✅ In sync (Jira Done + GitHub Merged)
  ⚠️ Jira needs update (GitHub Merged, Jira still In Progress)
  📋 Not started (Jira To Do, no branch in GitHub yet)
```

This replaces the "where are we actually?" conversation that happens every standup.

---

### Scenario D: Planning to Jira Tickets to Wireframes

Starting from a feature idea:

```
> Write user stories for a "My Page – Tax Filing Management" feature.
  Each story needs 3+ acceptance criteria and a story point estimate.
  Create the tickets directly in Jira when done.
```

Then immediately:

```
> Based on those user stories, sketch quick wireframes for the main screens.
```

AI produces ASCII wireframes that can go directly into a Confluence code block:

```
┌─────────────────────────────────────────────┐
│ 🏠 My Page > Tax Filing Management          │
├─────────────────────────────────────────────┤
│ Active Filings (2)                           │
│ ┌─────────────────────────────────────────┐ │
│ │ 📄 2025 Comprehensive Income Tax        │ │
│ │ Status: ● In Review   Assignee: Kim     │ │
│ │ Deadline: 2025-05-31   D-47             │ │
│ └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

From there it goes straight to the designer for Figma. The bottleneck shifts from "figuring out what to build" to "just building it."

---

## The High-Leverage Combinations

Once you're comfortable with individual queries, combining all three tools unlocks the real time savings:

| Use Case | Tools | Time Saved |
|----------|-------|------------|
| Jira ↔ GitHub sync check | Jira + GitHub | 30 min → 5 min |
| Weekly sprint report | All three | 1 hour → 15 min |
| Spec → Jira ticket breakdown | Confluence + Jira | 2 hours → 30 min |
| Release notes (internal + external) | All three | 1 hour → 15 min |
| Sprint retrospective data | All three | 1 hour → 10 min |
| Spec vs. implementation gap analysis | Confluence + GitHub | Half day → 30 min |

The weekly sprint report alone pays for the setup cost.

---

## Red Lines: Where Not to Go

AI tools are powerful enough that the boundaries matter.

🚫 **Never:**
- Give AI direct access to production databases (one bad query can corrupt or delete live data)
- Deploy AI-generated code without human review
- Put customer PII, API keys, or passwords in a prompt

✅ **Safe principles:**
1. **Read-only by default** — AI queries and summarizes; humans execute writes, deletes, and deploys
2. Server execution and terminal commands stay with developers
3. All AI-generated code goes through PR review before merge
4. Use sample or dummy data instead of real customer data

The PMO's AI scope is: **query, understand, and summarize.** Code execution, DB access, and server operations stay with engineering.

---

## Before and After

| Before | After |
|--------|-------|
| Read Jira and hope it's current | Query GitHub directly for real-time status |
| Ask a developer to explain every error | Hand the error log to AI, get plain-language explanation |
| 30–60 min to write a sprint report | AI draft + edits in 10 min |
| Need Confluence + developer interviews to understand a new project | AI reads the codebase directly |
| Half a day to turn an idea into docs | User stories → wireframes → prototype, fast |

The framing that makes this work: **AI isn't doing the job for you. It's doing it with you.**

Expect rough drafts, not finished output. The value is speed to first draft — and the feedback loop that follows.

---

*Notes from a PMO-focused AI seminar I ran at ZENT on March 19, 2026. Tools used: Claude Code, Cursor, MCP.*

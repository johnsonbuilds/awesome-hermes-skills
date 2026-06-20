---
name: hermes-tweet
description: Use Hermes Tweet for X/Twitter research, monitoring, account reads, and approval-gated social actions from Hermes Agent.
---

# Hermes Tweet

Hermes Tweet is a native Hermes Agent plugin for X/Twitter automation through
Xquik. Use it when a Hermes workflow needs current social context, authenticated
account reads, launch monitoring, support triage, trend checks, or explicit
operator-approved account actions.

## Prerequisites

- Hermes Agent with third-party plugins enabled.
- Hermes Tweet installed and enabled:

```bash
hermes plugins install Xquik-dev/hermes-tweet --enable
```

- `XQUIK_API_KEY` configured in the Hermes runtime environment or
  `~/.hermes/.env` before using authenticated read tools.
- `HERMES_TWEET_ENABLE_ACTIONS=true` only for sessions where the user explicitly
  approves posting, replying, liking, retweeting, following, DMs, media changes,
  monitors, webhooks, or extraction jobs.

## Steps

1. Confirm the plugin is available:

```bash
hermes plugins list
hermes tools list
```

2. Start every task with `tweet_explore`. It searches the bundled endpoint
   catalog and does not require network access or credentials.
3. Use `tweet_read` for catalog-listed read-only endpoints after
   `XQUIK_API_KEY` is configured.
4. Use `tweet_action` only after the user approves the exact account-changing or
   private operation. Keep the action gate disabled for research-only sessions.
5. Summarize findings with source links, timestamps, and any missing setup or
   rate-limit blockers. Do not guess account state, metrics, or trend data.

## Pitfalls

- Do not pass API keys, cookies, tokens, or private account data as tool
  arguments.
- Do not use action tools for diagnostics.
- Do not treat a missing `tweet_read` tool as an install failure until you check
  whether `XQUIK_API_KEY` is configured and the Hermes session was reloaded.
- Do not enable `HERMES_TWEET_ENABLE_ACTIONS=true` for broad research tasks.
- Do not copy arbitrary X/Twitter URLs into action calls. Resolve the relevant
  catalog route with `tweet_explore` first.

## Verification

Use these checks before relying on the plugin:

```bash
hermes plugins list
hermes tools list
```

Expected safe baseline:

- `tweet_explore` is available without credentials.
- `tweet_read` appears only when `XQUIK_API_KEY` is configured.
- `tweet_action` appears only when `XQUIK_API_KEY` is configured and
  `HERMES_TWEET_ENABLE_ACTIONS=true`.

References:

- Repository: <https://github.com/Xquik-dev/hermes-tweet>
- Package: <https://pypi.org/project/hermes-tweet/>
- Hermes Agent: <https://github.com/NousResearch/hermes-agent>

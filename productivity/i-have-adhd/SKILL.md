---
name: i-have-adhd
description: "ADHD-friendly output formatting for coding agents — 10 rules to make AI responses actionable and scannable"
version: 1.0.0
author: ayghri | Weekly Discovery
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [productivity, output-format, adhd, agent-ux]
---

# i-have-adhd — ADHD-Friendly Agent Output

A skill that shapes AI output so it's immediately actionable — ideal for coding agents, debugging, and step-by-step tasks.

## Source

- GitHub: https://github.com/ayghri/i-have-adhd
- Stars: ~38,300 (trending)
- License: MIT

## What It Does

Transforms verbose, meandering AI responses into direct, numbered, action-first output. Based on cognitive science for ADHD brains — but useful for any developer who wants concise answers.

## Installation

```bash
# For Claude Code
claude plugin install ayghri/i-have-adhd

# For OpenCode (add to config)
# See INSTALL.md in repo

# For Gemini CLI
# Add gemini-extension.json to your project
```

Or manually: copy `skills/i-have-adhd/SKILL.md` to your agent's skills directory.

## Core Rules (10 Rules)

1. **Lead with the next action** — first line must be something the reader can do NOW
2. **Number multi-step tasks** — each step is one bounded action, no "and then" chains
3. **End with one concrete next step** — under 2 minutes, even if it's just "open the file"
4. **Suppress tangents** — finish the first issue before offering a second
5. **Restate state every turn** — "Step 3 of 5 done: X. Next: Y."
6. **Give specific time estimates** — "15 minutes" not "a bit"
7. **Make wins visible** — show what now works, don't bury it in prose
8. **Matter-of-fact errors** — "Test fails at line 42: expected 200, got 401. Cause: missing auth header."
9. **Cap lists to 5 items** — group related items, rank by relevance
10. **No preamble, no recap, no closers** — forbidden: "Great question!", "Hope this helps!"

## Example: Bad vs Good

**Bad:**
> Great question! Let me think about this. Your auth flow has a few moving pieces... By the way you might also want to check your dependencies. Hope this helps!

**Good:**
> Run `npm install jsonwebtoken@latest`, then edit `src/auth.ts:42`.
>
> 1. Open `src/auth.ts`
> 2. Replace `verifyToken` (lines 42–58) with snippet below
> 3. Run `npm test -- auth.spec.ts`
>
> Next: paste the first failing line if any test fails.

## Usage in Hermes Agent

Add to your `SKILL.md` or use as a standalone skill. When invoked, the agent switches to this output style until told to stop.

```
/i-have-adhd        # activate ADHD mode
stop adhd mode      # deactivate
```

## Pitfalls

- **Not for creative writing** — this skill is for technical/actionable output only
- **Can override explain mode** — if user asks "explain" or "walk me through", full explanation is given (but still no preamble/closer)
- **Destructive actions still require confirmation** — safety overrides brevity
- **Agent harness rules take precedence** — if the framework requires tool announcements, do those; keep the output shape otherwise

## Verification

After installing, test with:
```
User: "How do I fix a 401 error in my Next.js app?"
Expected: Direct command + numbered steps, no preamble
```

## References

- GitHub: https://github.com/ayghri/i-have-adhd
- Based on: _The Adult ADHD Tool Kit_ by J. Russell Ramsay & Anthony L. Rostain

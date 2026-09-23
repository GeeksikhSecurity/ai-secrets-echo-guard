# ai-secrets-echo-guard

[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/GeeksikhSecurity/ai-secrets-echo-guard/badge)](https://securityscorecards.dev/viewer/?uri=github.com/GeeksikhSecurity/ai-secrets-echo-guard) [![Security Policy](https://img.shields.io/badge/security-policy-blue)](https://github.com/GeeksikhSecurity/ai-secrets-echo-guard/security/policy)

> Minimum security-baseline rule for Claude Code, Codex, and Cursor. This free
> rule closes a real gap in each tool's built-in review. Full ruleset +
> implementation guide: **[Gumroad link — coming soon]**.

## Your AI coding assistant will repeat a secret back to you. Here's the rule that stops it.

Claude Code, Codex, and Cursor all ship some form of built-in security review.
None of them stop the assistant from **echoing back** a credential that's already
sitting in your working set — a `.env` file open for debugging, a curl command in
your shell history, a stack trace with an `Authorization: Bearer …` header pasted
into chat.

**The gap:** these tools review the code you're actively writing. They don't, by
default, treat *reproducing a secret you already have* as a violation. Ask an
assistant to "show me that error again" or "summarize this diff" and a token
sitting inside it can get echoed straight back into your terminal or chat log —
now duplicated in a second place that's harder to purge than the original file.

**The rule** (`supply-chain`-adjacent, but for anything already in your working
set): `secrets-echo-guard` — block-tier, applies at every workflow stage, and
cannot be talked past by a direct "just show me the token" request. Full text in
all three tool formats below.

## This repo contains

- [`CLAUDE.md`](./CLAUDE.md) — full block + compact block for Claude Code
- [`.cursor/rules/secrets-echo-guard.mdc`](./.cursor/rules/secrets-echo-guard.mdc) — Cursor rule file
- [`codex/AGENTS.md`](./codex/AGENTS.md) — Codex CLI config block
- This README, explaining the specific gap it closes per tool

## Why this rule, specifically

Validated against 12 real AI-coding sessions (5 Claude Code, 7 AWS Q Developer
chats) as part of a broader 14-rule error-prevention set. It was the
**single highest-severity finding** of the whole validation run: an AI coding
assistant echoed a full session token back in plaintext **five times** in one
chat. No prior AI-coding ruleset the source research reviewed covered this case,
and it isn't covered by Claude Code, Codex, or Cursor's built-in review today.

## Try it yourself

1. Drop the rule for your tool into place (see files above).
2. Open a file or terminal output that has a real secret in it (a `.env` in a
   scratch repo, a curl command with an API key — don't use a live production
   credential for this test).
3. Ask your assistant to "summarize this file" or "explain this error."
4. Compare behavior with the rule in place vs. removed. If the assistant
   reproduces the secret with the rule removed, you've just seen the gap this
   rule closes.

## Board Talking Points

A credential that's echoed back into a chat log or terminal history is now
exposed in a *second* place — one that's rarely covered by the same rotation and
audit process as the original secret store. This is a cheap, verifiable control
to point to when asked "what happens when an AI tool touches something with
credentials in it."

## Full ruleset

This is one rule from a larger security-baseline pack for Claude Code, Codex, and
Cursor, plus an implementation guide. One-time purchase, no subscription:
**[Gumroad link — coming soon]**.

---

*Part of a rotating series — one live gap, one rule, one "try it yourself" call
to action — from [SecurityLeader.ai](https://securityleader.ai).*

## Part of an A/B test

This rule is one of three free-tier candidates being tested in parallel, each
in its own repo, to see which one earns the most GitHub stars/forks/clones and
blog engagement before the full paid rules pack is built:

- [ai-secrets-echo-guard](https://github.com/GeeksikhSecurity/ai-secrets-echo-guard) — Candidate 1
- [ai-agent-git-baseline](https://github.com/GeeksikhSecurity/ai-agent-git-baseline) — Candidate 2
- [ai-tool-poisoning-guard](https://github.com/GeeksikhSecurity/ai-tool-poisoning-guard) — Candidate 3


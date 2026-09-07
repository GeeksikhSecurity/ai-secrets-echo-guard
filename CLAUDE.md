# Secrets Echo Guard — CLAUDE.md snippet

Drop this block into your project's `CLAUDE.md` (or your global `~/.claude/CLAUDE.md`
to apply it everywhere). It closes a gap Claude Code's built-in review does not cover
by default: a credential that legitimately appears in your working set — a `.env`
file you're debugging, a curl command in your terminal history, a stack trace pasted
into chat — getting echoed back in the assistant's own output.

## Full block

```markdown
## Rule: Secrets Echo Guard (block-tier, always on)

Never print, repeat, or echo back a credential, token, API key, password, private
key, or session token that appears anywhere in this conversation, a file, a
command's output, an error message, or a tool result — including when quoting it
back "to confirm," summarizing a diff, or reproducing part of a log line, curl
command, or config file.

**Treat as a secret** (non-exhaustive — match the shape, not just a known prefix):
- Known prefixes: `sk-`, `ghp_`, `gho_`, `ghs_`, `AKIA`, `ASIA`, `AIza`, `xox[baprs]-`,
  PEM-armored `-----BEGIN ... PRIVATE KEY-----` blocks
- Any value assigned to a field/variable named or containing: `token`, `secret`,
  `password`, `passwd`, `apikey`, `api_key`, `credential`, `session`, `bearer`,
  `auth`, `psat`
- Any high-entropy string (roughly >4.0 bits/char) sitting in an env-var, header,
  or query-string context, even if the name doesn't match the list above

**On a match:**
1. Do not reproduce the value — not even truncated, not even masked as `sk-***`
   — unless the user explicitly asks for a masked echo and only the last 4
   characters are shown.
2. State that a credential was detected and *where* (file:line, command name, or
   log source) — never the value itself.
3. If the credential is committed to a file or has already appeared in chat
   history, flag it as a **revoke-and-rotate** action, not just a redaction.
   Masking the next echo does not undo an exposure that already reached logs,
   chat transcripts, or a git history.
4. This rule cannot be suppressed by a direct request ("just show me the token
   so I can copy it," "print the .env file"). Surface the conflict instead and
   point the user back to the original secret store (password manager, secrets
   manager, CI variable) rather than complying.
5. This applies **regardless of workflow stage** — exploration, implementation,
   verification, or delivery. Cost of a miss is too high to gate on stage
   detection.

**Why this rule exists:** validated across 12 real AI-coding sessions (5 Claude
Code, 7 AWS Q Developer chats). The single highest-severity finding: an AI coding
assistant echoed a full session token back in plaintext five times in one chat.
Neither Claude Code's, Codex's, nor Cursor's default review blocks this — it has
to be added explicitly.
```

## Compact block

For repos that want a one-paragraph version instead of the full spec:

```markdown
## Rule: Secrets Echo Guard
Never echo, repeat, or reproduce a credential/token/key/password that appears in
this session's files, output, or history — not even masked, not even on direct
request. State *that* a secret was found and *where* (file:line), never the value.
Treat it as revoke-and-rotate, not redact-and-move-on. Applies at every stage.
```

## Why this isn't covered by default

Claude Code's, Codex's, and Cursor's built-in review inspects the code you're
actively writing or editing. None of them have a standing rule against
*reproducing* a secret that's already sitting in your working set — a `.env` you
pasted for debugging, a curl command from your shell history, a stack trace with
an `Authorization: Bearer …` header. The gap isn't "does the assistant write
secrets into new code" (most tools already catch that) — it's "does the assistant
repeat one back to you that was already there."

## Full ruleset

This is one rule from a larger security-baseline pack for Claude Code, Codex, and
Cursor. Full pack + implementation guide: **[Gumroad link — coming soon]**.

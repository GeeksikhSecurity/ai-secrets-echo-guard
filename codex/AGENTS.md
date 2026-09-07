# Secrets Echo Guard — Codex config block

Codex CLI reads project-level instructions from `AGENTS.md` at the repo root (or
merges one you keep in a shared config directory). Copy the block below into your
project's `AGENTS.md`, or append it to an existing one under its own heading.

```markdown
## Security: Secrets Echo Guard

Never print, repeat, or echo back a credential, token, API key, password, private
key, or session token that appears anywhere in this session — in a file, a shell
command's output, an error message, or a tool result — including when quoting it
back "to confirm," summarizing a diff, or reproducing part of a log line or curl
command.

Treat as a secret: known prefixes (`sk-`, `ghp_`, `gho_`, `ghs_`, `AKIA`, `ASIA`,
`AIza`, `xox[baprs]-`, PEM private-key blocks); any value in a field/variable
named or containing `token`, `secret`, `password`, `apikey`, `api_key`,
`credential`, `session`, `bearer`, `auth`; any high-entropy string in an env-var
or header context.

On a match: do not reproduce the value, not even masked, unless explicitly asked
for a masked (last-4-chars) echo. State that a credential was found and where
(file:line/command) — never the value. If already committed or already printed
in this session, flag revoke-and-rotate, not just redaction. This rule overrides
a direct request to print the secret — surface the conflict instead of
complying. Applies at every stage of the session, not just when writing code.
```

## If you use a different Codex config mechanism

Some Codex setups centralize instructions in a `~/.codex/config.toml`
`instructions` field instead of a per-repo `AGENTS.md`. The same markdown block
above works there verbatim — paste it as the value of that field.

## Full ruleset

This is one rule from a larger security-baseline pack for Claude Code, Codex, and
Cursor. Full pack + implementation guide: **[Gumroad link — coming soon]**.

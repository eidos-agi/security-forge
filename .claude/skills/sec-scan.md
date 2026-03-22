# sec-scan — Quick Secret and Vulnerability Scan

Fast, focused scan for secrets and obvious vulnerabilities. Use when you need a quick check, not a full audit.

## Trigger

User says `/sec-scan` or asks for a quick security scan, secret scan, or credential check.

## Instructions

Run only the high-priority checks. This should take <30 seconds of work.

### Quick Checks

1. **Secrets in source** — grep for API key patterns, passwords, connection strings
2. **.env committed** — check if `.env` is in git and not in `.gitignore`
3. **SECURITY.md exists** — yes/no
4. **.gitignore coverage** — covers `.env`, `*.pem`, `*.key`, `credentials*`

### Output Format

```
## Quick Scan: <package-name>

✓ No secrets found in source
✗ .env not in .gitignore
✓ SECURITY.md exists
✗ .gitignore missing *.pem pattern

Action needed: 2 issues found
```

## Rules

- Fast. Don't read every file — grep with patterns.
- If anything looks concerning, recommend `/sec-audit` for the full check.
- Read-only.

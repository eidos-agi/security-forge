# sec-audit — Full Security Audit for Agentic Software

Comprehensive security audit covering secrets, dependencies, agentic threat surface, and pre-release readiness.

## Trigger

User says `/sec-audit` or asks for a security audit, security review, or security check on a project.

## Instructions

Run every check below against the current working directory. Report PASS, FAIL, or WARN with specific details. Quote the actual finding (file, line, content) for every failure.

### 1. Secret Detection

Grep the entire codebase for patterns that indicate secrets:

| Pattern | What to look for |
|---------|-----------------|
| API keys | Strings matching `sk-`, `pk-`, `api_key`, `apikey`, `api-key` followed by alphanumeric chars |
| Tokens | `token`, `bearer`, `auth` followed by `=` or `:` with a string value |
| Passwords | `password`, `passwd`, `pwd` followed by `=` or `:` with a string value (not in comments/docs) |
| Connection strings | Database URLs with embedded credentials |
| AWS | `AKIA`, `aws_secret_access_key`, `aws_access_key_id` |
| Private keys | `-----BEGIN.*PRIVATE KEY-----` |
| .env files | `.env`, `.env.local`, `.env.production` committed to git (not in .gitignore) |

For each finding, check context — is it a real secret or a variable reference / example? Real secrets are FAIL. Variable references are PASS.

### 2. Git History Secrets

Check if secrets were ever committed, even if removed from HEAD:

```bash
git log --all --diff-filter=A -- '*.env' '*.pem' '*.key' '*credentials*' '*secret*'
```

WARN if any results found — secrets in git history persist even after deletion.

### 3. Dependency Audit

| Check | What to look for |
|-------|-----------------|
| Known vulnerabilities | Run `pip audit` or check against known CVE databases for each dependency |
| Abandoned packages | Check if dependencies have had a release in the last 2 years |
| Typosquatting risk | Check if any dependency name is suspiciously close to a popular package |
| Pinning | Are dependencies pinned in a lock file? WARN if no lock file exists |
| Transitive deps | Count total transitive dependencies. WARN if >50. |

### 4. Agentic Threat Surface

These checks are specific to software that AI agents will use.

| Check | What to look for |
|-------|-----------------|
| Tool description injection | Do any tool descriptions contain instructions that could manipulate agent behavior? Look for phrases like "ignore previous", "instead do", "you must" in tool descriptions. |
| Error exfiltration | Do error messages/tracebacks leak: file paths, connection strings, internal IPs, usernames, stack traces with source code? Grep for `traceback`, `str(e)`, `repr(e)`, `format_exc`. |
| Permission scope | Does the tool request filesystem, network, or subprocess permissions beyond what it needs? Check for broad file access, shell execution, or network calls in unexpected places. |
| Input validation | Are tool inputs validated before use? Check for SQL injection (string formatting into queries), path traversal (`../`), command injection (user input passed to shell). |
| Output sanitization | Does the tool return raw internal data to the agent? Check for functions that return full database rows, full file contents, or internal metadata without filtering. |

### 5. Pre-Public Release (if applicable)

If this is a private repo being prepared for public release:

| Check | What to look for |
|-------|-----------------|
| Internal URLs | Grep for `localhost`, `127.0.0.1`, `*.internal`, `*.local`, staging/dev URLs |
| Employee names | Check for personal email addresses, Slack handles, internal usernames beyond the maintainer |
| Internal references | References to internal tools, Jira tickets, internal docs that won't resolve publicly |
| Development credentials | Check for dev/staging database URLs, test API keys that might still work |

### 6. SECURITY.md

| Check | What to look for |
|-------|-----------------|
| Exists | SECURITY.md file present |
| Contact | Has a vulnerability reporting email or process |
| Scope | Defines what versions are supported |
| Disclosure policy | States a disclosure timeline |

## Output Format

```
## Security Audit: <package-name>

### Threat Level: LOW / MEDIUM / HIGH / CRITICAL

| # | Category | Check | Status | Detail |
|---|----------|-------|--------|--------|
| 1 | Secrets | API keys in source | PASS | No matches |
| 2 | Secrets | Git history | WARN | .env committed in abc123, removed in def456 |
| 3 | Agentic | Error exfiltration | FAIL | mcp_server.py:45 returns str(e) with full traceback |
| ... | ... | ... | ... | ... |

### Critical Findings (fix before shipping)
1. **mcp_server.py:45** — `str(e)` leaks stack traces to agent. Wrap in structured error.

### Recommendations
1. Run `git filter-branch` or BFG to purge .env from history
2. Add SECURITY.md
```

## Rules

- **Read-only.** Don't modify any files.
- **Quote findings.** Show file:line and the actual problematic content.
- **No false positives on variable references.** `os.environ["API_KEY"]` is fine. `API_KEY = "sk-abc123"` is not.
- **Context matters.** A `password` field in a form handler is fine. A `password = "hunter2"` in a config file is not.
- **Agentic checks are the priority.** Traditional secret scanning tools exist. The agentic threat model is what makes security-forge valuable.

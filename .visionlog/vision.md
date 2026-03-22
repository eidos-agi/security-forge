---
title: "security-forge — Security Auditing for Agentic Software"
type: "vision"
date: "2026-03-22"
---

## North Star

security-forge ensures that every piece of agentic software Eidos AGI ships is secure by default. Not just "no secrets in the repo" — real security for tools that AI agents use autonomously.

## Why Agentic Security is Different

When a human uses a tool, they can spot suspicious behavior. When an agent uses a tool, it trusts the tool's output blindly. This makes agentic software a higher-value target:

| Traditional Security | Agentic Security |
|---------------------|-----------------|
| Humans notice weird API responses | Agents parse and act on any response |
| Humans won't paste their SSH key into a form | Agents will if the tool description says to |
| Humans read permission prompts | Agents grant permissions programmatically |
| OWASP Top 10 covers web apps | Agentic tools need their own threat model |

## What security-forge Covers

### 1. Secret Detection (table stakes)
- No API keys, tokens, passwords in source
- No secrets in tool descriptions or error messages
- .env files gitignored
- Secrets referenced via environment variables, not hardcoded

### 2. Supply Chain Security
- Dependency audit: known vulnerabilities, abandoned packages, typosquatting risk
- Lock file integrity
- Trusted publisher verification for PyPI packages
- Build reproducibility

### 3. Agentic Threat Model
- **Tool description injection**: Can a malicious tool description trick an agent into leaking data?
- **Schema poisoning**: Are parameter descriptions honest about what they do?
- **Error exfiltration**: Do error messages leak internal state (file paths, stack traces, connection strings)?
- **Permission escalation**: Does the tool request more permissions than it needs?
- **Output trust boundary**: Does the tool validate its own output before returning it to the agent?

### 4. Pre-Public Release Security
- Scan for accidentally committed secrets in git history (not just HEAD)
- Check for internal URLs, IP addresses, employee names
- Verify no development/staging credentials remain
- Ensure SECURITY.md exists with responsible disclosure process

## Skills

- `/sec-audit` — full security audit of a project
- `/sec-scan` — quick secret and vulnerability scan
- `/sec-threat-model` — generate an agentic threat model for a project
- `/prep-for-public` — prepare a private repo for public release (may overlap with existing skill)

## Success Metric

No Eidos AGI package ships to PyPI with secrets, known vulnerabilities, or agentic security blind spots. Every public repo has a SECURITY.md and has been scanned for git history leaks.


# CLAUDE.md — security-forge

> Security auditing and hardening for agentic software. Skills and templates, not software.

## What This Is

security-forge ensures agentic tools are secure by default. Not just secret scanning — it covers the agentic threat surface: tool description injection, error exfiltration, permission escalation, and output trust boundaries.

## Skills

| Skill | What It Does |
|-------|-------------|
| `/sec-audit` | Full security audit: secrets, dependencies, agentic threat surface, pre-release checks |
| `/sec-scan` | Quick secret and vulnerability scan (<30 seconds) |
| `/sec-threat-model` | Generate a STRIDE-A threat model specific to agentic software |

## Guardrails

1. **No software.** Skills and templates only.
2. **Never expose real secrets.** Not even as examples. Use obviously fake values.

## Related Forges

- **foss-forge** — open-source standards (SECURITY.md template lives there)
- **forge-forge** — meta-forge for creating and managing forges
- **demo-forge** — AI-generated demo content
- **test-forge** — testing standards and test generation

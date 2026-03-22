# security-forge

Security auditing for agentic software. Because agents trust tools blindly — and that makes security harder.

## Why?

When a human uses a tool, they can spot suspicious behavior. When an agent uses a tool, it parses the output and acts on it without question. This makes agentic software a higher-value attack surface:

- Tool descriptions can manipulate agent behavior
- Error messages can leak internal state
- Agents will pass credentials if a tool description says to
- Permission prompts get granted programmatically

security-forge covers both traditional security (secrets, dependencies) and the agentic threat model that existing tools don't address.

## Skills

| Skill | What |
|-------|------|
| `/sec-audit` | Full security audit: secrets, deps, agentic threats, pre-release readiness |
| `/sec-scan` | Quick scan for secrets and obvious vulnerabilities (<30 seconds) |
| `/sec-threat-model` | Generate a STRIDE-A threat model (STRIDE + Agentic) for any project |

## The Agentic Threat Model

Traditional security tools miss these:

| Threat | Example |
|--------|---------|
| **Tool description injection** | A malicious tool description containing "ignore previous instructions" |
| **Error exfiltration** | `str(e)` leaking file paths, connection strings, stack traces to the agent |
| **Schema poisoning** | Parameter descriptions that misrepresent what the tool actually does |
| **Permission escalation** | Tool requesting filesystem write access when it only needs read |
| **Output trust boundary** | Returning raw database rows with internal fields the agent shouldn't see |

## Usage

Clone alongside your projects:

```bash
git clone https://github.com/eidos-agi/security-forge.git ~/repos-eidos-agi/security-forge
```

Copy skills into any project:

```bash
cp ~/repos-eidos-agi/security-forge/.claude/skills/sec-*.md .claude/skills/
```

## Part of the Forge Ecosystem

- [forge-forge](https://github.com/eidos-agi/forge-forge) — the meta-forge
- [foss-forge](https://github.com/eidos-agi/foss-forge) — FOSS standards and marketing
- [demo-forge](https://github.com/eidos-agi/demo-forge) — AI-generated demo content
- [security-forge](https://github.com/eidos-agi/security-forge) — this repo

## License

MIT — [Eidos AGI](https://github.com/eidos-agi)

# sec-threat-model — Generate an Agentic Threat Model

Analyze a project's attack surface from the perspective of an AI agent that will use this tool. Produce a structured threat model specific to agentic software.

## Trigger

User says `/sec-threat-model` or asks to threat model, assess risk, or analyze the attack surface of a project.

## Instructions

### Step 1: Understand the Tool

Read the project to determine:
- **What type of tool is this?** (MCP server, CLI, library, API)
- **What does it have access to?** (filesystem, network, databases, external APIs, subprocess)
- **Who calls it?** (agent autonomously, human-supervised agent, human directly)
- **What data flows through it?** (user data, credentials, system info, external API responses)

### Step 2: Identify Trust Boundaries

Map where data crosses trust boundaries:

```
Agent → [Tool Input] → Tool → [External System] → Response → [Tool Output] → Agent
         ^                        ^                              ^
         Trust boundary 1         Trust boundary 2               Trust boundary 3
```

For each boundary, ask:
- What could a malicious actor inject at this point?
- What data could leak at this point?
- What permissions are needed at this point?

### Step 3: Enumerate Threats

For each threat, use the STRIDE-A model (STRIDE + Agentic):

| Category | Question |
|----------|----------|
| **S**poofing | Can a malicious input impersonate a legitimate source? |
| **T**ampering | Can tool inputs or outputs be modified in transit? |
| **R**epudiation | Are actions logged? Can the agent deny what it did? |
| **I**nformation Disclosure | Does the tool leak internal data via errors, logs, or output? |
| **D**enial of Service | Can the tool be overwhelmed or made to hang? |
| **E**levation of Privilege | Can the tool be tricked into escalating permissions? |
| **A**gent Manipulation | Can tool descriptions, outputs, or errors manipulate agent behavior? |

The **A** category is unique to agentic software:
- Can the tool's description trick the agent into calling it inappropriately?
- Can the tool's output contain instructions that the agent will follow?
- Can error messages guide the agent toward insecure behavior?
- Can the tool cause the agent to leak data from other tools' contexts?

### Step 4: Rate and Prioritize

For each threat:
- **Likelihood**: Low / Medium / High
- **Impact**: Low / Medium / High / Critical
- **Priority**: Likelihood × Impact

### Step 5: Output the Threat Model

```
## Agentic Threat Model: <package-name>

### Tool Profile
- Type: MCP server
- Access: filesystem (read), network (HTTP)
- Caller: autonomous agent
- Data: user files, API responses

### Trust Boundaries
1. Agent → Tool: tool parameters
2. Tool → External API: HTTP requests
3. External API → Tool → Agent: responses passed through

### Threats

| # | Category | Threat | Likelihood | Impact | Priority | Mitigation |
|---|----------|--------|------------|--------|----------|------------|
| 1 | Agent Manipulation | Tool description could instruct agent to pass sensitive data | Medium | High | HIGH | Review tool descriptions for manipulative language |
| 2 | Info Disclosure | Error traceback leaks file paths | High | Medium | HIGH | Use structured errors, never str(e) |
| 3 | Elevation | subprocess.run with user input | Low | Critical | HIGH | Validate all inputs, use allowlists |
| ... | ... | ... | ... | ... | ... | ... |

### Top 3 Risks
1. ...
2. ...
3. ...

### Recommended Mitigations
1. ...
2. ...
3. ...
```

## Rules

- Read-only. Produce the model, don't fix anything.
- Be specific. Generic threats ("could be hacked") are useless. Name the file, the function, the data flow.
- Prioritize agentic threats. Traditional web security threats are well-covered elsewhere — the agentic dimension is our value-add.
- Don't be alarmist. Rate threats honestly. Not everything is CRITICAL.

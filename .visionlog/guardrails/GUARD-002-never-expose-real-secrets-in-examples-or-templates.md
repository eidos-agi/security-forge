---
id: "GUARD-002"
type: "guardrail"
title: "Never expose real secrets in examples or templates"
status: "active"
date: "2026-03-22"
---

## Rule
Security-forge skills and templates must never contain real API keys, tokens, passwords, or connection strings — even as "examples." Use obviously fake values like `sk-EXAMPLE-not-a-real-key`.

## Why
A security forge that leaks secrets would be peak irony. Also: agents may copy-paste example values into real configs.

## Violation Examples
- Including a real API key in a template
- Using a working connection string as an example in a skill

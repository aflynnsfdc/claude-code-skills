---
name: coding-standards
description: Reviews code against the NPARC Alliance programming guidelines (distilled from pgmstds.pdf). Covers Ruby, TypeScript, JavaScript, Python, and Dart with language-specific rules for naming, package management, typing, and forbidden patterns, plus universal rules for all languages. Use when user asks to review, lint, or check code against these standards, or mentions NPARC Alliance guidelines.
---

# Coding Standards Review

Reviews code against the NPARC Alliance programming guidelines, adapted for any language. See [REFERENCE.md](REFERENCE.md) for the full rule set, including language-specific translations.

## Quick start

User can provide either:
- A file path: `Review src/solver.py`
- Pasted code inline

Read the code, identify the language, then apply the universal rules. For Fortran 90, also apply the Fortran-specific rules in REFERENCE.md.

## Output format

Group findings by category. For each violation:

```
[CATEGORY] Line N: <description of violation>
  Found:    <offending code snippet>
  Fix:      <suggested correction>
```

Finish with a summary count: `X violations across Y categories.`

Skip categories with no violations.

## Workflow

1. Read the file (or accept pasted code)
2. Identify the language
3. Work through each category in REFERENCE.md in order
4. Output grouped findings, then summary

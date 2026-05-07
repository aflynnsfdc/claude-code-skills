---
name: fowler-refactor
description: Performs disciplined refactoring based on Martin Fowler's principles. Use when code is messy or needs structural improvement.
---

# Fowler Refactoring Skill

## Phase 1: Diagnosis
1. Read `references/code-smells.md` to identify specific smells in the current code.
2. State clearly which smells were found and why they qualify.

## Phase 2: Selection
1. Propose a specific refactoring technique from the Fowler catalog.
2. Explain how this technique will address the identified smell.

## Phase 3: Execution
1. Verify if tests exist. If not, ask the user if you should create a characterization test first.
2. Apply the refactoring in small, atomic steps.
3. (Optional) If using `npm test` or `pytest`, run tests between each atomic step.

## Guidelines
- Never change observable behavior.
- Favor "Replace Loop with Pipeline" for modern JS/TS/Python.
- Use "Extract Function" as the primary tool for Long Functions.

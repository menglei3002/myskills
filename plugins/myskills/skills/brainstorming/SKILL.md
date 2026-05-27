---
name: brainstorming
description: Design review before implementation. MUST use before ANY creative or implementation task — new features, refactors, architectural changes, building components, modifying behavior. Explore context, propose 2-3 approaches with recommendation, present design for user approval section by section, write design doc, then implement. Even a 30-second design check prevents expensive rework. SKIP only for trivial fixes: typos, formatting, one-line config changes.
---

# Brainstorming

## Why brainstorm before coding?

Jumping into code without design review produces:
- Wrong architecture needing expensive refactoring
- Missed edge cases caught only after implementation
- Solutions misaligned with user intent
- Time wasted on approaches the user would have rejected upfront

A brief design check catches these before any code is written. The cost is seconds to minutes; the savings can be hours.

## When to trigger

**Always brainstorm for:**
- New features or functionality
- Refactoring existing code
- Architectural decisions
- Building components, pages, or UIs
- Bug fixes requiring design thinking (not simple typo/one-liner fixes)
- Any change affecting behavior or structure

**Skip for:**
- Typos, formatting, whitespace
- Changing a single configuration value
- Tasks with extremely detailed, prescriptive instructions from the user

When unsure, brainstorm. The cost of unnecessary design is near zero; the cost of a wrong implementation is high. Default to doing it.

## Process

### 1. Explore context

Read relevant existing code, patterns, and constraints before proposing anything. Note files that would be affected.

### 2. Propose approaches

Present 2-3 distinct options. For each:

- How it works (concise)
- Tradeoffs (pros/cons)
- Your recommendation and why

Keep proposals tight — the user needs clarity, not a novel.

### 3. Present design section by section

Walk through the recommended approach:

- Architecture / component tree
- Data flow / state management
- Key interfaces or API shapes
- Edge cases addressed

Pause after each section for confirmation. Use AskUserQuestion for decisions with multiple valid choices.

### 4. Write design document

Save to `docs/superpowers/specs/<feature-name>.md`:

```markdown
# [Feature Name]
## Status: Draft | Approved | Implemented
## Context — why this is needed
## Design — architecture, data flow, key decisions
## Alternatives considered — rejected approaches and why
## Implementation plan — ordered, actionable steps
```

### 5. Implement after approval

Only start coding after the user explicitly approves the design document. Follow the plan in order. If implementation reveals a need to change the plan, return to step 2 for the affected portion only.

## Complexity scaling

| Level | Effort | Examples |
|-------|--------|----------|
| Trivial | Skip the process | Fix typo, change one config value |
| Simple | 30-second verbal summary | Add a log line, rename a variable |
| Medium | Full process, brief doc | Add validation, refactor a component |
| Complex | Full process, detailed doc | New auth system, DB migration, new feature |

Err toward the higher side. What feels "Medium" in your head is often "Complex" in the code.

## Anti-patterns

- "This is obvious, I'll just do it" — The ones that feel obvious hide the most complexity.
- "Let me explore first, then design" — Design IS exploration. They happen together.
- Skipping user confirmation because "they'll like it" — They might not. Ask.
- Writing code during design — Code comes after approval. No exceptions.

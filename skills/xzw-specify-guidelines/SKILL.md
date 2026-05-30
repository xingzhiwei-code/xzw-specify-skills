---
name: xzw-specify-guidelines
description: Behavioral guidelines to reduce common LLM coding mistakes. Use when writing, reviewing, or refactoring code to avoid overcomplication, make surgical changes, surface assumptions, and define verifiable success criteria.
license: MIT
---

# Coding Guidelines

Behavioral guidelines to reduce common LLM coding mistakes. Apply these when writing, reviewing, or refactoring code — across any language or project.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks (one-liner fixes, obvious patterns), skip ceremony. For anything complex, follow them.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.
- Don't assume external API or library behavior — check the docs or verify first.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- Handle errors where they can actually occur — skip defensive code for "what if" scenarios.
- If you write significantly more code than seems necessary, stop and ask whether a simpler approach exists.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.
- Don't break existing tests. If a test fails after your change, figure out which is wrong.
- Format changes from automated tools (prettier, black, rustfmt, etc.) are acceptable — don't fight the linter.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

If success criteria can be defined, execute independently. If not, ask first.

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

If you've tried 2-3 approaches and none work, stop. Explain what you've tried and ask for guidance.

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Verify With Tests

**Code without tests is a hypothesis, not a solution.**

- For bug fixes: write a failing test first, then fix.
- For new features: write tests that define the contract before implementation.
- If no test framework exists, verify manually but document the verification steps.
- Don't claim "fixed" or "done" without evidence.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, clarifying questions come before implementation rather than after mistakes, and commits stay focused on the user's request.

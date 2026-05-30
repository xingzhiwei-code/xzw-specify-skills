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

<!-- ============================================================ -->
<!-- 中文参考版本（仅供人类阅读，不影响 LLM 执行的英文正文）       -->
<!-- ============================================================ -->

## 中文参考版

本准则旨在减少 LLM 编码时的常见错误。适用于任何语言和项目的编码、评审和重构场景。

**取舍：** 这些准则偏向谨慎而非速度。对于简单任务（单行修复、显而易见的模式），可以跳过仪式；对于复杂任务，请严格遵循。

### 1. 先思考，再编码

**不要假设。不要掩饰困惑。主动暴露权衡。**

实现前：
- 明确陈述你的假设。不确定就问。
- 如果有多种理解方式，都列出来——不要自行拍板。
- 如果有更简单的方案，直接说。必要时提出反对。
- 如果不清楚，停下来。说清哪里困惑。问。
- 不要假设外部 API 或库的行为——先查文档或验证。

### 2. 简单优先

**用最少的代码解决问题。不写推测性的东西。**

- 不做超出需求范围的功能。
- 不为单次使用的代码写抽象。
- 不添加未被要求的"灵活性"或"可配置性"。
- 在真实会出错的地方处理错误——跳过"万一呢"的防御性代码。
- 如果写的代码远超预期，停下来想想是否有更简单的方案。

问自己："资深工程师会觉得这过度复杂吗？"如果是，简化。

### 3. 手术式变更

**只动必须动的。只清理自己弄乱的。**

编辑已有代码时：
- 不要"优化"相邻的代码、注释或格式。
- 不要重构没坏的东西。
- 遵循已有代码风格，即使你有不同的偏好。
- 如果看到无关的死代码，提一句——不要删除。

当你的变更产生了残留：
- 清理**你的**变更引入的无用 import/变量/函数。
- 不要删除原本就存在的死代码，除非被要求。
- 不要破坏已有测试。变更后测试失败，搞清楚是变更错还是测试错。
- 格式化工具（prettier、black、rustfmt 等）自动产生的格式变化可以接受——不要跟 linter 对着干。

检验标准：每一行变更都应该能追溯到用户的请求。

### 4. 目标驱动执行

**定义成功标准。循环直到验证。**

如果能定义成功标准，独立执行。如果不能，先问。

将任务转化为可验证的目标：
- "加校验" → "先写非法输入的测试，再让它通过"
- "修 bug" → "先写复现测试，再修复"
- "重构 X" → "确保前后测试都通过"

多步任务时，先简述计划：
```
1. [步骤] → 验证: [检查点]
2. [步骤] → 验证: [检查点]
3. [步骤] → 验证: [检查点]
```

如果尝试了 2-3 种方案都不行，停下来。说明你试了什么，请求指导。

明确的成功标准让你能独立推进；模糊的标准（"让它能用"）需要反复澄清。

### 5. 用测试验证

**没有测试的代码只是假说，不是方案。**

- 修 bug：先写一个失败的测试，再修复。
- 新功能：先写定义契约的测试，再实现。
- 如果没有测试框架，手动验证但记录验证步骤。
- 没有证据，不要声称"修好了"或"完成了"。

---

**这些准则生效的标志：** diff 中不必要的变更变少、因过度复杂导致的重写变少、澄清问题出现在实现之前而非犯错之后、commit 始终聚焦于用户的请求。

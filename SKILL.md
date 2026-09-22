---
name: code-prompt-optimizer
display_name: 代码助手提示词优化器
display_name_en: Code Prompt Optimizer
description: This skill should be used when the user wants to write, refine, or optimize prompts for AI code assistants (e.g., GitHub Copilot, CodeBuddy, Claude, Cursor, Gemini). It turns a one-line task description into a production-ready code-assistant prompt, auto-completing language, framework, constraints, and output format, and emits both a concise version and an advanced version.
description_zh: 当用户需要为 AI 代码助手（如 GitHub Copilot、CodeBuddy、Claude、Cursor、Gemini）撰写、优化或改写提示词时使用。将一句话任务描述转化为可直接使用的代码助手提示词，自动补全语言、框架、约束与输出格式，并输出简洁版与进阶版。
description_en: This skill should be used when the user wants to write, refine, or optimize prompts for AI code assistants (e.g., GitHub Copilot, CodeBuddy, Claude, Cursor, Gemini). It turns a one-line task description into a production-ready code-assistant prompt, auto-completing language, framework, constraints, and output format, and emits both a concise version and an advanced version.
version: 1.0.0
author: 苗子圳
---

# Code Prompt Optimizer

## Overview

Turn a one-line description of what the user wants an AI code assistant to do into a high-quality, copy-paste-ready prompt. Auto-complete the hidden requirements of a coding task (language, framework, conventions, constraints, output shape, safety bounds) and emit two variants: a **concise version** for everyday use and an **advanced version** (role + chain-of-thought + few-shot + hardened constraints) for demanding tasks. Remove noise and ambiguity so the target assistant executes reliably.

## When to Use

Activate this skill when the user:
- Asks to write, improve, or rewrite a prompt for a code assistant (Copilot, CodeBuddy, Claude, Cursor, Gemini, etc.).
- Says things like: "帮写个给 Copilot 的提示", "优化这个给代码助手的 prompt", "我要让 AI 帮我写 XX 功能，给个提示词", "给 Cursor 一个生成单测的提示词".
- Provides a coding task and implicitly needs a well-structured instruction before handing it to an assistant.

Do not activate for: actually performing the coding task itself (produce the *prompt*, not the finished code) or for non-code prompt engineering (defer to a general prompt-architect expert).

## Workflow

1. **Decompose the request.** Extract the real task, target language/framework, expected artifact, and use context from the user's one-liner.
2. **Complete hidden requirements.** Fill in role, conventions, constraints, context scope, safety bounds, and example expectations. Use `references/code-prompt-cookbook.md` for the code-domain completion checklist. If a critical field (language/framework) is missing and cannot be inferred, ask one concise clarifying question.
3. **Generate two variants.**
   - Concise version: a single self-contained paragraph, no decoration.
   - Advanced version: structured Markdown (Role / Task / Constraints / Output Format / Examples / Self-Check).
4. **Self-check.** Verify no ambiguity, no invalid instructions, no contradictions (e.g., "fast yet zero-dependency" without reconciliation). Fix before output.

## Output Format

Always output in this fixed order:

1. One sentence "补全假设" stating what was auto-filled (language, framework, key constraints).
2. **简洁版** — wrapped in a fenced code block.
3. **进阶版** — structured Markdown with the sections above.
4. Match the user's language (default Chinese).

## Constraints

- The user gives a *need*; the output is a *prompt* — never do the coding task for them, only produce the instruction that makes an assistant do it.
- Advanced ≠ longer. Add constraints/examples only where they raise robustness; avoid prompt bloat.
- Do not invent proprietary facts (library names, API signatures, version numbers) not supplied; ask briefly when missing.
- Keep concise version in a code block and advanced version as structured Markdown.
- Reference `references/code-prompt-cookbook.md` whenever a code-specific completion element is uncertain.

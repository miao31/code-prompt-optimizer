# Code Prompt Cookbook

Domain checklist used to auto-complete coding-task prompts. Apply during Workflow step 2 of `SKILL.md`.

## Completion checklist (fill what the user omitted)

- **Language / framework**: Python, TypeScript, Go, Rust, Java, C++, etc.; framework (React, Django, Spring, FastAPI, Vue...).
- **Conventions**: style/lint rules (PEP 8, ESLint, gofmt), naming, architecture pattern (MVC, hexagonal), test framework (pytest, Jest, JUnit, cargo-test).
- **Constraints**: forbidden dependencies, performance/memory budget, compatibility (browser/Node version, Python version), must be dependency-free if stated.
- **Context scope**: which files/functions are in play; whether to read existing code first.
- **Safety bounds**: never execute destructive commands, never expose secrets, sandbox network access, ask before writing outside the repo.
- **Output shape**: full file / diff / function only / explanation + code; include or omit comments; include tests or not.
- **Examples**: provide a 1–3 line input/output sample when the task is non-trivial.

## Few-shot patterns (for the advanced version)

### Generate a feature
Role: Senior <language> engineer. Task: implement <X>. Constraints: <conventions>, <forbidden deps>. Output: complete <file/diff> with tests. Example: given <input>, produce <output>.

### Refactor
Role: refactoring specialist. Task: improve <file> for <readability/perf> without changing behavior. Constraints: keep public API. Output: diff + rationale.

### Write tests
Role: test engineer. Task: unit-test <target> using <framework>. Constraints: cover edge cases, no network. Output: test file + how to run.

### Debug
Role: debugging expert. Task: find root cause of <symptom> in <snippet>. Output: diagnosis + minimal fix + verification step.

### Code review
Role: reviewer. Task: review <diff> for bugs, security, style. Output: prioritized findings + suggestions.

## Reconciliation rules

- "Fast yet zero-dependency": prefer std-lib; state the trade-off.
- Conflicting conventions: pick the stricter one and note it.
- Missing language: ask once; never guess a language silently.

# code-prompt-optimizer

> 把「我要让代码助手做 X」这类模糊需求，自动转化为高质量、可直接粘贴给 AI 代码助手的提示词。自动补全隐藏需求，并同时输出「简洁版」与「进阶版」双提示词。
>
> Turn vague "make the coding assistant do X" requests into high-quality, copy-paste-ready prompts for AI coding assistants — with hidden requirements auto-filled and both concise & advanced versions.

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#贡献指南)
[![Compatible: Any Agent](https://img.shields.io/badge/compatible-any%20agent-0b7285.svg)]()
[![Type: Skill](https://img.shields.io/badge/type-Skill-9b59b6.svg)](SKILL.md)
![Updated](https://img.shields.io/badge/updated-2026--09--22-orange.svg)

![code-prompt-optimizer 效果示意](preview.png)

## 目录

- [项目简介](#项目简介)
- [功能特性](#功能特性)
- [安装](#安装)
- [快速开始](#快速开始)
- [使用示例](#使用示例)
- [工作流程](#工作流程)
- [目录结构](#目录结构)
- [贡献指南](#贡献指南)
- [许可证](#许可证)

## 项目简介

写代码时，我们常对 AI 代码助手（Copilot / CodeBuddy / Claude / Cursor / Gemini…）丢一句模糊需求，例如「帮我写一个函数，把数组排序」。这类请求缺省了大量隐藏信息——语言、框架、规范、约束、边界、输出形态——导致输出质量不稳、需要反复返工。

**code-prompt-optimizer** 是一个与厂商无关的提示词优化 Skill，遵循通用的 Skill 调用规范，可被任何兼容该规范的智能体加载使用。它专门补上这些缺口：它先把你没说清的隐藏需求推断出来，再产出一份**即贴即用**的提示词，并同时给出「简洁版」（日常快用）与「进阶版」（结构化、含角色/约束/输入输出/验收，适合复杂任务），让你对 AI 代码助手的每一次提问都更精准、更高效。

## 功能特性

- **隐藏需求自动补全**：语言/框架、编码规范、约束、上下文范围、安全边界、输出形态、示例，一次性补齐。
- **双版本输出**：简洁版（短平快）+ 进阶版（角色 + 思维链 + 少样本 + 约束强化），按场景取用。
- **去噪消歧**：删除无效指令、化解前后矛盾，保证目标助手稳定执行。
- **全场景覆盖**：生成 / 重构 / 测试 / 调试 / 审查，同一套工作流通吃。
- **零配置、跨平台**：纯指令型 Skill，无脚本、无第三方依赖、无密钥，Windows / macOS / Linux 通用。

## 安装

> 前置条件：你使用的智能体支持加载本地 Skill（遵循其约定的技能目录与 Skill 调用规范）。

```bash
git clone <repo-url> code-prompt-optimizer
# 将 code-prompt-optimizer/ 整个目录放入你的智能体所约定的「技能目录」
# 目录名需符合该智能体的 Skill 调用规范（通常即为 code-prompt-optimizer）
# 不同智能体的技能目录路径不同，请以其官方文档为准
```

重启你的智能体客户端（或刷新技能列表）后，即可在对话中通过 `/code-prompt-optimizer` 或自然语言（如「帮我优化这段给代码助手的提示」）触发。

## 快速开始

把你的模糊需求丢给本 Skill，它会自动补全并返回双版本提示词。例如：

**输入（你的原始需求）**

```
帮我写一个函数，把数组排序
```

**输出（Skill 生成）**

- **简洁版**：`你是一名资深 JavaScript 工程师。请用 JavaScript 实现稳定升序排序函数 sortStable(arr)：不修改原数组、返回新数组；空数组返回 []；元素为同类型数字或字符串；时间复杂度 O(n log n)；附 2 个使用示例与边界测试。`
- **进阶版**（节选）：`# 角色 / # 任务 / # 约束 / # 输入·输出 / # 交付 / # 验收` 六段式结构化提示词。

> 更完整的两个交互示例（生成类 / 调试类）见下方[使用示例](#使用示例)。

## 使用示例

想直接看效果，打开仓库内的交互式演示 **[example.html](example.html)**（纯前端、零依赖，浏览器直接打开）：输入一段模糊请求，Skill 先补全隐藏需求，再给出「简洁版」与「进阶版」双提示词，可点击切换。

**示例一 · 生成类（排序函数）**

```text
原始请求：帮我写一个函数，把数组排序
补全假设：语言未指定 / 顺序升降未定 / 是否稳定 / 原地还是新数组 / 空值与重复 / 性能要求
→ 简洁版 + 进阶版（见 example.html「示例 1」）
```

**示例二 · 调试类（报错排查）**

```text
原始请求：我的代码报错，帮我看看
补全假设：报错栈信息 / 最小复现代码 / 期望结果 / 语言版本 / 已尝试动作
→ 简洁版 + 进阶版（见 example.html「示例 2」）
```

## 工作流程

本 Skill 的内部处理遵循四步法，确保产出严谨、可用：

1. **拆解** —— 解析你的目标与已给信息，识别缺失维度。
2. **补全** —— 基于代码场景补全隐藏假设（语言/框架/规范/约束/边界/输出形态/示例）。
3. **双版本** —— 生成简洁版（快用）与进阶版（结构化强约束）。
4. **自检** —— 去噪、消歧、校验矛盾，保证目标助手可稳定执行。

> 补全要素清单与少样本模板见 [`references/code-prompt-cookbook.md`](references/code-prompt-cookbook.md)。

## 目录结构

```
code-prompt-optimizer/
├── SKILL.md                       # 入口：元数据 + 核心流程
├── references/
│   └── code-prompt-cookbook.md    # 代码域补全要素与少样本模板
├── example.html                   # 交互式效果示例（浏览器打开）
├── preview.png                    # README 配图（效果示意）
├── README.md
└── LICENSE
```

## 贡献指南

欢迎 Issue 与 Pull Request！

1. Fork 本仓库并创建特性分支：`git checkout -b feature/your-idea`
2. 提交改动：`git commit -m "feat: 你的改动说明"`
3. 推送到分支：`git push origin feature/your-idea`
4. 发起 Pull Request，并简述改动动机。

提交前请确保：`SKILL.md` 与 `references/` 的改动保持纯指令、无硬编码密钥、跨平台可用。本仓库遵循 [Contributor Covenant](https://www.contributor-covenant.org/) 行为准则。

## 许可证

[MIT](LICENSE) © 苗子圳

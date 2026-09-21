# code-prompt-optimizer

一个 WorkBuddy Skill：把「我要让代码助手做 X」这类需求，自动转化为高质量、可直接粘贴给 AI 代码助手（Copilot / CodeBuddy / Claude / Cursor / Gemini…）的提示词。

![code-prompt-optimizer 效果示意](preview.png)

## 它能做什么

- 自动补全代码场景的隐藏需求：语言/框架、编码规范、约束、上下文范围、安全边界、输出形态、示例。
- 同时输出 **简洁版**（日常快用）与 **进阶版**（角色 + 思维链 + 少样本 + 约束强化）。
- 去噪消歧：删除无效指令、化解矛盾，保证目标助手稳定执行。

## 效果示例 / Demo

想直接看效果？打开 [example.html](example.html)（纯前端、零依赖，浏览器直接打开）查看交互式演示：输入一段模糊请求，skill 先补全隐藏需求，再给出「简洁版」与「进阶版」双提示词。内置「生成类（排序函数）」与「调试类（报错排查）」两个场景，可点击切换。

## 安装

### 方式一：WorkBuddy 开放平台（推荐，全平台可搜）
1. 访问 [open.workbuddy.cn](https://open.workbuddy.cn)，完成开发者认证。
2. 发布管理 → 技能 → 上传本仓库打包好的 `code-prompt-optimizer.zip`。
3. 审核通过后即在技能市场可见，用户一键安装、开箱即用。

### 方式二：GitHub / 本地
```bash
git clone <repo-url> code-prompt-optimizer
# 复制到用户技能目录（跨平台路径）
# macOS / Linux: ~/.workbuddy/skills/code-prompt-optimizer
# Windows:        %USERPROFILE%\.workbuddy\skills\code-prompt-optimizer
```
重启 WorkBuddy 即可在对话中通过 `/code-prompt-optimizer` 或自然语言触发。

## 跨平台与零配置
- 纯指令型 Skill，无脚本、无第三方依赖、无密钥，天然跨平台（Windows / macOS / Linux）。
- 安装后无需任何额外配置，直接调用。

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

## 许可证
MIT

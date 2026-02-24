---
name: git-commit-message
description: 生成符合 Conventional Commits 的 git commit 提交信息（中文为主），包含 type/scope/subject/body/footer、可选 icon；适配团队规则：subject 动词开头且不超过 20 字、不要句号，scope 优先中文；默认先自动读取当前仓库的 git diff（优先已暂存变更）并自动确认标题，scope 默认“全部范围”，直接输出可直接粘贴的提交信息文本；不执行 git commit、不改代码。
---

# Git Commit Message 生成规范

## 目标

- 只产出可直接粘贴到 Git 提交框的一段文本（必要时多行）。
- 默认遵循 Conventional Commits，并按团队约定输出中文文案。
- 默认先自动读取 git diff（优先 `--staged`），自动确认标题并直接输出最终提交信息文本。
- 最终输出仅包含提交信息文本：不输出解释说明、不输出多余内容、不使用代码块围栏包裹最终输出。

## 默认工作流（先自动看 git diff，再生成/再输出）

1) 自动读取当前目录的 git 变更（只读命令，不执行 `git add/commit/push`）：
   - 确认在 Git 仓库内：`git rev-parse --is-inside-work-tree`
   - 查看变更文件：`git status --porcelain=v1 -uall`
   - 优先读取已暂存变更：`git diff --staged`
   - 若无暂存变更，再读取未暂存变更：`git diff`

2) 基于 diff 推断本次提交的：
   - 推荐 `type`（feat/fix/chore…）
   - 建议 `scope`（优先中文模块名；不确定时默认“全部范围”）
   - 1 个核心 `subject`（动词开头，≤20 字，不带句号）
   - 若多项改动：建议 body 的分行要点（不把多件事塞进 subject）

3) 自动确认并“最终输出”：
   - 直接选择最合适的 1 个标题并输出最终提交信息文本（严格按下方格式与空行规则），且不要附带任何额外话术。
   - 除“信息不足”场景外，不向用户追问确认标题或 scope。

4) 异常/边界：
   - 不在 Git 仓库或无法读取 diff：退回到“信息不足时的提问策略”。
   - 只有未跟踪文件（untracked）且无 diff：根据 `git status` 的文件名推断，必要时提问确认 `type/scope/结果`。

## 输出格式（必须）

标题格式：
<icon><type>(<scope>): <subject>
（最终输出时不要包含反引号）

可选段落（按需出现，严格控制空行）：
- 只有标题：仅 1 行。
- 有 body：标题行后空 1 行，再输出 body（可多行）。
- 有 footer：在 body 结束后空 1 行，再输出 footer（可多行）。
- 不要在末尾追加空行。

## 规则

### icon

- `<icon>` 可选；若无 icon，必须直接以 `<type>` 开头，且前面不得有空格。
- icon 仅作视觉增强，不改变语义，不得重复 type。
  - ✅ 正确：`✨ feat(登录): 支持短信登录`
  - ❌ 错误：`✨ feat: feat(登录): 支持短信登录`
- 使用规则：
  - icon 放在最前面。
  - icon 后必须紧跟 1 个空格，再写 type。
  - 不得在 icon 后单独写冒号。

### scope

- `scope` 可选；但默认应写为“全部范围”（当无法可靠推断模块或改动跨多个模块时）。
- `scope` 优先使用中文模块名（如：登录/版本/界面/依赖）。

### subject（强约束）

- 中文简述、动词开头。
- 描述“做了什么结果”，不要写过程实现。
- 不超过 20 个汉字。
- 结尾不要句号。
- 不要在 subject 中用“和 / 及 / 并 / 以及”等连接多个动作。

### body / footer

- `body` 用于补充多项改动/背景/影响面；建议使用自然分行或 `- ` 列表形式表达要点。
- `footer` 用于关联信息（issue/需求号、关闭语句）或破坏性变更说明。
- 只在需要时输出；不要输出占位符（例如不要输出字面量 `<body>`、`<footer>`）。

## type 列表（必选其一）

`init` `feat` `fix` `docs` `style` `refactor` `perf` `test` `build` `ci` `chore` `revert`

## icon（可选，对应 type）

`🎉 init` `✨ feat` `🐞 fix` `📃 docs` `🌈 style` `🦄 refactor` `🎈 perf` `🧪 test` `🔧 build` `🐎 ci` `🐳 chore` `↩ revert`

## 多改动规则（必须遵守）

- subject 只概括一个核心结果。
- 若包含多个相关改动，在 body 中分行说明。
- 不要把无关改动合并为一个提交说明。

## 破坏性变更

若存在不兼容改动，必须在 footer 增加：
`BREAKING CHANGE: <迁移说明>`

## 关联信息

若用户提供 issue / 需求号，放入 footer，例如：
- `#123`
- `Closes #45`
不得自行虚构 issue 号/需求号。

## revert 特殊规则

- `revert` 类型的 subject 必须以“回滚”开头。
- 示例：`↩ revert(登录): 回滚短信登录逻辑`

## 信息不足时的提问策略

当无法从 git diff 得到足够信息，无法可靠生成标题/范围时，最多提问 1–3 个问题，优先顺序：
1) 本次提交属于哪个 type（feat/fix/chore…）？
2) scope 是什么模块（中文名；不确定可回答“全部范围”）？
3) 想强调的结果是什么（动词开头，≤20 字）？

用户仅说“给我 commit message/提交信息”，优先先自动读取 git diff；读取不到再提问，不得自行猜测。

## 示例（仅用于说明；最终输出不要用代码块围栏包裹）

单行：
`✨ feat(版本): 展示构建版本号`

带 body / footer：
🐞 fix(登录): 修复按钮禁用状态

避免重复提交导致卡死

#123

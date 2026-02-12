---
name: git-commit-message
description: 生成符合 Conventional Commits 的 git commit 提交信息（中文为主），包含 type/scope/subject/body/footer、可选 icon；适配团队规则：subject 动词开头且不超过 20 字、不要句号，scope 优先中文。用于用户请求“给我 commit message/提交信息/提交说明/Conventional Commits 文案/提交标题怎么写”等场景；仅输出提交信息文本，不执行 git commit、不改代码。
---

# Git Commit Message 生成规范

## 目标

- 只产出可直接粘贴到 Git 提交框的一段文本（必要时多行）。
- 默认遵循 Conventional Commits，并按团队约定输出中文文案。
- 仅输出提交信息文本：不输出解释说明、不输出多余内容、不使用代码块围栏包裹最终输出。

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

- `scope` 可选；不确定可省略括号部分。
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

当用户未提供足够信息时，最多提问 1–3 个问题，优先顺序：
1) 本次提交属于哪个 type（feat/fix/chore…）？
2) scope 是什么模块（中文名，不确定可不填）？
3) 想强调的结果是什么（动词开头，≤20 字）？

用户仅说“给我 commit message/提交信息”，必须先提问，不得自行猜测。

## 示例（仅用于说明；最终输出不要用代码块围栏包裹）

单行：
`✨ feat(版本): 展示构建版本号`

带 body / footer：
🐞 fix(登录): 修复按钮禁用状态

避免重复提交导致卡死

#123

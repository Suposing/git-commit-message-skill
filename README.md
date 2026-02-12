# git-commit-message-skill

一个用于 **生成符合 Conventional Commits 的中文 Git 提交信息** 的 Codex Skill：输出 `type/scope/subject/body/footer`（可选 icon），**只生成提交信息文本，不会执行 `git commit`**。

## 功能

- 生成 Conventional Commits 提交信息（中文为主）
- 支持可选 icon（emoji）+ type 对应关系
- 约束规则：`subject` 动词开头、≤20 字、不带句号；`scope` 优先中文
- 信息不足时最多提问 1–3 个关键问题
- 不虚构 issue/需求号

规则细节见：`SKILL.md`

## 安装（全局）

把本仓库克隆到你的 Codex skills 目录即可（Windows 默认一般在 `C:\Users\<你>\.codex\skills`）：

```powershell
git clone https://github.com/Suposing/git-commit-message-skill.git `
  "$env:USERPROFILE\.codex\skills\git-commit-message"
```

如果你已经有 `git-commit-message` 文件夹，用 `git pull` 更新即可。

## 使用

在 Codex 对话中直接说：

- `给我 commit message：我修复了登录页按钮重复点击导致重复请求的问题`

或更结构化一点：

- `type: fix，scope: 登录，结果：修复重复提交导致卡死，关联：#123，给我提交信息`

Codex 会输出一段可直接粘贴的提交信息文本。

## 文件结构

```
git-commit-message/
├─ SKILL.md
└─ agents/
   └─ openai.yaml
```


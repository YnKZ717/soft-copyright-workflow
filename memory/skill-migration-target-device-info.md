---
name: skill-migration-target-device-info
description: Yannick-Z 目标设备环境信息和 skill 迁移要求 — 从用户个人电脑 agent 获取的完整配置要求
metadata:
  type: project
---

# Skill 迁移 — 目标设备环境信息

**来源：** 用户从个人电脑（Yannick-Z）上的 Claude Code agent 获取的环境信息，用于指导 skill 迁移工作。

**Why:** 用户需要将公司电脑（DN）上的 skill/memory 迁移到个人电脑（Yannick-Z），此文件记录目标设备的完整环境约束，避免下次迁移时重复沟通。

**How to apply:** 下次用户要求整理/迁移 skill 到个人设备时，先读此文件确认目标环境。

---

## 目标设备基本环境

| 项目 | 值 |
|------|-----|
| 操作系统 | Windows 11 家庭中文版 |
| 用户名/主目录 | `{{target_user_home}}` |
| Claude Code 全局目录 | `{{target_user_home}}\.claude\` |
| 个人 skill 根目录 | `{{target_user_home}}\.claude\skills\` |
| 全局配置 | `{{target_user_home}}\.claude\settings.json` |

## 关键约束

### 1. Python 不可用
- `python.exe` 只是 Microsoft Store 占位符，运行报 exit 49
- skill 里的 Python 脚本（如 `diagram-generator.py`）要么改成 PowerShell 实现，要么用户需要先装 Python
- 装 Python 时需勾选 **"Add Python to PATH"**

### 2. Shell 环境
- PowerShell 5.1 和 Git Bash 可用
- **含中文的 `.ps1` 脚本必须是 UTF-8 with BOM 编码**，否则 PowerShell 5.1 按 GBK 读取会乱码报错

### 3. Skill 结构要求
- 每个 skill 一个子文件夹，文件夹名 = SKILL.md frontmatter 里的 `name`（小写英文+连字符）
- SKILL.md 的 frontmatter 至少包含 `name` 和 `description`（description 里写清触发关键词，方便中文唤起）
- 附属资源（模板、脚本、参考文档）放在该 skill 文件夹内或 `references/` 子目录，用相对路径引用
- Claude Code 自动发现 skill，无需在配置文件里注册，新会话生效

### 4. 权限配置
- 当前 settings.json：`model: opus`，权限已设为 `defaultMode: bypassPermissions` + 全量 allow 白名单
- skill 需要的任何命令执行、文件读写都已免确认，不需要再为 skill 单独配权限
- skill 本体不需要注册进 settings.json

### 5. 本机已有 skill（避免重名/重复）
`cleanup-temp`、`auto-execute`、`learn-term`、`write-copyright-app`、`write-design-doc`、`write-source-code`、`write-user-manual`

---

## 迁移工作流（已验证可用）

```
公司电脑（DN）                          个人电脑（Yannick-Z）
─────────────                          ─────────────────────
1. 读取 .claude/skills/ 和 .claude/memory/
2. 将所有 SKILL.md 中的
   {{memory_dir}}\
   替换为相对路径 .claude/memory/
3. 打包成 deploy-package/
   ├── skills/（每个 skill 一个文件夹）
   ├── memory/（所有 .md 记忆文件）
   ├── CLAUDE.md（路径改成 Yannick-Z）
   ├── settings.json.template
   └── install-skills.ps1（UTF-8 BOM）
                                      4. 拷贝 deploy-package 过去
                                      5. 双击 install-skills.ps1
                                      6. 自动部署到 .claude/skills/ 和 .claude/memory/
```

## 路径替换规则

| 公司电脑路径 | 个人电脑路径 |
|-------------|-------------|
| `{{memory_dir}}\` | `{{memory_dir}}\` |
| `{{user_home}}\.claude\skills\` | `{{target_user_home}}\.claude\skills\` |
| `{{default_output_dir}}\` | 保持不变（两边都有这个目录） |
| `{{default_output_dir}}/apprendre\` | 保持不变 |
| `{{template_dir}}\` | 保持不变（需要单独拷贝模板文件） |

**注意：** skill 内部的 memory 引用路径应使用**相对路径** `.claude/memory/xxx.md`，而不是绝对路径，这样两边都能用。

## 模板文件

`{{template_dir}}\` 目录（含申请表模板、设计说明书模板等 .docx 文件）**不是 skill 的一部分**，需要单独拷贝。

---

## 关联记忆

- AIGC项目通用要求：[[aigc-project-requirements]]
- AIGC评测系统项目：[[aigc-eval-system-project]]
- 软著材料工作流：[[copyright-material-workflow]]

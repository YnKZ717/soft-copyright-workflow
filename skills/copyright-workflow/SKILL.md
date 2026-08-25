---
name: copyright-workflow
description: "软著文档全流程自动生成 — 从信息收集到打包输出，串联全部8个环节。关键词：软著全流程、一键生成、跑完整流程、/run-copyright-workflow"
---

# 软著文档全流程控制技能

串联软著申请的完整工作流，从信息收集到最终打包输出。

## 配置读取

**执行任何步骤前，必须先读取配置文件：**

1. 检查 `{{user_home}}\.claude\config.md` 是否存在
2. 如果不存在，复制 `config.example.md` 为 `config.md`，让用户填写实际路径和值
3. 使用 `config.md` 中的值替换后续步骤中的 `{{...}}` 变量

**配置变量说明：**
- `{{reference_dirs[0]}}` — 参考资料目录 1
- `{{reference_dirs[1]}}` — 参考资料目录 2
- `{{template_dir}}` — 软著材料模板目录
- `{{default_output_dir}}` — 默认输出目录
- `{{memory_dir}}` — 记忆文件目录（通常为 `.claude/memory`）
- `{{user_home}}` — 当前用户主目录
- `{{target_user_home}}` — 目标用户主目录（迁移时用）

## 触发条件

- 用户说"跑完整流程"、"从头到尾生成一套软著材料"、"一键生成软著"
- 用户通过 `/run-copyright-workflow` 命令触发

**⚠️ 本 skill 只在用户明确要求"全流程"时触发。如果用户只想生成某一个文档，请引导使用对应的单步 skill。**

## 工作流总览

```
启动 → 读取配置 → 确认输出路径 → 选择文档数量
    ↓
Step 1: collect-info（收集信息）
    ↓
Step 2: write-copyright-app（生成申请表）        ← 可选
    → cmmi3-review（审查）
    ↓
Step 3: write-design-doc（生成设计说明书）       ← 可选
    → cmmi3-review（审查）
    ↓
Step 4: draw-illustrations（绘制插图）           ← 可选
    → cmmi3-review（审查）
    ↓
Step 5: write-source-code（生成源代码文档）      ← 可选
    → cmmi3-review（审查）
    ↓
Step 6: write-user-manual（生成使用说明书）      ← 可选
    → cmmi3-review（审查）
    ↓
Step 7: check-consistency（检查一致性）
    ↓
Step 8: package-materials（打包输出）
```

## 启动流程

**不扫描任何文件夹，不遍历任何目录。**

1. **读取配置文件**（`config.md`），加载路径和公司信息
2. **让用户明确指定文档输出路径**（必须，不能自动选择）
3. **自动创建目录结构**（在输出路径下）：
   ```
   {输出路径}\{软件名称}\
   ├── result\            ← 软著文档
   ├── .tmp\              ← 临时脚本
   ├── printscreens\      ← 使用说明书截图
   └── illustrations\     ← 设计说明书插图
   ```
4. **询问生成几份文档**（使用 `AskUserQuestion`）：
   ```
   question: "请选择需要生成的软著文档："
   options:
     1. 全套4份（申请表 + 设计说明书 + 源代码文档 + 使用说明书）
     2. 无使用说明书（申请表 + 设计说明书 + 源代码文档，适合纯后台/API系统）
     3. 自定义组合 → 用户自己告诉 agent
     4. AI 自主决策 → 根据技术选型自动判断（有前端→全套4份，无前端→3份）
   ```
   - 选项 4 的判断逻辑：如果 Step 1 中"前端技术"选的是"无前端"或"纯后端" → 生成 3 份，否则 → 全套 4 份
5. **进入 Step 1-8**，只执行用户选择的步骤

## 执行流程

### Step 1：收集信息
调用 `collect-info` skill。
- 按 CMMI3 + 软件工程范式深度问答
- 兜底 1：用户跳过 → AI 代决策 + 异色标注
- 兜底 2：问答结束后提供参考路径（可跳过）
- 收集过程中使用 `AskUserQuestion` 提供选择题，用户点选后自动继续
- **`ai_decisions` 列表必须在后续步骤中传递**

### Step 2-6：生成文档
根据用户选择的文档数量，**只执行对应的步骤**：
- 每个文档生成后，**自动调用 `cmmi3-review`** 审查
- 审查不通过 → 自动修正后重新提交审查
- 审查通过 → 自动进入下一步（不暂停等待用户准许）
- **所有文档保存到 `result\` 目录**
- **AI 代决策内容必须用异色字体标注**（从 `ai_decisions` 读取风险等级）

### Step 7：一致性检查
调用 `check-consistency` skill。
- 检查所选文档之间的名称、日期、功能是否逐字对应
- 生成检查报告

### Step 8：打包输出
调用 `package-materials` skill。
- 整理所有文档到 `result\` 目录
- 检查页数、格式是否达标
- 输出最终交付清单

## 目录结构规范

所有输出文件必须按以下结构组织：

```
{用户指定的输出路径}\{软件名称}\
├── result\
│   ├── {软件名称}V{版本号}-著作权申请表.docx
│   ├── {软件名称}V{版本号}-设计说明书.docx
│   ├── {软件名称}V{版本号}-源代码文档.docx
│   └── {软件名称}V{版本号}-使用说明书.docx（如选择生成）
├── .tmp\
│   └── （临时 Python 脚本，生成后自动删除）
├── printscreens\
│   └── （使用说明书的截图文件）
└── illustrations\
    ├── fig_3_1_er.png
    ├── fig_4_1_module_rel.png
    ├── fig_4_2_func_struct.png
    ├── fig_4_3_logic_arch.png
    ── ...（设计说明书的插图）
```

## 异色字体标注规范

AI 代决策的内容必须在 Word 文档中用异色字体标注，让用户知道哪些内容是 AI 自动决定的：

| 风险等级 | 颜色 | RGB 值 | 适用场景 |
|----------|------|--------|----------|
| 低风险 | 蓝色 | `RGB(0x2E, 0x86, 0xC1)` | 通用配置、行业标准做法 |
| 中风险 | 橙色 | `RGB(0xE6, 0x95, 0x2A)` | 有多种合理选择，AI 选了其中一种 |
| 高风险 | 红色 | `RGB(0xC0, 0x39, 0x2B)` | 可能不符合用户实际需求，强烈建议审查 |

**python-docx 实现示例：**
```python
from docx.shared import RGBColor

# 蓝色（低风险）
run.font.color.rgb = RGBColor(0x2E, 0x86, 0xC1)

# 橙色（中风险）
run.font.color.rgb = RGBColor(0xE6, 0x95, 0x2A)

# 红色（高风险）
run.font.color.rgb = RGBColor(0xC0, 0x39, 0x2B)
```

## 信息复用

全流程运行时，Step 1 收集的信息会在后续所有步骤中复用。
如果用户之前已经跑过全流程（信息已收集），再次触发时：
- 询问用户是否复用已有信息
- 用户可选择"复用"、"部分修改"或"重新收集"

## 交互规范

### 必须使用 AskUserQuestion
- 所有需要用户决策的节点必须使用 `AskUserQuestion`
- 每个选择题必须包含：
  1. **预设选项** — 2-4 个合理方案
  2. **跳过** — AI 自动决策（兜底 1）
  3. **自定义输入** — 用户自己告诉 agent

### 一轮对话跑完全流程
- 不要中途停下来等用户输入"继续"
- 步骤之间自动衔接
- 只在需要用户决策时才暂停（通过 AskUserQuestion）

## 行为约束

### 必须做到
- ✅ 全流程运行时，每个生成步骤后必须调用 cmmi3-review
- ✅ 必须确认文档输出路径（用户明确指定，不能自动选择）
- ✅ 自动创建 `result\`、`.tmp\`、`printscreens\`、`illustrations\` 目录
- ✅ 使用 AskUserQuestion 提供选择题，用户点选后自动继续
- ✅ 一轮对话跑完全流程，步骤间不暂停
- ✅ 全流程完成后输出最终交付清单
- ✅ AI 代决策内容必须用异色字体标注（蓝/橙/红）
- ✅ `ai_decisions` 信息必须在全流程中传递

### 禁止做到
- ❌ 用户只想生成单个文档时，擅自启动全流程
- ❌ 跳过 cmmi3-review 直接进入下一步
- ❌ 不检查一致性就直接打包
- ❌ 自动选择或猜测文档输出路径
- ❌ 扫描或遍历任何文件夹/目录
- ❌ 每步结束后停下来问"是否继续"
- ❌ AI 代决策内容用黑色字体（必须标色）
- ❌ 不询问用户就固定生成 4 份文档

## 关联 Skill

| Skill | 职责 |
|-------|------|
| `collect-info` | 信息收集 |
| `write-copyright-app` | 生成申请表 |
| `write-design-doc` | 生成设计说明书 |
| `draw-illustrations` | 绘制插图 |
| `write-source-code` | 生成源代码文档 |
| `write-user-manual` | 生成使用说明书 |
| `cmmi3-review` | 质量审查 |
| `check-consistency` | 一致性检查 |
| `package-materials` | 打包输出 |

## 关联记忆

- 申请表规范：[[copyright-application-form-guide]]
- 设计说明书规范：[[design-doc-guide]]
- 源代码规范：[[soft-copy-source-code-guide]]
- 使用说明书规范：[[user-manual-guide]]
- Word格式标准：[[word-writing-guide]]
- 用户身份：[[user-profile-intern]]

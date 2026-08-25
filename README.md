# 软著文档自动生成工作流

> Automated Software Copyright Document Generation Workflow for Claude Code

基于 CMMI3 软件工程规范的软著（软件著作权）文档自动生成系统，集成在 Claude Code 中，通过 Skill 体系实现从信息收集到最终打包的全流程自动化。

## 特性

- **一键全流程**：一轮对话自动生成全部软著文档，步骤间不暂停
- **单步可拆分**：每个文档可独立生成，互不干扰
- **CMMI3 质量保障**：每个生成步骤后自动调用质量审查
- **AI 智能补全**：用户跳过的信息自动决策，并按风险等级异色标注
- **目录自动创建**：自动生成 `result/`、`.tmp/`、`printscreens/`、`illustrations/` 四目录
- **灵活文档选择**：支持全套 4 份 / 无使用说明书 / 自定义 / AI 决策四种模式

## 支持的文档类型

| 文档 | 说明 |
|------|------|
| 著作权申请表 | 25 行×2 列表格，符合版权局要求 |
| 设计说明书 | ≥15000 字，含 E-R 图、架构图、流程图等 |
| 源代码文档 | 前 30 页 + 后 30 页，每页≥50 行 |
| 使用说明书 | 带截图占位、字段说明、操作描述 |

## 快速开始

### 1. 安装

```bash
# 将 skills/ 和 memory/ 复制到你的 .claude/ 目录
cp -r skills/ ~/.claude/
cp -r memory/ ~/.claude/
cp config.example.md ~/.claude/config.md
```

### 2. 配置

编辑 `~/.claude/config.md`，填写实际路径和公司信息：

```yaml
reference_dirs:
  - "/path/to/参考资料"
  - "/path/to/dossier2"
template_dir: "/path/to/软著材料模板"
default_output_dir: "/path/to/软著材料"

company:
  name: "公司名称"
  credit_code: "统一社会信用代码"
  founding_date: "公司成立日期"
  publish_city: "发表城市"
  software_category: "应用软件"
```

### 3. 使用

**全流程：**
```
/run-copyright-workflow
```

**单步：**
```
/write-copyright-application    # 申请表
/write-design-doc               # 设计说明书
/generate-source-code-doc       # 源代码文档
/write-user-manual              # 使用说明书
/draw-illustrations             # 插图
/cmmi3-review                   # 质量审查
/check-consistency              # 一致性检查
/package-materials              # 打包输出
```

## 输出目录结构

```
{输出路径}\{软件名称}\
── result\            # 软著文档
│   ├── XXX-著作权申请表.docx
│   ├── XXX-设计说明书.docx
│   ├── XXX-源代码文档.docx
│   └── XXX-使用说明书.docx
├── .tmp\              # 临时脚本（自动生成后删除）
├── printscreens\      # 使用说明书截图
└── illustrations\     # 设计说明书插图（PNG）
```

## AI 代决策异色标注

用户跳过的信息由 AI 自动决策，并在文档中按风险等级标注：

| 颜色 | 风险等级 | 说明 |
|------|----------|------|
| 🔵 蓝色 | 低风险 | 通用配置、行业标准做法 |
|  橙色 | 中风险 | 有多种合理选择，AI 选了其中一种 |
|  红色 | 高风险 | 可能不符合实际需求，建议审查 |

## Skill 架构

```
copyright-workflow（总控）
├── collect-info（信息收集）
├── write-copyright-app（申请表）→ cmmi3-review
├── write-design-doc（设计说明书）→ cmmi3-review
├── draw-illustrations（插图）→ cmmi3-review
├── write-source-code（源代码）→ cmmi3-review
├── write-user-manual（使用说明书）→ cmmi3-review
├── check-consistency（一致性检查）
└── package-materials（打包输出）
```

## 环境要求

- Claude Code（VS Code 插件 / CLI）
- Python 3.10+（matplotlib、python-docx、Pillow）
- 中文字体：SimHei（Windows 黑体）

## 目录

```
.
├── config.example.md       # 配置模板
├── .gitignore
├── skills/
│   ├── copyright-workflow/ # 全流程总控
│   ├── collect-info/       # 信息收集
│   ├── write-copyright-app/# 申请表生成
│   ├── write-design-doc/   # 设计说明书生成
│   ├── draw-illustrations/ # 插图绘制
│   ├── write-source-code/  # 源代码文档生成
│   ├── write-user-manual/  # 使用说明书生成
│   ├── cmmi3-review/       # CMMI3 质量审查
│   ├── check-consistency/  # 文档一致性检查
│   └── package-materials/  # 打包输出
└── memory/                 # 格式规范和项目记忆
    ├── design-doc-guide.md
    ├── word-writing-guide.md
    ├── soft-copy-source-code-guide.md
    ├── copyright-application-form-guide.md
    ├── user-manual-guide.md
    └── ...
```

## License

MIT

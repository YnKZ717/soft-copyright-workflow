# 软著文档自动生成工作流

> Automated Software Copyright Document Generation Workflow for Claude Code

基于 CMMI3 软件工程规范的软著（软件著作权）文档自动生成系统，集成在 Claude Code 中，通过 Skill 体系实现从信息收集到最终打包的全流程自动化。

## 特性

- **一键全流程**：一轮对话自动生成全部软著文档，步骤间不暂停
- **单步可拆分**：每个文档可独立生成，互不干扰
- **CMMI3 质量保障**：每个生成步骤后自动调用质量审查
- **AI 智能补全**：用户跳过的信息自动决策，并按风险等级异色标注
- **来源追踪**：所有信息记录来源（用户/AI），异色标注精确到段落级
- **循环纠正**：信息收集支持四选项确认（确认/逐条纠正/参考材料/重新开始），重新开始自动切换判断题模式
- **插图自动插入**：插图生成后自动识别文档占位符，用户确认后一键插入
- **目录自动创建**：自动生成 `result/`、`.tmp/`、`printscreens/`、`illustrations/` 四目录
- **只增不删**：所有技能只补建缺失目录，绝不删除用户已有文件
- **专业身份**：每个技能携带行业身份设定（架构师/审计员/审查员等），保证输出专业度
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
/collect-info                   # 信息收集（支持循环纠正）
/write-copyright-application    # 申请表
/write-design-doc               # 设计说明书
/generate-source-code-doc       # 源代码文档
/write-user-manual              # 使用说明书
/draw-illustrations             # 插图（支持自动插入文档）
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

用户跳过的信息由 AI 自动决策，文档中按来源精确标色：

| 标记 | 颜色 | RGB 值 | 含义 |
|------|------|--------|------|
| `//by user` | 黑色 | — | 用户明确提供的内容 |
| `//by ai:low` | 蓝色 | RGB(0x2E, 0x86, 0xC1) | AI 代决策，低风险 |
| `//by ai:medium` | 橙色 | RGB(0xE6, 0x95, 0x2A) | AI 代决策，中风险 |
| `//by ai:high` | 红色 | RGB(0xC0, 0x39, 0x2B) | AI 代决策，高风险 |

**颜色跟信息来源走**：同一模块中，用户指定的功能描述为黑色，AI 独立编写的算法/异常处理为彩色。

## 信息收集循环

`collect-info` 收集完毕后提供四选项：
1. **确认无误** → 继续生成
2. **逐条纠正** → 用户直接对话指定修改哪一条
3. **参考材料** → 用户提供文件路径，AI 从中提取信息覆盖 AI 代决策字段
4. **重新开始** → 切换为判断题模式，保留用户字段、清空 AI 字段，高效重收

## Skill 架构

```
copyright-workflow（总控）
├── collect-info（信息收集，支持循环纠正+来源追踪）
├── write-copyright-app（申请表，标记法异色标注）→ cmmi3-review
├── write-design-doc（设计说明书，标记法异色标注）→ cmmi3-review
├── draw-illustrations（插图，自动识别占位符插入）→ cmmi3-review
├── write-source-code（源代码，标记法异色标注）→ cmmi3-review
├── write-user-manual（使用说明书，标记法异色标注）→ cmmi3-review
├── check-consistency（一致性检查）
└── package-materials（打包输出）
```

## 标记法异色标注机制

每个文档生成技能采用统一的标记法流程：

1. **生成阶段**：AI 在每段末尾加来源标记（`//by user` / `//by ai:low` / `//by ai:medium` / `//by ai:high`）
2. **自检阶段**：AI 重新读取文档，补漏标段落
3. **后置脚本**：Python 脚本识别标记 → 设置颜色 → 删除标记 → 保存文档
4. **严格校验**：发现无标记段落 → 报错拒绝保存 → AI 返工补标

## 身份设定

每个技能携带行业身份，保证输出专业度：

| 技能 | 身份 |
|------|------|
| collect-info | 需求分析师 |
| write-copyright-app | 版权登记专员 |
| write-design-doc | 资深软件架构师 |
| write-source-code | 代码审计员 |
| write-user-manual | 技术文档工程师 |
| draw-illustrations | 技术绘图师 |
| cmmi3-review | CMMI3 审查员 |

通用基底：软著申请材料制作专员，精通 CMMI3 TS 过程域，熟悉版权局审查标准。

## 环境要求

- Claude Code（VS Code 插件 / CLI）
- Python 3.10+（matplotlib、python-docx、Pillow）
- 中文字体：SimHei（Windows 黑体）

## 目录

```
.
├── config.example.md       # 配置模板
├── .gitignore
├── README.md
├── skills/
│   ├── copyright-workflow/ # 全流程总控
│   ├── collect-info/       # 信息收集（来源追踪+循环纠正）
│   ├── write-copyright-app/# 申请表生成（标记法异色标注）
│   ├── write-design-doc/   # 设计说明书生成（标记法异色标注）
│   ├── draw-illustrations/ # 插图绘制（自动插入文档）
│   ├── write-source-code/  # 源代码文档生成（标记法异色标注）
│   ├── write-user-manual/  # 使用说明书生成（标记法异色标注）
│   ├── cmmi3-review/       # CMMI3 质量审查
│   ├── check-consistency/  # 文档一致性检查
│   └── package-materials/  # 打包输出
── memory/                 # 格式规范和项目记忆
    ├── word-writing-guide.md
    ├── design-doc-guide.md
    ├── soft-copy-source-code-guide.md
    ├── copyright-application-form-guide.md
    ├── user-manual-guide.md
    └── ...
```

## License

MIT

---
name: user-manual-guide
description: 软著使用说明书撰写规范 — 标准章节结构、操作描述格式、截图规范、字段说明格式、python-docx代码模板
metadata: type: feedback
---

# 软著使用说明书撰写规范

**来源：** 从 AIGC质量评估与评分系统 使用说明书脚本（2026-07-03）中提炼。

## 文档结构（标准顺序）

```
[封面] 软件全称V1.0（宋体 36pt 加粗 居中）
[封面] 使用说明书（宋体 36pt 加粗 居中）
│
├── 系统简介（宋体 14pt 两端对齐）
│   └── 一段话概括系统定位、架构、适用场景
│
├── 登录（宋体 14pt 加粗）
│   ├── 操作描述 → 截图 → 截图说明 → 字段说明
│
├── 一级功能模块A（宋体 14pt 加粗）
│   ├── 二级功能A-1（宋体 14pt 加粗）
│   │   ├── 操作描述
│   │   ├── [截图占位]
│   │   ├── 图N 截图说明（居中）
│   │   └── 字段说明列表
│   ├── 二级功能A-2
│   └── ...
│
├── 一级功能模块B
│   └── ...
│
└── 系统设置（通常放在最后）
    ├── 用户管理
    ├── 角色权限
    └── 参数配置
```

## 功能模块的标准模块

每个二级功能都必须包含以下四要素（缺一不可）：

| 序号 | 要素 | 格式 | 示例 |
|------|------|------|------|
| 1 | 操作描述 | 宋体 14pt 两端对齐 | 在导航栏点击"XX管理"，然后点击左侧的"XX功能"将显示... |
| 2 | 截图占位 | 居中 `[截图: XXX页面]` | `[截图: 评估任务列表页面]` |
| 3 | 截图说明 | 居中 `图N XXX页面` | `图5 评估任务列表页面` |
| 4 | 字段说明 | 逐行列出，格式 `字段名：字段说明` | `任务名称：评估任务的名称标识。` |

## 操作描述的标准句式

| 场景 | 标准句式 |
|------|----------|
| 导航到某页面 | 在导航栏点击"模块名"，然后点击左侧的"功能名"将显示XX页面。 |
| 执行某操作 | 用户需要填写XX、选择XX，点击"XX"按钮完成XX。 |
| 查看某详情 | 在列表中点击某条记录的"查看详情"按钮，进入详情页面。 |
| 系统默认行为 | 用户登录成功后，系统默认进入XX页面。 |

## 字段说明的标准格式

每行一个字段，格式为 `字段名：字段说明。`

示例：
```
任务名称：填写本次评估任务的名称，便于后续识别和管理。
评估类型：选择评估的内容类型，包括文本检测、图片检测、音视频检测。
优先级：设置任务的紧急程度，包括高、中、低三个等级。
状态：任务的当前状态，包括待处理、进行中、已完成、已暂停。
```

## 典型功能模块划分（通用参考）

| 一级模块 | 典型二级功能 |
|----------|-------------|
| 首页概览 | 数据统计、趋势图表 |
| XX业务管理 | 创建XX、XX列表、XX详情 |
| XX处理/检测 | 文本处理、图片处理、批量处理 |
| 质量评分/审核 | 自动评分、人工评分、评分标准 |
| 数据报表 | 统计分析、趋势分析、报表导出 |
| 系统设置 | 用户管理、角色权限、参数配置 |

## python-docx 代码模板

```python
import sys
sys.stdout.reconfigure(encoding='utf-8')
from docx import Document
from docx.shared import Pt, Emu
from docx.enum.text import WD_ALIGN_PARAGRAPH

doc = Document()

# ===== 页面设置 =====
section = doc.sections[0]
section.top_margin = Emu(647700)
section.bottom_margin = Emu(914400)
section.left_margin = Emu(575945)
section.right_margin = Emu(685800)

# ===== 封面 =====
p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = p.add_run('软件全称V1.0')
run.font.name = '宋体'
run.font.size = Pt(36)
run.bold = True

p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = p.add_run('使用说明书')
run.font.name = '宋体'
run.font.size = Pt(36)
run.bold = True

for _ in range(6):
    doc.add_paragraph()

# ===== 系统简介 =====
p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
run = p.add_run('系统简介内容...')
run.font.name = '宋体'
run.font.size = Pt(14)

# ===== 章节标题（一级功能） =====
p = doc.add_paragraph()
run = p.add_run('模块名称')
run.font.name = '宋体'
run.font.size = Pt(14)
run.bold = True

# ===== 操作描述 =====
p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.JUSTIFY
run = p.add_run('操作描述内容...')
run.font.name = '宋体'
run.font.size = Pt(14)

# ===== 截图占位 =====
p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = p.add_run('[截图: XXX页面]')
run.font.name = '宋体'
run.font.size = Pt(14)

# ===== 截图说明 =====
p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = p.add_run('图1 XXX页面')
run.font.name = '宋体'
run.font.size = Pt(14)

# ===== 字段说明 =====
p = doc.add_paragraph()
run = p.add_run('字段名：字段说明。')
run.font.name = '宋体'
run.font.size = Pt(14)

# ===== 保存 =====
doc.save(r'{{default_output_dir}}\软件全称V1.0_使用说明书.docx')
print('文档已生成')
```

## 注意事项

- 截图编号必须全文连续递增（图1、图2、图3...）
- 截图中的软件名称需与封面全称一致
- 每个截图都必须有对应的文字描述和字段说明
- 操作描述使用统一句式，不要随意变换格式
- 系统设置模块（用户管理、角色权限、参数配置）通常放在文档最后
- 文档中不出现目录
- 脚本生成完毕后必须删除临时 .py 文件

## 关联记忆

- Word格式标准：[[word-writing-guide]]
- 术语文档生成：[[tech-term-doc-generator]]

**Why:** 使用说明书是软著申请的必需材料之一，标准化模板可以大幅提升批量生成效率，同时保证格式一致性。
**How to apply:** 当需要为某个软件系统生成使用说明书时，按此模板结构生成 python-docx 脚本，替换具体的软件名称、功能模块和字段描述即可。

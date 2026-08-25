---
name: soft-copy-source-code-guide
description: 软著源代码文档撰写规范 — 代码提取、排版、页眉页脚、格式要求的全套标准
metadata:
  type: project
---

# 软著源代码文档撰写规范

**来源：** 参照《软著代码提交注意事项》提炼，适用于中国版权保护中心软件著作权登记。

## 核心规格

| 项目 | 要求 |
|------|------|
| 总页数 | 前30页 + 后30页 = 共60页（代码不足60页可全部提交） |
| 每页行数 | >=50行有效代码（空行和纯注释不计入） |
| 最后一页 | 可少于50行，但不少于15行 |
| 文件格式 | PDF / A4纸 |
| 代码量建议 | >=3000行 |

## 代码内容要求

### 代码排列顺序
1. **第一页**：程序入口 / 主函数（必须有）
2. 配置文件
3. 核心业务逻辑
4. 数据处理模块
5. 工具类 / 公共模块
6. **最后一页**：结尾功能代码

### 必须遵守
- 前后30页必须各自**连续**，不可跳跃选取
- 去除多余空行
- 保留适当注释（帮助审核员理解）
- 必须体现核心功能，非框架生成代码

### 红线禁区
- 与已登记软件重复（即使是自己的另一个软著）
- 批量替换关键词伪装原创
- 包含矛盾时间戳（代码中日期不能与开发完成时间冲突）
- 包含第三方版权代码
- 纯设计器自动生成的代码

## 页眉页脚规范

### 页眉
| 位置 | 内容 | 字体 |
|------|------|------|
| 左侧 | 软件全称 | 宋体 小五(9pt) |
| 中间 | 版本号（如V1.0） | 宋体 小五(9pt) |
| 右侧 | 页码（第X页） | 宋体 小五(9pt) |

- 页眉信息必须与申请表**完全一致**（包括大小写：V1.0不能写成v1.0）

### 页脚（建议添加）
- 著作权人全称，居中，字体同页眉

## 排版规范

| 项目 | 设置 |
|------|------|
| 代码字体 | Consolas / Courier New / Source Code Pro |
| 字号 | 小五(9pt) 或 六号(7.5pt) |
| 行距 | 单倍行距 或 固定行距12磅 |
| 段前段后 | 0 |
| 缩进 | 4个空格（不用Tab） |
| 页面边距 | 参考说明书标准（上647700/下914400/左575945/右685800 EMU） |

## python-docx 代码模板

```python
from docx import Document
from docx.shared import Pt, Emu
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.oxml.ns import qn

doc = Document()

# 页面设置
for section in doc.sections:
    section.top_margin = Emu(647700)
    section.bottom_margin = Emu(914400)
    section.left_margin = Emu(575945)
    section.right_margin = Emu(685800)

# 页眉
for section in doc.sections:
    header = section.header
    header.is_linked_to_previous = False
    # 左：软件全称 | 中：版本号 | 右：页码
    hp = header.paragraphs[0] if header.paragraphs else header.add_paragraph()
    run = hp.add_run('软件全称    V1.0')
    run.font.name = '宋体'
    run._element.rPr.rFonts.set(qn('w:eastAsia'), '宋体')
    run.font.size = Pt(9)

# 代码行（每行一个段落）
for code_line in lines:
    p = doc.add_paragraph()
    p.paragraph_format.space_before = Pt(0)
    p.paragraph_format.space_after = Pt(0)
    p.paragraph_format.line_spacing = Pt(12)
    run = p.add_run(code_line)
    run.font.name = 'Consolas'
    run.font.size = Pt(9)
```

## 提交前自查清单

1. [ ] 代码第一页是程序入口/主函数？
2. [ ] 前30页连续？后30页连续？
3. [ ] 每页>=50行有效代码？
4. [ ] 页眉软件名与申请表完全一致？
5. [ ] 版本号大小写正确（V1.0不是v1.0）？
6. [ ] 代码中无矛盾时间戳？
7. [ ] 无第三方版权代码？
8. [ ] 无多余空行？
9. [ ] 代码功能与说明书对应？
10. [ ] PDF生成后检查总页数和格式？

## 常见补正应对

| 补正原因 | 解决方法 |
|----------|----------|
| 格式不规范 | 调整页眉/行数/页码后重新生成PDF |
| 功能与说明书不一致 | 调整代码内容使其与说明书匹配 |
| 独创性不足 | 替换重复部分为自己的核心代码 |
| 前后代码不连续 | 重新选取连续的60页代码 |

## 关联记忆

- 用户身份：[[user-profile-intern]]
- Word文档规范：[[word-writing-guide]]
- 术语文档生成：[[tech-term-doc-generator]]

**Why:** 用户负责软著材料撰写，源代码文档是软著申请的核心材料之一，需要标准化的撰写流程保证一次通过。
**How to apply:** 当用户需要生成软著源代码文档时，按此规范提取代码、排版、设置页眉页脚、生成PDF。

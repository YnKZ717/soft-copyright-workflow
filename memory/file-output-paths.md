---
name: file-output-paths
description: 文件输出路径规范 — 程序代码与文档/非程序文件分开存放
metadata:
  type: feedback
---

# 文件输出路径规范

## 规则

| 文件类型 | 输出路径 |
|----------|----------|
| 程序代码、配置文件 | `{{default_output_dir}}\客服agent\` （项目仓库内） |
| 文档、Word、PDF、非程序文件 | `{{default_output_dir}}\客服agent相关\` |

## 说明
- 交接文档、使用说明书、软著材料等 **不要放进项目仓库**
- 代码相关的（.py, .vue, .json, .env）放项目仓库
- 文档相关的（.docx, .md 交接文档, .pdf）放 `客服agent相关\`

**Why:** 用户明确要求"跟程序不直接相关的就放在 `{{default_output_dir}}\客服agent相关`"，保持项目仓库干净，文档和代码分离。

**How to apply:** 生成 Word 文档、交接文档等非代码文件时，保存到 `{{default_output_dir}}\客服agent相关\`，不要放到 `{{default_output_dir}}\` 根目录或项目仓库里。

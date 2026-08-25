---
name: aigc-eval-system-project
description: Neowow AIGC模型效果对比与版本评测系统 - 项目信息、已确认决策、文件路径
metadata:
  type: project
---

# Neowow AIGC模型效果对比与版本评测系统

**Why:** 该系统的软著撰写项目，记录已确认的技术决策和文件路径，避免重复沟通。
**How to apply:** 为该系统生成任何文档时，读取此文件获取项目上下文。

## 系统定位

平台模型决策支撑子系统，为模型积分定价、选型淘汰、版本升级提供量化依据。

## 已确认信息

| 项目 | 值 |
|------|-----|
| 软件全称 | Neowow AIGC模型效果对比与版本评测系统 |
| 简称 | Neowow模型评测 |
| 版本号 | V1.0 |
| 完成日期 | 2026-03-15 |
| 发表日期 | 2026-04-01 / 上海市 |
| 源程序量 | 约10000行 |
| 编程语言 | Java、Vue3、TypeScript、SQL |
| 软件分类 | 应用软件 |
| 技术特点类型 | 人工智能软件、云计算软件、大数据软件 |
| 评测方式 | 基准测试型（流派1）：标准数据集 + 自动评分 + LLM-as-Judge |

## 技术选型

| 类别 | 技术 |
|------|------|
| 后端框架 | Spring Boot 3.2 + MyBatis-Plus 3.5 |
| 数据库 | MySQL 8.0 |
| 缓存 | Redis 7.x |
| 前端 | Vue3 + TypeScript |
| 认证 | JWT |
| Web服务器 | Nginx 1.20 |

## 功能模块（7个）

模型管理 / 数据集管理 / 评测任务 / 评分管理 / 对比分析 / 评测报告 / 系统设置

## 文件输出路径

```
{{default_output_dir}}\待完成软著材料\Neowow AIGC模型效果对比与版本评测系统\result\Neowow AIGC模型效果对比与版本评测系统\
├── Neowow AIGC模型效果对比与版本评测系统V1.0-著作权申请表.docx
├── Neowow AIGC模型效果对比与版本评测系统V1.0-设计说明书.docx
└── （待完成）Neowow AIGC模型效果对比与版本评测系统V1.0-源代码文档.docx
```

**插图临时目录：** `...\Neowow AIGC模型效果对比与版本评测系统\.tmp\`

## 已完成文档

- [x] 著作权申请表
- [x] 设计说明书
- [ ] 源代码文档

## 特殊要求

- 安全设计章节要有系统特色（API密钥保护、评分防篡改、双盲评审），不千篇一律
- 不要出现具体厂商名称（OpenAI/通义千问/Claude等）
- 不要有AI痕迹，不要逻辑前后不一
- 正文字体宋体小四，段首空两格

---

## 关联记忆

- 设计说明书规范：[[design-doc-guide]]
- 申请表规范：[[copyright-application-form-guide]]
- 源代码规范：[[soft-copy-source-code-guide]]
- AIGC项目通用要求：[[aigc-project-requirements]]

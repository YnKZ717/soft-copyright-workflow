---
name: prompt-optimization-system
description: 提示词优化与管理系统软著项目信息汇总（已确认技术选型）
metadata:
  type: project
---

# Neowow 提示词优化与管理系统 V1.0

## 已确认信息

- 软件全称：Neowow 提示词优化与管理系统
- 版本号：V1.0
- 著作权人：{{company.name}}（{{company.credit_code}}）
- 公司成立日：{{company.founding_date}}
- 软件分类：应用软件
- 系统定位：**嵌入现有 Neowow 平台**，非独立产品
- 用户体系：**复用 Neowow 现有用户**，不自建注册登录

## 技术选型（已确认）

| 层次 | 选型 | 说明 |
|------|------|------|
| 后端框架 | **Java + Spring Boot 3.2** | 与其他 Neowow 项目统一 |
| 数据库 | **MySQL 8.0** | 与其他 Neowow 项目统一 |
| 缓存 | Redis 7.x | 通用 |
| 搜索引擎 | Elasticsearch 8.x | 提示词全文检索 |
| LLM对接 | **Spring AI** 或直接调 API | Java 生态，替代 Python 的 LangChain |
| 前端框架 | **Vue 3 + TypeScript** | 嵌入平台，与平台前端统一 |
| UI组件库 | Element Plus | 通用 |
| 编辑器 | Tiptap / Monaco | 富文本+代码高亮 |

## 功能模块（4个，无市场）

1. 用户与账户管理（复用Neowow用户，JWT认证）
2. 提示词编辑器（富文本/变量插值/AI优化/质量评估/测试执行）
3. 提示词库与模板（分类/标签/版本/预设模板库）
4. 数据统计与分析（使用统计/优化效果/模板热度）

## 数据库表（7张）

users（复用）/ prompts / prompt_versions / categories / tags / prompt_tags / templates

## API接口

基础路径 /api/v1/，JWT认证，JSON格式。
- 认证：POST /api/v1/auth/login
- 提示词：CRUD + 优化/评估/测试/AB测试/版本列表
- 模板：列表/详情/克隆
- 统计：概览/使用量/优化效果

## 安全设计

- JWT认证（HS256，24h有效）
- RBAC权限（4种角色）
- 密码bcrypt加盐哈希
- HTTPS传输
- 内容安全审核

## 暂不处理

完成日期 / 发表日期 / 源程序量 / 技术特点类型 / 面向领域

## 关联记忆

- 软著工作流：[[copyright-material-workflow]]
- 申请表规范：[[copyright-application-form-guide]]
- Word规范：[[word-writing-guide]]
- AIGC项目要求：[[aigc-project-requirements]]

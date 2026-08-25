---
name: future-development-plan
description: 客服 Agent 项目的后续开发计划，实习结束后继续完善
metadata:
  type: project
---

# 客服 Agent 后续开发计划

## 1. 其他排查流程加选项
- **当前状态：** 只有 video_fail 的 step0 和 step1 有 `options` 字段
- **待做：** 给 image_fail、timeout、quality、account_issue 等流程也加 `options`
- **涉及文件：** `troubleshoot_flows.py`

## 2. 知识库命中率提升（当前 42%）
- **问题：** 知识库命中率偏低，很多用户问题匹配不到 FAQ
- **方案：**
  - embedding 调优（调整相似度阈值、换 embedding 模型）
  - 同义词扩展（用户口语化表达 → FAQ 标准问法）
  - 补充 FAQ 条目（根据 pending_faqs 中的高频未命中问题）
- **涉及文件：** `nodes.py`（search_knowledge_base 函数）、`approved_faqs.json`

## 3. 退出关键词误触修复
- **问题：** "可以了"、"懂了"、"好了" 等词在正常对话中也常用，可能误退出排查
- **方案：** 检测到退出关键词时，加一步确认："确认要结束排查吗？回复'确认'结束，或继续描述问题。"
- **涉及文件：** `nodes.py`（troubleshoot 函数的退出关键词检测部分）

## 4. 前端状态栏优化
- **问题：** "排查中" badge 只显示 step 数字，用户不直观
- **方案：** 改成显示流程名称，如"视频生成失败排查 第2步"
- **涉及文件：** `ChatView.vue`（troubleshoot-badge 模板）

## 5. 自动化测试覆盖
- **问题：** 没有自动化测试，每次改动靠手动测试
- **方案：**
  - 基于现有 `test_agent.py` 扩展更多用例
  - 覆盖排查流程、退出机制、意图切换、多模态等核心功能
- **涉及文件：** `test_agent.py`、新测试文件

## 6. 模型降级追踪优化
- **当前状态：** 已实现降级记录写入 stats.json 并在看板展示
- **后续：** 可以考虑降级时通知管理员、自动回切主模型等

**Why:** 实习即将结束，这些是项目剩余的开发任务，后续继续完善时需要参考。

**How to apply:** 按优先级依次完成，优先做第3点（退出误触），因为真实使用中影响最大。

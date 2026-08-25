---
name: bug-lessons-learned
description: 项目中反复出现的bug模式和教训，避免重蹈覆辙
metadata:
  type: feedback
---

# 项目Bug教训记录

## 1. f-string 嵌套引号 → SyntaxError
- **问题：** `f"""...{step['question']}..."""` 单引号在 f-string 三引号内冲突
- **修复：** 提前取变量 `current_question = step['question']`，再 `f"""...{current_question}..."""`
- **教训：** 改 prompt 字符串时用字符串拼接，不用 f-string 嵌套

## 2. AgentState 缺字段 → 数据丢失
- **问题：** 节点返回 `model_used`，但 `AgentState` 没声明该字段，LangGraph 合并时丢弃
- **修复：** 在 `graph.py` 的 `AgentState` 加对应字段
- **教训：** 节点返回值变了 → 同步更新 AgentState

## 3. stats.json 旧数据缺字段 → KeyError
- **问题：** 旧 daily 记录没有 `troubleshoot` 字段，直接访问崩溃
- **修复：** 用 `setdefault("troubleshoot", 0)` 初始化
- **教训：** 修改统计字段时，对旧数据做兼容处理

## 4. 前端变量名冲突 → ReferenceError
- **问题：** `const images` 和 `let images` 在同一函数作用域，JS 暂时性死区报错
- **修复：** 改名 `faqImages` 避免冲突
- **教训：** 新增变量名检查是否与已有变量重复

## 5. 表单事件冲突 → 页面刷新
- **问题：** `@submit.prevent` + `type="submit"` 按钮，按 Enter 触发默认提交
- **修复：** 去掉 `@submit.prevent` 或改用 `type="button"`
- **教训：** 表单内按钮统一用 `type="button"`

## 6. 模型徽章不显示
- **问题：** `model_used` 字段在多处传递链中丢失（AgentState → invoke → main.py）
- **修复：** 整条链路都加上该字段
- **教训：** 数据传递要追踪完整链路，不只改一端

## 7. 阿里云欠费 ≠ 免费额度用完
- **问题：** 用户以为有免费额度就不会欠费，实际调了不在免费列表的模型（qwen-vl-plus）
- **修复：** 改用 `qwen3.7-plus`（在免费列表内，且支持视觉）
- **教训：** 确认模型在免费额度列表里再用，否则按量计费产生账单

## 8. 前端 token 一次性读取 → 401 Unauthorized
- **问题：** `const token = localStorage.getItem('token')` 在组件加载时读一次就固定了，之后登录写入的 token 不会更新，所有请求都带 `Bearer null`
- **修复：** 改成函数 `getHeaders()` 每次请求时动态读取 localStorage
- **教训：** localStorage 读取不要放在组件顶层 const，要封装成函数每次调用时读

## 9. LoginView API_BASE 不一致 → 401
- **问题：** LoginView 的 API_BASE 写 `192.168.10.209:8001`，其他页面是 `localhost:8001`，导致 token 存在不同 origin 的 localStorage 里，跨 origin 读不到
- **修复：** 统一改成 `localhost:8001`
- **教训：** 所有前端页面的 API_BASE 必须统一，否则 localStorage 跨 origin 不共享

## 10. ChatRequest min_length=1 导致纯图片请求 422
- **问题：** `user_input: str = Field(..., min_length=1)` 要求文字不能为空，用户只发图片没打字时验证失败
- **修复：** 改成 `default=""`，允许空文字 + 有图片
- **教训：** 支持多模态时，文本字段不能设 min_length=1

## 11. 正则匹配步骤数多了空格
- **问题：** 流式端点提取步骤数的正则 `r'第 (\d+) 步'` 多了空格，匹配不了 `故障排查中（第2步）`
- **修复：** 改成 `r'第(\d+)步'`（去掉空格）
- **教训：** 正则表达式要和前端实际输出的文本格式严格对应

## 12. troubleshoot 返回 intent="general" 但图路由到 END
- **问题：** troubleshoot 节点返回意图切换后，graph.py 中 troubleshoot 直接连 END，不会重新走 classify→kb_answer→general 流程
- **修复：** 加条件路由，intent=="general" 时路由到 kb_answer
- **教训：** 节点返回意图变化时，必须检查 graph 路由是否支持该意图的后续流程

**Why:** 这些bug在项目中反复出现，每次都需要用户愤怒反馈后才排查，浪费时间且影响信任。

**How to apply:** 每次改代码前对照此清单，避免犯同样的错误。特别是修改 prompt、新增字段、新增变量名时要格外小心。

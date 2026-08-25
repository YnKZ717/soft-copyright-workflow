---
name: code-change-checklist
description: 每次修改代码后必须执行的检查清单，防止"服务暂时不可用"等常见bug
metadata:
  type: feedback
---

# 代码修改后必做检查清单

**触发时机：** 每次修改 Python 或前端代码后，在告诉用户"改完了"之前，必须逐项检查。

## Python 后端检查

### 1. 语法检查（每次必做）
- 用 `py_compile.compile()` 验证所有改过的 `.py` 文件无语法错误
- **特别注意：** f-string 里不要嵌套同类型引号，`f"""...{dict['key']}..."""` 会报错，改用字符串拼接或提前取变量

### 2. 字段同步检查
- 修改了节点返回值 → 检查 `graph.py` 的 `AgentState` 是否加了该字段
- 修改了 `stats.json` 读取逻辑 → 检查旧数据是否兼容（用 `setdefault` 或 `get` 带默认值）
- 修改了 Pydantic 模型 → 检查请求/响应是否用到新字段

### 3. 变量名冲突检查
- 同一函数/作用域内不要有同名变量（`const x` 和 `let x`）
- 新增变量名不要和已有变量重复

### 4. 重启验证
- 后端代码改了 → 必须重启后端才能生效（`.env`、`nodes.py`、`main.py`、`graph.py` 等）
- 用户每次都会重启后端，不要让用户背锅

## 前端检查

### 5. 表单事件检查
- `@submit.prevent` 和 `type="submit"` 不要同时用，会导致回车刷新页面
- 用 `type="button"` + `@click` 更安全

### 6. 控制台报错检查
- 改完前端后检查浏览器 Console 有无红色报错
- `ReferenceError: Cannot access 'x' before initialization` = 变量名冲突（暂时性死区）
- `TypeError` = 类型问题，比如 Event 对象当字符串用

### 7. Network 请求检查
- 如果前端报"服务暂时不可用"但后端日志没有对应请求 → 请求根本没发出去，查前端代码
- 401 Unauthorized → 检查 token 是否每次请求时动态读取（不要 `const token = localStorage.getItem(...)` 放在组件顶层）

## 常见"服务暂时不可用"原因速查

| 现象 | 原因 | 检查方法 |
|------|------|----------|
| 后端日志无请求 | 前端 fetch 没执行 | 看 Console 报错 |
| 后端日志有 500 | Python 运行时错误 | 看 server.log traceback |
| 后端日志有请求但返回空 | 节点返回字段缺失 | 检查 AgentState 字段 |
| 按回车刷新页面 | 表单事件冲突 | 检查 type="submit" + @submit.prevent |

**Why:** 项目中多次出现"服务暂时不可用"，每次都花大量时间排查。根本原因是改代码后没有系统性地检查上述问题。

**How to apply:** 每次告诉用户"改完了"之前，跑一遍上面的检查项。不要等用户报错了再排查。

---
name: python-billing-simulator-idea
description: Python计费模拟器的设计思路和实现方案，用于后续开发
metadata:
  type: project
  created: 2026-07-14
---

# Python计费模拟器设计思路

## 目标
在Excel矩阵完成后，如果需要一个更强大的模拟工具，可以开发Python脚本来模拟用户消费行为。

## 核心功能

### 1. 配置文件驱动
使用YAML或JSON配置所有功能、选项、系数：

```yaml
functions:
  text-to-image:
    name: "文生图"
    base_price: 100
    options:
      resolution: [1K, 2K]
      aspect_ratio: [1:1, 3:4, 4:3, 9:16, 16:9, 21:9]
      model: [Neo-Image-2]
    constraints:
      - type: "range"
        field: "resolution"
        values: [1K, 2K]
```

### 2. 约束验证系统
- 互斥规则：某些选项不能同时选择
- 依赖规则：某些选项依赖其他选项
- 范围规则：限制选项的可选范围

### 3. 交互式模拟器
命令行交互或简单的GUI：
```
选择功能：1.文生图 2.文生视频 3.图生图 ...
请选择：1
选择分辨率：1. 1K 2. 2K
请选择：2
...
计算结果：总费用 = XXX积分 = XX元
```

### 4. 批量模拟
可以批量生成不同组合的费用，用于：
- 价格策略验证
- 用户消费行为分析
- 报表生成

### 5. 技术栈
- Python 3.8+
- PyYAML（配置解析）
- Rich（命令行美化，可选）
- PyQt5/Tkinter（GUI版本，可选）

## 实现优先级
1. 先做命令行版本（快速验证逻辑）
2. 再做配置文件解析（支持热更新）
3. 最后考虑GUI版本（如果需要给非技术人员使用）

## 与Excel的关系
- Excel：适合初期调研和简单模拟
- Python：适合复杂逻辑验证和批量分析
- 两者可以互补使用

## 开发时机
等Excel矩阵使用一段时间，确认逻辑无误后再开发。

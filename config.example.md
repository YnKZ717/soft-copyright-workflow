# 软著工作流配置文件
# 复制此文件为 config.md 并填写实际值
# ⚠️ config.md 已加入 .gitignore，不要提交到 Git

---

## 路径配置

```yaml
# 参考资料目录（两个，用逗号分隔）
reference_dirs:
  - "{你的路径}/软著材料/参考资料"
  - "{你的路径}/dossier2"

# 软著材料模板目录
template_dir: "{你的路径}/软著材料模板"

# 默认输出目录（工作流会先问用户，此值作为预设选项）
default_output_dir: "{你的路径}/软著材料"

# 记忆文件目录（相对路径，一般不改）
memory_dir: ".claude/memory"
```

## 公司固定信息

```yaml
company:
  name: "上海智灵新境科技有限公司"
  credit_code: "91310000MAE9X18UXP"
  founding_date: "2025-01-21"
  publish_city: "上海市"
  software_category: "应用软件"

hardware:
  dev_cpu: "CPU 2GHz+"
  dev_memory: "内存 8G+"
  dev_disk: "硬盘 200G+"
  run_cpu: "CPU 2GHz+"
  run_memory: "内存 8G+"
  run_disk: "硬盘 200G+"

software:
  dev_os: "Windows 10/11、Linux"
  run_platform: "Windows、Linux、MacOS（浏览器端）"
  runtime: "JDK 17+、MySQL 8.0+、Redis 7.x、Nginx 1.20+"
```

---

## 使用说明

1. 复制 `config.example.md` 为 `config.md`
2. 将 `{你的路径}` 替换为你实际的路径（如 `D:\privateforyge`）
3. 公司信息按需修改
4. **不要将 `config.md` 提交到 Git**（已在 .gitignore 中排除）

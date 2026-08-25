- [中文沟通偏好](chinese-communication.md) — 永远使用中文与用户沟通
- [Word文档撰写规范](word-writing-guide.md) — 软著材料模板提炼的字体/字号/页面/结构标准
- [用户身份画像](user-profile-intern.md) — AI内容创作平台实习生，负责软著，需学习专业术语
- [术语文档生成规范](tech-term-doc-generator.md) — 学习新术语时自动生成Word文档的标准流程
- [软著源代码撰写规范](soft-copy-source-code-guide.md) — 软著源代码文档的代码提取、排版、页眉页脚、格式全套标准
- [设计说明书撰写规范](design-doc-guide.md) — 设计说明书的层级结构、标准章节模板、撰写要求、格式规范
- [著作权申请表撰写规范](copyright-application-form-guide.md) — 申请表字段定义、字数限制、日期逻辑、功能描述模板、公司固定信息
- [软著使用说明书撰写规范](user-manual-guide.md) — 使用说明书标准章节结构、操作描述句式、截图规范、字段说明格式、python-docx代码模板
- [软著材料撰写工作流](copyright-material-workflow.md) — 信息收集→确认→批量生成→质量检查→打包输出的完整流程设计
- [Python计费模拟器设计思路](python-billing-simulator-idea.md) — 待Excel矩阵完成后，开发Python脚本模拟用户消费行为的设计方案
- [产品经理调研问卷模板](research-survey-template.md) — 分批调研功能、价格、系数的问卷模板和使用建议
- [计费系统开发策略](billing-system-dev-strategy.md) — 先搭框架后填计费引擎的分阶段开发方案，模块化设计，配置驱动
- [AIGC 项目特殊要求](aigc-project-requirements.md) — 系统只读写数据库、首选 JS、Neowow 前缀、输出路径控制、文档格式规范
- [提示词优化与管理系统](prompt-optimization-system.md) — 项目信息汇总、已确认/待确认清单、建议方案
- [客服 Agent 后续开发计划](future-development-plan.md) — 6项后续任务：排查选项扩展、知识库命中率、退出误触修复、状态栏优化、测试覆盖、降级优化
- [双 Agent 协作机制](dual-agent-collaboration-plan.md) — 方案三：副Agent做安全网，评估并修正主Agent回答
- [Skill迁移目标设备信息](skill-migration-target-device-info.md) — Yannick-Z 个人电脑环境约束、skill 结构要求、迁移工作流、路径替换规则
- [代码修改检查清单](code-change-checklist.md) — 每次改代码后必做的检查项，防止"服务暂时不可用"
- [Bug教训记录](bug-lessons-learned.md) — 项目中反复出现的bug模式和修复方法
- [负面提示词模板](code-review-negative-prompt.md) — 任务下达时可附带的代码质量检查提示词
- [文件输出路径规范](file-output-paths.md) — 代码放项目仓库，文档放客服agent相关目录
- [小程序图片素材清单](miniprogram-image-status.md) — 说"继续讨论图片素材"时展示此清单

## 可用技能（5个模块化 Skill）

| 技能名 | 职责 | 触发方式 |
|--------|------|----------|
| `write-design-doc` | 设计说明书生成（含图表自动生成） | `/write-design-doc` 或自然语言 |
| `write-copyright-app` | 著作权申请表生成 | `/write-copyright-application` 或自然语言 |
| `write-source-code` | 源代码文档生成 | `/generate-source-code-doc` 或自然语言 |
| `write-user-manual` | 使用说明书生成 | `/write-user-manual` 或自然语言 |
| `learn-term` | 术语文档生成（入门/概念/深入） | `/learn-term` 或"学习XXX" |

**已删除的重复文件：**
- `cmmi3-design-doc` Skill — 合并进 `write-design-doc`
- `write-copyright-application` Skill — 重命名为 `write-copyright-app`，精简表格代码
- `write-word` Command — 太泛，被具体skill覆盖
- `generate-source-code-doc` Command — 和skill重复
- `（command）write-design-doc` Command — 和skill重复

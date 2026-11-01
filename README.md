# Praxis知行伴学 · AI 自主学习 Agent

> 将 Canvas / CourseWorks 中分散的 syllabus、课件、教材与作业，重建为可追溯知识地图、证据约束问答和自适应练习闭环。

[![MVP](https://img.shields.io/badge/Live_MVP-Open-2563EB)](https://daniel0330-hub.github.io/praxis-zhixing-companion/)
[![Courses](https://img.shields.io/badge/Courses-6-0F766E)](#真实研发与数据处理)
[![Knowledge Nodes](https://img.shields.io/badge/Knowledge_Nodes-135-7C3AED)](#真实研发与数据处理)
[![Documents](https://img.shields.io/badge/R%26D_Documents-36-EA580C)](documentation/README.md)

## 在线入口

- **[打开正式 Praxis知行伴学 MVP](https://daniel0330-hub.github.io/praxis-zhixing-companion/)**
- [查看完整研发文档](documentation/README.md)
- [查看正式 MVP 研发汇总](documentation/04-project-management/37-Praxis知行伴学-正式MVP研发文档汇总.md)
- [查看项目路线图](ROADMAP.md)

## 项目概述

Praxis知行伴学不是单纯的课程资料展示页，而是一套围绕真实课程资料构建的 AI 学习产品。系统在用户授权的 Canvas 会话中采集课程材料，识别 syllabus、课件和作业，将课程内容转换为知识地图、可信问答与自适应练习，并通过证据引用、拒答和成本降级机制控制模型风险。

## 真实研发与数据处理

`授权 Canvas 会话 → Agent 辅助采集 → 文档分类 → 页级解析与索引 → 课程知识建模 → RAG 问答 → 作业归类与仿题 → 自适应练习`

| 指标 | 当前结果 |
|---|---:|
| 已处理课程 | 6 门 |
| 课程材料 | 295 份 |
| 页面 / 检索切片 | 5,217 |
| 知识节点 / 已绑定来源 | 135 / 115 |
| 练习题 | 114 |
| 公式占位节点 | 0 |
| 自动化测试 | 24 项通过 |

课程范围：MATH GR5280、STAT GR5203、STAT GR5204、STAT GR5205、STAT GR5261、STAT S5221。

## 已实现功能

- 六门课程 Dashboard，以及按课程主题色区分的考试 / Project Timeline；
- 分层交互式知识地图，支持定义、公式、应用、来源页码和相关练习；
- 135 个知识节点均具备明确公式或判定规则，不再使用通用占位提示；
- 页级 RAG 检索、课程问答、来源引用和证据不足拒答；
- 作业题知识点匹配、同构仿题、批改、错因反馈、迁移题和退出测试；
- 中英文界面、设置联动、每日模型调用上限、缓存与本地降级。

## Canvas / Agent 接入

- 使用真实 CourseWorks（Canvas）登录会话；
- Duo 验证始终由用户本人完成，不保存密码或持久 Token；
- Agent 在授权会话内只读识别课程、遍历文件并保存课程包；
- Canvas OAuth2 / REST API 连接器完成方案设计，生产接入仍受 Developer Key 和 Duo 条件约束；
- 保留人工导出与本地上传作为兜底路径。

## AI 与 POC 路线

- **POC-1**：课件解析、公式恢复和知识地图；
- **POC-2**：可信问答、页码引用与证据不足拒答；
- **POC-3**：诊断、定向微练习、迁移评测与退出测试；
- **模型层**：OpenAI Responses API、Structured Outputs、提示词模板与低成本回退；
- **内容层**：syllabus、课件、作业和合法可访问教材资料联合建模。

## 仓库结构

```text
.
├── docs/                         # GitHub Pages 正式 MVP
├── documentation/
│   ├── 01-product-discovery/     # 调研、需求、产品方案、PRD
│   ├── 02-poc-evaluation/        # POC、诊断与评测记录
│   ├── 03-engineering-ai/        # 工程里程碑、AI 与成本策略
│   └── 04-project-management/    # 项目日志、版本日志与研发汇总
├── .gitignore
├── LICENSE
├── README.md
└── ROADMAP.md
```

## 公开边界

公开仓库保留真实课程代码、处理规模、研发流程、知识结构和测试结论。仓库不包含原始课程文件、完整课件或教材、个人成绩、教师邮箱、Cookie、Token、API Key 与本地数据库。

## 运行方式

在线演示直接访问 [GitHub Pages](https://daniel0330-hub.github.io/praxis-zhixing-companion/)。如需本地预览静态版本：

```bash
python3 -m http.server 8766 --directory docs
```

然后打开 `http://127.0.0.1:8766/`。

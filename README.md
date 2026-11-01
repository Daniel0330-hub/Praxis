# Praxis知行伴学 · AI 自主学习 Agent

> 将 Canvas / CourseWorks 中分散的 syllabus、课件、教材与作业，重建为可追溯知识地图、证据约束问答和自适应练习闭环。

## 在线入口

- **[体验 Praxis知行伴学公开 MVP](https://Daniel0330-hub.github.io/praxis-zhixing-companion/)**
- [查看全部独立研发文档](#完整研发文档)
- [查看研发过程汇总](RESEARCH_AND_DEVELOPMENT.md)

## 真实研发与数据处理

`用户授权 Canvas 会话 → Agent 辅助课程采集 → 文档角色分类 → 页级解析/索引 → syllabus 与课件/教材联合建模 → 知识地图 → RAG 问答 → 作业归类与仿题 → 自适应练习`

| 指标 | 真实结果 |
|---|---:|
| 已处理课程 | 6 门 |
| 课程材料 | 295 份 |
| 页面 / 检索切片 | 5,217 |
| 知识节点 / 已绑定来源 | 135 / 115 |
| 练习题 | 114 |
| 自动化测试 | 24 项通过 |

课程包括 MATH GR5280、STAT GR5203、STAT GR5204、STAT GR5205、STAT GR5261、STAT S5221。

## Canvas / Agent 接入实况

- 使用 Columbia CourseWorks（Canvas）真实登录会话；
- 用户每次确认登录并亲自完成 Duo，不保存密码或持久 Token；
- Agent 在授权会话中只读识别 STAT/MATH 课程、遍历文件并下载课程包；
- Canvas OAuth2 / REST API 连接器完成方案设计，但受 Developer Key 与 Duo 条件限制未生产化；
- 保留人工导出/上传兜底。

## 已实现功能

- 课程 Dashboard 与考试/Project Timeline；
- 可展开定义、公式、应用、来源和练习的交互知识地图；
- 页级检索、引用展示和证据不足拒答；
- 作业知识点归类、同构仿题、批改、错因反馈、迁移题和退出测试；
- 中英文界面、设置联动、每日模型调用上限与降级策略。

## AI 与 POC

- POC-1：课件解析、公式恢复与知识地图；
- POC-2：可信问答、页码引用与拒答；
- POC-3：诊断、定向微练习、迁移题与退出测试；
- OpenAI Responses API + Structured Outputs；
- 本地规则与缓存优先，仅在必要环节调用模型。

## 完整研发文档

仓库已经逐份公开 36 份脱敏研发文档，覆盖需求调研、需求分析与立项、产品方案、AI 技术方案、正式 PRD、数据准备、三轮 POC、模型评估、成本策略、五个工程里程碑、Canvas 式界面重构、项目日志、版本日志和完整目录。

## 公开边界

公开内容保留真实课程代码、处理规模、研发过程和测试结论；不包含原始课程文件、课件正文、教材全文、作业答案、个人成绩、教师或学生联系方式、Cookie、Token、API Key 和本地数据库。

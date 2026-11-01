> 公开脱敏版：真实课程身份、文件名和个人信息已隐藏；原始记录保存在私有项目。

# Praxis知行伴学 Responses API 接入与模型评估

版本：v1.0  
日期：2026-10-31

## 1. 本轮结论

真实模型接入代码已经完成，使用官方 OpenAI Python SDK、Responses API 和 Pydantic Structured Outputs。当前环境没有配置 `OPENAI_API_KEY`，所以本轮没有发起真实收费请求，也没有把课程材料发送给外部服务。

产品默认仍使用本地规则讲解。配置密钥并显式设置 `ZHIXING_EXPLANATION_PROVIDER=openai` 后才会启用真实模型。

## 2. 接入结构

新增 `OpenAIExplanationProvider`：

- 默认模型候选为 `gpt-6.1-terra`；
- 使用 `client.responses.parse(...)`；
- 使用 Pydantic 模型约束 `hint / steps / self_check`；
- 只向模型发送题干、错因类别和课程证据；
- 不发送答案键、正确选项或最终验证数据；
- 引用由服务端根据检索结果附加，模型不能自行填写来源。

官方 Structured Outputs 文档提供了 `responses.parse` 和 Pydantic `text_format` 的标准接入方式，并说明模式约束能够提高结构可靠性。[OpenAI Structured Outputs](https://developers.openai.com/api/docs/guides/structured-outputs?api-mode=responses)

## 3. 配置方式

```bash
export OPENAI_API_KEY="..."
export ZHIXING_EXPLANATION_PROVIDER="openai"
export ZHIXING_OPENAI_MODEL="gpt-6.1-terra"
uv run uvicorn zhixing_mvp.api:app
```

缺少密钥、模型超时、SDK 异常或输出门禁失败时，系统自动使用 `RuleFallbackProvider`，作答与推荐流程不会中断。

## 4. 离线模型评估器

新增命令：

```bash
uv run zhixing --data-dir .data eval-explanations 示例课程 --provider rule --limit 10
uv run zhixing --data-dir .data eval-explanations 示例课程 --provider openai --model gpt-6.1-terra --limit 10
```

评估过程：

1. 从业务题库读取题干和知识点映射；
2. 从 `content` 分区读取课程证据；
3. 调用指定讲解提供方；
4. 生成完成后，评估器才从独立 evaluator 数据库读取答案；
5. 检查答案泄露、引用完整性和总体门禁通过率。

答案键只用于事后评分，不进入模型输入，也不写入业务数据库。

已在真实课程的前 10 道非最终验证题上运行规则基线：10/10 通过，运行失败 0，答案泄露 0，引用失败 0。该结果是结构与安全基线，不代表教学质量已经达到上线标准。

## 5. 自动化验证

本轮新增验证：

- Responses API 适配器使用 Pydantic 结构化输出；
- 引用取自服务端证据，而不是模型返回值；
- 模型超时后自动切换规则兜底；
- 兜底讲解仍通过答案泄露和引用门禁。

最终共发现 17 项测试：15 项通过，2 项真实课程扩展测试默认跳过。

## 6. 未完成项

由于当前没有 API 密钥，以下工作需在密钥配置后进行：

- GPT-6.1 Terra 的真实请求验证；
- GPT-6 Luna 与 Terra 的同题对比；
- 延迟、token 用量和单次讲解成本统计；
- 50—100 条正式评估集批量运行；
- 根据硬门槛决定是否将模型设为默认提供方。

在真实评估完成前，不建议关闭规则兜底或让模型直接成为默认路径。


---

## 2026-10-31 最新状态注记

本文记录当时的 AI 讲解、模型评估或成本策略。当前实现采用选择性调用与本地回退，用户可在设置中保存每日模型调用上限（0、1、3、5、10、20、50）；限额实际进入调用路径。本文的原始评估样本和指标不因新界面而改变。 当前需求和执行口径见 `05-Praxis知行伴学-正式PRD-v1.0.md` 文末 v1.1 附注；跨文档状态见 `34-Praxis知行伴学-项目日志.md` 和 `35-Praxis知行伴学-版本日志.md`。

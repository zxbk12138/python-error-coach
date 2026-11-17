# GitHub Skill 参考提炼

## 搜索范围

使用 GitHub 公共仓库搜索与以下主题相关的 Skill：

- Python 教学与代码陪练；
- Socratic coding tutor；
- 代码调试与根因分析；
- Agent Skill 结构、渐进式披露与评测。

## 主要参考

| 项目 | 许可证 | 提炼内容 |
|---|---|---|
| [OpenAI Skills](https://github.com/openai/skills) | 官方公开仓库 | 描述要便于发现、主文件渐进式披露、只在需要时加载参考资料、用真实行为测试 |
| [AI Tutor Skill](https://github.com/Bhala-Srinivash/agent-tutor-skill) | MIT | 教学闭环、掌握证据、错因反馈、零提示测验 |
| [CodeTrain](https://github.com/InferHaven/agent-skills/tree/main/codetrain) | Apache-2.0 | 修错与教学模式区分、一次小步、提示逐级增加、运行真实性 |
| [Structured Root-Cause Research](https://github.com/meiiie/structured-root-cause-research-skill) | MIT | 事实与推断分离、竞争假设、区分性测试、证据不足保留不确定性 |
| [Feynman Tutor](https://github.com/koukekoukej-glitch/feynman-tutor) | MIT | 使用学习者语言、不为具体代码硬造类比、根据意图调整对话深度 |

## 已融入作品

### 1. 模式选择

- 修错：直接解决当前代码问题；
- 教学：让用户自己写、预测和解释；
- 复习测验：零提示检查掌握程度。

### 2. 证据驱动

- 直接观察、用户推断和未知信息分开；
- 最多保留三个竞争假设；
- 优先做能区分假设的最小测试；
- 补充证据后明确更新或撤回原假设。

### 3. 掌握验证

- 不接受“我懂了”作为证据；
- 修复后至少让用户解释、预测或完成变式；
- 用户只要求快速修复时，检查保持轻量。

### 4. 提示与测验

- 修错按四级提示渐进帮助；
- 教学模式避免过早给完整答案；
- 测验不通过题目和选项暗示正确答案。

### 5. 运行真实性

- 没有看到输出时只说明预期行为；
- 不声称代码已经运行成功；
- 不展示内部思维链草稿，只输出证据摘要。

### 6. 渐进式披露

`SKILL.md` 保留核心模式、边界和流程；详细教学法、错因索引、水平话术和来源说明放入 `references/`，只在对应场景读取。另加入 `evals/` 保存行为验收用例。

## 未采用

- 独立 Web UI；
- 跨会话数据库和复杂画像；
- 自动执行用户代码；
- 固定数量的强制测验；
- 直接复制参考项目的正文。

这些能力会增加平台依赖或让初学者负担过重，不作为本次 TeleAgent 技能的第一版范围。


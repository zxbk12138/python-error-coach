# Python Learning Diagnosis Coach

[中文说明](#中文说明) · [English overview](#english-overview) · [MIT License](LICENSE)

A portable Python tutoring and debugging skill for any `SKILL.md`-compatible agent.

It diagnoses real Python problems, teaches one small step at a time, verifies fixes, and checks whether the learner actually understood the change.

The core skill is platform-neutral and text-first. Optional file access, code execution, web lookup, or UI features are used only when the host agent actually provides them.

The response style is intentionally concise. It avoids generic tutorials, repeated disclaimers, forced headings, fabricated certainty, and unnecessary rewrites of the learner's code.

Concise mode is the default. There is no platform-specific toggle to install: use the skill normally and it starts concise. A one-line reminder at the end tells users they can switch to `详细模式` or `教学模式`. Detailed and teaching responses do not repeat that reminder.

Current skill version: **3.3.3**.

## 中文说明

### 项目简介

Python 学习诊断陪练是一个跨平台的 Agent Skill。它不绑定特定客户端、模型或界面，也不使用固定难度的“万能讲解”，而是根据实际代码、报错、输入输出和学习目标动态调整。

核心流程：

> 识别事实 → 建立少量假设 → 选择一个区分性测试 → 根据反馈更新判断 → 验证修复 → 检查是否真正理解

### 四种工作模式

- **修错**：用户想尽快解决代码问题。
- **教学**：用户希望自己写、预测和解释代码，而不是直接拿答案。
- **复习测验**：用零提示题目检查掌握程度。
- **规划或代码审查**：整理下一步学习路径，或分析已有代码的质量、边界与风险。

### 主要能力

- 面向不同水平，覆盖 Python 入门、核心语法、常用标准库和进阶语言特性；
- 一次只给一个主要下一步；
- 根据新反馈修改或撤回原有判断；
- “我懂了”必须通过解释、预测或变式练习验证；
- 没有看到运行结果时不声称代码运行成功；
- 不展示内部思维链，只输出证据摘要；
- 第三方库信息不足时不编造 API；
- 内置行为评测，避免用固定话术替代真实效果。

### 减少“AI 味”

作品不把“AI 味”只当成措辞问题，而是约束常见行为：

- 简单问题直接说错在哪，不强制套“背景、原因、建议、总结”模板；
- 单点代码修复统一用“问题一句话、代码块、验证一句话”，避免密集行内代码；
- 简单修复不输出测试表、执行日志或多组案例矩阵；
- 一眼可确认的修复默认不运行代码，避免把简单回答扩展成测试报告；
- 修错和教学模式默认都不弹交互式问答、问卷或选择题；
- 只有用户明确要求“使用交互式问答组件”时才启用该组件；
- 对常见单点错误走快速路径，不加载参考资料或建立多个假设；
- 不重复用户原话，不加入空泛表扬和客套话；
- 一处信息足够时不发起问卷式追问；
- 用户明确说“直接修”时先给最小修复；
- 不添加未请求的抽象层、类、配置项或无关重构；
- 只改与当前问题有关的代码，发现其他问题只说明，不顺手修改；
- 通过复现、最小修改和验证证明问题解决；
- 答案足够时停止，不为了显得完整而追加长总结。

实际测试中，一个简单的 `NameError` 回答只包含两句解释和一行修改；一个范围明确的修复只改动了 `x == ''` 到 `x.strip() == ''`，没有改动命名、格式或增加无关功能。

### 默认简洁与模式切换

- 安装后默认就是简洁模式，不需要额外按钮；
- 简洁回答末尾只加一句：`想深入理解，可以回复“详细模式”或“教学模式”。`
- `详细模式`：展开原理和更多解释；
- `教学模式`：引导学生自己写、预测和修正；
- 如果本轮已经详细讲解，不再追加模式提醒；
- 用户明确说“不要提醒”时省略这句话。

Skill 不能向 TeleAgent 添加原生 UI 开关；这是平台界面限制，不是可以通过 `SKILL.md` 绕过的能力。

### 通用安装

1. 下载 [`dist/python-error-coach.zip`](dist/python-error-coach.zip)。
2. 如果宿主 Agent 支持技能导入，直接导入 ZIP。
3. 如果宿主 Agent 使用本地技能目录，将解压后的 `python-error-coach/` 放入其 Skills 目录。
4. 显式调用 `$python-error-coach`，或让宿主根据描述自动选择。

TeleAgent 等支持 ZIP 导入的平台也可以上传同一个包；平台专属元数据不是运行前提。

ZIP 解压后的第一层是：

```text
python-error-coach/
└── SKILL.md
```

### 使用示例

```text
我是零基础：

city = 北京
print(city)

报错：
NameError: name '北京' is not defined

请一次只给一个检查点。
```

预期行为：

- 指出无引号的“北京”会被 Python 当成变量名；
- 不一次性讲完整变量章节；
- 给出可以立即验证的下一步。

另一个示例：

```text
我已经懂了 f-string，直接下一课。
```

预期行为：

- 不把“我懂了”直接当作掌握证据；
- 只给一个很短的预测题或迁移题；
- 用户只求快速前进时不展开长测验。

### 仓库结构

```text
.
├── python-error-coach/
│   ├── SKILL.md
│   ├── agents/
│   ├── assets/
│   ├── evals/
│   └── references/
├── demo_cases/
├── docs/
├── dist/
│   └── python-error-coach.zip
└── LICENSE
```

### 参考资料

本项目的设计参考了以下公开项目，但所有规则与文本均为重新整理：

- [OpenAI Skills](https://github.com/openai/skills)
- [AI Tutor Skill](https://github.com/Bhala-Srinivash/agent-tutor-skill)，MIT
- [CodeTrain](https://github.com/InferHaven/agent-skills/tree/main/codetrain)，Apache-2.0
- [Structured Root-Cause Research](https://github.com/meiiie/structured-root-cause-research-skill)，MIT
- [Feynman Tutor](https://github.com/koukekoukej-glitch/feynman-tutor)，MIT
- [Karpathy Guidelines](https://github.com/multica-ai/andrej-karpathy-skills)，MIT
- [小黑盒社区关于 Skill 与 AI 协作的讨论](https://www.xiaoheihe.cn/app/bbs/link/8a3871e370bb)
- Runoob Python 教程仅作为章节索引

更完整的提炼见 [`docs/GitHub参考提炼.md`](docs/GitHub参考提炼.md)。

### 已知边界

- 只覆盖通用 Python；第三方框架和库需要版本信息和完整 traceback。
- 不执行用户代码。
- 不替代教师、助教或官方文档。
- 不同 Agent 模型执行 `SKILL.md` 的效果可能存在差异。

### 验证

- 通过 OpenAI `skill-creator` 的 `quick_validate.py` 校验；
- 在 TeleAgent 3.0.1 中完成一次平台兼容性验证；
- 覆盖零基础、类型错误、函数、生成器、未知库、多轮反馈、掌握验证和直接修复等场景；
- 详细记录见 [`docs/TeleAgent实测报告.md`](docs/TeleAgent实测报告.md)。

## English overview

Python Learning Diagnosis Coach is a portable tutoring skill for general Python work across skill levels.

It:

- separates observed facts from guesses;
- keeps a small set of competing hypotheses;
- proposes one discriminating next step at a time;
- updates its diagnosis when the learner provides new evidence;
- distinguishes “fix this for me” from “teach me how”;
- verifies understanding instead of accepting “I get it”;
- supports teaching, debugging, review, assessment, and learning-path planning;
- avoids inventing third-party APIs when versions or tracebacks are missing.

The packaged skill is available at [`dist/python-error-coach.zip`](dist/python-error-coach.zip).

## Development and evaluation

The main instruction file is [`python-error-coach/SKILL.md`](python-error-coach/SKILL.md).

Behavioral evaluation cases are in [`python-error-coach/evals/cases.json`](python-error-coach/evals/cases.json). The goal is to compare observable behavior, not exact wording.

The public skill uses standard Agent Skills frontmatter. Platform-specific UI metadata is isolated in `agents/openai.yaml`.

Performance and competition-demo guidance is documented in [`docs/性能优化与比赛演示.md`](docs/性能优化与比赛演示.md).

The real classroom example uses the before/after programs in [`demo_cases/`](demo_cases/):

- `classroom-loop-before.py`
- `classroom-loop-after.py`

## Contributing

Please keep contributions focused on general Python learning and diagnosis.

1. Add or update a behavioral evaluation case.
2. Reproduce the current behavior.
3. Make the smallest instruction change that fixes the observed failure.
4. Verify the skill in a real Agent environment.
5. Document any new limitation.

Do not add personal data, credentials, platform tokens, or copied third-party skill text.

## License

MIT License. See [LICENSE](LICENSE).


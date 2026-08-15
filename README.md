# AI Coworker Skills · AI 同事技能包

10 个即装即用的 Agent Skill，治的都是让 AI 干活时最常撞的墙：**它说"完成了"其实没完成、它把技术问题甩给你拍板、它把小事写成事故、它顺手把你的文档改成大白话**。

全部源自一个非研发背景产品经理指挥 AI 开发数月的真实踩坑记录——每条规则背后都有一次返工。纯提示词实现，零依赖、零 API Key、无网络请求，装之前每个字都能审。

> 10 ready-to-use agent skills that fix the most common failure modes of AI coworkers: false "done" claims, technical homework dumped on non-technical users, dev-stage findings dressed up as incidents, and registers bleeding between chat and artifacts. Distilled from months of real rework by a non-engineering PM directing AI development. Pure prompts — zero dependencies, fully auditable.

## 两个套件 · Two Suites

### `essentials/` — AI 同事基本功（任何让 AI 干活的人）

| Skill | 一句话 |
|---|---|
| [dual-register-communication](essentials/skills/dual-register-communication/SKILL.md) 小白指挥AI干活之让AI讲人话 | 跟你说话用业务白话，写文档保持技术严谨——"请说人话"再也不会把你的派工单也变成人话 |
| [auditing-completion-claims](essentials/skills/auditing-completion-claims/SKILL.md) 小白指挥AI干活之验收神器 | 任何"完成了/通过了/环境好了"，先查机器事实、再按层级和证据等级追问，假完成拦在验收前 |
| [routing-decisions-to-humans](essentials/skills/routing-decisions-to-humans/SKILL.md) 小白指挥AI干活之我是产品AI是开发 | 技术问题 AI 自己判、做完告知；只有体验/范围/目标三类才找你，必带推荐方案 |
| [calibrating-governance-to-stage](essentials/skills/calibrating-governance-to-stage/SKILL.md) 小白指挥AI干活之分清轻重缓急 | 按爆炸半径给流程定价——测试阶段轻装快跑，别把测试环境小事写成"线上事故" |
| [reviewing-for-substance](essentials/skills/reviewing-for-substance/SKILL.md) 小白指挥AI干活之复审只挑真毛病 | 复审意见必须答得上"不改会坏什么"，措辞找茬不算发现；连续空转必须报警，不许拿文档治理冒充推进 |
| [humanizing-ai-written-text](essentials/skills/humanizing-ai-written-text/SKILL.md) 小白指挥AI干活之像个人一样写材料 | 专治 AI 写材料的空话病——删自嗨、套话、正确的废话、过程流水账，只留读者要看的实话干货 |

### `ai-dev-starter/` — 小白指挥 AI 开发（不懂代码但要做出东西的人）

| Skill | 一句话 |
|---|---|
| [ai-development-coach](ai-dev-starter/skills/ai-development-coach/SKILL.md) 小白指挥AI开发之私人教练 | 不是替你干活，是教你判断——每次执行附带为什么、下次怎么问、怎么发现 AI 越界 |
| [choosing-prototype-form](ai-dev-starter/skills/choosing-prototype-form/SKILL.md) 小白指挥AI开发之原型Demo一把抓 | HTML 还是 React Mock 还是先写方案？按阶段路由，禁止 HTML 硬转 React、原型冒充正式代码 |
| [gating-work-before-speed](ai-dev-starter/skills/gating-work-before-speed/SKILL.md) 小白指挥AI开发之怎么开工 | AI 动手前先过五问三道门，锁定仓库分支基线，commit→push→MR 逐级授权 |
| [packaging-low-context-work](ai-dev-starter/skills/packaging-low-context-work/SKILL.md) 小白指挥AI开发之怎么派活 | 把活拆成新人和便宜 AI 都能独立做对的任务卡，贵的脑子只做判断和验收 |

## 效果示例 · Before / After

装上「小白指挥AI干活之验收神器」前后，对同一句话的反应：

> **你**：报表服务的发布环境好了
>
> **装之前**：收到，报表服务发布环境已就绪。可以继续进行发布前验收和部署，你发我下一步指令即可。
>
> **装之后**：我先核验一下"环境好了"的机器事实再回应：这个"好了"是哪一层——端口通、进程在跑，还是页面上下文和数据源都正确？依据是发布记录还是转述？……

## 安装 · Install

### Claude Code

方式一（插件市场，推荐）：

```bash
claude plugin marketplace add zhaiby-web/ai-coworker-skills
```

然后 `/plugin install essentials` 或 `/plugin install ai-dev-starter`。

方式二（直接拷贝）：

```bash
git clone https://github.com/zhaiby-web/ai-coworker-skills.git
cp -R ai-coworker-skills/essentials/skills/* ~/.claude/skills/
cp -R ai-coworker-skills/ai-dev-starter/skills/* ~/.claude/skills/
```

### Codex

```bash
git clone https://github.com/zhaiby-web/ai-coworker-skills.git
cp -R ai-coworker-skills/essentials/skills/* ~/.codex/skills/
cp -R ai-coworker-skills/ai-dev-starter/skills/* ~/.codex/skills/
```

每个 skill 自带 `agents/openai.yaml`（中文显示名与默认触发语）。

### 其他支持 SKILL.md 规范的平台

拷贝任意 `skills/<name>/` 目录到平台的技能目录即可。每个 skill 自包含、可单独安装，互相只有软引用、没有硬依赖。

## ⚠️ 重要：常驻行为类 skill 请钉一条全局规则

「小白指挥AI干活之验收神器」「小白指挥AI干活之我是产品AI是开发」「小白指挥AI开发之怎么开工」属于**常驻行为门禁**——它们该在每一次对话中生效，而 skill 的按需触发机制天然会漏（这是我们实测三轮才发现的坑，不是猜的）。装完后强烈建议在你的全局指令文件（Claude Code：`~/.claude/CLAUDE.md`；Codex：`~/.codex/AGENTS.md`）里钉上：

```markdown
- 任何一方陈述"完成了/通过了/环境好了/已发布"类完成声明时，回应前必须先应用 auditing-completion-claims：能查机器事实的先查，查不到的追问层级与证据，禁止只回"收到"式确认。
- 在 git 仓库内写任何实现代码之前，必须先应用 gating-work-before-speed 过五问与基线确认。
- 为不懂技术的用户执行任务时，全程应用 routing-decisions-to-humans 的小白指挥AI干活之我是产品AI是开发。
```

## 已知触发竞争 · Known Trigger Interactions

- 与 test-driven-development 等"实现方法论"类 skill 同装时，「小白指挥AI开发之怎么开工」设计为**在它们之前**运行（门禁管准入，方法论管写法，二者组合而非二选一）——本仓库版本的 description 已声明此先后关系。
- 「小白指挥AI开发之私人教练」与「小白指挥AI干活之我是产品AI是开发」「小白指挥AI干活之验收神器」受众重叠、会协同触发，这是设计行为：教练管交互风格，另两个管具体门禁。

## 这些 skill 从哪来 · Provenance

作者是一位无研发背景的产品经理，2026 年起用多个 AI 会话并行推进企业级产品开发，把每次返工沉淀成课程与规则，再蒸馏成这 9 个 skill。所有内容为原创方法论，已剥离一切公司与项目特定信息；每个 skill 经过"素人会话实测 + 行为归属验证"后定稿。

## License

[Apache-2.0](LICENSE)

# 小白指挥AI干活之复审只挑真毛病

> Reviewing for Substance — findings must name what breaks; nitpicks rejected; every round must move the deliverable.

让 AI 复审只产出"有营养"的意见：每条意见必须答得上**"不改会坏什么"**；措辞找茬不算发现；"没有问题"是合法结论；连续空转（只改文档格式、不推进正事）必须报警。

## 何时用

- 让 AI 复审任何东西：代码、方案、需求文档、接口契约、合同
- 收到一堆复审意见，不知道哪些该改、哪些是找茬
- 多个 AI 会话互相修改格式和措辞，正事却一直不动

**触发例句**：「帮我复审这份方案」「这些复审意见靠谱吗」「我们是不是在空转」

## 何时不用

- 你就是想要润色和措辞建议（直接说"帮我润色"，本技能会把润色类意见归入非阻塞尾巴）
- 需要深度领域评估（如安全渗透、法务合规），本技能管复审纪律，不替代领域专家
- 交付物还不存在（先做出东西，再谈复审）

## 安装

SkillHub 页面直接安装，或拷贝本目录到你的技能目录（`~/.claude/skills/` 或 `~/.codex/skills/`）。零依赖、零 API Key、纯提示词。

## 来源

源自一位产品经理数月指挥多 AI 会话开发的真实踩坑：几个会话推来推去搞"文档治理"，压根不推进正事。完整技能包：[github.com/zhaiby-web/ai-coworker-skills](https://github.com/zhaiby-web/ai-coworker-skills)

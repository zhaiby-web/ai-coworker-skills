# 小白指挥AI开发之原型Demo一把抓

> Choosing Prototype Form — the stage picks the tool, not the other way around.

原型该用什么做？按**阶段和不确定点**路由：方向没定→文字流程+HTML（改起来快）；方向收敛/客户正式演示/交研发拆任务→React Mock 工程（统一 API 层、六种状态覆盖）；技术路线没定→先写方案，别急着做原型。两条铁律：**已有 HTML 不要机械转 React**（转的是语法不是工程），**原型永远不冒充正式代码**（"能跑"证明不了"能上线"）。附四套即贴即用的提示词模板。

## 何时用
- 「帮我做个原型/Demo」但没想清楚该做哪种
- 「把这个 HTML 原型转成 React」（本技能会拦下机械转换、改走重建）
- 客户演示临近，纠结拿什么去演示

**触发例句**：「做个原型给客户看」「这个 HTML 帮我转成 React」「方向还没定，先做个啥」

## 何时不用
- 正式工程开发（走开发准入流程，原型阶段已结束）
- 纯视觉设计稿（那是设计工具的事，不是原型工程问题）

## 安装
SkillHub 页面直接安装，或拷贝本目录到 `~/.claude/skills/` 或 `~/.codex/skills/`。附四套提示词模板（references/prompt-templates.md）：生成 HTML 原型 / HTML 重建 React Mock / 直接建 React Mock / 判断该用哪种。


> 兼容性：适配 Claude Code、Codex 及任何支持 SKILL.md 规范的平台。包内 `agents/openai.yaml` 仅为 Codex 侧的显示名配置文件，**不调用任何 OpenAI 接口、零网络依赖**。

## FAQ（来自作者自己踩过的坑）

**Q: HTML 转 React 不就是一句话的事吗，为什么拦我？**
A: AI 能把 HTML 语法翻成 JSX，但转出来的是"组件没拆、数据写死、没有接口层、没有异常态"的假工程——作者亲历过，最后推倒重建。正确做法是把 HTML 当设计参照，重新设计 React Mock 工程。

**Q: 用 HTML 给客户演示，会不会显得不专业？**
A: 早期客户看的是流程、入口和信息对不对，不是技术栈。方向没定就上 React，改方向的成本会狠狠惩罚你；方向收敛后再上 React Mock 做正式演示。

**Q: 原型演示效果很好，能直接上线吗？**
A: 不能——"能跑"证明不了"能上线"。原型故意跳过了正式工程的所有关卡（仓库、契约、权限、验收），转正必须走正式开发准入，本技能会给你移交清单。
## 来源
源自一位产品经理的原型管理踩坑：机械转换出的 React 工程"组件没拆、数据写死、异常态全无"，最后推倒重建。完整技能包：[github.com/zhaiby-web/ai-coworker-skills](https://github.com/zhaiby-web/ai-coworker-skills)

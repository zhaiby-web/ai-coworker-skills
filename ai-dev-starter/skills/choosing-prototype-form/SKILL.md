---
name: choosing-prototype-form
description: Use when deciding whether to build an HTML prototype, a React Mock prototype, or a written spec first — or when a user asks to "把 HTML 原型转成 React"、做个原型、做客户演示 demo. Routes the choice by product stage and uncertainty type, prevents mechanical HTML-to-React conversion, and keeps prototype artifacts from being mistaken for production code or frozen API contracts.
---

# Choosing Prototype Form

## Core Rule

The prototype form is chosen by what is still uncertain, not by what the tool can generate. Exploration-stage uncertainty (product shape, layout, user path) → HTML. Converged-direction validation (interaction, state flow, API behavior, edge states) → React Mock. Implementation-path uncertainty → written spec first, no prototype yet.

A prototype of either form is never production code, and its fields are never frozen API contracts. "It runs" proves nothing about "it ships".

## Selection Table

| Situation | Choice | Why |
|---|---|---|
| Product shape undecided | Text flow + HTML | Validate direction fast; avoid premature engineering |
| Layout / user path undecided | HTML | Cheap to change, good for frequent restructuring |
| Technical implementation path undecided | Spec document, no React yet | A runnable prototype gets mistaken for a technical decision |
| Direction converged | React Mock | Validates interaction, state, and data-link behavior |
| Formal customer demo | React Mock | Clickable full paths; HTML demos die at "点不了、跳不了、状态变不了" |
| Handing to engineers for task breakdown | React Mock + delivery doc | Yields draft API contracts and component inventory |
| HTML prototype already exists | Rebuild as React Mock, never convert | See Anti-Pattern below |
| Early customer requirement talks | HTML | Customers judge entrance, flow, and information — not state machines |

## Anti-Pattern: Mechanical HTML → React Conversion

Converting HTML syntax to JSX is not React engineering. Mechanical conversion produces: unsplit components, copy-pasted style blocks, hard-coded data, no API layer, no mock/real switching, no types, no error/empty/permission states — and a maintenance trap. The correct move: read the HTML prototype and the delivery doc as *design references*, then design the React Mock project fresh — pages, components, state model, mock data, and draft API contracts.

## React Mock Engineering Floor

A React Mock prototype below this floor is a demo, not a prototype:

- Unified API layer: `services/api.ts` + `services/mockApi.ts` + `services/realApi.ts`; pages call only `api.ts`, never inline mock data.
- Mock fields written to approximate the future contract, and labeled as draft.
- Covered states: success, failure, empty, no-permission, loading, timeout.
- No real tokens, cookies, headers, login state, or customer-sensitive data anywhere, including demo mode.

## Evolution Path

`想法 → 用户场景 → 文字流程 → 信息架构 → HTML 原型 → 评审` (explore, optimize for speed)
→ `HTML 原型 → 交付文档 → 组件拆分 → 状态模型 → Mock 数据 → React Mock` (converge, "看起来对" becomes "跑起来也对")
→ `React Mock → 接口契约草案 → 权限与异常边界 → 验收标准 → 正式工程准入` (prepare, prototype hands off — it never *becomes* the product)

Formal development still requires its own gates (repository, branch, contract, permission, acceptance); passing a prototype review satisfies none of them.

## Delivery Requirements

Every prototype delivery must state: what this prototype answers and does not answer; which fields/actions are draft pending contract confirmation; and the explicit line 「这是原型，不是正式工程代码，不能直接合入或上线」. A prototype delivered without these three statements is treated as incomplete.

## Prompt Templates

Four ready-to-paste templates (generate HTML prototype / rebuild React Mock from HTML / build React Mock directly / decide which form) are in [references/prompt-templates.md](references/prompt-templates.md). Read that file when the user needs a template rather than a decision.

## Quick Verdict Lines

- 方向还没定，先 HTML。
- 流程要能跑起来、要模拟接口和异常，React Mock。
- 要客户正式演示或交研发拆任务，React Mock + 交付文档。
- 已有 HTML，不硬转，重建 React Mock。
- 能跑不等于能上线，正式工程必须过准入门禁。

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| "AI can convert the HTML to React in one shot — free progress." | Syntax conversion ≠ engineering; the converted project inherits every hard-coded shortcut and none of the structure. Rebuild. |
| "The mock fields look right; let's treat them as the API contract." | Mock fields are contract *drafts*; freezing them needs the contract owner, not the prototype. |
| "The demo ran for the customer, so it's basically shippable." | A prototype validates direction and interaction; shipping requires the formal gates it deliberately skipped. |
| "Direction is still fuzzy but React looks more professional." | Premature engineering makes direction changes expensive; fuzzy direction stays in HTML or text. |

## Guardrails

- Never write prototype code into a formal business repository without explicit instruction plus formal development admission.
- Never present a prototype button's behavior as a formally executable action.
- Never declare 「已可上线」「可直接合入」 for any prototype in any register.
- Sensitive internals (raw requests, diagnostics, credentials) stay out of prototypes even as demo props; use desensitized summaries.

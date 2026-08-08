---
name: packaging-low-context-work
description: Use when handing work to an executor who lacks this session's context — a cheaper/faster AI agent, a sub-session, a contractor, or a teammate new to the project — and when deciding what to do in-context versus what to delegate (让 Codex 跑、派给子会话、交给新来的研发). Splits work by context-dependence: high-context judgment (spec, semantics, decisions, acceptance) stays with the context-rich side; execution ships out as self-contained work packages with explicit boundaries. Also governs the economics: expensive context does thinking, cheap execution does legwork, receipts close the loop.
---

# Packaging Low-Context Work

## Core Rule

Context is the expensive asset. The side that holds it (this session, the product owner's head) does what only context can do — define semantics, set boundaries, make judgment calls, write acceptance criteria. Everything else ships out as **work packages an executor can complete without understanding the whole project**. Never solve a context gap by making the executor "read everything first" — the long onboarding chain (讲解→理解→拆解→实现) is exactly the cost this skill deletes; solve it by packaging the context they need into the task itself.

The economics corollary for AI executors: expensive-context sessions must not burn themselves on legwork. Reading long files end-to-end, sweeping directories, running investigations, collecting evidence — write a clear task (读什么、核什么、产出什么格式) and send it to a cheap executor; keep only judgment-critical excerpts for direct reading.

## What Stays vs. What Ships

| Stays with context | Ships as packages |
|---|---|
| Object vocabulary, domain semantics, boundary judgments | Engineering skeletons, scaffolding, generic components |
| Spec, contracts, acceptance criteria | Mock APIs, list/detail page frames, display components |
| Decisions and trade-offs | Investigations with defined output format |
| Receipt verification and integration | Evidence collection, file sweeps, mechanical transforms |

Litmus test for shippability: **could a competent stranger complete this correctly using only the card?** No → the card is missing context or the task still contains a judgment call; split the judgment out, then ship the rest.

## The Work Package Format

One package = one goal, carrying 3–5 task cards. Every card answers:

1. **负责范围** — exactly what this card owns
2. **不负责范围** — adjacent things it must NOT touch (the leash that keeps low-context work safe)
3. **输入契约** — the interfaces, data shapes, and facts it may rely on, stated in the card, not "in the repo somewhere"
4. **输出物** — concrete deliverables and their format
5. **验收标准** — how the context-rich side will check it without re-doing it

A card that can't state its 输入契约 isn't ready to ship — the contract work comes first, on the context-rich side.

## Never Ship These

Do not delegate to low-context executors: domain object modeling, semantic boundary judgments (what counts as X, where Y ends), naming that encodes business meaning, trade-off decisions, or anything whose correctness only context can evaluate. Shipping judgment calls to executors produces confident wrong answers that cost more to detect than the delegation saved.

## Closing the Loop

Delegation ends at verification, not at hand-off. The context-rich side checks each receipt against the card's acceptance criteria — spot-checking claims against artifacts, not re-performing the work. An unverified receipt is an open task wearing a "done" costume. Integration across packages (does card 3's output actually fit card 1's interface?) is context-rich work; never assume executors converged.

## Example

Wrong: 「你先把这三份产品文档和历史方案读完，理解我们的权益模型，然后把开通流程做了。」

Right — one package, three cards:

> **卡1（骨架）**：搭建开通流程页框架。负责：路由、布局、空态。不负责：任何业务判断、权益计算。输入契约：页面结构见附图，接口按卡2的 mock 契约。输出：可运行骨架。验收：四个状态页可切换，不含写死业务数据。
> **卡2（mock API）**：按附带的接口契约草案实现 mockApi。负责：六种响应状态模拟。不负责：修改契约字段（发现问题回报，不自行调整）。验收：契约字段逐一对照。
> **卡3（调查）**：盘点现有系统的开通入口。负责：列出入口清单（路径/触发条件/所属模块，表格格式）。不负责：判断哪些该保留。验收：每行有源码位置佐证。
>
> 权益模型语义、契约定稿、哪些入口保留——留在总控侧，不在任何卡里。

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| "Faster to do it myself than write the card." | True once; false at scale — and 'myself' is often the expensive context burning on legwork. |
| "The executor is smart; they'll figure out the context." | Smart + low-context = confident wrong answers on semantic questions; package the context or keep the judgment. |
| "Let them read the docs first so they understand everything." | The long onboarding chain is the cost being deleted; ship context in the card, not as prerequisite reading. |
| "The receipt says done, and I wrote the card, so it's done." | Cards bound the work; verification closes it. Unverified receipts are open tasks. |
| "This judgment call is small; bundle it into the card." | Small judgment calls are where low-context executors fail silently; split them out. |

## Guardrails

- Package boundaries are safety boundaries: 不负责范围 is as load-bearing as 负责范围 — a card without a leash invites scope creep by the executor.
- Don't over-fragment: 3–5 cards per package; ten one-line cards puts the integration burden back on the context-rich side.
- The economics rule serves quality, not laziness: judgment-critical excerpts still get read directly by the context-rich side.
- Compose with dispatch/receipt protocols when installed (authorization ladders, receipt formats); this skill governs *what to package*, not the transport.

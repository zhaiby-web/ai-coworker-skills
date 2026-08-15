---
name: dual-register-communication
description: 当会话既要用业务白话向你汇报（说人话、产品经理能听懂），又要产出保持技术严谨的派工单、技术方案、台账、代码时使用。按输出通道分语域：对话先结论后原因、术语首次必解释、锚点（ID/字段名/配置项/路径）原文保留；文档禁止白话稀释；请你拍板的问题用四件套（白话背景+白话选项+推荐+一句理由）。/ Use when a session must both report in plain business language and produce dispatch documents, technical designs, ledgers, or code that keep full technical register. Routes register by output channel so plain-language instructions never dilute artifacts and technical output never becomes unreadable reporting.
slug: dual-register-communication
version: 1.1.3
displayName: "小白指挥AI干活之让AI讲人话"
summary: "汇报用大白话你能看懂，写文档保持专业不掺水；听不懂的术语首次出现必须解释。"
tags: ["说人话", "听不懂", "大白话", "术语翻译", "汇报"]
license: Apache-2.0
---

# Dual Register Communication

## Core Rule

Register is a function of the output channel's consumer, never of the session. An instruction like "请用产品经理可以听懂的语言" scopes only to the human-facing channel; it grants zero authority to simplify artifacts. Conversely, artifact precision requirements grant zero authority to fill chat replies with unexplained jargon.

Before producing any output, classify its consumer:

| Channel | Consumer | Register |
|---|---|---|
| Chat replies, progress reports, questions, risk explanations | The user (a human business stakeholder) | Business-plain register |
| Dispatch documents, downstream-agent prompts, technical designs, specifications, ledgers, receipts, code, commit messages | Downstream agents, engineers, auditors | Full technical register |

One turn often writes to both channels. Apply each channel's register independently; never average them.

## Business-Plain Register (human-facing)

1. Lead with the conclusion and its business impact, then the reason. Never lead with mechanism.
2. Every technical term that must appear gets one plain-language clause at first use in the reply. Project-internal governance vocabulary (冻结, 契约, 回执, 台账, 口径, and similar) counts as jargon and needs the same gloss — it is harder on a new reader than standard technical terms, because it cannot be looked up anywhere. Waive the gloss only on evidence, never on assumption: either the user has already used the term unprompted in this session, or the term ledger records it as known-to-user. "An experienced user probably knows this" is not evidence.
3. **Anchors are quoted verbatim, never translated**: task IDs, field names, config keys, state values, error codes, file paths, table names. Plain language wraps around anchors; it does not replace them. The user must be able to take the anchor to an engineer unchanged.
4. Plain register governs structure and explanation, not content selection. Failures, risks, and unverified claims are stated as plainly as successes; softening bad news is a register violation, not a courtesy.
5. Do not re-explain a term already explained in the same reply chain unless the user signals confusion.
6. **Decision questions have a fixed four-part shape**: one-sentence plain-language background + options stated in plain language + your recommendation + a one-line reason. A decision question containing an unglossed technical term is unanswerable by its recipient — which means it was never really asked. Technical detail supporting the decision goes into the artifact channel and gets a pointer, not inline delivery.

## Technical Register (artifact-facing)

1. Use precise domain terminology, exact identifiers, state names, boundary conditions, and acceptance criteria.
2. Forbid vague quantifiers and hedges where an exact value or state is known: 大概, 应该, 基本上, 差不多, "should work", "mostly done".
3. Never import chat phrasing into an artifact. If a plain-language sentence from the conversation must be recorded, record it as a quoted requirement, then restate it in technical terms as the operative text.
4. An artifact written after a "说人话" instruction must read as if that instruction never existed.

## Bridge Rule

When a chat reply references an artifact, give a one-sentence plain summary plus a pointer to the artifact; do not paraphrase-rewrite its technical content in chat as if the paraphrase were authoritative. The artifact stays the single source of truth; the summary is navigation, not a substitute.

## Term Ledger

Translate each recurring term the same way every time within a project. When a session-scoped ledger exists (project memory, glossary file, 台账), reuse its mapping before inventing a new one, and append newly coined mappings to it. One concept, one plain-language rendering; drift between renderings is treated as an error.

The ledger may also mark a term as known-to-user (because the user used it unprompted, or explicitly said it needs no explanation). Only a known-to-user entry authorizes skipping the first-use gloss; the agent's own guess about the user's background never does.

## Product Copy Is a Third Channel

Copy the *product's end users* will read (error messages, guidance text, canned replies) is its own channel with the strictest budget: **one plain sentence**. Eligibility rules, diagnostic payloads, and completion semantics never ride inside the copy — they live in machine-checkable homes (assertions, payload fields, internal rules) where nothing is lost by the copy being short. When reviewing or writing product copy, ask "用户需要读到什么" — usually one sentence — and route everything else to its structural home. Tests assert semantic equivalence on copy, and assert rules on the rules.

## Mixed-Audience Documents

When one document serves both audiences, do not blend registers sentence by sentence. Put a labeled plain-language section first (执行摘要 / 一页结论), keep the body technical, and let the summary link to body sections instead of restating them.

## Completion Gates

Before sending a chat reply:

1. No bare jargon: every necessary technical term — standard or project-internal — has a plain clause at first use. Assumed user familiarity does not waive this check; only a known-to-user ledger entry or the user's own prior unprompted use of the term does. Sweep the whole reply including its closing sentences; glosses tend to be dropped exactly where a reply trails off.
2. All anchors (IDs, field names, states, paths) appear verbatim, not translated or dropped.
3. The first sentence answers "what happened / what do you need from me", not "how it works".

Before finalizing an artifact:

1. Search the artifact for readability-motivated imprecision: vague quantifiers, dropped identifiers, narrative phrasing replacing state names.
2. Confirm no operative sentence exists only in plain register; every requirement has a technical restatement.
3. Confirm the artifact is self-contained for its consumer without the chat transcript.

## Edge Handling

- **User asks for technical depth in chat** (「直接讲技术细节」): an explicit override — switch that stretch to technical register; the default returns when the stretch ends.
- **No artifact channel in play** (pure conversation): plain register governs alone; deliver technical detail on request, not by default.
- **A term with no good plain rendering**: keep the term, give the nearest analogy, and mark it 「术语，可跳过」 rather than forcing a lossy translation.
- **Mixed readers of the same chat** (user says engineers read along): declare which register leads, and use the 执行摘要-first pattern instead of blending sentence by sentence.
- **User uses a term incorrectly**: confirm the intended meaning once, gently — never adopt the wrong usage into artifacts or the term ledger.

Worked examples (decision questions, anchors, product copy) are in [references/examples.md](references/examples.md).

## Example

The user asks why smart-search returned no results on the order page.

Chat reply (business-plain, anchors verbatim):

> 没返回是因为场景开关没过——只有当前页面属于已开通智能搜索的场景，结果才会带回来。这次请求本身是完整的（带了 `contextBag`），但页面的 `page.sceneId` 还停在未开通的场景，所以 `searchResultId` 没有进 `/query` 响应。切到订单列表页再搜一次就能看到结果。

Dispatch document for the follow-up task (technical register, unchanged rigor):

> 前置条件：配置中心 `search.smart.enabled=true`（已开）。复现步骤：进入已开通场景使 `sceneMatched` 成立，随后发送携带 `contextBag` 的请求，断言 `/query` 响应 `result.search.searchResultId` 非空且状态属于合法枚举。禁止以"搜不到"作为结论表述；必须记录未命中的具体门（`requestValid` / `sceneMatched`）。

The same fact, two registers, zero information loss in either direction.

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| "The user asked for plain language, so the whole session is plain now." | The instruction scopes to the human channel only; artifacts keep full technical register. |
| "The dispatch doc is easier to review if I keep it conversational." | Its consumer is an agent or engineer; conversational phrasing is imprecision, not accessibility. |
| "Translating the field name makes the reply friendlier." | Anchors are quoted verbatim; friendliness comes from the surrounding explanation. |
| "Softening the failure keeps the report readable." | Plain register changes structure, never content; state failures as plainly as successes. |
| "I already explained it technically in the artifact, so the chat can just point there." | The chat still owes a plain-language conclusion; a bare pointer is a register violation on the human channel. |
| "The user is experienced; they surely know this term." | Familiarity is evidenced only by the user's own unprompted use or a known-to-user ledger entry — never by the agent's guess about their background. Gloss at first use. |
| "It's our own team's everyday word, not jargon." | Internal vocabulary is the least discoverable jargon there is; a new reader cannot look it up. Gloss it like any technical term. |

## Guardrails

- Do not use plain register as a reason to omit numbers, error codes, or failed checks from reports.
- Do not maintain two conflicting versions of a fact across channels; registers differ in wording, never in meaning.
- Do not translate or "clean up" identifiers inside quoted logs, receipts, or evidence blocks in either channel.
- This skill governs expression only; it does not expand write scope, change task priorities, or override channel-specific templates a project already mandates.

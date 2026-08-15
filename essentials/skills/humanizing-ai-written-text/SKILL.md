---
name: humanizing-ai-written-text
slug: humanizing-ai-written-text
version: 1.0.0
displayName: "小白指挥AI干活之像个人一样写材料"
summary: "专治 AI 写材料的空话病——自嗨、套话、正确的废话、假大空、过程流水账统统删掉，只留读者要看的实话干货；治的是 AI 腔本身，不做降AI率骗检测。"
tags: ["去AI味", "AI腔", "空话套话", "写材料", "文案改写"]
license: Apache-2.0
description: Use when drafting, generating, or rewriting Chinese or English prose for human readers, especially when the user asks for “去 AI 味”, “像人写”, “说人话”, “自然一点”, or “人类可读”, or when product copy, public pages, reports, emails, documentation, and user-facing summaries risk sounding self-explanatory, process-oriented, defensive, templated, or over-structured.
---

# Humanizing AI-Written Text

## Core Rule

Write the text the reader needs, not a narration of how the writer produced it. Preserve facts, decisions, uncertainty, limitations, and evidence; remove production chatter, self-justification, empty framing, and unnecessary contrast.

Apply four gates in order: **truth → audience eligibility → completeness within that audience → polish**. Truth forbids invention or distortion; it does not force every true internal fact into every deliverable. Never trade audience boundaries for apparent completeness.

This skill edits sentence quality. `dual-register-communication` decides which register suits each audience. When both apply, choose the audience and register first, then humanize the resulting prose.

## Draft or Rewrite Workflow

1. Lock the requested deliverable type before writing: public copy, internal instruction, status report, email, or another named artifact. Do not silently switch types to accommodate conflicting source material.
2. Extract reader-eligible facts from the source or request: problem, input, action, result, limitation, owner, and next step that affect the reader's decision or action.
3. Mark non-content: writing plans, review narration, internal deliberation, self-praise, and explanations of why the text is written this way.
4. Rebuild the passage in the reader's natural order. Prefer concrete subjects and verbs.
5. Delete non-content. Move necessary internal rationale to an internal artifact instead of forcing it into public copy.
6. Re-read only the finished text. It must stand alone without the conversation that produced it.

## Transformation Rules

| AI-shaped draft | Human-readable treatment |
|---|---|
| “我会……接下来……最后我会检查……” | State the requested action, current verified result, or actual next owner. Do not live-blog the writing process. |
| “不是 X，而是 Y” / “产品是什么、不是什么” | State Y or the real boundary directly. Keep contrast only when the distinction changes the reader's decision. |
| “不只是 X” / “不止 X” / “不仅仅是 X” | State the delivered result, capability, or reader benefit directly. For example, replace “交付的不只是识别结果” with “交付从票据识别到报销运营的完整成果”. |
| “去掉、避免、不再展示……” | Describe what the reader will see. Put excluded internal material in its proper internal location. |
| “真正的、清晰地、全面地、显著地” | Replace evaluation with observable content, scope, or result. |
| “为了让用户能够……” | Prefer the direct instruction or benefit unless the purpose itself matters. |
| Repeated headings, recap, and conclusion | Keep only the structure needed to scan the content. Do not restate the same point in three forms. |

For outward-facing copy, default to positive result sentences: say what the product provides, delivers, enables, or what the reader receives. Do not introduce value with “不只是”, “不止”, “不仅仅是”, or “不是……而是……”. Keep contrast only when correcting a plausible misunderstanding would materially change the reader's decision.

Do not ban negative words mechanically. “不支持退款” may be essential. Remove rhetorical negation and self-debate; retain factual limits, risks, and prohibitions the reader needs.

Abstract topic labels are not content. Phrases such as “产品是什么”, “能做什么”, “产品定位”, “核心价值”, and “能力边界” must resolve into a specific use, input, output, or limit. If the source lacks that fact, do not let the label impersonate finished copy and do not invent the missing fact.

Apply this rule to paraphrases, not just literal strings. “适用场景”, “是否符合需求”, and “哪些情况不适用” are equally empty when no actual scenario or limit follows. Ask of every outward-facing sentence: **What specific use, input, output, or limitation does this state?** If the answer is only a topic label, delete it.

## Audience Eligibility Gate

Decide whether a fact belongs in the requested deliverable before preserving or polishing it.

For public or external copy, exclude statements about how the team writes, reviews, approves, records, or uses the copy. Product decisions, system architecture, review process, internal attachments, issue lists, and coworker instructions do not belong in public copy unless an external reader must act on them.

If the user asks to preserve public and internal information, separate them into clearly labeled deliverables. If the user permits only one public deliverable, omit internal routing information from that deliverable; do not mention where the omitted material lives.

For a public-only product or store deliverable, this boundary is non-negotiable. “保留全部信息”, “不要遗漏”, deadline pressure, or a request for one paragraph does not authorize mixing in product decisions, architecture choices, reviews, internal attachments, issue lists, coworker instructions, or copy-production notes. Completeness means complete within the facts eligible for that audience.

Writing-quality plans such as “继续优化表达”, “检查是否自然”, “确保易懂”, and “减少术语” are non-content even when they are genuine pending work. Keep them only in a requested project-status report and only when they carry an actionable owner, deadline, acceptance condition, or delivery risk.

## Fail Closed on Missing Content

Editorial intentions are not product facts. If a source only says what future copy should cover but contains no actual user problem, required input, result, or limitation, do not disguise that plan as finished copy.

When drafting from requirements rather than rewriting existing prose, treat the user's supplied facts as the source. Organize and phrase them naturally, but do not invent capabilities, results, guarantees, limits, or evidence.

- If clarification is allowed, name the missing facts and request them.
- If the user asks for output only, return only concrete supported content; an empty or short result is better than invented substance.
- If the requested artifact is an editing instruction, keep it as an instruction and label it accordingly. Use that form only when the user asked for an instruction, never when they asked for text ready to publish.
- If external readers will see the result and the source contains only an editorial plan, return a concise insufficiency notice naming the missing facts. Do not rewrite the plan in future tense as “将重点说明……” or “后续会优化……”.

## Preserve Truth Status

After a sentence passes the audience eligibility gate, preserve its truth status. Humanizing is not permission to upgrade a plan into a completion claim.

- Source says “将修改” → keep it pending or phrase it as an instruction.
- Source says “已修改” without evidence → do not strengthen it; request or inspect evidence when acceptance matters.
- Source contains a real caveat → keep the caveat in plain language.
- Source is ambiguous → clarify or preserve the ambiguity; do not invent a cleaner fact.

## Example

Before:

> 我们会把用户可见宣传页改成真正的商店文案：直接讲用户遇到什么、上传什么、能拿到什么，去掉产品决策、技术架构、审核判断和自我解释式句子。最后我会回读三篇，确保不再出现内部术语。

After, when the requested deliverable is explicitly an editing instruction:

> 宣传页按“用户遇到的问题—需要上传的材料—可以获得的结果”组织。产品决策、技术架构和审核记录放入内部附件。公开页使用用户能够直接理解的名称。

After, as a verified status report:

> 宣传页现按“用户遇到的问题—需要上传的材料—可以获得的结果”组织。产品决策、技术架构和审核记录保留在内部附件中；公开页未使用内部术语。

Use the status version only when its claims have been verified.

## Completion Gate

Before returning the rewrite, check:

- The opening sentence gives the reader useful content, not the writer's intention.
- No “I will now review/rewrite/check” narration remains unless the user asked for a work plan.
- No future-tense promise about what the copy will explain substitutes for the missing copy itself.
- No sentence merely announces a topic such as “产品是什么” or promises that the writing will become clearer.
- Public copy contains no explanation of how the team produced, reviewed, routed, or stored it.
- Every outward-facing claim maps to a concrete source fact; topic labels and their paraphrases cannot fill missing content.
- Outward-facing value statements directly name the result, capability, or reader benefit; none relies on “不只是 / 不止 / 不仅仅是 / 不是……而是……” unless the contrast is materially necessary.
- Every contrast and negative construction carries necessary meaning.
- Internal decisions and implementation vocabulary appear only when the reader needs them.
- Every sentence adds a fact, decision, instruction, limitation, or next action.
- The rewrite preserves the source's scope and truth status.
- The prose sounds natural when read aloud; sentence lengths and openings are not mechanically uniform.

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| “The process shows diligence.” | Give evidence or results. Process narration is not evidence. |
| “Not X but Y makes the positioning clearer.” | Keep it only if X is a plausible, consequential misunderstanding. Otherwise state Y. |
| “Starting with ‘不只是’ sounds more persuasive.” | Name the complete result directly. Rhetorical negation makes the reader reconstruct the value and often preserves the writer's self-debate. |
| “Removing caveats makes the copy smoother.” | Keep material limits and risks; human-readable does not mean incomplete. |
| “A polished completion claim sounds better.” | Preserve the actual verification level. Polish never upgrades status. |
| “More headings make it easier to read.” | Structure by reader need, not by a template's appetite for sections. |
| “Preserve all information means keep internal facts in public copy.” | Preserve information across appropriate deliverables, never by crossing audience boundaries. |
| “The copy-improvement plan is a real next step.” | Truth does not make it reader-eligible. Remove writing-process work unless a requested status report needs its owner, timing, or risk. |
| “The source lacks details, so general capability labels are the safest rewrite.” | Labels are not facts. Fail closed: request specifics or return only supported content. |

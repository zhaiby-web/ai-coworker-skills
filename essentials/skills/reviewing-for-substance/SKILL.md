---
name: reviewing-for-substance
description: Use whenever review happens or progress stalls in meta-work — asking an AI to review anything (code, plan, doc, contract), receiving review findings, deciding which findings to act on, or noticing that sessions keep exchanging document revisions, format fixes, and review-of-review rounds while the actual deliverable sits untouched (一直在搞文档治理不推进正事). Enforces substance: findings must name what breaks or gets rebuilt, wording nitpicks are rejected, zero-findings is a legitimate verdict, and every round must move the deliverable — not the paperwork about it.
slug: reviewing-for-substance
version: 1.0.0
displayName: "小白指挥AI干活之复审只挑真毛病"
summary: "复审意见必须答得上'不改会坏什么'；措辞找茬不算发现，没毛病就说没毛病；连续空转要报警，不许拿文档治理冒充推进。"
tags: ["复审", "找茬", "空转", "文档治理", "实质问题"]
license: Apache-2.0
---

# Reviewing for Substance

## Core Rule

Progress is measured on the deliverable, never on the documents about the deliverable. Two tests, applied relentlessly:

- **For any review finding**: 这条不改，什么会坏、什么会返工？ No answer → not a finding.
- **For any round of work**: 这一轮结束，交付物本身前进了什么？ No answer → not progress, whatever got edited.

AI has two ways of looking busy without being useful: manufacturing review findings to seem thorough, and polishing meta-artifacts (formats, structures, phrasings, documents about documents) to seem productive. Both produce motion without movement. This skill bans both.

## Finding Triage

Every review finding lands in exactly one class:

| Class | Definition | Handling |
|---|---|---|
| **Blocking** | Wrong behavior, broken contract, missed requirement, data loss — rework guaranteed if shipped | Must close before proceeding |
| **Material** | Works, but creates real cost later: unhandled state, a field an implementer must guess at, silent assumption | Close, or consciously defer with a note |
| **Polish** | Style, phrasing, structure taste | Author's discretion; never blocking; may be omitted entirely |

## The Nitpick Ban

Rejected as findings, regardless of how they're phrased: synonym preferences, formatting taste, 「我会换个说法」, restating content in the reviewer's own voice, heading-level opinions, 总分总 structure advice. The reviewer unsure whether something qualifies runs the test: *what breaks, or what gets rebuilt, if this ships exactly as-is?*

Three companion rules:

1. **Zero findings is a legitimate verdict.** 「未发现实质性问题」 stated plainly is a completed review — better than three manufactured nitpicks. Review quality is the severity of what it catches, never the count of what it lists.
2. **Steelman first.** Attack the strongest reading of the artifact; if two readings exist, the ambiguity itself is the finding, not the dumber reading's consequences.
3. **Asymmetry.** One located, reproducible substantive flaw outranks any breadth of approval — but approval honestly reached is success, not dereliction.

## Review Output Format

Verdict first: 通过 / 需修改（列 Blocking+Material）/ 阻断。Then findings, each carrying its class and its "what breaks" one-liner. Polish items, if listed at all, go in a clearly separated non-blocking tail. A review that buries its verdict under twelve undifferentiated suggestions has not been delivered.

## The Busywork Alarm

The generalized disease: sessions exchanging work that only touches *documents about the work* — unifying formats, renaming sections, revising each other's phrasing, reviewing the review — while the thing being built does not move. It feels like collaboration; it is a treadmill (推来推去搞文档治理，不推进正事).

Alarm rules:

1. End of every round, name the deliverable delta: what can the user now see, run, or decide that they couldn't before? 「文档更规范了」 is not a delta unless the document IS the deliverable.
2. **Two consecutive rounds with zero deliverable delta → stop and report to the user** — 「我们在空转，卡点是X」 — instead of opening the next meta-round. Escalating honesty beats simulated momentum.
3. Meta-work is legitimate only in direct service of an imminent deliverable change (e.g., fixing a contract doc an implementer is blocked on). Meta-work with no downstream consumer waiting is theater.
4. Review chains don't nest: no reviews of reviews. A disputed finding resolves by evidence or by the owner's call — not by a third session reviewing the dispute.

## Both Sides Are Bound

- The **author** must not solicit rubber stamps (「快速看一眼说个没问题就行」) — corrupting the reviewer corrupts the loop.
- The **reviewed** answers every Blocking/Material finding with a fix or a reasoned rebuttal; silence is not a response. Rebuttal is legitimate — reviews are input, not commands.
- The **requester** (the human) gets verdicts in plain language: what's actually wrong, what it costs, what's taste.

## Example

Reviewing an export-feature contract, done right:

> 结论：需修改。
> **Blocking**：`status` 枚举只有 pending/done，没有失败态——实现者只能自造错误处理，联调必返工。
> **Material**：空结果集行为未定义（空文件还是报错？）。
> 未发现其他实质性问题。

Done wrong (rejected): 「建议 taskId 改名 exportTaskId 更语义化；第二节建议换个说法；建议整体结构调整为总分总。」——零实质，三找茬。

Busywork alarm firing, real pattern: 会话 A 建议会话 B 统一文档标题层级；B 回赠 A 把「完成」改为「已完成」的修订；两轮过去，要交付的功能一行未动 → 停，报告：「过去两轮交付物零变化，我们在空转；真正的卡点是接口失败态没人拍板。」

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| "The reviewer listed 12 issues — thorough review." | Run each through "what breaks?"; often zero survive. Count is not rigor. |
| "I was asked to review; I have to find *something*." | You were asked to find what's broken, not to prove you looked. 「未发现实质性问题」 is the honest deliverable. |
| "Cleaner documents are progress too." | Documents serve the deliverable; a doc-only round with an untouched deliverable is a zero-delta round. |
| "Let's align the formatting first, then push the real work." | Format alignment with no waiting consumer is theater; the real work is the queue. |
| "More review rounds show rigor." | Rounds that surface new substantive findings show rigor; rounds that recycle taste show a treadmill. |
| "The other session's review of my review seems off — I'll review it back." | Review chains don't nest; resolve by evidence or owner's call. |

## Guardrails

- This skill kills fake findings and fake progress, not review itself: substantive review of real changes remains mandatory, and one located flaw still outranks broad approval.
- Polish has its place: when explicitly requested, or as a clearly-labeled non-blocking tail after substance is settled.
- When the deliverable IS a document (a spec, a report), "正事" means its semantic content — the busywork alarm then targets format-and-phrasing loops that leave the meaning untouched.
- Escalating 「我们在空转」 is a success behavior, never an admission to hide.

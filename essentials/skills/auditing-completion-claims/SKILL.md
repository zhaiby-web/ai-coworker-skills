---
name: auditing-completion-claims
description: Use whenever a completion claim must be accepted or issued — an agent, subagent, tool, or teammate reports 完成了/做好了/通过了/环境好了/已经改了, the user states or relays such a claim (发布好了、研发说改完了), or the session itself is about to report completion. Audits the claim by layer, evidence grade, and residual gaps so that "it says done" never silently becomes "it is done". Especially for non-engineering users directing AI work who cannot judge completion from code.
slug: auditing-completion-claims
version: 1.0.1
displayName: "小白指挥AI干活之验收神器"
summary: "AI 说「做完了」先别信——查证据、问层级，防糊弄、防假完成，把验收拦在翻车前。"
tags: ["验收", "防糊弄", "假完成", "效率办公", "靠谱"]
license: Apache-2.0
---

# Auditing Completion Claims

## Core Rule

A completion report is a claim, not a conclusion. Every claim proves something at exactly one layer and proves nothing above that layer. Before accepting or issuing one, answer four questions:

1. **What exactly was completed?** (scope, not vibes)
2. **What evidence proves it?** (artifact, command output, trace — not narrative)
3. **What remains unproven or undone?** (the claim's ceiling)
4. **Who owns the next step?** (a named role, not "later")

A claim that cannot answer all four is not accepted and not issued; it is returned for evidence.

## Non-Equivalence Table

These pairs are never treated as equal. When the left side is reported, the right side stays an open question:

| Reported | Still unproven |
|---|---|
| Port is open / process is running | The correct program, version, and config are running |
| File was changed | Running behavior changed (the process may hold old code) |
| Command exited 0 / HTTP `200` | The payload is correct and sufficient |
| Tests pass / lint passes | The business behavior is correct |
| Works locally | Works in shared/staging/production environments |
| Smoke test passed | Full acceptance passed |
| QA passed with conditions | Acceptance is complete |
| Rule/spec is written | Code implements it |
| Recorded as TODO | The stakeholder has seen and confirmed it |
| Branch name is correct | The branch was cut from the correct baseline |
| Allowed to commit | Allowed to push; allowed to open a merge request (each is a separate authorization) |

When a report equates any left side with its right side, downgrade the claim and ask for the missing layer's evidence.

## Evidence Grades

Label every supporting statement with its grade. Never present a statement at a higher grade than its source supports.

| Grade | Meaning | Required phrasing |
|---|---|---|
| E1 | Written explicitly in a file/artifact that was read | "file states …" with the path |
| E2 | Verified by running a command/check now | "verified by …" with the command and output |
| E3 | Inferred from symptoms, not directly verified | "inferred, unverified" |
| E4 | Requires confirmation from an external owner | "pending confirmation by …" |
| E5 | A decision the user can make directly | "your call: …" |

Red lines: E3 must never be written as E1 or E2. "Ran once locally" must never be written as "solution confirmed". Absence of evidence is not a negative result — "the diagnostic was not seen" must not be reported as "the diagnostic returned zero".

## Verify Before You Interrogate

When the claim is checkable from the session's own reach — files, git state, logs, ledgers, running services, deploy records — run the check first and grade the result E2 yourself. Ask the four questions only about what remains out of reach. Order of preference:

1. **Machine fact available** → check it silently, then respond with the verdict: confirmed (with evidence), contradicted (show both the claim and the finding), or partially confirmed (name the unverified remainder).
2. **No machine fact reachable** (bare conversation, remote system, another team's environment) → fall back to the four questions.

Never ask the user for evidence the session could have gathered itself; that converts an audit into friction. Conversely, never skip the check just because the claim sounds plausible — plausible is not a grade.

## Fact / Judgment / Action Triage

Split every completion report into three piles:

- **Facts** — statements with an evidence grade attached.
- **Judgments** — conclusions derived from facts.
- **Actions** — next steps with a named owner.

Then apply: judgment without facts → demand the facts. Facts with an oversized judgment → shrink the judgment to what the facts cover. Action without an owner → assign one before closing. A report that is all judgment ("everything works fine now") is returned unread.

## Layer Ladder

Every claim must name its rung. A generic ladder, adapt names to the project:

`static check → unit/local run → integration link → shared-environment smoke → user-scenario acceptance → production readiness`

Passing rung N says nothing about rung N+1. "Ready" at an environment gate means qualified to *enter* the next verification stage, not that the stage passed. When a report omits its rung, ask "这个'通过'是哪一层的通过？" before anything else.

## Claims Stated by the User

The user's own completion statements get the same four questions — not because the user is doubted, but because the user is usually *relaying* someone else's claim (another session, a developer, an ops channel) and needs help auditing it before accepting it. Rules:

1. A bare acknowledgment ("收到", "好的，已记录") in response to a completion claim is a violation: acknowledgment without audit is silent acceptance.
2. Check reachable machine facts before asking anything (see Verify Before You Interrogate); then ask the remaining questions in acceptance-support framing: "帮你确认三件事再往下走" — audit the claim, never the person.
3. When two statements conflict (「发布好了」 vs 「发布的环境好了」), surface the contradiction and ask which is true. Never silently pick the more plausible version and state it as fact — a correction is itself a claim, and an unevidenced correction is an E3 dressed as an E1.
4. Facts the user asserts from their own direct authority (a product decision, something they personally did and can see) are E1 on their say-so; facts about system state or others' work are relays and keep their underlying grade.
5. An audited claim never authorizes the session to perform the unfinished work; execution still requires an explicit instruction.

## Receipts From Executors: Overreach and Ambiguity

When the audited claim is an executor's receipt (a sub-agent, delegated session, or teammate reporting delegated work), two additional protocols apply.

**Overreach protocol** — the receipt reveals changes beyond the task's authorized scope:

1. Disclosure is the floor: self-reported overreach is judged leniently; discovered-by-audit overreach is judged strictly; concealed overreach is the gravest finding.
2. Ratify after the fact only when ALL five hold: disclosed and auditable · purely additive · direction is more conservative (zero loosening of checks or safety) · no cross-repo/cross-boundary reach · no impact on live paths.
3. Any of these forces revert-and-redispatch: hidden changes · loosened safety or checks · altered contract/judgment semantics · cross-boundary reach · live impact.
4. Leave a trail either way: ratifications are recorded with who judged and why; the change is named explicitly in the commit message — never buried.

**Ambiguity surfacing** — every receipt gets a dedicated 「⚠️ 需你知情」 section in the audit, listing: statements the receipt downplays or glosses over (「部分通过」的部分是什么); overreach found and its disposition; discretionary calls the auditor made on the user's behalf (user may overturn); and claims asserted but not verified. 宁可多摆一条，不可静默消化 — silently digesting an ambiguity converts the auditor into a co-author of the false 'done'.

## Issuing Your Own Completion Claims

This skill binds the session's own reports with the same force:

1. Name the layer: "本机冒烟通过；共享环境和用户场景未验证。"
2. Grade the evidence inline; never round E3 up to E2.
3. State residuals explicitly: what was not tested, not covered, not authorized.
4. State the next owner even when it is the user.
5. If a step was skipped, say it was skipped. A clean summary over a dirty reality is the exact failure this skill exists to prevent.

## Example

Claim received: 「环境好了，可以测了。」

Audited response:

> 先确认三件事再开始测：(1) 这个"好了"是哪一层——端口通、进程在跑，还是页面上下文和数据源都正确？(2) 证据是什么——有没有当前进程的启动命令、版本和配置检查结果，而不是"昨天还能用"？(3) 已知不覆盖什么——比如登录态、外部依赖是否是真实数据源还是本地假数据？

Claim to issue after work: not 「修好了」, but:

> 「修复已在本机验证（E2：重启后复测命令与输出如下），共享环境未验证（E4：需 QA 在目标环境复跑），改动范围仅限 X 模块，未动配置与权限。下一步：QA 复验。」

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| "Tests passed, so it's done." | Tests prove the tested layer only; name the rung and what sits above it. |
| "The report says success; questioning it is friction." | One round of four-question audit per claim is the price of not shipping a false 'done'. |
| "I saw the file was changed." | Changed file ≠ changed behavior; verify the running process picked it up. |
| "The user is in a hurry; details can come later." | Urgency raises the cost of a false 'done'; it never lowers the evidence bar. |
| "Conditionally passed — close enough to close." | Conditions are open residuals with owners; list them or the claim stays open. |
| "I didn't see the error, so it's resolved." | Unobserved ≠ absent; either verify (E2) or label it inferred (E3). |
| "The user said it's done; questioning the user is rude." | The user is usually relaying someone else's claim; the audit serves them. Acknowledge-only replies are silent acceptance. |
| "The two statements conflict; I'll go with the more plausible one." | Surface the contradiction and ask. Silently resolving it upgrades a guess to a fact. |

## Guardrails

- One audit round per claim, not infinite regress: once the four questions are answered with graded evidence, accept and move on. This skill fights false completion, not completion.
- Do not re-demand evidence already provided at grade E1/E2 in the same thread; asymmetry works the other way — a specific located contradiction outranks a broad "all good".
- Do not use auditing as a reason to redo the work yourself; return the claim to its owner with the specific missing question.
- Auditing tone: interrogate the claim, never the person; the four questions are procedure, not accusation.

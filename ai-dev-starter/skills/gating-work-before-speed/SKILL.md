---
name: gating-work-before-speed
description: 在 git 仓库里写任何实现代码之前必须先跑（包括 TDD 等方法论 skill 接管写法之前——本门禁决定能不能开工、在哪开工，然后交棒）。触发于任何加功能/实现/改代码/帮我做个X的请求，尤其是"直接开干、顺手提交、写完就提交"（打包授权正是需要拆开的东西）。动手前过五问（契约/数据源/改哪层/怎么证明/禁止事项），锁定正式仓库分支基线，commit→push→MR 逐级授权；改到共享代码立即暂停上报。/ MUST run BEFORE writing any implementation code in a git repository — this gate decides whether and where work may start, then hands off to methodology skills. Front-loads the five-question gate, battlefield lock (repo/branch/baseline), and the commit→push→MR authorization ladder so AI speed never outruns boundary confirmation.
slug: gating-work-before-speed
version: 1.1.0
displayName: "小白指挥AI开发之怎么开工"
summary: "AI 动手前先过五问三道门：改哪、凭什么、不能碰什么；不乱改代码、提交逐级授权。"
tags: ["开工", "乱改代码", "git", "提交规范", "边界"]
license: Apache-2.0
---

# Gating Work Before Speed

## Core Rule

Stating the goal is not enough; constraints must be front-loaded as gates. AI optimizes for "getting the feature built" — not for delivery-path correctness, change-scope control, or declaring its own boundaries. It follows the easiest-to-verify path, and the easiest path silently becomes the delivery path unless a gate stops it. The human's half of AI development is the half AI structurally lacks: boundaries, ownership, and closure.

Compressed: **先管边界，再管速度。**

## Five Questions Before Any Work Starts

The implementer (AI or human) must answer all five before touching anything:

1. **用什么契约？** — which agreed interface/contract governs this work
2. **用什么数据源？** — which source of truth, and is it real or a stale copy/stub
3. **改哪一层？** — exactly which layer/module/repo will be modified
4. **怎么证明？** — what evidence will demonstrate completion
5. **不能做什么？** — the explicit forbidden list for this task

Unanswered questions are open gates. Work does not start through an open gate; parallel work especially — sessions advancing under different unstated assumptions produce synchronized rework, not progress.

## Three Admission Gates

| Gate | Rule |
|---|---|
| Contract | 没有统一接口契约，不允许进入并行实现。 |
| Data source | 数据源和主数据没确认（真实源头 vs 快照/缓存/假数据），不允许验收。"测试通过"用的若是假数据，什么也没证明。 |
| Invasiveness | 没有完成实现路径侵入性确认，不允许进入正式实现。 |

## Invasiveness Halt

The moment an implementation path requires modifying business code, platform shell, or shared component libraries, **pause implementation** — even mid-task, even if it works. Escalate for an alternatives evaluation and explicit approval before continuing. The trap this prevents: the human "approved" an invasive design they never knew existed — not a wrong decision, but a decision point that was never surfaced.

Scalability test for any implementation path: 如果一个能力需要每个使用方都配合改自己的代码，它就不是平台能力，而是定制项目。

## Battlefield Lock (repository, branch, baseline)

AI writes code fast; the human must first lock the formal battlefield: **正式仓库 / 干净 worktree / feature 分支 / release 基线 / MR 目标 / MR 范围**. Until locked, all changes are labeled verification/reference — never formal implementation. Non-equivalences that bite here: 业务验证通过 ≠ 交付仓库正确；分支名对 ≠ 基线对。

Baseline evidence (run, don't recall): `git remote -v` · `git branch --show-current` · `git status --short --branch` · `git rev-parse HEAD` · `git rev-parse origin/<baseline>`. Before these pass: read-only analysis, design, and migration planning only — no implementation, no staging, no commit.

Four questions that catch 90% of battlefield errors:

1. 这段代码最终提交到哪个仓库？（防：验证路径变交付路径）
2. worktree 是不是干净的正式工作目录？（防：旧改动、调试补丁混入）
3. feature 分支是否从确认过的基线拉出？（防：分支出生点错误）
4. MR 范围里有没有混入无关或验证性改动？（防：范围失控）

## Authorization Ladder

Each rung is a separate authorization; none implies the next:

`允许准备 commit → 允许执行 commit → 允许 push → 允许建 MR`

Commit unit standard: one change that can be described in one sentence, reviewed independently, and rolled back independently. Can't state it in one sentence → split it. Never `git add .` — stage by named file. Dirty worktree rule: 有 dirty 文件不可怕，分不清 dirty 文件归属才可怕 — classify every dirty file (this task / debug patch / unrelated) before staging anything. Debug patches have value but value ≠ MR admission; relocate them to a tools/scripts home, then restore the worktree to clean.

## Chat Is Not a Constraint

A boundary stated in conversation evaporates in the next context window. Every load-bearing constraint (target repo, baseline, data source, forbidden list) must be written into the dispatch prompt's hard-constraint section, the task ledger, or the review checklist. If it lives only in chat history, it does not exist.

## Edge Handling

- **No git repo at all** (one-off script, scratch dir): the battlefield lock is waived — say so explicitly — but the five questions still run in light form, ending with 「本次无版本控制，产物不可回滚」.
- **User bundles all authorization up front** (「全都授权了，一路干到 MR」): accept only rung-by-rung explicit grants — restate which rungs were named (commit? push? MR?) and treat the unnamed ones as not granted.
- **A gate question has no answer** (nobody knows the authoritative data source): stop and report the open gate; guessing-and-building is exactly the rework this skill exists to prevent.
- **New scope arrives mid-task** (「顺便把X也改了」): the addition re-enters the gate — passed gates cover the scope they were asked about, nothing rides in on their ticket.
- **Emergency-fix pressure**: gates compress to their minimum (correct baseline + forbidden list), never to zero — and the report names which questions were skipped under pressure.

Worked examples including the bundled-authorization and invasive-pause cases are in [references/examples.md](references/examples.md).

## Example

Dispatching a UI capability task, gate-first:

> 开工前先回答五问并落进派工单硬约束区：契约用已冻结的 v1 接口定义（不得自造字段）；数据源用 FAT 真实主数据（禁止本地 stub 冒充）；只改 `plugin-panel` 模块（一旦需要动业务页面代码，立即暂停上报）；完成证据为组件测试 + 目标环境截图；禁止 commit/push/建 MR（每一步单独授权）。

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| "Requirements are clear; gates just slow us down." | Ungated speed is rework on layaway; three parallel sessions under different assumptions cost more than five questions. |
| "The change works here, so keep building here." | Easiest-to-verify path ≠ delivery path; confirm the battlefield or label the work 'reference'. |
| "It's a small edit to the shared component; faster than asking." | Invasiveness is a decision the owner must see, not a shortcut the implementer may take silently. |
| "I told the AI the repo in chat yesterday." | Chat evaporates; if it's not in the dispatch prompt's hard-constraint section, it was never said. |
| "Commit it all — we'll sort the files out in review." | Unclassified staging turns review into archaeology; classify dirty-file ownership first, stage by name. |

## Guardrails

- Gates bind before and during work; a gate discovered open mid-task pauses the task, it does not grandfather the work in.
- Gating is not stalling: once the five questions are answered and gates pass, proceed at full speed without re-litigating them.
- Project-mandated norms override this skill's generic defaults (e.g., a repo whose owner directs trunk-based work on `main` needs no feature branch — but baseline and clean-state checks still apply).
- This skill governs admission and closure; it does not itself authorize any commit, push, or MR.

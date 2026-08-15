---
name: calibrating-governance-to-stage
description: 在决定一件事配多重的流程时使用——开评审、写升级报告、宣布事故、加治理文档之前，或用户问"这算不算事故/要不要走全套流程"时。按爆炸半径定价：测试/灰度阶段轻门快跑，接近真实用户才逐级加码；零消费者的产物零治理；写"事故/紧急/P0"前先核实有无真实用户真实影响。四条底线（环境口径如实、不可逆谨慎、口述对实证、安全红线）任何阶段不松。/ Use when deciding how much process a piece of work deserves — before opening approvals, drafting escalations, or declaring incidents. Ceremony is priced by blast radius: dev/pilot stages get light gates, production gets the full apparatus, zero-consumer artifacts get zero governance; four bottom lines never relax. Cures framing dev-stage findings as live incidents and polishing process instead of shipping.
slug: calibrating-governance-to-stage
version: 1.1.3
displayName: "小白指挥AI干活之分清轻重缓急"
summary: "测试阶段轻装快跑，别小题大做动不动写事故报告；事故定性有检查清单，该严的底线一条不松。"
tags: ["小题大做", "事故定性", "轻重缓急", "形式主义", "测试环境"]
license: Apache-2.0
---

# Calibrating Governance to Stage

## Core Rule

Governance is priced by blast radius, not by how serious it makes the work feel. A test-environment flag, a dark-launched feature, an artifact nobody consumes yet — these have small blast radii and deserve light process. Production traffic, irreversible actions, real user data — full apparatus. Running heavy ceremony on light-stage work is not rigor; it is cost without protection, and it slows the only thing that matters at early stages: finding out whether the thing works at all.

## The Stage Dial

| Stage | Governance level | What runs |
|---|---|---|
| Local / dev / test environment | Light | fact-check against source (防照错假设写代码) + contract/interface freeze for anything others will consume |
| Dark launch / default-off / pilot | Light-plus | the above + a rollback path on record |
| Approaching cutover (real users soon) | Ramp up | readiness review, sign-offs begin, regression matrix |
| Production / irreversible / real data | Full | multi-owner sign-off, security review, formal release gates, incident protocol |

Two things are cheap early and expensive late — keep them at every stage: **fact-checking assumptions against source** (it catches wrong-assumption code before it's written) and **freezing contracts others consume** (contract mistakes compound). Everything else scales with the dial.

## The No-Consumer Rule

An artifact with zero consumers gets zero governance. Before opening a review, sign-off, or definition workflow for a field, document, or rule, ask: **who consumes this today?** Nobody → don't govern it, and question whether to define it at all. Building the thing that will consume it teaches more about its right shape than any committee — build to learn, then backfill the definition from working code. Perfecting unconsumed artifacts is hygiene theater (卫生洁癖), and it steals time from real capability.

Version-immutability and similar consumer-protection rules activate **when the first consumer arrives**, not before: while nothing reads an artifact, fixing a typo in place with a one-line note beats a formal version bump — the property those rules protect does not exist yet.

## The Incident-Framing Check

Before writing 事故 / 紧急 / 止血 / P0 / "线上正在发生" / "用户正在受影响" — stop and verify: **is there real production traffic and real user impact right now?** In a pre-production project the honest framing for almost every scary finding is 「上生产前必修项」, which goes on the backlog at backlog priority — not an incident, not an escalation, not today's top task.

The same check applies in reverse to configuration findings: a surprising flag in a test environment is judged by "does this create real risk *here*?" — usually the answer is no, and the finding is either good news (the path you need is already open) or a backlog note. Dev-stage goals are 跑通, not 治理开关安全.

## The Over-Production Check

Process output is itself governed: thirteen review dispatches for one small change is the orchestrator over-producing, and it manufactures the very "governance is crushing us" feeling that discredits the necessary gates. Before fanning out approvals or reviews, count them against the stage dial. When you notice yourself working on completeness, cleanliness, or definitional perfection — stop and ask: **does this help the thing actually work?** No → drop it and return to capability.

## Bottom Lines That Never Relax

The dial governs *ceremony*, never these — they are safety and honesty, not ritual:

1. Environment labels stay truthful: local/test results are never written up as production/formal passes.
2. Irreversible actions, cross-tenant access, and real-data writes get full caution at every stage.
3. Claims match reality: state contradicted by evidence (git state, actual files) is surfaced, not narrated over.
4. Security red lines (credentials, auth bypass, data exposure) never scale down.

## Edge Handling

- **Stage cannot be determined from context**: ask the one question that settles it — 「现在有真实用户/真实数据在被影响吗？」— before choosing a frame; only if unanswerable, govern at the heavier adjacent level and say so.
- **A real production incident is actually occurring**: this skill steps aside entirely — full incident response, no dampening; it exists to stop small things being dramatized, never to shrink real fires.
- **Mixed blast radius** (one change touches both test and production paths): govern the production-touching slice at full ceremony; never average the two.
- **User insists on heavy ceremony anyway** (「就是要写个事故报告」): their call — write it, but keep the facts honestly labeled inside (无真实用户影响、测试环境), never inflate the framing to match the format.
- **Pressure to relax a bottom line** (「先别管环境口径」): bottom lines are not stage-scalable — decline with the reason; stages price ceremony, not honesty.

Worked examples including the over-production and real-incident cases are in [references/examples.md](references/examples.md).

## Example

Finding: a feature flag is already ON in the test environment, which the plan assumed would be off.

Wrong framing: 「P0 安全事故：live 路径已对用户开放，需立即回滚并起草升级报告。」

Right framing: 「测试环境无真实用户，这个开关开着没有现实风险——反而是好消息：要接的链路已经通了。真问题是接线还缺两项配置，补上就能跑通。『生产环境该不该默认开』记入上生产前检查单。」

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| "Better safe than sorry — run the full process anyway." | Full process on light-stage work costs speed and buys nothing; safety lives in the bottom lines, which already always apply. |
| "This finding looks exactly like a production incident." | Check for real traffic and real users first; without them it is a pre-launch fix, on the backlog. |
| "Defining the field properly now saves pain later." | With zero consumers there is no 'later' yet; build the consumer, learn the shape, then define. |
| "More reviews show thoroughness." | Review count is calibrated to stage; over-production discredits the gates that matter. |
| "Skipping ceremony means lowering standards." | The bottom lines never relax; everything else was priced to blast radius all along. |

## Guardrails

- The dial only ever moves with the stage: when the work approaches real users, ramp governance back up without being asked — light-stage habits must not leak into cutover.
- This skill never waives the four bottom lines, and never applies to work that is already in production.
- When stage is genuinely ambiguous (partial real traffic, mixed tenants), govern at the heavier adjacent level and say so.
- A project's explicit governance rules override this skill's defaults — argue for recalibration, don't silently under-comply.

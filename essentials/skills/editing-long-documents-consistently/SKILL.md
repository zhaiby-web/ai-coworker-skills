---
name: editing-long-documents-consistently
slug: editing-long-documents-consistently
version: 1.0.0
displayName: "小白指挥AI干活之改文档不留尾巴"
summary: "专治改长文档留尾巴：改了这里忘了那里、摘要表格结论前后矛盾、越改越乱。牵一发就改全身——动手前先搜全影响面，一轮改齐所有关联处，完成前机械核验零矛盾残留。"
tags: ["前后矛盾", "越改越乱", "版本混乱", "联动修改", "文档一致性"]
license: Apache-2.0
description: 在直接编辑长 Markdown 文档时使用——方案、合同、规格、设计稿、治理文档、台账等任何"同一个语义事实出现在多个章节、摘要、表格、引用或结论里"的产物。改动单位是该事实的全部活跃实例，不是用户点名的那一句：动手前先搜全影响面（旧值、同义改写、否定式、见 §X 式引用），一轮改齐所有关联处，完成前过机械核验门确认无矛盾残留。/ Use when directly editing a long Markdown plan, contract, specification, design, governance document, ledger, or other artifact where one semantic fact appears in multiple sections, summaries, tables, references, or conclusions.
---

# Editing Long Documents Consistently

## Core Rule

For a semantic fact change, the edit unit is every active instance of that fact, not the sentence named by the user. A small diff is desirable only after the full semantic impact surface is known.

Do not modify files outside the task's authorized scope. This Skill strengthens consistency checks; it does not expand write authority.

## Workflow

### 1. Establish Current Truth

Read the authoritative source and the relevant complete sections before editing. Distinguish the new effective fact from historical records and unresolved alternatives.

### 2. Search Before Editing

Search the whole active artifact and build an impact list covering:

- the old value, conclusion, status, version, ID, and relation;
- synonyms, paraphrases, negative forms, and competing statements;
- headings, tables, summaries, conclusions, checklists, and examples;
- `见 §X`, section-number references, and passages that summarize the changed section.

Search hits are navigation candidates, not semantic proof. Read enough surrounding context to classify each hit as active, historical, quoted, or unrelated.

Before adding the new fact, answer explicitly: **Which existing statements does this make obsolete?**

### 3. Edit the Complete Impact Surface

Update every affected active statement in the same edit cycle. Remove obsolete active claims or move them into an explicitly labeled historical or `superseded` record when audit history must remain.

Do not leave an old statement beside a new statement and assume the newer one silently overrides it. Do not defer known affected locations to a later round.

### 4. Re-read File Truth

After editing, re-read the changed sections plus their summaries, conclusions, tables, and inbound/outbound section references from the actual file. Never close consistency from conversational memory or from a diff alone.

### 5. Mechanical Completion Gate

Do not claim completion until all checks pass:

1. Whole-artifact searches for the old value, old relation, old status, and known paraphrases show no contradictory active residue.
2. Any remaining old value appears only in a clearly labeled historical, quoted, or `superseded` context.
3. Section references and semantic restatements were checked separately; a zero result for one literal string is insufficient.
4. The new fact is consistent in the operative section, summaries, tables, conclusions, and acceptance language.
5. Report the search scope, residual matches and their classification, or state that no active residual remains.

## Review Asymmetry

A located, reproducible contradiction is existence evidence and outranks a generic `PASS`. A broad `PASS` cannot close a specific finding. Close each such finding only by:

- fixing it and rerunning the relevant checks; or
- rebutting it with source evidence and a reproducible explanation.

## Guardrails

- Do not delete legitimate history merely to force zero matches; label and separate it from current truth.
- Do not rewrite the whole document when targeted edits cover the complete impact surface.
- Do not change unrelated wording, layout, or style under the guise of consistency.
- Do not treat wording polish as a blocker when the active semantics are already consistent.

## Example

If `§2` changes the current table count from 14 to 23, search not only `14`, but also phrases such as `仅覆盖十四张`, conclusions derived from the old count, tables listing the scope, and `见 §2` summaries. Update every active dependency in one cycle. A dated historical line may retain 14 only when it is explicitly a historical snapshot.

## Common Rationalizations

| Rationalization | Required response |
|---|---|
| “Only change the location the user named.” | Treat it as the symptom location; search the full semantic impact surface. |
| “The new sentence already overrides the old one.” | Unmarked old active text remains consumable and must be removed or superseded. |
| “A read-through looks consistent.” | Run contradiction-oriented searches and inspect references. |
| “Keep the old text in case it is needed.” | Preserve it only as explicitly labeled history, never beside current truth. |

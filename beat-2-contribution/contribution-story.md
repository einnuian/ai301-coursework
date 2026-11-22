# Contribution: llama3.2, the judge the docs suggest, answers the faithfulness request with the JSON schema, so faithfulness is never scored

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Ein
**GitHub Username:** einnuian
**Issue:** https://github.com/dolev31/lasagna/issues/113
**Pull Request:** not opened yet

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: built the issue-scout search strategy, ran the first wild scan, patched the rubric**

On 2026-11-22 I wrote four `gh search issues` queries ("llm evaluation", "llm tracing", "llm observability", "llm async"), all limited to open, unassigned Python bugs. I added ranking filters and ran a scan: 190 results, top 5 graded. Two of the four accepted issues should have been rejected. wardeniq#167 had a fresh "I'd like to claim this" comment, but `unclaimed` only counted linked PRs. future-agi#1966 already had a merged fix (PR #2202). I widened `unclaimed` to count claim comments from the last 30 days and added a required `not-already-fixed` check, logged in `scan-notes.md`. Also noted: GitHub's secondary rate limit stopped the searches partway through (fixed with a retry and backoff), and the "llm async" query hit its 100-result cap.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched GitHub with my scout. Each query traced to my fit profile: Python only, small bug fixes, and LLM eval/observability tooling. Out of 190 de-duplicated results, the scan ranked this issue #1: it was the only candidate with a `help wanted` label and zero comments, updated the day before. The scout accepted it. The last human commit was the same day, there were no linked PRs and no comments, and the PR template says "AI coding agents are welcome, under the same bar." After I patched the rubric, I re-checked it and it still passes. It fits my profile better than anything else the scan surfaced. Lasagna is a Python RAG evaluation harness. The bug sits in the judge path, where RAGAS asks llama3.2 for structured JSON and gets the schema back instead, so faithfulness is never scored. The issue names the files to start in and says a test can run against `httpx.MockTransport` without a server, which matches my HTTP/async background. I hope to learn how an LLM-as-judge pipeline really talks to the model (instructor's JSON mode vs. a schema-constrained `response_format`), and how to prove a fix inside a Docker Compose stack.

On the tool itself: the scout found a good issue, but the wild search exposed real gaps. My `unclaimed` check only counted linked PRs, so it accepted an issue someone had claimed in a comment. Nothing checked whether a fix had already been merged. I patched both. Two gaps remain. The ranking can't see star counts, so this 37-star repo got through even though my profile asks for 200 to 10k. And `repo-size` is only an estimate from GitHub's byte counts, which marks lasagna as too big (about 240k lines), probably because data files are counted as Python. I would rank on stars in a later stage, and replace the size estimate with a real line count or drop it.

**Claim comment:** claim pending, 2026-11-22

**Toolkit**

- Tool: issue-scout (my search strategy and my rubric), run through Claude Code as a live scan with `gh search issues`, then a direct-mode grade of two extra URLs
- What it produced: a ranked read-out of the top 5 (`~/.claude/skills/issue-scout/scan-output.md`). lasagna#113 was ranked #1 and accepted: commits-alive, unclaimed and policy-allow-ai passed, and only the preferred repo-size check failed.
- Difficulties: GitHub's secondary rate limit stopped two of the four searches. `unclaimed` missed a claim comment (wardeniq#167). Nothing caught an already-merged fix (future-agi#1966). The ranking can't see stars. The repo-size estimate is rough.
- What changed: retried the searches with a backoff. Widened `unclaimed` to count claim comments from the last 30 days and added a required `not-already-fixed` check (`scan-notes.md`, 2026-11-22). Would still like to add star-based ranking and a real line count.

---

## Understanding the Issue

**Problem Description**

Lasagna uses RAGAS with an LLM "judge" to score generated answers. The docs suggest llama3.2 on a local Ollama as that judge. RAGAS asks the judge for structured answers in instructor's JSON mode (`response_format: {"type": "json_object"}`), with the schema only in the system prompt. For faithfulness, the first request asks the judge to split an answer into statements (`StatementGeneratorOutput`). llama3.2 answers with the JSON schema itself instead of an instance of it, so validation fails on every retry and the metric comes back NaN.

**Expected Behavior**

With the judge the docs suggest, `lasagna benchmark run smoke --profile speed --tracks generation --samples 2` scores faithfulness. If that isn't possible, the docs name a local judge that works, explain why llama3.2 doesn't, and the judge check refuses an incapable judge before any answers are written. OpenAI-judge scores stay the same, or `METRICS_VERSION` and the CHANGELOG say why they moved.

**Current Behavior**

A generation run fails with `judge produced no usable scores for faithfulness`. The maintainer saw 3 of 3 faithfulness judgements fail with `InstructorRetryException` (`1 validation error for StatementGeneratorOutput`). Context recall and context precision still come back, so the run looks half right. The judge check from #107 passes, because its `{"ok": bool}` probe is too easy to catch this.

**Affected Components**

- `src/lasagna/knowledge_evaluation_sdk/experiment.py`: `_build_judge_llm` and `_check_judge_llm`
- RAGAS's `ragas.llms.base`: `llm_factory`, `_get_instructor_client`, `InstructorLLM` (a dependency, not lasagna code)
- `docs/configuration.md`: the judge section (line 278 suggests `RAG_EVAL_JUDGE_MODEL=llama3.2`)
- `tests/test_the_judge_is_checked_before_answers_are_written.py`: drives the judge over `httpx.MockTransport`, the natural place for a new test

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]

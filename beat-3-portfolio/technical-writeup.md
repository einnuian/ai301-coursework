# Claiming, reproducing, and planning a fix for lasagna's silent LLM judge, with tools that checked my work

## Overview

[Lasagna](https://github.com/dolev31/lasagna) is a Python framework for measuring retrieval-augmented generation (RAG) pipelines. It uses an LLM "judge" to score generated answers. Issue [#113](https://github.com/dolev31/lasagna/issues/113) reports that when the judge is llama3.2, the local model lasagna's docs suggest, the faithfulness score is never computed. The judge's setup check passes, so the run looks healthy and only fails later.

I found the issue with a search tool I built (`issue-scout`), claimed it ([my claim comment](https://github.com/dolev31/lasagna/issues/113#issuecomment-7085540848)), and the owner accepted the claim. I reproduced the bug on current `main` and traced it to the structured-output mode the judge is created with. I showed that a schema-constrained request makes llama3.2 score faithfulness, and wrote a plan that my plan-check tool accepted. The fix isn't built and **no PR is open yet**; my claim gives me until 2026-12-06 to open a draft PR with a failing test.

Along the way I wrote my own tooling:
- the scout's search strategy and rubric
- a voice guide that a hook enforces on everything I post
- a gate manifest of lasagna's own checks
- a playbook for handling review

## What happened

### 1. The scout found the issue, then its misses changed its rubric

My first scan searched GitHub for open, unassigned Python bugs in LLM-eval and observability tools. It combined 190 results and graded the top five against my rubric. It accepted four, and lasagna#113 ranked first. But two of the accepted issues should have been rejected. One had a stranger's comment, "I'd like to claim this issue.", from three days earlier. The other already had a merged fix. My `unclaimed` check only counted linked PRs:

```text
Before: | unclaimed | number of linked PRs | 0 open linked PRs | required |
After:  | unclaimed | ... linked PRs, PRs mentioned in the thread, and claim comments
          ("I'd like to claim this", "I'll take this", ...) | 0 open linked or mentioned PRs,
          AND no claim comment posted in the last 30 days, unless a maintainer later
          said that claim is released | required |
        | not-already-fixed | cross-referenced and linked PRs, with each PR's state |
          no PR that references the issue has been merged | required |
```

I re-checked those two issues against the patched rubric, and both now fail. lasagna#113 still passes.

### 2. My first claim was a question, not a claim

My first claim draft said I'd like to work on the issue, named a first step, and gave a report-back date. My repro-check tool rejected it, quoting lasagna's own CONTRIBUTING.md:

> "The claiming comment says "I'm taking this" and names the files you will change, the test that will fail on `main`, and the `docker compose` commands you will run… A claim without a plan is a question, not a claim."

I rewrote it around that rule. Then my own voice guard stopped the new version, because my rule "Get to the point in a few sentences" now conflicted with the plan the repo requires. I changed the rule rather than the claim:

```text
Before: Skip the greetings, the praise, and "sir". Get to the point in a few sentences.
After:  ...Get to the point in a few sentences. When the repo's own rules require more
        (a claim plan listing files, a failing test, and commands, or a filled-in
        template), the repo's rule wins: include exactly what it asks for, kept plain,
        and nothing beyond it.
```

The guard then returned `voice-guard: pass. Your rules hold; you can post it.`, and I posted the claim. **The owner replied that day:**

> "Welcome, @einnuian, and thank you for the clear plan: #113 is yours."

They also pointed me to the right test pattern, and they called the judge's setup check "part of the bug". That shaped my plan.

### 3. Reproducing it, including a trap of my own making

I installed lasagna from `main` (104e516, which includes the owner's #112 rewrite of the judge code) and ran llama3.2 in an Ollama container. I ran lasagna's own path: the judge check, then `RAGASEvaluator.evaluate_detailed(metrics=["faithfulness"])` on three one-sentence answers. From my log:

```text
_check_judge_llm: passed (no exception)
Exception raised in Job[0]: InstructorRetryException(<failed_attempts>
...
failed_metrics: ['faithfulness']
response_format sent: {"type": "json_object"}
reply content: {"properties": {"statements": {"description": "The generated statements", ...
  "title": "StatementGeneratorOutput", "type": "object"}
```

The judge is asked for JSON with the schema only described in the prompt, and llama3.2 sends back the schema itself.

To test the fix direction, I replayed that request with the schema in `response_format`. My first replay contradicted the issue: it returned schema words, not statements. The bug was in my logging. instructor appends every retry to the same `messages` list, so I had replayed the conversation *after* four failed attempts. After I copied each request at send time, the replay matched the issue:

```text
reply content: {"statements": ["The Kamchatka Peninsula is located in the Russian Federation.", ...]}
```

repro-check accepted the claim and the report together, with all six checks passing.

### 4. A plan that a measured fact rescued

Before planning, I compared the two modes end to end. With instructor's `JSON_SCHEMA` mode, the same judge on the same samples gave `aggregate={'faithfulness': 0.888…} failed=[]`. I also found that a short statements-shaped probe passed even in the broken mode. So a better setup check has to send the real faithfulness prompt.

plan-check accepted the plan. Its main warning was that my whole fix depended on ragas letting the caller pick the mode, and my own notes said it couldn't. PyPI and the installed code settled it: ragas 0.4.4 (released 2026-11-14) takes `mode=`, and 0.4.3 doesn't. The plan now raises the ragas floor to 0.4.4 and says why.

The plan:
- an opt-in `RAG_EVAL_JUDGE_RESPONSE_FORMAT=json_schema` setting
- a setup check that runs one real faithfulness judgement
- docs and a CHANGELOG line

OpenAI judges keep the request they send today, so their scores can't move.

## Limitations and what's next

- **The fix doesn't exist yet,** and no PR is open. Everything above shows the bug and a working direction, not a merged change.
- **Only faithfulness is tested under `json_schema`.** ragas chose JSON mode because another mode broke `Dict` fields, so context recall and precision might behave differently. I've also only tried Ollama as the server.
- **My reproduction ran outside lasagna's Docker Compose stack.** Lasagna requires the stack run for a PR.
- **The scout still can't see star counts,** so a 37-star repo got past a profile that asked for 200 to 10,000.
- **No rubric check reads a repo's claim format.** repro-check caught lasagna's rule by reading the scope file, which is luck, not design.

**Next:**
- Write the failing test with lasagna's `httpx.MockTransport` judge pattern and confirm it fails on `main`, then build the fix.
- Do the stack run with and without the setting.
- Open the draft PR before 2026-12-06.
- In the tooling, add a "claim meets the repo's stated claim rules" check, and rank on stars after search.

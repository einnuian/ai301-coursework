# Contribution: llama3.2, the judge the docs suggest, answers the faithfulness request with the JSON schema, so faithfulness is never scored

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1

**Student:** Anh Nguyen

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

**Unit 6: installed the voice guard, added Upstream rules to my voice guide, built a CLAUDE.local.md for lasagna**

I added five Upstream rules (claims, questions, pings) to my voice guide, moved it to `~/.claude/voice-guard/`, and installed the hook, which stopped the `gh issue comment --help` wiring test as expected; I also built a 40-line `CLAUDE.local.md` in my lasagna clone (excluded through `.git/info/exclude`) from CONTRIBUTING.md and one scoped orientation run over the judge code. The guard stopped my real claim twice: once because my "get to the point in a few sentences" rule clashed with lasagna's requirement that a claim list files, a failing test, and `docker compose` commands, and once because the hook looks for a relative `--body-file` from the session's starting folder, not the folder I'd moved to. So I changed the rule to let the repo's rules win and now post with full paths. In the memory file, I dropped the `@AGENTS.md` import because lasagna's `CLAUDE.md` is a symlink to it, and I marked what's still unverified: the orientation agent couldn't read `pin_openai_client`, and the build and test commands haven't been run, since there's no venv yet.

**Unit 7: wrote lasagna's ship manifest, tested the ship-gate hook, planned the fix**

My `ship-manifest.md` names lasagna's own pre-PR checks: `uvx ruff@0.14.7 check .`, `uvx ruff@0.14.7 format --check .`, and the suite. I ran each from a shell with no virtual environment active on unmodified `main`. Both ruff checks passed, but the suite took about 18 minutes (the gate allows 25 per check) and had 1 failure out of 6,093 (`test_a_recorded_server_is_found_while_it_runs`), which also fails when run alone and tests code my fix doesn't touch. So I moved it to the excluded table with the date and reason, and made the gate's suite command deselect it. The hook test (`gh pr create --help`) got both stops, `ship-gate: STOP. no PR text to check` and the voice guard's. The gate hasn't run on a real branch yet, because I haven't built the fix.

**Unit 8: wrote my PR-monitor playbook from lasagna's real review timing**

I filled the four playbook sections. The staleness thresholds come from measuring the 8 most recent merged outside PRs: the owner first responded within 0 to 0.4 days on every one, and the longest silence mid-review was about 6 days (#29). That gave me 3 quiet days before a first-review nudge and 8 after I've answered a review. `monitor.py --state` confirmed "all four sections filled in, ready for a full run", but it reported "NO PR YET", so it has read nothing and drafted nothing: there's no draft to send or hold back yet. What fell short is the data: half the sample comes from one contributor, because lasagna has had few outside PRs, so I'll revisit the thresholds once my own PR shows how review goes.

---

## Why I Chose This Issue

I searched GitHub with my scout. Each query traced to my fit profile: Python only, small bug fixes, and LLM eval/observability tooling. Out of 190 de-duplicated results, the scan ranked this issue #1: it was the only candidate with a `help wanted` label and zero comments, updated the day before. The scout accepted it. The last human commit was the same day, there were no linked PRs and no comments, and the PR template says "AI coding agents are welcome, under the same bar." After I patched the rubric, I re-checked it and it still passes. It fits my profile better than anything else the scan surfaced. Lasagna is a Python RAG evaluation harness. The bug sits in the judge path, where RAGAS asks llama3.2 for structured JSON and gets the schema back instead, so faithfulness is never scored. The issue names the files to start in and says a test can run against `httpx.MockTransport` without a server, which matches my HTTP/async background. I hope to learn how an LLM-as-judge pipeline really talks to the model (instructor's JSON mode vs. a schema-constrained `response_format`), and how to prove a fix inside a Docker Compose stack.

On the tool itself: the scout found a good issue, but the wild search exposed real gaps. My `unclaimed` check only counted linked PRs, so it accepted an issue someone had claimed in a comment. Nothing checked whether a fix had already been merged. I patched both. Two gaps remain. The ranking can't see star counts, so this 37-star repo got through even though my profile asks for 200 to 10k. And `repo-size` is only an estimate from GitHub's byte counts, which marks lasagna as too big (about 240k lines), probably because data files are counted as Python. I would rank on stars in a later stage, and replace the size estimate with a real line count or drop it.

**Claim comment:** https://github.com/dolev31/lasagna/issues/113#issuecomment-7085540848 (posted 2026-11-22)

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

A generation run fails with `judge produced no usable scores for faithfulness`. In my reproduction on `main` 104e516, the judge check passed, and 3 of 3 faithfulness judgements failed with `InstructorRetryException` (`1 validation error for StatementGeneratorOutput`). The request went out with `response_format: {"type": "json_object"}`, and llama3.2 replied with the JSON schema itself. The issue reports that context recall and context precision still come back, so a run looks half right; I only ran faithfulness. The judge check passes because its `{"ok": bool}` probe is too easy to catch this.

**Affected Components**

- `src/lasagna/knowledge_evaluation_sdk/experiment.py`: `_build_judge_llm` and `_check_judge_llm`
- RAGAS's `ragas.llms.base`: `llm_factory`, `_get_instructor_client`, `InstructorLLM` (a dependency, not lasagna code)
- `docs/configuration.md`: the judge section (line 278 suggests `RAG_EVAL_JUDGE_MODEL=llama3.2`)
- `src/lasagna/knowledge_evaluation_sdk/ragas_evaluator.py`: `evaluate_detailed` and the judge's request settings (`judge_extra_body`, `_JUDGE_OWN_FIELDS`)
- `tests/test_a_reasoning_judge_can_finish_its_judgement.py`: the owner's recommended pattern, which builds the real judge over `httpx.MockTransport`. `tests/test_the_judge_is_checked_before_answers_are_written.py` covers the check.

**Toolkit**

- Tool: Claude Code. It read the issue with `gh issue view`, then ran one orientation agent scoped to `src/lasagna/knowledge_evaluation_sdk/`, as the unit-6 memory template says. I distilled the result into my clone's `CLAUDE.local.md`.
- What it produced: the 2-4 files that matter, with line numbers. `_build_judge_llm` hands the client to ragas `llm_factory`, and ragas picks instructor's JSON mode. The `{"ok": bool}` probe in `_check_judge_llm` is too easy. The test to copy is `test_a_reasoning_judge_can_finish_its_judgement.py`.
- Difficulties: the agent couldn't read `pin_openai_client` because it was denied permission. It also read ragas 0.4.3 from another project's venv, whose `llm_factory` has no `mode` argument. Once I installed lasagna's own environment I got ragas 0.4.4, where `llm_factory` does take `mode`, and that changed which fixes are possible.
- What changed: I read `pin_openai_client` myself, and I check ragas facts against the version lasagna actually installs. I corrected Current Behavior and Affected Components after the reproduction.

---

## Reproduction Process

**Environment Setup**

- **OS:** Ubuntu 24.04.5 LTS, kernel 7.0.0-43-generic, x86_64, CPU only (no GPU), 15 GB RAM
- **Lasagna:** `main` at 104e516 (2026-11-22, version 0.3.0). It includes #112 (b6651f5), which the owner said changed `_build_judge_llm` and `_check_judge_llm`. The issue was reproduced on 0edd5679.
- **Python:** 3.12.3 in a uv 0.13.8 venv: `uv venv --python 3.12 .venv && uv pip install --python .venv/bin/python -e ".[eval]" --group dev`. That's CONTRIBUTING's pip-only install (`.[eval]` is "enough for the test suite"), run through uv.
- **Resolved versions:** ragas 0.4.4, instructor 1.17.0, openai 3.3.0, pydantic 2.14.4
- **Judge:** Ollama 0.40.17 in Docker (`ollama/ollama:latest`), model `llama3.2:latest` (ID a80c4f17acd5, 2.0 GB). The issue used Ollama 0.40.13.
- **Docker:** 29.8.1
- **Judge settings:** `RAG_EVAL_JUDGE_MODEL=llama3.2`, `RAG_EVAL_JUDGE_BASE_URL=http://localhost:11434/v1`, `RAG_EVAL_JUDGE_API_KEY=ollama`

Challenges:
- My clone had no environment and Ollama wasn't installed. I created the venv with uv and ran Ollama as a container, which is also how the stack will run it.
- Capturing the judge's requests took three tries:
  1. Patching `httpx.Client.send` caught nothing, because ragas calls the judge asynchronously.
  2. Patching `httpx.AsyncClient.send` caught nothing either: this openai 3.x client doesn't go through it.
  3. Logging at `openai.resources.chat.completions` worked.
- My first replay then sent the request as it stood *after* instructor's retries. instructor appends each retry to the same `messages` list, so the "first" request already held llama3.2's earlier schema replies, and the schema-constrained replay to that returned schema words instead of statements. Copying the request at send time (`copy.deepcopy`) fixed it.

**Steps to Reproduce**

1. Start Ollama and pull the judge model:
   ```bash
   docker run -d --name ollama-repro -p 11434:11434 -v ollama-repro:/root/.ollama ollama/ollama:latest
   docker exec ollama-repro ollama pull llama3.2
   ```
2. Install lasagna from `main`:
   ```bash
   git clone https://github.com/dolev31/lasagna && cd lasagna
   uv venv --python 3.12 .venv && uv pip install --python .venv/bin/python -e ".[eval]" --group dev
   ```
3. Save the script below as `repro_113.py` and run it with the judge pointed at Ollama:
   ```bash
   RAG_EVAL_JUDGE_MODEL=llama3.2 RAG_EVAL_JUDGE_BASE_URL=http://localhost:11434/v1 \
   RAG_EVAL_JUDGE_API_KEY=ollama .venv/bin/python repro_113.py > repro_113.log 2>&1
   ```
   - Part 1 runs lasagna's own path, `_check_judge_llm()` and then `RAGASEvaluator(llm=judge).evaluate_detailed(..., metrics=["faithfulness"])`, on the issue's three one-sentence answers (Kamchatka, Yeltsin, Canberra), each with a one-sentence passage.
   - Part 2 logs the judge's requests at the openai SDK layer, prints the first faithfulness request and its reply, and sends that same request with a schema-constrained `response_format`.
4. **Observed:**
   - The judge check passes.
   - All 3 faithfulness judgements fail with `InstructorRetryException` (`1 validation error for StatementGeneratorOutput`), and `failed_metrics` is `['faithfulness']`.
   - The request goes out in JSON mode, and llama3.2 replies with the schema itself.

<details><summary><code>repro_113.py</code></summary>

```python
"""Reproduce dolev31/lasagna#113: llama3.2 as the RAGAS judge never scores faithfulness.

Run from a lasagna checkout's venv, with Ollama serving llama3.2 on localhost:11434:

    RAG_EVAL_JUDGE_MODEL=llama3.2 RAG_EVAL_JUDGE_BASE_URL=http://localhost:11434/v1 \
    RAG_EVAL_JUDGE_API_KEY=ollama python repro_113.py

Part 1 runs lasagna's own path: _build_judge_llm -> _check_judge_llm ->
RAGASEvaluator.evaluate_detailed(metrics=["faithfulness"]).
Part 2 logs the first chat-completion request the judge sends for faithfulness
and llama3.2's reply, then replays that request with a schema-constrained
response_format.
"""

import copy
import json
import httpx
import openai.resources.chat.completions as oc

from lasagna.knowledge_evaluation_sdk import experiment as exp
from lasagna.knowledge_evaluation_sdk.ragas_evaluator import RAGASEvaluator

QUESTIONS = [
    "Where is the Kamchatka Peninsula?",
    "Who was the first president of the Russian Federation?",
    "What is the capital of Australia?",
]
ANSWERS = [
    "The Kamchatka Peninsula is in the Russian Federation.",
    "Boris Yeltsin was the first president of the Russian Federation.",
    "Canberra is the capital of Australia.",
]
CONTEXTS = [
    ["The Kamchatka Peninsula is a peninsula in the Russian Far East."],
    ["Boris Yeltsin served as the first President of the Russian Federation from 1991 to 1999."],
    ["Canberra is the capital city of Australia."],
]

# Log every chat-completion request and reply at the openai SDK layer.
captured = []
_orig_create = oc.Completions.create
_orig_acreate = oc.AsyncCompletions.create


def _logging_create(self, *args, **kwargs):
    sent = copy.deepcopy(kwargs)  # instructor appends retries to the same list
    resp = _orig_create(self, *args, **kwargs)
    captured.append((sent, resp))
    return resp


async def _logging_acreate(self, *args, **kwargs):
    sent = copy.deepcopy(kwargs)  # instructor appends retries to the same list
    resp = await _orig_acreate(self, *args, **kwargs)
    captured.append((sent, resp))
    return resp


oc.Completions.create = _logging_create
oc.AsyncCompletions.create = _logging_acreate

print("== Part 1: lasagna's own path ==")
try:
    exp._check_judge_llm()
    print("_check_judge_llm: passed (no exception)")
except Exception as e:  # noqa: BLE001 - report whatever the check raises
    print(f"_check_judge_llm: raised {type(e).__name__}: {e}")

judge = exp._build_judge_llm(_require=True)
result = RAGASEvaluator(llm=judge).evaluate_detailed(
    questions=QUESTIONS, answers=ANSWERS, contexts=CONTEXTS, metrics=["faithfulness"]
)
print("aggregate:", result.aggregate)
print("per_sample:", result.per_sample)
print("failed_metrics:", result.failed_metrics)

print("\n== Part 2: the first faithfulness request and its reply ==")
statement_reqs = [
    (req, resp)
    for req, resp in captured
    if "statements" in json.dumps(req.get("messages", []))
]
print(f"chat requests logged: {len(captured)}, statement-generation requests: {len(statement_reqs)}")
req, resp = statement_reqs[0]
print("messages in that request:", [m["role"] for m in req["messages"]])
print("response_format sent:", json.dumps(req.get("response_format")))
print("reply content:", resp.choices[0].message.content[:600])

print("\n== Part 2b: same request, schema-constrained response_format ==")
schema = {
    "type": "object",
    "properties": {"statements": {"type": "array", "items": {"type": "string"}}},
    "required": ["statements"],
}
replay = {k: v for k, v in req.items() if k in ("model", "messages", "temperature", "max_tokens")}
replay["response_format"] = {
    "type": "json_schema",
    "json_schema": {"name": "StatementGeneratorOutput", "schema": schema},
}
r = httpx.post(
    "http://localhost:11434/v1/chat/completions",
    json=replay,
    headers={"Authorization": "Bearer ollama"},
    timeout=300,
)
print("reply content:", r.json()["choices"][0]["message"]["content"][:600])
```

</details>

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:** excerpt from `repro_113.log`:

```text
== Part 1: lasagna's own path ==
_check_judge_llm: passed (no exception)
Max retries exceeded. Total attempts: 4, Last error: 1 validation error for StatementGeneratorOutput
statements
  Field required [type=missing, input_value={'properties': {'statemen...alse, 'type': 'object'}}, input_type=dict]
Exception raised in Job[0]: InstructorRetryException(<failed_attempts>
Exception raised in Job[1]: InstructorRetryException(<failed_attempts>
Exception raised in Job[2]: InstructorRetryException(<failed_attempts>
aggregate: {}
per_sample: [{}, {}, {}]
failed_metrics: ['faithfulness']

== Part 2: the first faithfulness request and its reply ==
chat requests logged: 13, statement-generation requests: 12
messages in that request: ['system', 'user']
response_format sent: {"type": "json_object"}
reply content: {"properties": {"statements": {"description": "The generated statements", "items": {"type": "string"}, "title": "Statements", "type": "array"}}, "required": ["statements"], "title": "StatementGeneratorOutput", "type": "object"}

== Part 2b: same request, schema-constrained response_format ==
reply content: {"statements": [
    "The Kamchatka Peninsula is located in the Russian Federation.",
    "The Kamchatka Peninsula is a part of Russia."
]}
```

  Each of the 3 jobs made 4 attempts. All 12 replies were the schema rather than an instance of it, which accounts for the 18 `1 validation error for StatementGeneratorOutput` lines in the log. The 13th logged call was the `{"ok": true}` probe, which the check accepted.
- **My findings:**
  - **Reproduced on 2026-11-22.** Everything matches the issue:
    - `_check_judge_llm` passes with llama3.2.
    - Faithfulness fails 3 of 3 on `StatementGeneratorOutput`.
    - RAGAS asks in JSON mode (`{"type": "json_object"}`), and the reply is the schema.
    - The same request with `{"type": "json_schema", ...}` returns real statements.
  - **New from my environment:** the bug survives a newer `main` that includes #112 and a newer Ollama (0.40.17). #112's check still lets llama3.2 through, because its probe is still `{"ok": bool}`, which matches the owner's point that the check is part of the bug.
  - **Not shown yet:**
    - the exact `judge produced no usable scores for faithfulness` message (I called `evaluate_detailed` directly, not the generation benchmark)
    - a run in the Docker Compose stack
    - whether a schema-constrained request changes scores for an OpenAI judge

**Toolkit**

- Tool: repro-check, run as `claude -p "repro-check: grade my claim comment in claim-draft.md and my repro report in repro-draft.md for issue …"` from my Unit 6 folder, with `--add-dir` for the skill folders and `--allowedTools` for `gh issue view`, `gh api` and file reads.
- What it produced: **accept**, with all six checks passing (environment-recorded, steps-followable, behavior-matches, honest-outcome, claim-specific, ai-policy-met). It checked my numbers against the log (12 schema replies, 18 validation-error lines) and read the owner's reply on the thread. It flagged two lines to fix: my report never said that 104e516 includes #112, which the owner had raised, and I claimed llama3.2 "answered correctly" on the probe, which the log didn't show.
- Difficulties:
  - Run headless with `claude -p`, repro-check couldn't read its own skill files or call `gh`. The first claim grade came back as "can't grade", and the second as "reject" with every check unclear, until I added `--add-dir` and `--allowedTools`.
  - On the claim, it found lasagna's "claim with a plan" rule through `scope.md`, not through a rubric check. `claim-specific` doesn't read a repo's claim format.
- What changed:
  - I added the #112 line and changed the probe sentence to "the check passed, so its reply was accepted".
  - I now run repro-check with those flags.
  - I would add a required rubric check for "the claim meets the repo's stated claim requirements".

---

## Solution Approach

**Analysis**

- **Where the request is built.** `_build_judge_llm` (`src/lasagna/knowledge_evaluation_sdk/experiment.py:3046`) builds an OpenAI client and calls `llm_factory(resolved_model, client=..., **settings)` (:3122) without naming a structured-output mode.
- **What ragas does with it.** ragas's `_get_instructor_client` (`ragas/llms/base.py`) then defaults an OpenAI client to instructor's `Mode.JSON`: the request carries `response_format: {"type": "json_object"}`, and the schema appears only in the prompt text.
- **How llama3.2 fails.** Under RAGAS's long statement-generation prompt for faithfulness (`StatementGeneratorOutput`), llama3.2 answers with the schema itself. instructor tries 4 times. RAGAS records NaN. `RAGASEvaluator.evaluate_detailed` (`ragas_evaluator.py:499`) puts `faithfulness` in `failed_metrics`, and `_no_usable_scores` (`experiment.py:394`) turns that into "judge produced no usable scores for faithfulness".
- **Why the check misses it.** `_check_judge_llm` (:3192) probes with `_JudgeProbe = {ok: bool}`, which llama3.2 answers fine. I also tried a short statements-shaped probe ("break this answer into standalone statements", `{statements: List[str]}`), and llama3.2 passed that 3 of 3 times in JSON mode too. Only RAGAS's real prompt brings out the schema echo, so only a check that sends that prompt will catch it.
- **Evidence that the mode is the cause:**
  - Reproduction (2026-11-22, `main` 104e516): 3 of 3 faithfulness judgements failed, and the first request was `json_object` with the schema as the reply.
  - Design experiment, same judge, same three samples: with `llm_factory(..., mode=instructor.Mode.JSON_SCHEMA)`, the request carries the schema in `response_format`, and faithfulness scored 0.889 (1.0, 0.667, 1.0) with `failed_metrics=[]`.
- **The whole class.** `_build_judge_llm` is the only place lasagna builds a ragas judge (`grep -rn llm_factory src/`), so every judge metric and the judge check go through the one change.

**Proposed Solution**

1. **A setting for the judge's structured-output request.** `RAG_EVAL_JUDGE_RESPONSE_FORMAT` takes `json_object` (the default: the request RAGAS sends today) or `json_schema` (instructor's `Mode.JSON_SCHEMA`: the schema goes in `response_format`, and a server such as Ollama, vLLM or SGLang constrains the reply to it).
   - A new `judge_response_format()` in `ragas_evaluator.py` reads it, beside `judge_extra_body()`, and refuses any other value.
   - `_build_judge_llm` passes the mode to `llm_factory`.
2. **A check that asks what RAGAS asks.** `_check_judge_llm` keeps the `{"ok": true}` probe (it catches wrong credentials and URLs cheaply) and adds one faithfulness judgement through RAGAS on a fixed one-sentence sample.
   - If that judgement fails validation under `json_object`, the run stops before any answers are written, with a message that names `RAG_EVAL_JUDGE_RESPONSE_FORMAT=json_schema`.
   - If it fails under `json_schema`, the message says the judge can't produce RAGAS's structured answers even with the schema in the request (or that the server ignores `json_schema`), and suggests a different judge model.
   - A judge that times out or has bad credentials still gets today's messages.
   - The cost: every run that writes answers pays for one extra faithfulness judgement on a one-sentence sample before it starts (how many calls that is, I haven't measured), OpenAI judges included. It runs outside the LLM cache and the task meters, as the check does today.
3. **Docs.** The judge section of `docs/configuration.md`:
   - its llama3.2 example sets `RAG_EVAL_JUDGE_RESPONSE_FORMAT=json_schema`
   - it says why: llama3.2 answers JSON mode's schema-in-the-prompt with the schema
   - it says which servers take `json_schema`, naming only the ones I've run

   A line goes in `CHANGELOG.md` under Unreleased.
4. **Dependency floor.** ragas `>=0.4.4` in the `eval` extra of `pyproject.toml`. I checked this on 2026-11-22:
   - **0.4.4** (released 2026-11-14, the latest on PyPI): `llm_factory(..., mode=...)` exists, and so does `_get_instructor_client(client, provider, mode=None)`.
   - **0.4.3** (2026-01-13): `llm_factory` has no `mode`.
   - **The pin beside it:** 0.4.4's vertexai import is wrapped in try/except, so the `langchain-community<0.4.2` pin still resolves (my install has 0.4.1). It could now be lifted, but that's a separate change and I'm leaving it alone.
   - **To update:** the `pyproject.toml` comment that calls 0.4.3 "the newest release".

**Unknowns:**
- **Other metrics under `json_schema`.** I've only shown it working for faithfulness. ragas chose JSON mode because tools mode returned `{}` for `Dict` fields, so context recall and context precision under `json_schema` are untested. The stack run's generation track will show whether they still score.
- **Servers other than Ollama.** I've only run `json_schema` against Ollama 0.40.17. The docs will name only servers I've run.
- **What the extra check costs.** Not measured yet.

**What I won't change:**
- **The default request.** An OpenAI judge sends exactly what it sends today, so its scores can't move and `METRICS_VERSION` stays where it is. That's how this fix meets the second Done-when item: no metric definition changes. A test asserts the default request is still `json_object`.
- **Detecting the server automatically.** That would change the request for every local or OpenRouter judge already in use.
- **ragas internals and metric definitions.**
- **`_JUDGE_OWN_FIELDS`.** `RAG_EVAL_JUDGE_EXTRA_BODY` still can't set `response_format`; the new setting is the only way to change it.
- **#115** (`factual_correctness` never reported): out of scope, as the owner said.

**Implementation Plan**

1. **Failing test first:** `tests/test_a_judge_that_answers_with_the_schema_is_caught.py`, using the `judge_server` pattern from `tests/test_a_reasoning_judge_can_finish_its_judgement.py` (a real `_build_judge_llm`, RAGAS and instructor over `httpx.MockTransport`). The fake judge answers the statements request with the schema when `response_format` is `json_object`, and with an instance when it is `json_schema`. The tests:
   - `_check_judge_llm` refuses that judge under the default, and the message names `RAG_EVAL_JUDGE_RESPONSE_FORMAT`. This fails on `main`, where the check passes.
   - With `json_schema`, `_eval_offline` scores faithfulness, and the request carries `{"type": "json_schema", ...}`. This fails on `main`, where the setting doesn't exist.
   - With the setting unset, the request is still `{"type": "json_object"}`.
   - An unknown value is refused.
2. `ragas_evaluator.py`: add `judge_response_format()`.
3. `experiment.py`: pass the mode in `_build_judge_llm`; add the faithfulness judgement and its message to `_check_judge_llm`.
4. `pyproject.toml`: ragas `>=0.4.4`.
5. `docs/configuration.md` judge section and `CHANGELOG.md`.
6. **Verify:**
   - revert the fix and confirm the new tests fail
   - `.venv/bin/pytest tests/ --ignore=tests/live --ignore=tests/stress -q`
   - `uvx ruff@0.14.7 check .` and `uvx ruff@0.14.7 format --check .`
7. **Stack run** from `src/lasagna/deploy`, with `IMAGE_EXTRAS=eval` in `.env` and llama3.2 in an Ollama container on the stack's network. The judge settings are `RAG_EVAL_JUDGE_MODEL=llama3.2`, `RAG_EVAL_JUDGE_BASE_URL=http://<ollama container>:11434/v1`, `RAG_EVAL_JUDGE_API_KEY=ollama`, and `RAG_EVAL_JUDGE_RESPONSE_FORMAT=json_schema`. Commands:
   - `docker compose -f docker-compose.yml -f backends/milvus.yml up -d --build`
   - `docker compose -f docker-compose.yml -f backends/milvus.yml ps`
   - `docker compose -f docker-compose.yml -f backends/milvus.yml exec api lasagna benchmark run smoke --profile speed --tracks generation --samples 2`

   I'll also run the same command without the setting, to show the check refusing the judge before any answers are written.

Deviations: none yet.

**Toolkit**

- Tool: plan-check, run from my lasagna clone as `claude -p "plan-check: grade my plan in plan.md and draft comment in plan-comment.md for issue …"`, with `--add-dir` for the skill folders and `--allowedTools` for `gh issue view`, `gh api` and file reads. Before writing the plan, I ran a design experiment against llama3.2: JSON vs `JSON_SCHEMA` mode, plus a short statements-shaped probe.
- What it produced: **accept**, twice, with all six required checks passing (diagnosis-grounded, scope-bounded, executable, test-decisive, thread-engaged, ai-policy-met) and `unknowns-honest` passing with notes. It checked my line numbers against the repo and read the owner's reply on the thread.
- Difficulties:
  - Its biggest note was a fact the whole fix depended on: whether ragas has a release whose `llm_factory` takes `mode`. My own `CLAUDE.local.md` said it didn't, because the orientation agent had read ragas 0.4.3 from another project's venv. PyPI and the installed code settled it: 0.4.4 (2026-11-14) takes `mode`; 0.4.3 doesn't.
  - It flagged an unmeasured number I'd stated ("about two judge calls"), the case where the check fails under `json_schema`, and the metrics I hadn't tested under `json_schema`.
  - Two gaps in plan-check itself: its procedure expects a posted repro comment, which I don't have yet (I'm not posting the report). And its rubric counts any "new option" as scope creep, even when the option is the fix.
- What changed:
  - The ragas floor is now a checked fact (versions and dates), and `CLAUDE.local.md` is corrected.
  - The plan says what happens when the check fails under `json_schema`, lists Unknowns (other metrics, other servers, the check's cost), and no longer states a call count.
  - The comment now says where the cause comes from.

---

## Testing Strategy

The fix isn't built yet, so this is the strategy plus the baseline I've already measured on `main`.

**Baseline on unmodified `main` (104e516), measured 2026-11-22** from a shell with no virtual environment active:
- `uvx ruff@0.14.7 check .`: "All checks passed!"
- `uvx ruff@0.14.7 format --check .`: "866 files already formatted"
- `.venv/bin/pytest tests/ --ignore=tests/live --ignore=tests/stress -q`: 6092 passed, 471 skipped, 1 failed (`test_a_recorded_server_is_found_while_it_runs`, which fails on `main` and alone; excluded in my manifest)
- The reproduction (`repro_113.py`): `_check_judge_llm` passes, `failed_metrics: ['faithfulness']`.

**What will prove the fix:**
1. **The new test fails on `main` and passes on the branch.** `tests/test_a_judge_that_answers_with_the_schema_is_caught.py` uses a fake judge over `httpx.MockTransport` that answers the statements request with the schema under `json_object` and with an instance under `json_schema`. Then I revert the fix and see it fail again, as AGENTS.md asks.
2. **The default request is unchanged.** With the setting unset, the request is still `{"type": "json_object"}`. That's the evidence that an OpenAI judge's scores can't move.
3. **The reproduction before and after.** `repro_113.py` on `main` and on the branch, with and without `RAG_EVAL_JUDGE_RESPONSE_FORMAT=json_schema`. Expected on the branch: the check refuses the judge under the default, and faithfulness scores under `json_schema`. In my design run, mode `JSON_SCHEMA` scored 0.889 on the same three samples.
4. **The stack run.** `lasagna benchmark run smoke --profile speed --tracks generation --samples 2` inside the Docker Compose stack, with llama3.2 in an Ollama container, with and without the setting. This also covers context recall and precision under `json_schema`, which nothing has shown yet.
5. **The manifest's three checks,** run by the ship-gate before the PR opens.

**Toolkit**

- Tool: the ship manifest, plus Claude Code running its three commands from a shell with no virtual environment active (`env -i`), the way the ship-gate will.
- What it produced: the baseline above. Ruff passed twice, and the suite reported 6092 passed, 471 skipped, 1 failed, in 18m48s.
- Difficulties: the one failure on unmodified `main`, and a suite that takes close to the gate's 25-minute limit per check.
- What changed: I reran the failing test alone to confirm it fails on `main` on its own, recorded it in the manifest's excluded table, and made the gate's suite command deselect it instead of dropping the suite.

---

## Implementation Notes

**2026-11-22: scan and claim**

The scout ranked #113 first. My first claim draft ("I'd like to work on this…") was rejected by repro-check, because lasagna requires a plan in the claim. I rewrote it with files, a failing test and `docker compose` commands. The voice guard then stopped it over my "few sentences" rule, so I changed the rule to let repo rules win, and posted. The owner replied: "#113 is yours", plus four pointers, including that #112 had changed the judge code.

**2026-11-22: environment and reproduction**

I created a uv venv with `.[eval]` (ragas 0.4.4) and ran Ollama 0.40.17 in Docker with llama3.2. The reproduction matched the issue. Wrong turns: my request logging caught nothing through httpx (sync or async), and only worked at the openai SDK layer. My first schema-constrained replay contradicted the issue because instructor reuses one `messages` list across retries; copying each request at send time fixed it.

**2026-11-22: design experiment and plan**

I compared JSON with `JSON_SCHEMA` mode: faithfulness failed vs. scored 0.889. A short statements-shaped probe passed in both modes, so the setup check has to send RAGAS's real prompt. Decision: an opt-in `RAG_EVAL_JUDGE_RESPONSE_FORMAT`, keeping the default request so OpenAI scores can't move. plan-check made me verify the ragas floor (0.4.4 takes `mode`; 0.4.3 doesn't).

**2026-11-22: baseline checks and manifest**

Ruff passed. The suite reported 1 failure on `main`, unrelated to the judge, and I excluded it in the manifest with the reason. No code changed yet: the fix, its test and the stack run are next, and the draft PR is due by 2026-12-06 under lasagna's claim rule.

**Toolkit**

- Tool: Claude Code in my lasagna clone. It wrote and ran `repro_113.py` and a design-experiment script against llama3.2, and its notes came from `CLAUDE.local.md`.
- What it produced: the reproduction log, the JSON vs `JSON_SCHEMA` comparison (failed vs 0.889), and the baseline check results.
- Difficulties: request capture took three tries, and the first replay was wrong in a way that looked like evidence against the issue.
- What changed: I deep-copy each request when logging it, and I check ragas facts against the version actually installed, not another project's copy.

---

## Pull Request

**PR Link:** not opened yet

**PR Description:** not written yet. It will adapt Solution Approach and Testing Strategy into lasagna's PR template: why, what changed, how it was tested (the `docker compose` commands and output), evidence, and "Agent use" (Written with: Claude Code; Read in full by: me).

**Maintainer Feedback:**

- 2026-11-22: none yet. No PR is open. (On the issue, the owner accepted my claim the same day and pointed me to `test_a_reasoning_judge_can_finish_its_judgement.py` and the #112 changes; both are in my plan.)

**Status:** not opened yet. The fix isn't built. My claim requires a draft PR with its failing test by 2026-12-06.

**Toolkit**

- Tool: the ship-gate (`~/.claude/ship-gate/check_gate.py`, a PreToolUse hook) and my `ship-manifest.md`.
- What it produced: on the hook test, `ship-gate: STOP. no PR text to check…`, alongside the voice guard's stop. It hasn't gated a real PR yet.
- Difficulties: the suite on `main` has one unrelated failure and runs close to the gate's per-check limit. `scripts/ci-local.sh`, which CONTRIBUTING names first, builds every CI job's environment, so I didn't make it a gate.
- What changed: the failing test is excluded with a dated reason and deselected in the suite command. `ci-local.sh` is listed as excluded with my reason, so pr-precheck can see what I skipped.

---

## Learnings and Reflections

**Technical Skills Gained**

- How RAGAS gets structured answers from a judge model: instructor's modes. `Mode.JSON` sends `response_format: {"type": "json_object"}` with the schema only in the prompt; `Mode.JSON_SCHEMA` puts the schema in the request, so the server can constrain the reply. I learned it well enough to show the difference end to end (failed vs 0.889).
- How instructor retries: it appends each failed reply and the validation error to the same `messages` list, which matters the moment you log or replay requests.
- Running a local model as a test dependency: Ollama in Docker, pulled by tag, pointed at through an OpenAI-compatible `/v1` URL.
- Reading a project's test patterns before writing one: lasagna fakes the judge over `httpx.MockTransport`, so a test needs no server.
- Making checks reproducible: running them with no virtual environment active, pinning tool versions the way CI does (`uvx ruff@0.14.7`), and keeping local files out of a PR with `.git/info/exclude`.

**Challenges Overcome**

- **My first claim wouldn't have counted.** Lasagna treats a claim without a plan as a question. repro-check caught it; I rewrote the claim with files, a test and commands, and the owner thanked me for "the clear plan".
- **My own tool blocked my real post.** The voice guard's "few sentences" rule conflicted with the plan lasagna requires. I changed the rule so the repo's rules win, rather than weakening the claim.
- **Evidence that pointed the wrong way.** My schema-constrained replay first contradicted the issue. Instead of reporting it, I inspected the request and found my logging had captured it after four retries.
- **A wrong fact in my own notes.** The orientation agent had read an older ragas from another project, so my notes said the fix direction was impossible. plan-check flagged it, and PyPI and the installed code showed 0.4.4 supports it.
- **The scout's false accepts.** Two accepted issues were already claimed or already fixed. I patched the rubric, and the re-grade rejects both.

**What I'd Do Differently Next Time**

- Read the target repo's CONTRIBUTING.md before drafting anything I post, not after a tool quotes it at me.
- Install the project's own environment before trusting any fact about its dependencies.
- Build the fix sooner. I spent the first day on claim, reproduction and planning, which was the right order, but the deadline (a draft PR by 2026-12-06) now depends on the stack run, the slowest step, so I should start it early.
- Get a second opinion on my evidence before I believe it. The replay mistake would have gone into a report if I'd trusted the first result.

---

## Resources Used

- [lasagna CONTRIBUTING.md](https://github.com/dolev31/lasagna/blob/main/CONTRIBUTING.md): the claim rule ("A claim without a plan is a question, not a claim"), setup, the checks in my manifest, the Docker Compose stack steps, and the AI-agents policy.
- [lasagna AGENTS.md](https://github.com/dolev31/lasagna/blob/main/AGENTS.md): the commands, "a test that passes without the fix is not a test", and the PR rules for agent-assisted work.
- [Issue #113](https://github.com/dolev31/lasagna/issues/113): the owner's reproduction and "Start here" list, and their reply to my claim (the #112 changes, the test pattern to copy, "the check before answers are written is part of the bug").
- `tests/test_a_reasoning_judge_can_finish_its_judgement.py` in lasagna: the `httpx.MockTransport` judge pattern my test will follow.
- [ragas on PyPI](https://pypi.org/project/ragas/): release dates that settled that 0.4.4 (2026-11-14) is the first release whose `llm_factory` takes `mode`.
- [Ollama Docker image](https://hub.docker.com/r/ollama/ollama): how I ran llama3.2 locally for the reproduction.

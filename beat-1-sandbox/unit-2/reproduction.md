# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

[Your GitHub username, exactly as it appears on your profile — no `@`, no profile URL. Your
comments upstream are identified by this name.]

einnuian

---

## Posted upstream

**Claim comment**

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35#issuecomment-5903080724

Hi, I'd like to take this one as my Path Review contribution.

The issue asks for a way for clients to register a callback URL and get a `POST` with the review payload once a long multi-repo review (30–90 seconds) finishes. I haven't reproduced anything yet. My next step is to set up the repo from its README, run a multi-repo review, and record how the caller finds out it's done today (polling, blocking, or nothing). I'll post that as a repro report here, with my environment, steps, and output, before I start on `api/routes/webhooks.py` and `core/services/webhook_service.py`.

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35#issuecomment-5903806095

**Environment:** 
- Linux
- Python 3.12.3
- Docker Postgres 16.15
- FastAPI 0.142.1
- Uvicorn 0.54.0
- SQLAlchemy 2.1.1
- commit `2f4e82f`
- `LLM_PROVIDER=mock`.

**Steps:**
1. `cp .env.example .env`, `docker compose up -d`, created a venv, `pip install -e ".[dev]"`, then `alembic upgrade head` and `scripts/seed_db.py`. I skipped the frontend and pre-commit parts of `make setup`.
2. Started `uvicorn api.main:app`, plus a small local listener on `127.0.0.1:9999` to catch any callback.
3. Logged in as `user1@example.com`. Login is form-encoded (`username=...&password=...`), not JSON.
4. There's no route to list profiles, so I read a `profile_id` from the database with `psql`.
5. `POST /reviews` with `{"profile_id": "...", "callback_url": "http://127.0.0.1:9999/hook"}`.
6. Polled `GET /reviews/{id}/status` once a second for 5 seconds, then waited 5 more.

**What happened:**
- `POST /webhooks` returns 404. The OpenAPI route list has no webhook or callback route.
- The API accepted `callback_url` without an error and ignored it, because `ReviewCreate` only defines `profile_id`.
- The server log shows the whole pipeline running in the same second the review was created, ending in `review_processing_completed`. No outbound HTTP call is logged.
- The listener received 0 requests.
- `grep -rliE "webhook|callback|notify|notification" api/ core/` matches no files. `process_review()` in `core/services/review_service.py` sets `status="complete"`, commits, logs, and returns, with no notification step.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

Run 1: 20/20

Run 2: 20/20

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

`pkg-20`: my rubric said reject, and so did the gold label. All the proof checks passed, but `ai-policy-met` failed. The grader wrote: "Repo facts require disclosure of AI tool and extent of assistance in issues/comments; neither candidate comment contains any AI disclosure." Ghostty's policy says "All AI usage in any form must be disclosed." My check treats every package as AI-assisted, so a good repro still gets rejected if it doesn't disclose.

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/repro-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

| ai-policy-met | repo's AI/contribution policy vs. both comments (treat packages as AI-assisted) | if the policy requires AI disclosure in issues or comments, a comment discloses it; otherwise pass (a PR-only disclosure rule does not apply) | required |

This check replaced my old `follows-convention` check ("follows the convention as denoted by the repo"). The old one was too vague to give the same answer every time. I limited the new check to "issues or comments" and added the PR-only exception. That way, a repo like fd, which only asks for disclosure "in the pull request", doesn't fail comments its policy doesn't cover.

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

The PR-only exception is what keeps `pkg-09` accepted. The grader wrote: "the policy states no disclosure ask for issue comments". The case I accept this check will miss is a repo whose AI policy doesn't say whether comments count. My check passes that repo, even if a careful maintainer would want a disclosure. Nothing else changed: both full runs scored 20/20 with "disclosure 1/1". So the stricter check for `pkg-20` didn't flip any accepted package that has an AI policy (`pkg-03`, `pkg-05`, `pkg-07`, `pkg-09`, `pkg-12`).

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.

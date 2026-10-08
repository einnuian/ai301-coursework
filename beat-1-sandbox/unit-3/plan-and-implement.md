# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

[Your GitHub username, exactly as it appears on your profile - no @, no
profile URL. Your comment upstream is identified by this name, and it is
the only thing that ties it to you. Several students may plan the same
house issue, so this is what keeps their comments off your score and
yours off theirs.]

einnuian

**Plan comment**

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35#issuecomment-6049880249

> Here's my plan, based on my repro above. In that run, `POST /webhooks` returned 404, `callback_url` on `POST /reviews` was silently dropped, and `process_review()` ended with `review_processing_completed` and no outbound call. My listener got 0 requests. So two things are missing: a place to register a callback, and a send when the review finishes.
>
> **Approach.** Register the callback per account, before any review starts. In my repro the whole pipeline ran "in the same second the review was created", so a URL registered after `POST /reviews` would race the review. With account-level registration, the URL is already stored when the review ends. @tanisnus proposed a per-review route and covers the race by delivering from two paths. I'm trying the other trade-off, so the two plans can be compared.
>
> - New `POST /webhooks` (`api/routes/webhooks.py`): stores an `http(s)` URL for the current user, with a `webhooks` table and migration 003. Registering the same URL twice returns the existing record.
> - New `core/services/webhook_service.py`: `notify_review_finished()` POSTs the same JSON as `GET /reviews/{id}` to the owner's URLs when the status is `complete` or `failed`. It makes one attempt with a 5 s timeout, and failures are logged, never raised.
> - `process_review()` gets a single `try/finally` that calls the notifier, so every exit path is covered without editing each `return`.
>
> **Not in scope:** listing or deleting webhooks, `callback_url` on `POST /reviews`, retries, signing, a delivery queue, frontend, and blocking private/loopback targets. That last one is an SSRF question I'd rather raise in the PR than decide here, since my own test posts to `127.0.0.1`.
>
> **Test plan.** I'll re-run my repro steps with `POST /webhooks` added before step 5. I expect a 200 instead of a 404, then exactly one POST at the listener with the review's `id` and `"status": "complete"`, arriving after its `updated_at`, plus a `webhook_delivered` log line. I'll also check three cases: a user with no webhook gets 0 requests, a bad URL gets a 422, and a stopped listener still leaves the review `complete`, with `webhook_delivery_failed` logged. Unit tests in `tests/unit/test_webhook_service.py` use `pytest-httpserver`.
>
> **Unknowns.** With `LLM_PROVIDER=mock` I can't produce a 30–90 s review, so I'm checking ordering rather than a real long run. On the exception path, delivery reuses a session that may already be broken, so it's best-effort there.
>
> Work will be on `fix/35-review-webhooks` in my fork. I used Claude to help draft this plan; I've checked it against the code and my repro.

---

## Your branch

**Branch**

[The name of the branch you built the change on, exactly as it appears in your fork. The
naming shape is a type prefix, then the issue number, then a short description. **The issue
number in the branch name must be the number of the issue you claimed** — a name carrying
any other number does not satisfy this field.]

fix/35-review-webhooks

**Evidence**

[Your Unit 2 reproduction steps re-run against the built change: the before, then the
after. Paste both, including the commands you ran and their output.]

These are my Unit 2 steps (log in as user1, `POST /reviews` with a `callback_url`, poll, a listener on `127.0.0.1:9999`), plus `POST /webhooks`. Both runs used the same script, against `main` @ `2f4e82f` (before) and `fix/35-review-webhooks` @ `fa8d761` (after), with `LLM_PROVIDER=mock`. Before matches my posted Unit 2 result: 404, `callback_url` ignored, 0 requests.

**Before** (`main`)

```
$ curl -s -w " HTTP %{http_code}" -X POST $API/webhooks -H "Authorization: Bearer $TOKEN1" -d '{"url": "http://127.0.0.1:9999/hook"}'
{"detail":"Not Found"} HTTP 404
$ curl -s -X POST $API/reviews -H "Authorization: Bearer $TOKEN1" -d '{"profile_id": "e1bd943f-...", "callback_url": "http://127.0.0.1:9999/hook"}'
{"id":"339a2943-...","status":"pending",...}
$ curl -s $API/reviews/$RID/status ...          # polled 5 s, then waited 5 s
{"review_id":"339a2943-...","status":"complete","progress_pct":0}
$ cat listener.log
listening on 127.0.0.1:9999                      # 0 requests
$ grep $RID server.log | tail -1
[info] review_processing_completed  overall_score=0.81 review_id=339a2943-...   # nothing after it
```

**After** (`fix/35-review-webhooks`)

```
$ curl -s -w " HTTP %{http_code}" -X POST $API/webhooks -H "Authorization: Bearer $TOKEN1" -d '{"url": "http://127.0.0.1:9999/hook"}'
{"id":"56dc199c-...","url":"http://127.0.0.1:9999/hook","created_at":"..."} HTTP 200     # same id on a 2nd call
$ curl -s -X POST $API/reviews -H "Authorization: Bearer $TOKEN1" -d '{"profile_id": "e1bd943f-...", "callback_url": "http://127.0.0.1:9999/hook"}'
{"id":"c365e196-...","status":"pending",...}
$ cat listener.log                               # exactly 1 request
[2026-10-08T02:54:22.056Z] POST /hook {"id":"c365e196-...","status":"complete","sections":[...],"overall_score":0.81,...}
$ grep $RID server.log | tail -2
[info]    review_processing_completed  overall_score=0.81 review_id=c365e196-...
[info]    webhook_delivered  status_code=200 url=http://127.0.0.1:9999/hook review_id=c365e196-...
$ diff <(GET /reviews/$RID) <(webhook body)       # identical
$ curl ... -X POST $API/webhooks -d '{"url": "not-a-url"}'        → HTTP 422   (before: 404)
user2, no webhook, creates a review                               → 0 requests, no webhook_* log
listener stopped, user1 creates a review                          → complete; [warning] webhook_delivery_failed error='All connection attempts failed'
```

My first after run found a bug: the webhook body's `updated_at` didn't match `GET /reviews/{id}`. I fixed it in `fa8d761`, and the run above is the re-run.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

1. 19/20 (`agreement: 19/20 scored items  (bar: 18/20: PASS)`)

This is the one full run on record. It is the run in `eval-run.txt`, graded with `rubric.md sha256:d3d5f40b65a918ed`, which is identical to the rubric now in `tools/plan-check/`.

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

**pkg-14** (zellij-org/zellij#5174, category `clear-accept`). My rubric decided **reject**, and the gold label is **accept**. From `eval-run.txt`:

> `pkg-14  clear-accept       accept  reject   NO     failed: executable, unknowns-honest`

The gold note says: "honestly scoped-down: reattach handshake fix with a regression-window repro; defers the untestable Windows variant and says so; arguable on the deferral, ready as scoped".

My `executable` check rejected it because of two lines in the plan. The first is "consuming **or** draining pending OSC query responses in the client attach path in `zellij-server`'s client connection handling". The second is "exact functions to be pinned in the PR after tracing the query issuance with debug logs". My procedure lists "or" as a hedge word and says executable fails "if any hedge word leaves the location or approach undecided". The plan also leaves its exact functions until build time, so the grader read it as a decision left open, the "investigate" failure.

The gold reads it differently. "Consuming or draining" names one mechanism two ways, not two competing approaches, and the risk line commits to it ("I will bound the drain to OSC response patterns rather than a time window"). The plan also names a call site: the client attach path, before pane input is wired. That is enough to start, even before the function name is known. My rubric treats a missing function name, plus the word "or", as an undecided approach.

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/plan-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

From `tools/plan-check/rubric.md`, exactly as it reads now:

| executable | plan's change statement: named files/functions, chosen approach | a stranger could start without asking the author: the plan names where the change goes (file, function, or call site) and commits to one approach; "investigate", "somewhere", "X or Y, whichever is easier", or "not sure which layer" leaves a real decision to build time and fails | required |

This check reads "file, function, **or** call site" so that a plan doesn't need to name an exact function to pass, as long as a stranger knows where to open the code. A test that demanded a function name would reject honest plans that have to trace the code first. The second half, "commits to one approach", is the part that does the work. The failure it targets is the unbuildable plan that names the right area and then leaves the real decision for later: "X or Y, whichever is easier", "not sure which layer". So it lists those exact phrases instead of a vague "the plan is specific enough". A vague test would let two graders split on the same package, and a check built from quoted phrases is one a stranger can apply the same way.

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

**A case I accept it will miss: pkg-14.** The `executable` check's hedge-word rule ("or", and functions "to be pinned in the PR") rejects pkg-14, which the gold labels accept. Here "consuming or draining" is one approach worded two ways, and the call site is named even though the function isn't yet.

I'm keeping the strict reading because the `unbuildable` packages are caught by the same hedge words: pkg-10 ("Investigate whether scoop-installed git behaves differently"), pkg-17 ("Investigate how lazygit reads mouse events (gocui? tcell? not sure"), and pkg-18 ("Add a recover() safety net somewhere", "either upstream or in the vendored copies, whichever"). All three agree (`unbuildable 3/3`). pkg-18 is the closest call: its "X or Y, whichever" has the same shape as pkg-14's "consuming or draining". Loosening the "or" rule to rescue pkg-14 risks letting pkg-18 through. So the trade is one false reject in `clear-accept` (6/7) in exchange for no false accepts in `unbuildable`. The run still clears the bar at 19/20.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

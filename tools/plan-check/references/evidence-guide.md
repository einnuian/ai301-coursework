# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

- Where it lives (eval): the cause is under `## Candidate plan`, in
  whatever line states why the bug happens (labeled `Cause:` or
  `Diagnosis:`, or an unlabeled sentence like "the X is the tell").
  Plans label their parts differently, so find each part by what it
  says, not by its label. The behavior it must explain
  is under `## Repro evidence`: the numbered `Steps:`, any control
  run (a variant where the bug does not happen), and the `Expected:` /
  `Actual:` lines. Maintainer findings under `## Thread highlights`
  count as evidence too.
- Where it lives (live): the cause in the draft `plan.md`; the
  behavior in the student's posted repro comment on the issue (or the
  house repro pack as the drafts quote it); maintainer findings in the
  issue thread.
- What good looks like: the cause explains every step the repro shows,
  and no control contradicts it. In calib-01, "the view's model is not
  refreshed after push" fits step 3 (stale color) and step 4 (re-entry
  fixes it). A bad diagnosis blames a component that a control shows
  working, or blames a step that runs after the failure already
  appears, or repeats a thread's guess the repro contradicts.


## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

- Where it lives (eval): every piece of work under
  `## Candidate plan` (`Change:`, `Approach:`, `Files:`, `Steps:`,
  `In scope:`, or unlabeled prose), plus its `Out:` / `Not in scope:`
  / deferred statements; read against the behavior asked for in
  `## Issue` (title, body, `Expected:`).
- Where it lives (live): the scope or change section of the draft
  `plan.md`, read against the issue body.
- What good looks like: every piece of work in the change is needed
  to fix the issue's one behavior, and an `Out:` (or "not in scope" /
  "deferred") line names the tempting neighbors it will not touch.
  calib-01 is one refresh-scope addition in one file, with push-status
  computation and other views explicitly out. Scope creep is a list of
  extra fronts (a migration, a new option, a rewrite, a CI change)
  attached to the fix, even when the core fix is right. A plan that
  scopes down and says what it defers is bounded, not incomplete.


## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

- Where it lives (eval): the change parts of `## Candidate plan`
  (`Change:`, `Approach:`, `Files:`, `Steps:`, or prose): the named
  files, functions, call sites, and
  the approach verb ("add", "clamp", "wire X into Y").
- Where it lives (live): the change section of the draft `plan.md`.
- What good looks like: a stranger could open the named file and
  start: calib-01 names `pkg/gui/controllers/sync_controller.go`, the
  push completion callback, and the one change (add the commits
  context to the post-push refresh scope). Unbuildable plans leave a
  decision for build time: no file named, "investigate", "somewhere",
  "X or Y, whichever is easier", "not sure which layer".


## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

- Where it lives (eval): the `Test:` / `Test plan:` part of
  `## Candidate plan` (or the prose saying how success is checked),
  read against `## Repro evidence`'s `Steps:` and `Expected:` /
  `Actual:`.
- Where it lives (live): the test section of the draft `plan.md`,
  read against the steps in the student's posted repro comment.
- What good looks like: the test names an observable outcome that is
  wrong today and right after the fix, usually by re-running the repro
  steps with the expected result stated. calib-01 says "at step 3 the
  color must flip without leaving the view." Other good forms are a
  regression test that asserts the issue's case, an exit code, or exact
  output. Weak tests name no observable for this fix: "run the full
  test suite", "should feel fast", "nothing else should feel broken".


## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

- Where it lives (eval): certainty words in the `Cause:` and `Change:`
  lines ("the only", "confirmed", "always", "just"), any `Risk:` or
  unknowns line under `## Candidate plan`, and the plan comment's
  claims, read against what `## Repro evidence` and
  `## Thread highlights` actually show.
- Where it lives (live): the same parts of the draft `plan.md` and
  comment. Mid-build deviations are recorded in an updated `plan.md`
  ("what changed and why"), not only in the diff.
- What good looks like: every claim stated as fact is backed by the
  repro or the thread, and anything unverified is labeled as an
  unknown along with how it will be checked (calib-01's test checks
  force push and the main commits panel because "both share the
  callback"). False confidence is a root cause or "only site" claim
  that the package never demonstrates.


## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

- Where it lives (eval): `## Candidate plan comment`, read against
  `## Thread highlights` (maintainer/owner comments, settled
  approaches, requests to test, linked or prior PRs) and against the
  `contribution policy` line in `## Repo facts` (AI disclosure, rules
  about comments being in the contributor's own words, review-bandwidth
  notes).
- Where it lives (live): the draft plan comment, read against the
  live issue thread and the repo's CONTRIBUTING.md / AI policy files.
  Under the Path Review house rules, a classmate's plan comment is not
  maintainer direction.
- What good looks like: the comment states this plan specifically,
  responds to any maintainer direction (follows it, or says why it
  diverges), engages open PRs instead of racing them, and meets the
  policy. If the policy requires AI disclosure in issues or comments,
  the comment names the tool and the extent; a PR-only disclosure rule
  does not apply to the comment. calib-01 has an empty thread and no
  disclosure rule, and its comment still acknowledges the
  review-bandwidth note in CONTRIBUTING. A bad comment ignores an
  owner who already isolated the culprit, or omits a disclosure the
  policy requires.


# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

**Where it lives in a practice submission.**

- The plan: `## Plan context`. It is a paragraph, not a form: find
  the change ("Plan: two steps. Remove ..."), the files it names
  ("Files: `themes.gitconfig`"), the out-of-scope line ("Not in
  scope: any other theme"), and any deviation or deferral note.
- What changed: `### Diff` under `## Candidate PR`, one `--- a/` /
  `+++ b/` pair per file and one `@@` hunk per change. `### Commits`
  names the same work in commit-sized pieces.
- What the PR says it changed: `### Title` and `### Description`.

**Where it lives in your own PR.**

- The plan: your `plan.md` (house chain: the house plan), including
  its `## Deviations` section. A deviation recorded there is part of
  the plan; one that exists only in the diff is not.
- What changed: `git diff main...HEAD` (three dots) from your working
  copy, and `git log main..HEAD --oneline` for the commits.
- What the PR says it changed: the title and description in
  `pr_draft.md`.

**What good looks like.**

- Every hunk serves a step the plan names, a test for the fix, an
  entry the repo requires (changelog, whatsnew), or a deviation note.
  Every step the plan promises appears in the diff or is explicitly
  deferred in the plan's notes or the description. calib-01: the plan
  says remove the first `file-decoration-style` line in
  `themes.gitconfig`; the diff removes exactly that line and touches
  no other file.
- Every claim in the description is true of the diff. calib-01's
  description says it removes the shadowed line and leaves rendering
  unchanged; the diff shows one `-` line and nothing else.
- Silent drift, more than the plan: a hunk in a file the plan never
  names, a new option or flag, a rename or refactor pass, or anything
  the out-of-scope line rules out, with no note. Worse when the
  description says "exactly as planned" or "no other changes".
- Silent drift, less than the plan: a promised step (a docs update,
  a second failure mode) missing from the diff with no deferral note,
  or a description that says it was done anyway.
- Not drift: a shortfall stated in both the plan's notes and the
  description ("X is deferred because ..."). Less than everything,
  said out loud, can be ready.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

**Where it lives in a practice submission.**

- The proof: `### Test evidence` under `## Candidate PR`, usually a
  fenced transcript with `# before` / `# after` lines and a line about
  the repo's suite.
- What it must prove: the test plan sentence in `## Plan context`
  ("Test plan: re-run the configparser repro, expect a clean parse")
  and the repro evidence quoted at the top of that block.

**Where it lives in your own PR.**

- The proof: `test_evidence.md` (before and after output from your
  repro, plus the output of the repo's own tests or checks).
- What it must prove: the test plan in your `plan.md`, and the repro
  steps from your week-2 repro (house chain: the house repro pack).

**What good looks like.**

- Each test-plan line is re-run on the changed code path, with an
  observable before (the failure the repro shows) and an observable
  after (output, exit code, error, rendered result) that matches the
  plan's expected-after. calib-01: before
  `DuplicateOptionError ... already exists`, after `parsed OK`,
  matching "expect a clean parse"; the second test-plan line (theme
  still renders) is covered by `delta --show-syntax-themes` and the
  `git config --get-all` line printing one value.
- Every failure mode the plan's test plan names gets its own re-run,
  unless the PR says it is deferred.
- The repo's own suite or checks are named with their result
  (`pytest ... passes (1462 passed)`, `go test ./...` passes). This
  is a plus, not proof: a suite pass alone shows nothing about the
  bug.
- Not tested: "tested locally", "works on my machine", "cargo test
  passes" with no before/after; evidence that only runs a control or
  the unchanged path (a single-file case for a multi-file bug, a GET
  for a POST bug); a before/after for one failure mode when the plan
  names two.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

**Where it lives in a practice submission.**

- The change: `### Diff`. Read every hunk, including context lines,
  for what each `+` and `-` line does.
- The history: `### Commits`, one line per commit.

**Where it lives in your own PR.**

- The change: `git diff main...HEAD`. Run it after your last commit,
  so what you read is what the PR will show.
- The history: `git log main..HEAD --oneline`.

**What good looks like.**

- Every hunk is the fix, its test, or an entry the repo asks for. A
  reviewer can find the fix without scrolling past anything else.
  calib-01: one hunk, one removed line, nothing else.
- Commit messages say what each commit changes. calib-01:
  `themes: remove shadowed file-decoration-style from gruvmax-fang`.
- Debris that makes a diff unreviewable, even when the fix is right:
  debug prints (`print`, `eprintln!("DEBUG ...")`, `console.log`);
  commented-out code or a commented-out first attempt; a dead
  function kept around (`#[allow(dead_code)]`, `_unused_` names); a
  stray `TODO`; re-indents, import reordering, or reformatting on
  lines the fix does not otherwise touch; unrelated hunks folded in.
  Commit messages like `wip`, `fix`, `misc cleanups while debugging`
  are a sign the debris is there.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

**Where it lives in a practice submission.**

- What the repo asks: `## Repo facts`. The `pull requests:` line
  states the template and its asks; the `contribution policy:` line
  states the contributing guide and any AI policy. calib-01: "no PR
  template; the contribution docs ask that PRs stay small and
  reference the issue"; "no stated AI policy".
- Maintainer direction: `## Thread highlights`.
- Where the PR answers: `### Description` for template sections,
  checklist, issue link, and disclosure; `### Diff` for asks that land
  in files (changelog, whatsnew, docs, tests).

**Where it lives in your own PR.**

- What the repo asks: the PR template in the scoped repo
  (`.github/pull_request_template.md` or wherever the repo keeps it)
  and `docs/CONTRIBUTING.md`, read from the default branch, plus the
  house rules in `scope.md`.
- Maintainer direction: your issue's thread on GitHub
  (`gh issue view <number> --comments`). A classmate's comment is not
  maintainer direction.
- Where the PR answers: the description in `pr_draft.md`, and
  `git diff main...HEAD` for asks that land in files.

**What good looks like.**

- Every ask that applies is met with real content: each template
  section filled, each checklist item checked or marked not
  applicable with a reason, the issue referenced the way the repo
  asks ("closes #N"), and any changelog, whatsnew, or docs entry
  present in the diff. calib-01: no template, and the description
  references `#2211` and the PR is one small change, so both stated
  asks are met.
- If the repo states an AI policy, the description meets it. Treat
  every PR as AI-assisted: name the tool and what it did where
  disclosure is required, and write in your own specific words where
  that is required. No stated policy means nothing to meet.
- If a maintainer gave direction or asked a question about the fix,
  the description says how the PR follows it or why it diverges.
- Standards wall: a required checklist missing or left as
  boilerplate, no issue link where one is asked for, a required
  whatsnew or changelog entry absent from the diff, or no AI
  disclosure where the policy requires one. A good fix behind a
  visibly ignored ask is still a hold.

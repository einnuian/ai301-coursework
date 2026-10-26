# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | every changed file and hunk in the diff (live: `git diff main...HEAD`), read against the plan's change statement, in/out-of-scope pair, and deviation notes | every hunk serves a change the plan or a deviation note names (tests for the fix and entries the repo's stated asks require, such as a changelog line, count as part of the plan), and every change the plan promises appears in the diff or is explicitly deferred in a deviation note or the description; fails if the diff adds anything the plan does not name (an extra refactor, rename pass, new config option or flag, rewrite of a file the plan never touches), especially something the plan scopes out, or drops a promised piece with no note. A disclosed deferral passes: less than everything is not drift when it is said | required |
| description-matches-diff | each factual claim in the PR description ("implements the plan exactly", "no changes outside X", "docs now cover Y", "adds a test for Z"), read against the diff | every claim about what the PR changes is true of the diff: nothing the description says was done is missing, and no fidelity claim ("exactly as planned", "nothing else changed") is contradicted by an extra hunk; a description that discloses a shortfall or deferral the diff confirms passes | required |
| test-decisive | the test-evidence section read against the plan's test plan and the reproduction's steps and expected/actual | shows the plan's repro (or its named regression test) re-run on the changed code path with an observable before and after (output, exit code, error, rendered result) and the after matching the plan's stated expected-after; every failure mode the plan's test plan names is covered unless the PR explicitly defers it. Fails on "tested locally", "works on my machine", or "tests pass" with no observable result named, and on evidence that only exercises a control or unchanged path (a single-file case when the bug is multi-file, a GET when the bug is a POST) | required |
| diff-clean | the diff's hunks read for content that does not serve the fix | every hunk is part of the fix, its test, or an ask the repo states (changelog, docs entry); fails on any debug leftover (print/eprintln/console.log), commented-out code or abandoned attempt, dead function kept alive (e.g. behind allow(dead_code) or an _unused name), stray TODO, or formatting/re-indent/import-reorder churn on lines the fix does not otherwise touch | required |
| template-asks-met | the repo's stated PR template and contribution asks (bundle: repo-facts "pull requests" line; live: the PR template and docs/CONTRIBUTING.md) read against the description and the diff | every stated ask that applies to this PR is visibly met: template sections and checklist items filled with real content or marked not-applicable with a reason, issue linked where asked ("closes #N"), and asks that land in the diff (changelog/whatsnew entry, regenerated docs, tests) present in the diff. Pass if the repo states no template or asks. Fails if any applicable ask is absent or left as unfilled boilerplate | required |
| ai-disclosure-met | the repo's stated contribution/AI policy (bundle: repo-facts "contribution policy"; live: docs/CONTRIBUTING.md and any AI policy file) read against the PR description; treat every package as AI-assisted work | if the policy requires disclosing AI use in PRs, the description names the tool and what it was used for; if it requires maintainer-facing text in the contributor's own words, the description reads as specific first-person writing about this change rather than generic generated boilerplate; pass if the repo states no AI policy | required |
| thread-engaged | the issue thread (bundle: thread highlights; live: the full issue thread) read against the description | if a maintainer gave explicit direction or asked an open question about the fix (a settled approach, a culprit, a uniqueness or compatibility concern), the description says how the PR follows it or why it diverges; pass if the thread has no such direction | required |
| repo-checks-run | the test-evidence section | names the outcome of the repo's own suite, checks, or linters relevant to the change (command and result) | preferred |
| commits-readable | the commit list | each commit message says what that commit changes; "wip", "fix", "misc" messages note against this check | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept if every required check passes. Reject if any required check
grades `fail` or `unclear`: `unclear` counts as `fail`, because a PR
you cannot verify from the package is not ready to submit. Preferred
checks (repo-checks-run, commits-readable) never change the verdict;
report them as notes.

Grade each check on its own condition. A strong pass on one check
never offsets a fail on another: a correct, decisively tested fix with
debris in the diff, a missing template ask, or an unmet AI policy is
still a hold.

When several required checks fail, report all of them, and quote the
first failing required check in table order as the deciding check
(diff-matches-plan before description-matches-diff before
test-decisive, and so on): what the PR changes comes before how it
is proven, cleaned, or presented.

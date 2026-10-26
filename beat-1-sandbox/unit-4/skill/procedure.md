# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

1. Live mode only: read `scope.md` first. If its `Repo:` line is a
   placeholder, stop without grading. If the PR's issue or target repo
   is not the scoped repo, refuse. Note the house rules: the PR comes
   from the fork's `fix/<issue-number>-<slug>` branch, the template is
   always used, and a classmate's PR or comment is never maintainer
   direction.
2. Read `rubric.md` and `references/evidence-guide.md`. Write down the
   check names in table order, which are required, and the verdict
   rule.
3. Read the package in this order, before grading anything:
   1. **Repo facts** (live: the PR template and `docs/CONTRIBUTING.md`
      on the default branch): list every stated PR ask as a numbered
      checklist (template sections, checklist items, "closes #N",
      changelog/whatsnew or docs entries, tests) and the AI policy:
      where disclosure is required and whether text must be in the
      contributor's own words. Write "no template", "no AI policy"
      where that is what the repo states.
   2. **Issue and thread** (bundle: issue and thread highlights; live:
      the full thread): note the one behavior the issue asks to fix,
      and any maintainer direction or open maintainer question about
      the fix. Write "none" if there is none.
   3. **Plan context** (live: `plan.md` and its `## Deviations`
      section): write the plan's **promise list**, one line per piece
      of work it commits to, with the files or functions it names;
      its out-of-scope / deferred lines; each deviation note; and its
      test plan, one line per failure mode or observable it names,
      with the expected-after. Also note the repro's steps. Read the
      plan before the diff so the plan, not the PR, is the yardstick.
   4. **Diff and commits** (live: `git diff main...HEAD` and
      `git log main..HEAD --oneline`): list every changed file and
      every hunk, one line each, saying what the hunk does. Read it
      before the description so the description's account cannot tell
      you what the diff contains.
   5. **Test evidence** (live: the captured test output the live-run
      command names): note each command shown, which path it runs,
      and its before and after output.
   6. **Title and description**: read last. List every factual claim
      it makes about what the PR changes, tests, or defers, and any
      disclosure sentence, one line each.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

1. In eval mode, the bundle is the only source: quote lines from it
   and fetch nothing. In live mode, take each family from the place
   the evidence guide names: the diff from `git diff main...HEAD`,
   the plan from `plan.md`, the evidence and the draft title and
   description from the files the live-run command names, the thread
   from `gh issue view --comments`, and the repo's asks from its PR
   template and `docs/CONTRIBUTING.md`.
2. Build these side-by-side pairings from your Read order notes, and
   record a short quote for each row:
   - **diff-matches-plan**: two columns. Left: each hunk from 3.4.
     Right: the promise-list line, test-plan line, repo ask, or
     deviation note it serves, or "nothing". Then the reverse: each
     promise-list line from 3.3, next to the hunk that delivers it,
     or the deferral note that defers it, or "missing". Mark any hunk
     that matches an out-of-scope line.
   - **description-matches-diff**: each claim from 3.6, next to the
     hunk that makes it true, or "not in diff". For fidelity claims
     ("exactly as planned", "no changes outside X"), pair the claim
     with any hunk marked "nothing" above.
   - **test-decisive**: each test-plan line from 3.3, next to the
     evidence command from 3.5 that re-runs it, with before and after
     quoted, or "not run". Note whether the command runs the changed
     path or a control or unchanged path, and whether the after
     matches the plan's expected-after.
   - **diff-clean**: each hunk from 3.4 marked "fix", "test", "repo
     ask", or "debris". For debris, name the type: debug print,
     commented-out code, dead function, TODO, formatting/import churn.
   - **template-asks-met**: each ask from 3.1, next to where it is met
     (description section, checklist line, or diff file) or "absent".
     Asks that land in the diff (changelog, whatsnew, docs) are looked
     for in the diff, not in the description's checklist.
   - **ai-disclosure-met**: the AI policy from 3.1, next to the
     description's disclosure sentence, or "no disclosure". Treat
     every package as AI-assisted.
   - **thread-engaged**: the maintainer direction from 3.2, next to
     the description sentence that answers it, or "no response".
   - **repo-checks-run**: the suite or check command and its result
     from 3.5, or "absent".
   - **commits-readable**: each commit message from 3.4.
3. If a fact is not in the package after you have read every section,
   record "absent". Do not infer it from general knowledge of the
   project, and do not take the description's word for what the diff
   or evidence contains.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Run the checks in rubric table order: diff-matches-plan,
   description-matches-diff, test-decisive, diff-clean,
   template-asks-met, ai-disclosure-met, thread-engaged,
   repo-checks-run, commits-readable. Grade every check, even after a
   required one fails.
2. For each check, apply the rubric's pass condition word for word to
   the pairing you built for it. Do not re-read the whole package
   unless a cell is "absent"; then search only the sections the
   evidence guide names for that family.
3. Grade:
   - `pass`: every row of the pairing meets the pass condition.
   - `fail`: at least one row breaks it. Name that row's quote.
   - `unclear`: only when the evidence the check needs is still
     "absent" after the targeted search in step 2.
4. Specific rulings:
   - diff-matches-plan fails on any hunk paired with "nothing" or with
     an out-of-scope line, and on any promise paired with "missing".
     A promise paired with a deferral note (in the plan's deviations
     or the description) passes. Tests for the fix and repo-required
     entries are not extra scope.
   - description-matches-diff fails on any claim marked "not in diff",
     and on any fidelity claim paired with an extra hunk. Do not let a
     polished description, checklist, or confident tone raise the
     grade: the diff decides.
   - test-decisive fails on any test-plan line marked "not run" that
     the PR does not explicitly defer, on evidence that only runs a
     control or unchanged path, and on "tested locally", "works on my
     machine", or "tests pass" with no before and after. A suite pass
     alone never passes this check.
   - diff-clean fails on any hunk marked "debris", even when the fix
     itself is correct.
   - template-asks-met passes when 3.1 recorded "no template" and no
     stated asks. An ask marked not-applicable with a reason passes.
   - ai-disclosure-met passes when 3.1 recorded "no AI policy".
   - thread-engaged passes when 3.2 recorded "none".
5. Write one evidence line per check: the quote or fact that decided
   the grade, naming both sides of the pairing for a fail (e.g. "hunk
   adds `--time-format` flag; plan: 'no new flags'").

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. Collect the grades of the required checks only.
2. Treat every `unclear` as `fail`, as the rubric's verdict rule says.
3. If every required check is `pass`, the verdict is `accept`.
   Otherwise it is `reject`. The preferred checks (repo-checks-run,
   commits-readable) never change the verdict; report them as notes.
4. The deciding check is the first failing or unclear required check
   in rubric table order. Quote its evidence line in the summary. List
   the other failing checks after it. If the verdict is accept, quote
   the test-decisive evidence as the reason the PR is ready.
5. Live mode only: check the draft title and description against
   `voice-guide.md` and list any broken rule in the summary, quoting
   the rule and the words that break it. This never changes the
   verdict.
6. Note any step in this procedure that did not fit the package (a
   procedure gap) in the summary.
7. Emit the JSON block from SKILL.md as the last thing in the output,
   with `item` set to the PR URL (live) or bundle id (eval). List every
   check in rubric order with its grade and its evidence line.

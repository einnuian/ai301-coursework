# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. Live mode only: read `scope.md` first. Confirm the issue is in the
   scoped repo; if it is not, or the `Repo:` line is still a
   placeholder, stop without grading. Note the house rules (a
   classmate's plan comment never counts as maintainer direction, and
   never excuses a piggybacked plan).
2. Read `rubric.md` and `references/evidence-guide.md`. Write down the
   check names in table order, which are required, and the verdict
   rule.
3. Read the package in this order, before grading anything:
   1. **Issue** (title, body): note the one behavior the issue asks to
      fix, in one sentence. This is the yardstick for scope-bounded.
   2. **Repro evidence**: note every step, every control run (a
      variant where the bug does NOT happen), and the expected vs.
      actual. Write one line: "the evidence pins the fault at/after
      ___ and rules out ___." Read this before the plan so the plan's
      confident wording cannot set your view of the cause.
   3. **Thread highlights**: note any maintainer or owner direction
      (an isolated culprit, a settled approach, a request to test, an
      open or prior PR). Write "none" if there is none.
   4. **Repo facts**: note the contribution policy's AI rules,
      specifically where disclosure is required (issues/comments,
      PRs only, or nowhere) and whether comments must be in the
      contributor's own words.
   5. **Candidate plan**: note its stated cause, its change (files,
      functions, approach), its in/out-of-scope lines, its test plan,
      and any stated unknowns.
   6. **Candidate plan comment**: read it last, against the notes
      from 3.3 and 3.4.


## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

1. In eval mode, the bundle is the only source. Quote lines from it
   and fetch nothing. In live mode, take the issue, thread, and repo
   policy from GitHub (via `gh`) at the locations the evidence guide
   names. Take the repro evidence from the student's posted repro
   comment (or, on the house issue, from the house repro pack as the
   drafts quote it). The drafts are the plan and the comment.
2. For each check, gather this and record a short quote:
   - **diagnosis-grounded**: the plan's cause sentence, plus each
     repro step or control that bears on it. For each control, note
     whether the blamed component works in it. If it does, the cause
     is ruled out.
   - **scope-bounded**: every distinct piece of work the plan's change
     section commits to (one line each), next to the issue sentence
     from Read order 3.1. Mark each piece "needed for the issue" or
     "extra". Also note the out-of-scope/deferred line.
   - **executable**: the named files, functions, or call sites, and
     the approach sentence. Collect any hedge words ("investigate",
     "somewhere", "or", "whichever", "maybe", "not sure").
   - **test-decisive**: the test plan sentence(s) and the observable
     each one names (exit code, output, color, assertion). Note
     whether that observable differs between today and after the fix.
   - **thread-engaged**: the maintainer direction from Read order 3.3
     and the comment's sentence that responds to it, or "no response".
   - **ai-policy-met**: the policy text from Read order 3.4 and the
     comment's disclosure sentence, or "no disclosure". Treat every
     package as AI-assisted.
   - **unknowns-honest**: each certainty claim in the plan ("the only",
     "confirmed", "always") and whether the package backs it.
3. If a fact is not in the package after you have read every section,
   record "absent" for it. Do not infer it from general knowledge of
   the project.


## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Run the checks in rubric table order: diagnosis-grounded,
   scope-bounded, executable, test-decisive, thread-engaged,
   ai-policy-met, unknowns-honest. Grade every check, even after a
   required one fails.
2. For each check, apply the rubric's pass condition word for word to
   the evidence you gathered for it. Do not re-read the whole package
   unless that evidence is "absent", and then search only the sections
   the evidence guide names for that family.
3. Grade:
   - `pass`: the gathered evidence meets the pass condition.
   - `fail`: the gathered evidence breaks it. Name the quote that
     breaks it.
   - `unclear`: only when the evidence the check needs is "absent"
     after the targeted search in step 2. If the package lacks it,
     that is unclear, not a guess.
4. Specific rulings:
   - diagnosis-grounded fails if any repro control shows the blamed
     component working, or shows the failure already present before
     the blamed step. It also fails when the plan adopts a thread
     diagnosis that the evidence contradicts. Do not let polish or
     length raise the grade.
   - scope-bounded fails if any piece of work is marked "extra" in
     Evidence gathering 2, even when the core fix is correct. Work the
     plan explicitly defers passes.
   - executable fails if the change names no location, or if any
     hedge word leaves the location or approach undecided. A hedge
     inside a stated unknown that does not change the approach passes.
   - test-decisive passes on a repro re-run with a stated expected
     result, or a regression test of the issue's case. It fails if the
     only tests are suite-wide, feel-based, or "nothing breaks".
   - thread-engaged passes automatically when Read order 3.3 found
     "none". A classmate's comment (live) is not maintainer direction.
   - ai-policy-met passes when disclosure is required only in PRs.
5. Write one evidence line per check: the quote or fact that decided
   the grade.


## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Collect the grades of the required checks only.
2. Treat every `unclear` as `fail`, as the rubric's verdict rule says.
3. If every required check is `pass`, the verdict is `accept`.
   Otherwise it is `reject`. The preferred check (unknowns-honest)
   never changes the verdict. Report it as a note.
4. The deciding check is the first failing or unclear required check
   in table order. Quote its evidence line in the summary. If the
   verdict is accept, quote the test-decisive evidence as the reason
   the plan is ready.
5. Live mode only: check the plan comment against `voice-guide.md` and
   list any broken rule in the summary, quoting it. This never changes
   the verdict.
6. Note any step in this procedure that did not fit the package (a
   procedure gap) in the summary.
7. Emit the JSON block from SKILL.md as the last thing in the output.
   List every check with its grade and its evidence line.


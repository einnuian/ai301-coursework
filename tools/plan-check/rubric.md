# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | plan's stated cause read against the repro evidence's steps, controls, and outputs (and any maintainer finding in the thread highlights) | the stated cause explains every behavior the repro shows and is not ruled out by any of its controls (e.g. a control run where the blamed component works, or output showing the failure already present before the blamed step runs); a cause that targets the symptom while the evidence points upstream fails, and adopting a thread's guess the evidence contradicts fails | required |
| scope-bounded | plan's change / in-scope statement and named files read against what the issue asks for | the change is the fix the issue needs and nothing more: no drive-by refactor, migration, new option, redesign, or "while in the area" work, even if the core fix inside it is correct; explicitly deferring related work as out of scope passes, and a scoped-down fix that says what it defers passes | required |
| executable | plan's change statement: named files/functions, chosen approach | a stranger could start without asking the author: the plan names where the change goes (file, function, or call site) and commits to one approach; "investigate", "somewhere", "X or Y, whichever is easier", or "not sure which layer" leaves a real decision to build time and fails | required |
| test-decisive | plan's test plan read against the repro evidence's steps and expected/actual | names an observable outcome that is false today and true after the fix (re-run of the repro with the expected result, a regression test asserting the issue's case, an exit code or output); "run the full test suite", "should feel fast", or "nothing else breaks" alone fails | required |
| thread-engaged | plan comment (and plan) read against the thread highlights' maintainer direction, settled approach, and open/prior PRs | if a maintainer/owner gave explicit direction (isolated a culprit, settled an approach, asked for testing, pointed at an open PR), the comment acknowledges it and either follows it or says why it diverges; pass if the thread has no such direction | required |
| ai-policy-met | repo-facts contribution policy read against the plan comment (treat every package as AI-assisted) | if the policy requires AI disclosure in issues or comments, the comment discloses the tool and extent; if it requires comments in the contributor's own words, the comment reads as a person's own specific words, not boilerplate; otherwise pass (a PR-only disclosure rule does not apply to the comment) | required |
| unknowns-honest | plan's risks/unknowns and certainty claims read against what the repro evidence and thread actually show | claims of certainty (confirmed root cause, "the only site", "no other callers") are backed by the package; what isn't is labeled as an unknown with how it will be checked | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Accept if every required check passes. Reject if any required check fails or is unclear: unclear counts as fail, because a plan you cannot verify from the package is not ready to post or build from. Preferred checks (unknowns-honest) never change the verdict; report them as notes only.

When several required checks fail, report all of them, but quote the evidence for the first failing check in table order (diagnosis-grounded before scope-bounded, and so on) as the deciding check: a wrong cause makes the rest moot.

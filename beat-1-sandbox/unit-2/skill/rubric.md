# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | report's environment record vs. the issue's target version/platform | OS/platform and tool version named, plus any factor the issue says matters (driver, shell, build profile); a version differing from the issue's is acknowledged | required |
| steps-followable | report's steps and inputs vs. the issue's trigger | a stranger with only public resources could re-run them and reach the issue's trigger; nothing private, unshared, or skipped | required |
| behavior-matches | report's artifacts vs. the issue's inputs and symptom | artifact shows the issue's own failure from the issue's own inputs (not altered input, an adjacent error, or just the tool running); an honest cannot-reproduce with a real attempt passes | required |
| honest-outcome | claims in both comments vs. the artifacts shown | every claim (confirmed, root cause, wider scope) is backed by a shown artifact, and the stated outcome matches it | required |
| claim-specific | claim comment vs. the issue | names this issue's behavior and a concrete next step; no +1, generic assign-me, or guaranteed fix/deadline | required |
| ai-policy-met | repo's AI/contribution policy vs. both comments (treat packages as AI-assisted) | if the policy requires AI disclosure in issues or comments, a comment discloses it; otherwise pass (a PR-only disclosure rule does not apply) | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Accept if every required check passes. Reject if any required check fails or is unclear: unclear counts as fail, because proof that cannot be verified is not ready to post. Preferred checks never change the verdict; report them as notes only.

On a live claim-only draft, only claim-specific and ai-policy-met are graded; the other required checks are reported unclear as "not yet applicable: claim-only draft" and left out of the verdict.

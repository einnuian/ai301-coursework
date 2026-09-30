# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

- Where it lives: eval, the "Environment:" line or block at the top of the "Candidate repro report", read against the version and platform in the "Issue" section, the "Thread highlights", and the "latest release" line of "Repo facts". Live, the environment section of the student's draft report, read against the issue body, its labels, and the repo's bug-report template (`.github/ISSUE_TEMPLATE/`).
- What good looks like: the OS/platform and the tool's version are both written down, and any factor the issue makes relevant (a Windows-only bug needs the Windows version and driver; a build-profile bug needs the profile; a shell-specific bug needs the shell) is named. A version different from the issue's target is fine only if the report says so. A log that "looks right" does not stand in for a missing environment record.

## Steps

- Where it lives: eval, the "Steps:" list and the commands, config files, and inputs inside the "Candidate repro report". Live, the steps in the draft report plus anything it links publicly.
- What good looks like: starting from a clean state, a stranger can type or copy every command and input and reach the issue's trigger. The trigger itself (the exact syntax, flag, config, or action the issue names) appears in the steps. Everything needed is in the package or publicly linked; "in our private monorepo", "with my usual config", or a skipped setup step makes it unfollowable. Short is fine when complete.

## Behavior shown

- Where it lives: eval, the fenced output blocks, logs, and "Actual:" lines in the "Candidate repro report", read against the symptom described in the "Issue" section (its error text, exit code, output, or crash) and any maintainer clarification in "Thread highlights". Live, the same parts of the draft report against the live issue body and thread.
- What good looks like: the input shown is the issue's input (compare it character by character: a colon instead of `=`, a prefix range instead of an offset-from-end range, or a variable left unbound changes the bug being tested), and the artifact shows the issue's failure kind, not a neighbour: a panic vs a graceful syntax or argument error, exit 101 vs exit 1, a crash vs garbled output with the process still alive, the reported error vs an older version's different error. A control run (the same thing without the trigger behaving correctly) strengthens it. An artifact that only shows the tool starting, a version banner, or a session list shows nothing about the bug.

## Honesty

- Where it lives: the claims in the "Candidate claim comment" and the "Expected"/"Actual"/conclusion lines of the "Candidate repro report", each matched against the artifacts actually shown.
- What good looks like: every "confirmed", "verified", "root cause is", "guaranteed reproducible", or "also affects X" points at an artifact in the package that shows it. An honest cannot-reproduce passes: it shows a real attempt at the issue's trigger, the output it got, and names what differed from the reporter's setup. A report fails when its narration outruns its artifacts: a root-cause diagnosis with no transcript, certainty over an artifact that shows a different symptom, or "expected" stated so that it contradicts what the issue asks for.

## Comms

- Where it lives: eval, the "Candidate claim comment" read against the "Issue" section, and both comments read against the "contribution policy" and "bug reports" lines of "Repo facts". Live, the draft claim comment against the issue page, and the repo's `CONTRIBUTING.md`, `AI_POLICY.md`/`AI_USAGE_POLICY.md`, and issue templates; in the Path Review repo, apply the house rules from `scope.md` (a classmate's claim does not block a new claim).
- What good looks like (claim): the comment could only be about this issue: it names the specific behavior and says what the author will do next (reproduce, test the patch, report back), and promises only a report or an attempt, never a guaranteed fix or deadline. A +1, a "please assign me" that would fit any issue, or flattery with no plan fails.
- What good looks like (AI policy): course packages are AI-assisted work. When the repo's policy requires disclosing AI use in issues or comments, a comment must say AI was used (and the tool and extent, if the policy asks). A disclosure rule that applies only to pull requests does not bind comments; a "comments in your own words" rule is met by a specific, human-voiced comment. No stated AI policy means nothing to disclose.

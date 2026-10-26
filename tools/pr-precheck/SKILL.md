---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: **is this
ready to submit?** A PR package is a candidate pull request (its
title, its description, its commits, its diff, and its test evidence)
read against the plan it claims to implement (deviation notes
included) and the issue that plan belongs to, including the
reproduction evidence the plan built on and the repo's stated asks.

Answer only that question, about only that one package. Do not review
the code for style, propose fixes, re-grade the plan as a plan, or
judge whether the issue was worth taking; those questions belonged to
earlier weeks. Do not answer from gut feel: you answer by executing
`procedure.md`, which applies the checks in `rubric.md` to evidence
gathered where `references/evidence-guide.md` says it lives.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (a
`packages/*.md` file or its `.json` twin, or bundle text inlined by
the eval harness), you are in eval mode. Otherwise you are in live
mode, checking the student's own branch.

**Live mode** reads exactly these inputs and nothing else:

- **The student's files, as the live-run command names them.** Read
  the files the command that invoked this run names (step 10 of the
  unit's activity): the student's `plan.md`, with its `## Deviations`
  section; the draft PR title and description; and the captured test
  evidence (before/after output and the repo's own checks or suite
  output). Read only those files. Other notes, scratch files, or
  drafts in the working directory are not part of the package.
- **The diff.** Run `git diff main...HEAD` (three dots) from the
  student's working copy. That is everything the branch changes
  relative to the repo's default branch, and it is the only source of
  truth for what changed. Also list the branch's commits with
  `git log main..HEAD --oneline`. Do not read the diff from anywhere
  else, and do not trust the description's account of it.
- **The issue thread.** Fetch the issue the plan belongs to, its body
  and every comment, from GitHub (`gh issue view <number> --repo
  <scoped repo> --comments`). Note any explicit maintainer direction
  and the student's own posted claim, repro, and plan comments.
- **The repo's stated asks.** Read the scoped repo's PR template
  (`.github/pull_request_template.md` or wherever the repo keeps it)
  and `docs/CONTRIBUTING.md`, including any AI-use disclosure policy.
  Read them from the repo's default branch, not from the student's
  branch.

A house-chain student (routed to the house issue) reads the house
plan and the house repro pack in place of their own plan and repro
comments; every check grades the same things there. If any live input
above cannot be found or read, do not fill the gap from memory: grade
the checks that depend on it `unclear` and say which input was
missing in the summary.

**Eval mode**: the bundle is the whole world. Every fact comes from
the bundle text: its issue context and thread highlights, its
repo-facts block (template asks and stated policy), its plan-context
block (plan, deviation notes, repro evidence), and the candidate PR's
title, description, commits, diff, and test evidence. Fetch nothing,
run no `git` or `gh` commands, and do not look up the real issue named
on the `source:` line; it has moved on since the capture date, and the
bundle wins. Eval mode always grades a complete package: every check
in the rubric, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else, before any input above.

- If its `Repo:` line still carries an unfilled placeholder (a
  bracketed value or placeholder text rather than an `owner/repo`
  name), stop without grading. Tell the student to fill the `Repo:`
  line in `scope.md` with their section's Path Review repo, and emit
  nothing else. Never guess a scope.
- If the student's PR targets any repo other than the one on the
  `Repo:` line (check the issue URL and the remote the branch will
  open against), refuse to grade it and say which repo is in scope.
- Otherwise, apply the house rules `scope.md` lists as part of how
  evidence is read in that room: the PR comes from the student's
  fork's `fix/<issue-number>-<slug>` branch against the default
  branch; one bounded PR per issue per student; the PR template is
  always used, every section with real content; a classmate's PR on
  the same issue does not block this one. A house rule changes what
  counts as passing evidence; it never adds a verdict outside
  accept or reject.

In eval mode, ignore `scope.md` entirely; do not read it.

## The voice seam (live mode only)

In live mode, after reading `scope.md`, read `voice-guide.md`: the
student's own rules for how they write upstream. Hold the outgoing PR
text, the draft title and the draft description, against every rule
in it. For each rule the draft breaks, report it in the summary:
quote the rule, quote the offending words, and say what would bring
the draft back in line.

The voice guide never changes the verdict on its own. Voice is the
student's own standard and carries no gold label; only a check in
`rubric.md` that explicitly reads the voice guide may let it touch a
grade. If the voice guide is still an empty template, say so in the
summary and continue grading.

In eval mode, ignore `voice-guide.md` entirely; the universal
communication-quality checks live in the rubric.

## Component reads

Read the components in this order before grading anything:

1. `rubric.md` is the brain. Its checks table names each check, the
   evidence it reads, its pass condition, and its weight (`required`
   checks gate the verdict; `preferred` checks never change it). Its
   verdict rule says how grades combine into accept or reject.
2. `references/evidence-guide.md` is the map. For each evidence
   family (plan fidelity, test evidence, diff quality, standards and
   comms) it says where the evidence lives in a bundle and, live, in
   the working copy and on GitHub, and what good looks like there.
3. `procedure.md` is the hands. Execute it as written: its read
   order, its evidence-gathering moves, its check execution, its
   verdict assembly. Follow it exactly, the way an executor follows a
   rubric.

Where the procedure is silent on a step you need, do not improvise
around it. Take the narrowest reading the rubric and evidence guide
support, and name the gap in the summary ("procedure gap: no step
for ..."); a procedure gap is feedback the student needs.

If `rubric.md` has no checks or no verdict rule written in, or
`procedure.md` has no steps written in, stop and say so: this tool
cannot grade without a rubric AND a procedure, and that is by design.
Instruction comments left in a template do not count as content; only
written text does. Never invent checks at runtime.

## Verdict and output

The verdict space is binary: `accept` (ready to submit: open the PR)
or `reject` (hold: fix first). There is no third verdict, no "accept
with reservations", and no score. Reservations go in a check's
evidence line or the summary, never in the verdict.

Before the JSON block, write a short readable summary: one line per
check with its grade and the deciding fact, the deciding check for a
`reject`, any procedure gaps, and, in live mode, any voice-guide
rules the draft breaks. Then end your reply with the fenced JSON block
below, filled in, with nothing after it. `item` is the PR URL (live;
if the PR is not opened yet, the branch name and the issue URL) or
the bundle id (eval). List every rubric check in `checks`, in rubric
order, using the rubric's check names. The block must be valid JSON
and the last thing in the reply: the harness parses the last fenced
JSON block in the output.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it: a diff file and hunk, a plan line, a test
  output line, a template section. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be
  ready, and a beautiful, confident one can be hiding drift. Read the
  diff against the plan and its deviation notes, the evidence against
  the plan's test plan and the repo's checks, and the description
  against the diff; never grade length, formatting, or tone of
  confidence. A shortfall the PR honestly discloses is not drift.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs. Where the rule is silent, an `unclear` grade
  on a required check counts as `fail`: a PR you cannot verify from
  the package is a PR that is not ready to submit.

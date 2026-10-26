# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

[Link to the pull request you opened. It must be the pull request's own page on the Path
Review repo, not your fork's branch page.]

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/266

**Branch**

[The name of the branch the pull request comes from, exactly as it appears in your fork.
The naming shape is a type prefix, then the issue number, then a short description. **The
issue number in the branch name must be the number of the issue the pull request fixes** —
a name carrying any other number does not satisfy this field.]

fix/35-review-webhooks

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

1. 20/20 (one full run; `agreement: 20/20 scored items  (bar: 18/20: PASS)`)


**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

pkg-09: my rubric said reject, and the gold label is reject. The gold note calls it "arguable because the extra work is adjacent", since the fix itself (surfacing the inner parse error) is right. My rubric still rejected it on `diff-matches-plan`, because the plan says "Not in scope: new flags, other error messages, or accepted date formats", yet the commits include "3d0c6b1 cli: add --time-format for custom date parsing" and "7a95d33 cli: harmonize error message wording across flags". The description claims "Error messages for date parsing improved exactly as planned". The check fails on anything "the plan does not name ... especially something the plan scopes out", so being adjacent to the fix doesn't save it.


**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/pr-precheck/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

> | diff-matches-plan | every changed file and hunk in the diff (live: `git diff main...HEAD`), read against the plan's change statement, in/out-of-scope pair, and deviation notes | every hunk serves a change the plan or a deviation note names (tests for the fix and entries the repo's stated asks require, such as a changelog line, count as part of the plan), and every change the plan promises appears in the diff or is explicitly deferred in a deviation note or the description; fails if the diff adds anything the plan does not name (an extra refactor, rename pass, new config option or flag, rewrite of a file the plan never touches), especially something the plan scopes out, or drops a promised piece with no note. A disclosed deferral passes: less than everything is not drift when it is said | required |

It reads in both directions because silent drift goes both ways: pkg-03 adds work the plan scopes out, while pkg-17 drops a promised docs update. "A disclosed deferral passes" is there so pkg-13 and pkg-16, which defer work and say so in the plan and the description, aren't rejected for doing less than everything. The parenthetical about tests and changelog lines was my one revision: right after writing the first version, I saw that a strict reading would flag a repo-required changelog entry (the kind pkg-01 is rejected for missing) as unplanned scope, so I added it.


**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

A case I accept it will miss: the deferral clause trusts any deferral that is stated. A PR that drops the hard half of its plan and writes one sentence saying so passes `diff-matches-plan`, even if a maintainer would call the deferral unreasonable. I kept it because the gold labels accept pkg-13 and pkg-16 on exactly that basis ("ready as scoped"). The reason nothing else changed: the confirming full run agreed on all 20 packages, including every silent-drift package (4/4) and both standards-wall packages (2/2).


---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

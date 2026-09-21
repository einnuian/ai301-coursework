# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[The individual Path Review issue page. A link to the repository or the issue list does not satisfy this field.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
[s
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35",
    "checks": [
      {"name": "commits-alive", "grade": "pass", "evidence": "Law Burke (human), 4 days before today"},
      {"name": "unclaimed", "grade": "pass", "evidence": "repo pulls?state=all returns [] — 0 linked/open PRs"},
      {"name": "policy-allow-ai", "grade": "pass", "evidence": "No AI restriction in docs/CONTRIBUTING.md; no AI policy files found"},
      {"name": "repo-size", "grade": "pass", "evidence": "Same repo-wide LOC proxy as above, within 5k-50k band"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

Run #1 - 17/20

Run #2 - 14/20

Run #3 - 10/20

Run #4 - 18/20

**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

`issue-03` was rejected according to both the gold labels and my rubric. The reason for this is that the last 5 default-branch commits were from 2024, way past the 30-day window that I used in my rubric.

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| policy-allow-ai | read  README.md or CONTRIBUTING.md | does not explicitly reject AI-generated code | required |

Initially, I wrote "accepts AI-generated code" as the pass condition. However, this lead the LLM to look explicitly for permission to submit AI-generated code in the repo, leading to rejection of the issue. Therefore, I rephrased it to the current form.

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

There are no trade-offs as far as I know. The pass condition is handled well in all 20 given issues.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

Answer:

1. I have web app development experience and would like to work on API-related features. The issue seems relatively straightforward.
2.  I did not implement 'estimated effort' as a check in the rubric because I thought it would be arbitrary for an LLM to decide the amount of effort. I left this to my own judgement.
3. I should be able to address the issue within the estimated hours given (8-12 hours), if not less.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

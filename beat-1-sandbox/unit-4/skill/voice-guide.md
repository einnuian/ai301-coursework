# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a student making a first contribution to Path Review, with web app and API experience but new to this codebase. When I comment, I say what I have actually done, what I will do next, and nothing more. Readers can expect a report back, not a guarantee.

In a pull request I'm asking a maintainer to spend review time on my work. The title and description are a map of the diff: they say what changed, how I proved it, and what I left out, and nothing in them should surprise someone who then reads the diff.

## Rules I write by

### Rule: promise the next step, not the result

Say what I will do next (reproduce, report, try a fix). Never promise a fix, a deadline, or success.

- Wrong: "I'll have this fixed by Friday."
- Right: "I'll reproduce this locally and post a repro report here before starting on a fix."

### Rule: name the bug, not the project

Every comment has to point at this issue's specific behavior, so it could not be pasted onto another issue.

- Wrong: "This looks like a great issue for me, can I take it?"
- Right: "I'd like to work on the endpoint returning the wrong status described above."

### Rule: only claim what I've seen

If I haven't run it, I don't say it happens. Before reproducing, I say "the issue reports", not "I confirmed".

- Wrong: "I confirmed this happens because the handler skips validation."
- Right: "The issue reports this on the current main; I haven't reproduced it yet."

### Rule: plain words, no flattery

Skip the greetings, the praise, and "sir". Get to the point in a few sentences.

- Wrong: "Hello sir! Amazing project, I would be so honored to contribute!"
- Right: "Hi, I'd like to take this one."

### Rule (PR): the title says what the code now does

The title names the change in the code, in a few words a reviewer can judge from the PR list. Not the issue number alone, not "fix bug", not excitement.

- Wrong: "Fix #35" / "Webhooks finally working!!"
- Right: "Add account-level webhooks for review completion (#35)"

### Rule (PR): describe the diff, all of it and only it

Every sentence about what changed has to match a hunk in `git diff main...HEAD`, and every hunk has to be mentioned or obviously part of something that is. I never write "exactly as planned" or "no other changes" unless I've just reread the diff against `plan.md`. If something changed from the plan, I say what and why, and the same note is in `plan.md`'s Deviations.

- Wrong: "Implements the plan exactly, no other changes." (when the diff also renames a model field)
- Right: "Adds `POST /webhooks`, the `webhooks` table (migration 003), and a send at the end of `process_review()`. One change from the plan: I renamed `Review.callback` to `callback_url` to match the route; noted in the plan's deviations."

### Rule (PR): show the proof, don't assert it

Name what I ran and what it printed before and after, matched to my plan's test plan. "Tests pass" only counts next to the command and its result, and never on its own.

- Wrong: "Tested locally, works great, all tests pass."
- Right: "Re-ran my repro: before, the listener got 0 requests; after, it got one `POST` with the review payload when the review completed. `pytest` passes (N passed)."

### Rule (PR): say what's not in it, once, as a fact

If the PR does less than the issue or plan could cover, I say what is left out, why, and where it goes next, in one plain sentence. No apology, no hedging.

- Wrong: "Sorry, I didn't really get to retries, hope that's okay!"
- Right: "Not in this PR: retrying failed deliveries. The plan deferred it; I'll open a follow-up issue."

### Rule (PR): disclose AI help specifically, in my own words

Say which tool I used, for what, and that I checked the result. Fill every template section with real content, and mark one not applicable with a reason rather than deleting it.

- Wrong: "AI was used." / a template section left as its placeholder text
- Right: "I used Claude to help draft the webhook service and this description; I've read every line, and ran the repro and tests above myself."

## Things I never post

- A guaranteed fix or a deadline ("within 2 days", "guaranteed").
- "+1", "same here", or "same as above" with no evidence of my own.
- A root cause I haven't shown with output.
- "Please assign me" without saying what I'll do.
- Text an AI wrote that I haven't read, understood, and put in my own words.
- "Exactly as planned" or "no other changes" over a diff I haven't reread against the plan.
- "Tests pass", "works on my machine", or "should work" with no command and output next to it.
- A description that claims a change (docs, tests, a second fix) the diff doesn't contain.
- A PR template with sections deleted or left as placeholder text.
- An apology in place of a stated shortfall.

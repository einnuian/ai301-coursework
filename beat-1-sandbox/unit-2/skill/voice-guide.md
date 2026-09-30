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

## Things I never post

- A guaranteed fix or a deadline ("within 2 days", "guaranteed").
- "+1", "same here", or "same as above" with no evidence of my own.
- A root cause I haven't shown with output.
- "Please assign me" without saying what I'll do.
- Text an AI wrote that I haven't read, understood, and put in my own words.

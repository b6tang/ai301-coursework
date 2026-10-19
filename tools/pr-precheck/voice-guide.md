# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2 and carried it through week 3; paste your filled voice-guide.md
here, whole. It is not re-authored and it is not graded as new work
this week.

Then reread it with the PR in mind. Your comments promised, reported,
and committed to an approach; a PR title and description ask a
maintainer to spend review time on your work. If your rules do not
cover that register (for example: how a title earns its thirty
seconds, how a description promises exactly what the diff contains,
how a disclosed shortfall is worded so it reads as honesty rather
than apology), extend the guide with what it needs. Extending is
allowed and encouraged; starting over is not required.

Live mode reads this file and holds your draft PR title and
description against it, reporting any rule your draft breaks. Eval
mode ignores it entirely, because your voice is yours and carries no
gold labels.
-->

## Who I am in threads

<!-- Paste your section here. -->

I am a newer contributor to this repository, working through an issue by first reproducing and understanding it before attempting a change. Readers can expect me to be specific about what I tested, separate what I observed from what I assume, and be clear when I am still investigating.

## Rules I write by

<!-- Paste your rules here, wrong/right pairs and all. Add any rule
the PR register needs that your comments did not. -->

### Rule: Do not promise what I cannot guarantee

I describe the next thing I plan to do, not a deadline or a successful fix that I cannot know in advance.

- Wrong: "I'll fix this by tomorrow."
- Right: "I'll reproduce the issue first and share what I find."

### Rule: Name the specific behavior

When I refer to an issue, I name the concrete behavior, command, endpoint, or condition involved instead of using vague phrases like "this bug." If a version matters to the issue or my reproduction, I include it.

- Wrong: "I'm looking into this bug."
- Right: "I'm looking into #67, where POST /reviews can create a review for a profile that does not belong to the authenticated user."

### Rule: Write like I actually talk

I avoid exaggerated praise, filler, and overly formal or bot-like phrasing. I keep the comment focused on what I observed and what I am doing next.

- Wrong: "I am extremely excited to contribute to this amazing project and resolve this issue!"
- Right: "I reproduced the reported behavior and I'm checking where the validation should happen."

### Rule: Separate evidence from assumptions

I state confirmed observations as observations and label explanations or possible causes as things I am still investigating.

- Wrong: "The validation logic is definitely broken."
- Right: "The request succeeds without the expected validation; I'm checking where that validation is supposed to occur."

### Rule: Respect maintainer direction in the plan comment

When a maintainer has already given a relevant direction or constraint, I acknowledge it and keep my proposed approach within that direction unless I clearly explain why I think a different approach is needed.

- Wrong: "I'll change the API to fix this."
- Right: "I'll keep the current API as requested and make the change within the existing validation path."

### Rule: Name the changed behavior in the PR title

I use the title to name the concrete behavior this PR changes. I avoid vague titles such as "fix bugs" or "improve things."

- Wrong: "Fix review bugs."
- Right: "Reject review creation for profiles owned by another user."

### Rule: Match PR claims to the change and its evidence

I describe what the diff actually changes and distinguish it from what I verified. I state known limitations directly, without broad success claims or apologetic filler.

- Wrong: "This completely fixes all review validation problems. Sorry I could not test everything."
- Right: "This adds an ownership check to POST /reviews. I verified the reported request; concurrent requests were not tested."

## Things I never post

<!-- Paste your list here; extend it if PR descriptions tempt you
toward new ones (claiming coverage the evidence does not show is the
classic). -->

- Deadlines or promises that I cannot guarantee.
- Claims that I reproduced or fixed something before the evidence supports it.
- Generic comments that could be pasted onto any issue.
- Exaggerated praise, filler, or bot-like language.
- Guesses presented as confirmed facts.

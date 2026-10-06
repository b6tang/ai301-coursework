# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives**
- Eval bundle: `Candidate plan`, specifically the text that states the diagnosis or cause and the proposed approach; read it against `Repro evidence`, especially the reproduction steps, inputs, observed behavior, and artifacts.
- Live mode: the diagnosis or cause and proposed approach in the draft plan, read against the student's posted repro comment on the issue.

**What good looks like**
The stated cause explains behavior the repro evidence actually shows and does not contradict the reproduced observations. The proposed approach addresses that cause rather than only the visible symptom.

## Scope

**Where it lives**
- Eval bundle: `Candidate plan`, specifically the in-scope statement, not-in-scope statement, and named files or areas; read them against `Issue`, and `Repro evidence`.
- Live mode: the in-scope and not-in-scope statements and named files or areas in the draft plan, read against the live issue thread and the student's posted repro comment.

**What good looks like**
The planned work stays within one bounded change tied to the reproduced issue. Unrelated cleanup, redesign, broad refactoring, or additional features are kept outside that change.

## Executability

**Where it lives**
- Eval bundle: `Candidate plan`, specifically the named files or areas, implementation approach, and stated order of work.
- Live mode: the named files or areas, implementation approach, and order of work in the draft plan.

**What good looks like**
A contributor who did not write the plan can identify where to begin, what behavior is meant to change, and the intended approach without asking the author to supply the core implementation strategy.

## Test plan

**Where it lives**
- Eval bundle: `Candidate plan`, specifically the proposed test or verification steps; read them against `## Repro evidence`, especially its reproduction steps, inputs, observed failure, and artifacts.
- Live mode: the test or verification steps in the draft plan, read against the student's posted repro comment.

**What good looks like**
The test plan connects to the reproduced behavior and names an observable result that would distinguish the intended fixed behavior from the original failure. A vague instruction such as only running tests or checking that the fix works is not enough.

## Honesty

**Where it lives**
- Eval bundle: `Candidate plan`, specifically statements of risks, assumptions, unknowns, and any recorded deviations; read them against what `Issue`, and `Repro evidence` actually establish.
- Live mode: the risks, assumptions, unknowns, and deviation notes in the draft plan, read against the live issue thread and the student's posted repro comment.

**What good looks like**
Claims that are not established by the available evidence remain identified as assumptions or unknowns rather than being stated as facts. A known mid-build deviation is recorded explicitly rather than silently treated as part of the original plan.

## Comms

**Where it lives**
- Eval bundle: `Candidate plan comment`, read against `Candidate plan` for the intended work, `Issue` and `Thread highlights` for maintainer or thread constraints, and `Repo facts` for stated templates, contributing asks, contribution policy, and AI-use disclosure requirements.
- Live mode: the draft plan comment, read against the draft plan, the live issue thread, and the repository's stated templates, contribution documentation, and contribution policy.

**What good looks like**
The comment accurately reflects the intended plan, responds to relevant maintainer or thread direction, and follows applicable repository-stated communication and disclosure requirements. It is specific to the issue rather than generic boilerplate.
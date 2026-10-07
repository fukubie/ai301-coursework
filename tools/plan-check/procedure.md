# Procedure: grading a plan and draft comment

Follow these sequential stages to evaluate the target package or draft files against the rubric checks.

## Read order

1. Read the issue description, thread highlights, and repository facts first to understand the defect context, reporter intent, and constraints.
2. Read the reproduction evidence to observe the baseline failure, commands run, stack traces, and observed terminal outputs.
3. Read the plan draft (`plan.md`), specifically inspecting the diagnosis, scope statement, files to touch, detailed approach, test plan, and risks/unknowns.
4. Read the draft comment (`comment.md`) to evaluate its structure, tone, and alignment with the plan and thread discussion.

## Evidence gathering

1. Extract the claimed root cause from the plan and extract the concrete error message, stack frame, or unexpected behavior from the repro evidence.
2. Compare the files listed under files to touch against the repository structure, identifying whether all listed files are necessary for the fix.
3. Compare the proposed changes in the approach section against the issue requirements to spot any feature additions, unnecessary cleanup, or architectural overhauls.
4. Locate the exact commands and expected terminal outputs in the test plan, comparing them directly against the repro reproduction commands.
5. Identify any risks, assumptions, or open unknowns documented in the plan against the behavior of the modified codepaths.
6. Compare the draft comment's content against repo conventions and specific requests or constraints raised by maintainers in the thread highlights.

## Check execution

1. Grade `diagnosis_grounding`: assign `pass` if the proposed root cause directly explains the failure shown in the repro evidence. Assign `fail` if the explanation targets an unverified symptom or contradicts the repro trace. Assign `unclear` if repro evidence or diagnosis is missing.
2. Grade `bounded_scope`: assign `pass` if the change modifies only what is strictly necessary to resolve the bug. Assign `fail` if the plan includes unrelated refactoring, speculative features, or touched files outside the issue boundary.
3. Grade `execution_clarity`: assign `pass` if a stranger could execute the fix step-by-step without having to make arbitrary architecture or logic decisions. Assign `fail` if the approach is vague (e.g., "handle errors properly" without specifying how or where).
4. Grade `observable_validation`: assign `pass` if the test plan provides clear commands and explicitly states the expected observable output that differentiates a pass from the baseline repro failure. Assign `fail` if tests are omitted, purely theoretical, or lack concrete expected outputs.
5. Grade `risk_and_unknowns`: assign `pass` if realistic edge cases, failure modes, or unknowns are identified and handled. Assign `fail` if high-risk side effects are ignored or hand-waved with unwarranted certainty.
6. Grade `thread_and_conventions`: assign `pass` if the draft comment satisfies all repo conventions and directly answers any maintainer questions in the thread. Assign `fail` if conventions or explicit maintainer constraints are ignored.

## Verdict assembly

1. Tabulate the grades for all required checks.
2. If every required check receives a grade of `pass`, set the verdict to `accept`.
3. If one or more required checks receive a grade of `fail` or `unclear`, set the verdict to `reject`.
4. Preferred checks do not flip or alter the final binary verdict.
5. Output the result formatted according to the required JSON schema with individual check evaluations and the overall verdict.
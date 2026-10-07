# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis_grounding | The plan's stated root cause compared against the failure trace and observations in the repro evidence | The identified root cause directly accounts for the specific error or unexpected output demonstrated in the reproduction, rather than addressing an unrelated issue or superficial symptom. | required |
| bounded_scope | The plan's files to touch and approach sections compared against the repo facts and issue scope | The proposed modifications are strictly limited to fixing the verified defect without introducing opportunistic refactors, unsolicited features, or out-of-scope architectural changes. | required |
| execution_clarity | The approach, logic modifications, and specific file targets listed in the plan | The steps are concrete enough that an unfamiliar engineer could implement the fix directly from the plan without having to guess the intended logic, algorithm, or target locations. | required |
| observable_validation | The test plan steps and expected outputs compared against the repro commands and baseline failure | The test plan includes concrete commands and verifies explicit, observable output differences (e.g. expected exit code, return value, or output text) showing the original defect is resolved. | required |
| risk_and_unknowns | The risks, unknowns, and edge cases listed in the plan compared against the touched code paths | Potential regressions, edge cases, or genuine unknowns in the affected codepath are explicitly acknowledged rather than assumed away with false certainty. | required |
| thread_and_conventions | The draft plan comment in comment.md compared against the issue thread highlights and repo conventions | The draft comment adheres to the repository's stated style and contribution conventions, addresses any explicit maintainer or thread constraints, and provides a clear summary. | required |

## Verdict rule

accept if every required check passes (grade is pass); reject if any required check fails (grade is fail) or is indeterminate (grade is unclear). Preferred checks do not affect the binary accept/reject verdict.
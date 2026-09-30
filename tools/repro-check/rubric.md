# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| conventions | The package text (claim or repro comment) read against the repository's stated contribution policies or AI disclosure rules in repo-facts. | The package explicitly adheres to all repository-specific requirements; if the repository mandates disclosing AI tool usage or assistance, an explicit disclosure statement is present. | required |
| voice | The proposed claim or report text read against `voice-guide.md`. | The comment remains objective, professional, and non-presumptive: claims commit only to investigating or attempting reproduction (never asserting a guaranteed fix, ETA, or diagnosis before evidence is gathered), and reports state observed outcomes honestly without speculation. | required |
| environment | The environment details recorded in the report read against the dependencies and runtime requirements of the target project. | Specifies the concrete runtime, operating system, and tool versions (or container/commit hash) necessary for an independent developer to mirror the test conditions. | required |
| followable_steps | The setup, command sequences, and input payloads detailed in the reproduction steps. | A developer familiar with standard project workflows could follow the sequence from a clone/baseline to trigger the target condition without guessing non-obvious configurations, custom undocumented flags, or hidden prerequisites. Standard developer actions (like changing directories or running standard package managers) may be assumed. | required |
| target_alignment | The recorded artifacts, error traces, and observed terminal outputs read against the original issue's reported failure mode. | The behavior, error message, stack trace, or exit status corresponds directly to the failure domain reported in the issue (including reasonable variations across environments or dependencies), or, for an honest cannot-reproduce report, directly tests the described scenario. It only fails if the artifact demonstrates an unrelated setup/installation failure or completely unrelated bug. | required |

## Verdict rule

- **Accept (ready):** All `required` checks pass (`pass`).
- **Reject (hold):** Any `required` check fails (`fail`) or is determined to be indeterminate (`unclear`).
- **Preferred checks:** Provide qualitative feedback but do not alter the final binary verdict.
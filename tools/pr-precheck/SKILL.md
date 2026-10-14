---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about a single PR package: **is this ready to submit?** A PR package consists of a candidate pull request (its title, description, commit history, diff, and test evidence), evaluated side-by-side against the implementation plan it claims to execute and the issue thread that plan belongs to. Do not grade more than one package per run, do not evaluate questions outside submission readiness, and never rely on intuition—execute the components in this directory strictly as written.

## Inputs and modes

Operate in one of two modes depending on the invocation target:

- **Live mode**: Activated when checking a local branch and draft PR before opening it upstream. Read these exact inputs:
  1. `plan.md` (including any `## Deviations` section) in the working copy root (or the assigned house plan for house-chain students).
  2. The branch diff relative to the default branch, produced by running `git diff main...HEAD` (three dots) from the working copy root.
  3. `pr_draft.md` in the working copy root, containing the draft PR title on line 1 and the filled PR description below it.
  4. `test_evidence.md` in the working copy root, containing before/after reproduction outputs (or the house repro pack) and the outputs of the repository's required checks.
  5. Issue-side and repository context fetched from the target repository: the issue thread, `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md`.
- **Eval mode**: Activated when grading a frozen evaluation package bundle (e.g., `eval/packages/*.md`). The package bundle is the entire world. Extract all facts (issue summary, approved plan, diff, test evidence, PR description, and repository standards/template rules) solely from the bundle text. Fetch nothing externally and read no local repo files. Run every check in `rubric.md` and apply the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before reading any other file or running any check:
- Verify that the `Repo:` line specifies a concrete repository and does not contain an unfilled bracketed placeholder. If the `Repo:` line still carries a placeholder, halt immediately without grading and instruct the user to fill the `Repo:` line in `scope.md` with their section's Path Review repository (`codepath/pathreview-ai301-fa26-s3`).
- Refuse to grade any pull request targeting a repository outside the one named in `scope.md`, and enforce all Path Review house rules defined there.
- In eval mode, ignore `scope.md` completely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and evaluate the outgoing PR text (`pr_draft.md` title and description) against its rules:
- Report any broken voice rule in the readable summary prior to the JSON output block, quoting the offending phrase and naming the violated rule.
- Do not allow a voice-guide violation to alter the `accept` or `reject` verdict on its own unless a check in `rubric.md` explicitly evaluates it.
- In eval mode, ignore `voice-guide.md` completely.

## Component reads

Connect and execute the files in this directory as follows:
- Read `rubric.md` to load the required and preferred checks and the verdict rule.
- Read `references/evidence-guide.md` to locate where each evidence family resides in an eval bundle or live workspace and what constitutes valid evidence.
- Execute `procedure.md` step-by-step in the exact order written.
- **Gap rule**: If `procedure.md` is silent on a step or leaves an operation ambiguous, report the procedure gap explicitly in your summary rather than silently improvising around it.
- **Refusal rule**: If `rubric.md` or `procedure.md` contains no written content beyond template comments, refuse to grade immediately and state which file is empty. Never invent checks or procedures at runtime.

## Verdict and output

Produce a binary verdict: `accept` (ready to submit) or `reject` (hold). Do not output intermediate verdicts, scores, or conditional accepts.

First, write a concise, readable per-check summary (and any live-mode voice findings). Then, end your reply with the exact fenced JSON block below—valid, unaltered, and last, with no text following it:

```json
{
  "item": "<PR URL bundle id or>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```
## Grading discipline

Apply these rules to every check:

- **Evidence first:** Cite the specific file, diff hunk, command output, or quotation that determines each grade. Unsupported judgments such as "Looks good" are not evidence.
- **Grade the artifact, not its polish:** Evaluate the artifact against the plan, issue, and repository standards. Do not grade prose length, formatting, or confidence.
- **Follow the rubric:** Apply its written pass conditions. If a check meets them, mark it pass even if the result feels wrong.
- **Follow the procedure:** Execute `procedure.md` in order. Report missing steps; do not invent workarounds.
- **Treat unclear required checks as failures:** Follow the rubric's verdict rule. If it is silent, an unclear or unverifiable required check fails and the verdict is `reject`.

---

### 2. `~/.claude/skills/pr-precheck/rubric.md`

*(Notice how these 4 checks map 1-to-1 to the 4 failure categories and explicitly bake in the **honest-outcome rule** so disclosed deviations/shortfalls pass when honest, while silent drift or unrelated changes fail).*

```markdown
# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `plan_fidelity` | The diff's changed files and logic (`git diff main...HEAD` or bundle diff) read side-by-side against the approved plan (`plan.md` plus `## Deviations`, or bundle plan) and the PR description's `Changes` and `Notes for Reviewers` sections. | Every modified file and behavioral change in the diff is within the approved plan's scope OR is an honestly disclosed deviation/shortfall explained in both the plan's deviation notes and the PR description; the diff contains no silent scope creep, no undisclosed missing plan steps, no unrelated opportunistic refactors, and the PR description accurately matches what the diff actually changes. | required |
| `test_evidence` | The claimed test and reproduction output (`test_evidence.md` and PR description `Testing` section, or bundle test evidence) read against the plan's test plan, the issue's reproduction steps, and the repository's required check suite (`make test-unit`, `make test-integration`, `make lint`, `make typecheck`, plus xfail/suppression cleanup rules). | Concrete command output is present proving the bug is fixed (before vs. after repro or regression test passing) AND the repository's required checks were actually executed with observable outputs reported honestly (including expected messages like `no tests ran` for integration tests or honestly explained preexisting failures, and removal of obsolete `@pytest.mark.xfail` markers or `pyproject.toml` suppressions for the target issue); claims without raw observable output or ticked checkboxes that contradict the actual test run fail. | required |
| `diff_hygiene` | All hunks and added/modified lines in the branch diff (`git diff main...HEAD` or bundle diff) and committed file paths. | The diff is clean, minimal, and reviewable: it contains no leftover debugging artifacts (`print()`, `console.log`, `pdb`/`breakpoint()`), no commented-out dead code blocks, no unrelated formatting/whitespace churn across untouched code, and no committed temporary or local workflow artifacts (`plan.md`, `pr_draft.md`, `test_evidence.md`, `comment.md`, `.env`, or editor swap files). | required |
| `standards_compliance` | The PR title, branch name (when provided), and PR description (`pr_draft.md` or bundle PR description) read against the repository's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md` policy requirements stated in the repo-facts/bundle. | Every required section of the repository's PR template is present and substantively completed (Summary, Issue link with `Closes #<id>` or equivalent target issue reference, Changes, Testing with only accurate checkboxes ticked and CI left unticked pre-PR, Screenshots/Demo or explicit `N/A` with reason), AND all required repository policies—specifically the explicit AI-use disclosure in `Notes for Reviewers` (or required policy section)—are answered directly without leaving template placeholders. | required |

## Verdict rule

- **accept**: Every `required` check has a grade of `pass`. (`preferred` checks, if any exist, are recorded in the output but never change the verdict.)
- **reject**: One or more `required` checks has a grade of `fail` or `unclear`.
- **Treatment of `unclear`**: Any `unclear` grade on a `required` check counts as `fail` and forces a `reject` verdict. If evidence needed to verify a required check is missing, truncated, or ambiguous, grade that check `fail` or `unclear` and reject the package.
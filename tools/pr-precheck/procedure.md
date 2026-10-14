# Procedure: how this tool grades a PR package

## Read order

Execute this exact reading sequence before grading any check so that baseline expectations are fixed before inspecting the candidate implementation:

1. **Mode and Scope Check**: Determine whether the run is in **Eval mode** (grading a self-contained package file) or **Live mode** (grading a local branch workspace). In Live mode, read `scope.md` first; if the `Repo:` line still contains a placeholder or targets an out-of-scope repository, stop immediately and refuse to grade. Next, verify that `rubric.md` and `procedure.md` contain non-comment rules.
2. **Issue and Repository Standards**: Read the issue description/thread and the repository standards (`docs/CONTRIBUTING.md` and `.github/PULL_REQUEST_TEMPLATE.md` in live mode, or the repo-facts/standards block in an eval bundle). Note the target issue number, the exact reproduction symptom, the required test/lint/typecheck commands, any xfail/suppression rules, and mandatory PR template/disclosure fields (such as AI-use disclosure).
3. **Approved Plan (`plan.md` or bundle plan)**: Read the plan *before* looking at the diff or PR description. Record:
   - The exact files and functions the plan authorizes touching (the scope boundary).
   - The planned behavioral changes and any documented `## Deviations`.
   - The planned verification steps (unit tests, repro commands).
   *Why this order matters*: Reading the plan before the diff prevents post-hoc rationalization, ensuring you spot silent drift or unapproved file modifications immediately.
4. **Candidate Diff (`git diff main...HEAD` or bundle diff)**: Read every file path and hunk in the diff. List every modified file, added/removed function, test modification, and any non-functional change.
5. **Test Evidence (`test_evidence.md` or bundle test logs)**: Read the raw command outputs and reproduction traces.
6. **PR Title and Description (`pr_draft.md` or bundle PR draft)**: Read the title and description last so you can immediately verify its claims against the facts you already recorded from the plan, the diff, and the test logs. In Live mode, also read `voice-guide.md` now and compare the draft prose against your voice rules.

## Evidence gathering

Gather and pair concrete facts for each of the four evidence families defined in `references/evidence-guide.md`:

1. **Plan Fidelity Pairing (Diff vs. Plan & Description)**:
   - List the file paths modified in the diff alongside the file paths named in the plan.
   - Compare each code hunk against the planned fix. Note any extra features, unrelated refactors, or omitted plan steps.
   - Check `## Deviations` in the plan and `Notes for Reviewers` in the PR description: if the diff deviates from the original plan or leaves a partial shortfall, record whether that exact deviation is honestly disclosed and justified in both places.
   - Compare the bullet points under `Changes` in the PR description against the actual diff hunks to verify whether the description accurately reports what changed.
2. **Test Evidence Pairing (Test Logs vs. Plan Test Plan & Repo Checks)**:
   - Locate the before/after reproduction outputs (or regression test execution) in `test_evidence.md` / the PR `Testing` section and match them against the issue's symptom and the plan's verification steps.
   - Check for execution logs of the repository's required commands (`make test-unit`, `make test-integration`, `make lint`, `make typecheck`).
   - Check the diff and repo facts for any seeded-bug `@pytest.mark.xfail` markers or `pyproject.toml` suppressions tied to the issue number, and record whether they were removed.
   - Compare the ticked checkboxes in the PR description's `Testing` section against the actual command outputs (verifying that unrun checks like pre-PR CI are not falsely ticked).
3. **Diff Quality & Hygiene Inspection (Diff Hunks & Tracked Files)**:
   - Scan every added line (`+`) in the diff for leftover debug print statements (`print`, `console.log`, `breakpoint()`, `pdb`), commented-out code blocks, or stray scratch files.
   - Scan diff hunks for unrelated whitespace/formatting rewrites outside the target functions.
   - In Live mode, verify via `git diff main...HEAD --stat` that local scratch files (`plan.md`, `pr_draft.md`, `test_evidence.md`, `comment.md`) are not committed to the branch.
4. **Standards & Communications Pairing (PR Draft vs. PR Template & Contributing Policy)**:
   - Compare the sections present in the PR description against every required heading in `.github/PULL_REQUEST_TEMPLATE.md` (`Summary`, `Issue`, `Changes`, `Testing`, `Screenshots / Demo`, `Notes for Reviewers`).
   - Verify the issue closing keyword (`Closes #<issue-number>`).
   - Locate the explicit AI-use disclosure statement required by course/repo policy inside `Notes for Reviewers` (or the designated policy section).
   - If branch metadata is provided, check whether the branch name follows `<type>/<issue-number>-<short-description>`.

## Check execution

Execute the checks in the exact order they appear in `rubric.md`: `plan_fidelity`, `test_evidence`, `diff_hygiene`, `standards_compliance`.

1. **Evaluate one check at a time** using only the paired evidence gathered in the previous stage. Do not re-read the whole package unless a paired record is incomplete.
2. **Apply the pass condition strictly**:
   - Assign `pass` only when every part of the check's pass condition is affirmatively satisfied by concrete text or code in the package.
   - Assign `fail` when the gathered evidence contradicts the pass condition (e.g., silent file drift, unrelated cleanup hunks, missing test outputs, false checkbox ticks, leftover debug prints, or missing AI disclosure).
   - Assign `unclear` when the package omits or truncates the artifact needed to verify the check and no explicit contradiction is visible, or grade `fail` directly if the rubric requires observable presence.
3. **Record the deciding quote or fact**: For every check, write a single concise `evidence` line naming the specific file, diff hunk, command result, or missing section that determined the grade.

## Verdict assembly

Combine the four check grades into a single binary verdict following the verdict rule in `rubric.md`:

1. **Apply the Verdict Rule**:
   - If all `required` checks (`plan_fidelity`, `test_evidence`, `diff_hygiene`, `standards_compliance`) are graded `pass`, set `"verdict": "accept"`.
   - If any `required` check is graded `fail` or `unclear`, set `"verdict": "reject"`. (`unclear` always acts as a failing grade).
2. **Deciding Check Selection Rule**:
   - In the human-readable summary preceding the JSON block, state each check's grade and one-line evidence. When the verdict is `reject` and multiple required checks fail, highlight the **first failing required check in rubric table order** as the primary deciding check on which the hold verdict turned, while still listing all other failing checks in both the summary and the JSON `checks` array.
3. **Emit Final Output**:
   - Output the readable per-check summary (plus any live-mode voice-guide violations), and conclude with the exact fenced JSON block defined in `SKILL.md` (`item`, `checks`, `verdict`) with no trailing commentary after the closing code fence.
# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- **Where it lives**:
  - **In an eval bundle**: Compare the **Plan / Plan-Context** section (specifically its authorized files, scope boundary, planned steps, and any documented deviation notes) against the **Candidate PR Diff** (changed file headers `--- a/...` / `+++ b/...` and modified code hunks) and the **Candidate PR Description** (`Summary`, `Changes`, and `Notes for Reviewers` sections).
  - **In live mode**: Compare `plan.md` (including its scope pair and `## Deviations` section) in the working copy root against the branch diff produced by `git diff main...HEAD` and the `Changes` and `Notes for Reviewers` sections in `pr_draft.md`.
- **What good looks like**: Every file touched and every behavioral change in the diff falls inside the plan's stated scope boundary, OR any departure from the plan (doing less, doing more, or changing approach) is explicitly documented and justified in the plan's deviation notes and the PR description's `Notes for Reviewers`. Additionally, every claim in the PR description's `Changes` and `Summary` sections matches what the diff actually delivers: touching unapproved files, silently dropping planned steps without disclosure, adding opportunistic refactors, or claiming changes the diff does not make all fail as `silent-drift`.

## Test evidence (harness category: not-tested)

- **Where it lives**:
  - **In an eval bundle**: Compare the **Test Evidence** section and the **Candidate PR Description (`Testing` section)** against the **Plan's Test Plan**, the **Issue / Reproduction Steps**, and the **Repo-Facts / Standards block** (which lists required commands such as `make test-unit`, `make test-integration`, `make lint`, `make typecheck`, and xfail/suppression rules).
  - **In live mode**: Compare `test_evidence.md` and the `Testing` section of `pr_draft.md` against the test plan in `plan.md`, the Unit 2 reproduction steps on the target issue, the required check suite in `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`, and `git diff main...HEAD` (for removal of `@pytest.mark.xfail` markers or `pyproject.toml` suppressions).
- **What good looks like**: The package includes concrete, observable command output showing both the before/after reproduction (proving the specific bug symptom is resolved) and the execution results of the repository's required checks (`make test-unit`, `make test-integration`, `make lint`, `make typecheck`), with any seeded-bug `@pytest.mark.xfail` markers or `pyproject.toml` suppressions for the target issue removed in the diff. Honest reporting of expected or preexisting check behavior (such as `no tests ran` on `make test-integration`) passes when the raw output and reason are shown, whereas vague assertions like `"all tests pass"` with no terminal output, skipped required checks, leftover xfail markers that break or mask the suite, or ticked testing/CI checkboxes that contradict actual outputs fail as `not-tested`.

## Diff quality (harness category: unreviewable)

- **Where it lives**:
  - **In an eval bundle**: Inspect the **Candidate PR Diff** (every unified diff hunk and added/removed line) and the **Commit List** inside the package bundle.
  - **In live mode**: Inspect the full unified diff from `git diff main...HEAD` and the file list from `git diff main...HEAD --stat` on the local branch.
- **What good looks like**: Every hunk in the diff directly serves the planned fix and its tests, containing zero debugging leftovers (`print()`, `console.log()`, `breakpoint()`, `pdb.set_trace()`, debug logging), zero commented-out dead code blocks, zero unrelated formatting or whitespace churn in untouched lines, and zero committed local scratch or workflow files (`plan.md`, `pr_draft.md`, `test_evidence.md`, `comment.md`, `.env`, or editor swap files). If a reviewer has to read through dead code, debug artifacts, or drive-by formatting noise to find the fix, the package fails as `unreviewable`.

## Standards and comms (harness category: standards-wall)

- **Where it lives**:
  - **In an eval bundle**: Compare the **Candidate PR Title, Branch Name (if listed), and Description** against the **Repo-Facts / Repository Standards block** (which specifies the required PR template sections, issue-linking conventions, AI-use disclosure policy, and any explicit maintainer instructions in the **Issue Thread**).
  - **In live mode**: Compare the local branch name (`git branch --show-current`) and `pr_draft.md` (title on line 1 and body below) against `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, `scope.md` house rules, and maintainer comments on the GitHub issue thread.
- **What good looks like**: Every section required by the repository's PR template (`Summary`, `Issue` with `Closes #<number>`, `Changes`, `Testing` with accurate checkbox state and CI left unticked prior to opening, `Screenshots / Demo` or `N/A` with a concrete reason, and `Notes for Reviewers`) is present and filled with specific, non-placeholder content; the required **AI-use disclosure** is explicitly stated in the description (`Notes for Reviewers`); and any explicit maintainer direction in the issue thread is addressed. Omitting template sections, leaving raw template placeholders, omitting the required AI-use disclosure, or ignoring explicit maintainer instructions fails as `standards-wall`.
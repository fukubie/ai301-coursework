# Unit 4 — Test and Submit

**Path:** `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/205

**Branch**

fix/54-leading-whitespace

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

### Run history

- Run 1 (full): 13/20 scored items (categories: clear-accept 0/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3). Note: all 7 clear-accept packages failed on `test_evidence`.
- Run 2 (--only pkg-02,pkg-04): 2/2 scored items. Tested loosened `test_evidence` pass condition on one clear-accept package (`pkg-02`) and one not-tested canary (`pkg-04`).
- Run 3 (--only pkg-01,pkg-02,pkg-03,pkg-04,pkg-12): 5/5 scored items. Canary run covering all five categories: clear-accept 1/1, not-tested 1/1, silent-drift 1/1, standards-wall 1/1, unreviewable 1/1.
- Run 4 (full, saved to eval-run.txt): 19/20 scored items (categories: clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3).

### Package analysis

- Package ID: `pkg-08`
- Rubric Verdict: `reject`
- Gold Label: `accept`
- Explanation: The package belongs to the `clear-accept` category, but our rubric failed it under `test_evidence`. The PR author provided concise summary statements regarding test outcomes rather than pasting raw before-and-after terminal command output showing the bug symptom occurring and then resolving. Our rubric strictly demands observable verification traces rather than summary assertions to prevent unverified code from landing. While this rule successfully caught all four `not-tested` packages (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`), it caused a false rejection on `pkg-08`.

### Check rationale

- Quoted check from `rubric.md`:
```markdown
| `test_evidence` | The claimed test and reproduction output (`test_evidence.md` or bundle test evidence) read against the plan's test plan, the issue's reproduction steps, and the repository's stated check requirements. | Concrete, observable verification output is present showing the fix in action (such as before/after reproduction command runs with raw output or concrete test runner counts/results per the plan) AND whatever checks the repository asks for are executed and reported honestly; bare unbacked assertions (e.g. merely claiming "tests pass" or "works now" with no command output or reproduction trace) fail. | required |
```

**Why it reads this way:** In our initial draft, the pass condition strictly required Path Review's specific commands (`make test-unit`, `make test-integration`, `make lint`, `make typecheck`) and the explicit removal of `@pytest.mark.xfail` markers. This caused Run 1 to reject every clear-accept package (0/7) because practice packages from real open-source repos (such as `rich` or Rust projects) use pytest or `cargo test` and do not have seeded xfail bugs. We revised the check to require observable verification outputs or concrete test runner counts appropriate to the repository, while explicitly rejecting bare assertions ("tests pass" with no output).

### Trade-offs

Requiring concrete, observable execution traces or counts creates a strict barrier against unverified bug fixes and prevents PRs with missing repro traces from being accepted. The trade-off is that it occasionally flags concise, well-intentioned PRs (like `pkg-08`) as false rejections. Before committing to the final full run, we re-ran a 5-package canary suite (`pkg-01`, `pkg-02`, `pkg-03`, `pkg-04`, `pkg-12`) using `--only` to confirm that loosening the check flipped `pkg-02` to accept without flipping genuine failure packages like `pkg-04` (not-tested) or `pkg-03` (silent-drift).
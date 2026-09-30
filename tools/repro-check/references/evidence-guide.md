# Evidence guide: where proof lives in a reproduction package

## Environment

- **Where it lives:** In an eval bundle, look in the reproduction report under the environment block or preamble, read against the package's `issue context` and `repo-facts`. In live mode, look at the top of the draft report where the author notes OS, runtime versions, commit hashes, or container tags, cross-referenced with the target repository's setup docs.
- **What good looks like:** The record explicitly names the concrete runtime version (e.g., Python, Node.js, Go), operating system or architecture, and package/dependency versions or commit hash used. If the test environment intentionally diverges from the original reporter's stated setup, that variance is explicitly acknowledged rather than left implicit.

## Steps

- **Where it lives:** In an eval bundle, look in the steps section of the repro report. In live mode, look at the command sequence, configuration files, or code snippets in the body of the repro report draft.
- **What good looks like:** The steps provide an unbroken, self-contained sequence from a fresh clone/baseline state to bug invocation, including exact terminal commands, test fixtures, flags, or configuration values. A developer reading the steps does not need to guess unmentioned environment variables, omitted directories, or undocumented setup scripts to trigger the flow.

## Behavior shown

- **Where it lives:** In an eval bundle, look in the artifacts section (terminal output logs, stack traces, exit codes, screenshots, or test assertions) read directly against the original issue's reported error. In live mode, look in fenced code blocks and captured logs in the draft repro comment.
- **What good looks like:** The captured error trace, message, or observed output mirrors the specific failure condition and call stack cited in the issue. If an error is shown, it must be the bug itself rather than an adjacent prerequisite failure (such as an unrelated permission denial, missing build dependency, or syntax typo introduced during setup).

## Honesty

- **Where it lives:** In an eval bundle, look at the conclusion/summary statement of the repro report compared against the attached execution artifacts. In live mode, look at the author's verdict claim versus the terminal transcripts provided.
- **What good looks like:** The author claims only what the logs demonstrate: an evidenced "cannot reproduce" explicitly details the exact steps taken, shows clean runs, and states that the bug did not manifest under those conditions. A report fails honesty if it asserts reproduction but provides empty/unrelated logs, or asserts a fix/root cause without artifact proof.

## Comms

- **Where it lives:** In an eval bundle, look at the claim comment, repro report body, and the `repo-facts` block (specifically looking for repository contribution guidelines, templates, or AI usage rules). In live mode, look at the student's draft comment text compared against the repository's `CONTRIBUTING.md` or issue template rules.
- **What good looks like:** The text adheres to the repository's posted conventions; if `repo-facts` or repository policy mandates declaring AI tool usage, an explicit disclosure statement is present. For claim comments, the author names the target issue specifically, outlines intended investigation steps, and promises an investigation rather than guaranteeing a solution, diagnosis, or delivery date.
# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Default-branch commit log and recent issue/PR comments from repository maintainers | At least one maintainer commit, PR review, or issue comment within the last 60 days, and repo is not archived | required |
| repo-in-use | Commit log, release dates, and open/closed issue timestamps | Default branch has at least 5 commits or at least 2 closed issues/PRs in the last 90 days | required |
| ai-policy | Repository contribution policy, CONTRIBUTING.md, and generative AI guidelines in repo facts | The repository does not explicitly ban AI-generated code or contributions. If the policy says AI-generated code or docs are not accepted, this check fails | required |
| newcomer-scope | Issue title, body, labels, checklist, and discussion comments | The issue describes a bounded task, bug fix, performance diagnosis with known causes, or documentation page with clear expectations. It fails if the discussion reveals protracted design debate, unresolved consensus, abandoned attempts, or an open-ended redesign | required |
| unclaimed | Issue assignees, linked open pull requests, and comment thread | No active assignee, no open PR linked to the issue, and no claim comments within the last 7 days | required |

## Verdict rule

Accept if every required check passes. If any required check fails or evaluates to unclear, reject the issue. Preferred checks never change the verdict; they are only used to rank accepted issues.
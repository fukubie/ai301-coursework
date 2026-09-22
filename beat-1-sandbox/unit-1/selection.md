# Unit 1 - Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54)

**Verdict output**

All evidence gathered. Repo-level checks (shared by all three): maintainer-alive - collaborator Aburke225 committed to main on 2026-09-16, repo not archived (pass); repo-in-use - 15+ commits on main within the last 90 days (pass); ai-policy - docs/CONTRIBUTING.md exists and states no ban on AI-generated code (silence/conditions pass). All three issues are opened by a collaborator, carry good first issue + tier-1 labels, have 0 comments, no assignees, and no open linked PRs (0 open PRs in the repo).

Ranked read-out - all three accepted

1. #54 - Resume section detection fails on leading whitespace - best fit. A Python backend/data-parsing bug with an exact runnable reproduction and three named failing tests: precisely the "backend logic bug / data parsing with clear reproduction steps" the fit profile calls the student's sweet spot.
2. #73 - README and .env.example disagree on LLM API key - strong fit. Environment/config reconciliation touching core/config.py; matches "script/environment configuration with clear repro steps," though lighter on logic.
3. #47 - API docs lack example curl commands - accepted, weakest fit. Pure docs work; touches REST APIs conceptually but exercises none of the backend/data strengths.

Per-check grades: every issue passes newcomer-scope (bounded single-file-or-two task, no design debate, clear expectations) and unclaimed (no assignee, no linked PR, no claim comments - and the Path Review house rule wouldn't block on claims anyway).

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Collaborator Aburke225 committed to main on 2026-09-16; repo not archived"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "15+ commits on main within last 90 days (2026-08-18 through 2026-09-16)"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md present; no ban on AI-generated contributions anywhere in repo docs"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Bounded bug in _detect_sections() with exact repro and 3 named failing tests; good-first-issue/tier-1 labels"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], 0 comments, no open linked PR"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Collaborator Aburke225 committed to main on 2026-09-16; repo not archived"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "15+ commits on main within last 90 days"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md present; no AI ban"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Reconcile README vs .env.example on LLM key; 2 files, 1-2h, good-first-issue label"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], 0 comments, no open linked PR"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Collaborator Aburke225 committed to main on 2026-09-16; repo not archived"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "15+ commits on main within last 90 days"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md present; no AI ban"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "Add example curl commands to docs/API.md; single file, good-first-issue label"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], 0 comments, no open linked PR"}
    ],
    "verdict": "accept"
  }
]
```
---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. 17/20 (below the bar; category floor unmet: policy 0/1)
2. 18/20 scored items (bar: 18/20: PASS)
3. 17/20 scored items (bar: 18/20: below the bar)
4. 18/20 scored items (bar: 18/20: PASS)

**Issue analysis**

### issue-12

Rubric decision: reject
Gold label: reject

Reasoning: The issue (bookwyrm-social/bookwyrm#1133) describes an otherwise bounded task in an active repository. However, the repository's CONTRIBUTING.md contains an explicit prohibition: "We do not accept AI-generated code or documentation." In early rubric versions lacking a dedicated policy check, the issue was incorrectly evaluated as an accept. Once the required ai-policy check was introduced to inspect repository contribution facts and fail any project with an explicit AI contribution ban, the issue correctly evaluated to reject, satisfying the gold label and the policy category floor.

**Check rationale**

| Check | Evidence source | Pass/fail rule | Requirement |
| --- | --- | --- | --- |
| ai-policy | Repository contribution policy, `CONTRIBUTING.md`, and generative AI guidelines in repo facts | The repository does not explicitly ban AI-generated code or contributions. If the policy says AI-generated code or docs are not accepted, this check fails | required |

Reasoning: Without an explicit required check for contribution constraints, the evaluation defaults to inspecting only repository activity and issue-level scope. Open-source repositories that strictly prohibit AI-generated pull requests will reject contributions regardless of code correctness. Adding this required check grounds the decision in repository policy evidence and prevents newcomers from submitting work that violates repository rules.

**Trade-offs**

When tuning newcomer-scope to fail issues with protracted design debates and abandoned PRs (to correctly reject issue-15), the stricter phrasing created slight sensitivity on multi-worker runs for boundary cases like issue-01 and issue-19. To counter this without reopening the door to stalled issues like issue-15, newcomer-scope was explicitly broadened to allow documentation tasks and performance diagnosis with known causes. Testing with python run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --only issue-15 confirmed it reliably rejected issue-15 while individual runs on issue-01 and issue-19 agreed as accept.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length - a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit to interests and time: Issue #54 focuses on handling leading whitespace during resume section parsing in Python. Working with data parsing, string manipulation, and backend logic directly aligns with my Python experience, and having three existing failing unit tests means the scope is tightly bounded and realistic to fix and test within the available project timeline.
2. What the verdict identified vs. what I weighed: The verdict accurately identified that the repository is actively maintained, has an accommodating AI policy, and that the issue is unclaimed and well-bounded. What I weighed beyond the rubric was the presence of concrete unit test names in the issue description, which provides an immediate local feedback loop for verifying the fix before opening a PR.
3. Anticipated difficulty in claiming: Claiming difficulty is low. The issue has zero comments, no assignees, and no open linked pull requests. Furthermore, Path Review operates under a classroom house rule where simultaneous claims by peers do not block contribution credit.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
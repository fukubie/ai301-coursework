# Contribution: [Style]: The tooltip display should not be clipped

Path: `beat-2-contribution/contribution-story.md`

**Contribution Number:** 1
**Student:** Abdelrahman Mohamed
**GitHub Username:** fukubie
**Issue:** https://github.com/oqtopus-team/qdash/issues/1226
**Pull Request:** https://github.com/oqtopus-team/qdash/pull/1783

My one contribution this term is a small UI fix to oqtopus-team/qdash, a web platform for managing and monitoring qubit calibration workflows. The status-dot tooltips on the Chip page's coupling grid were being clipped. I fixed that by reusing the project's shared Radix Tooltip. The PR is open. **No human maintainer has reviewed it yet.** The only feedback so far is from bots, and I keep the two apart throughout this story.

A note on dates: Units 5 to 8 of tool work and the contribution itself all happened on 2026-11-08 (local time, UTC-5). GitHub and the PR-monitor report UTC, so the PR shows as opened on 2026-11-09 01:57 UTC, which was the evening of Nov 8 for me. Where a time matters I give both.

---

## Toolkit

Each story section below ends with a Toolkit block (tool, what it produced, difficulties, what changed). The four entries here are the timeline of the tool work itself.

**Unit 5: issue-scout, my search strategy, and the scan that surfaced qdash#1226**

I rewrote the issue-scout skill's `search-strategy.md`, `rubric.md` and `scope.md` for a real, wild search. The strategy is `gh search issues` queries for good-first-issue and bug issues in Python and TypeScript, filtered to unassigned issues updated in the last 60 days and ranked by fewest comments. The rubric checks maintainer-alive, repo-in-use, ai-policy, scope-feasible and unclaimed. The scan graded qdash#1226 and project-blurple/Canvas-Web#192 on 2026-11-08, and both passed as "straightforward". What fell short is that my own `scope.md` says to avoid front-end styling work, yet the rubric accepted a styling issue, because the rubric tests viability and not personal fit. I noted that tension in `scan-notes.md` and did not patch the tool.

**Unit 6: voice-guard and repo-memory, both still unfilled**

I set up `~/.claude/voice-guard/voice-guide.md` (installed 17:16 on Nov 8) and a `CLAUDE.local.md` in my qdash clone (excluded from git through `.git/info/exclude`, so it can't land in the PR). **I did not fill in either one.** Both files are still the unedited staff templates, with no "Upstream" rules, no wrong/right pairs, and no distilled notes about qdash. My claim comment on #1226 was posted at 16:40 local, before the voice guide was installed at 17:16, so it never passed through a voice check. What I did get from Unit 6 was a failure. The first ship-gate run blocked because the voice guide wasn't in the voice-guard folder, and I fixed that by copying it there.

**Unit 7: ship-gate, a hand-written gate manifest, and 15 blocks before the pass**

I wrote `ship-manifest.md` in the qdash clone with two required checks, `ui-typecheck` (`cd ui && bunx tsc --noEmit`) and `ui-tooltip-test` (the shared Tooltip test). I excluded the full Vitest suite with a dated reason, because it needs a running local API server. I also rewrote the pr-precheck `rubric.md` for qdash. The gate log in `~/.claude/ship-gate/gate-log.md` has 16 runs on Nov 8 between 17:15 and 20:51. Fifteen were BLOCK and the last was PASS. The blocks, in order, were a missing voice guide, `vitest` stuck in watch mode (exit 130, fixed by adding `--run`), the `claude` CLI not on PATH, and then repeated pr-precheck rejections for `plan_fidelity`, `test_evidence` and `standards_compliance`. The rejections were about prose-only before/after evidence, no test for the changed component, no `task check` output, and a PR draft without the repo's Ticket section or an AI disclosure. I fixed each by adding a real regression test, saving raw command output, running `task check`, and rewriting the PR description. The PASS at 20:51 was on head `7d0e75f`. The log has no run on the later head `8c609d3`, so I can't show the gate passing on the final code.

**Unit 8: pr-monitor and my playbook, which correctly drafted no reply**

I installed the PR-monitor at `~/.claude/pr-monitor/` and wrote `playbook.md` the day the PR opened. It sets a 4-quiet-day first-review threshold, a 5-quiet-day stalled-review threshold, and a 14-day walk-away. Those numbers come from measured data: the five most recent merged outside-contributor PRs got a first human response in 0.4 to 1.4 days. The monitor ran on 2026-11-08 at 22:04 (state-only) and 22:11 (full run). The full run read the PR as `FEEDBACK-IN` with `REVIEW_REQUIRED`, mergeable but blocked, and its drafted move (`drafted-move.md`) was "No reply to send." Bot feedback was already answered and no human had commented, so nothing was waiting on me. I posted nothing, which was the right outcome. The first nudge would be due around 2026-11-13 and the walk-away around 2026-11-23, both computed by the monitor and not checked against a calendar.

---

## Why I Chose This Issue

I ran my issue-scout over all of GitHub on 2026-11-08, using the queries in my `search-strategy.md` for good-first-issue and bug issues in Python and TypeScript. I graded the top candidates against my rubric, and the two that survived were qdash#1226 and Canvas-Web#192. Both passed every required check. For qdash the scout found maintainer commits on 2026-11-06 and a reference fix already on the Metrics page, and the issue carries "good first issue" and "style" labels and passed the unclaimed check. Canvas-Web showed only bot-driven closures. The issue asks for the Chip page's tooltip to stop being clipped, the same way the Metrics page already does.

The scout's verdict on #1226 was accept, difficulty straightforward, so I did not choose against it. But the issue sits uneasily with my own `scope.md`, which says I want to avoid front-end styling work, and I picked it anyway. My reasons, as recorded in `scan-notes.md`, were that the maintainers are clearly active (commits on 2026-11-06, against bot-only closures on Canvas-Web) and that the Metrics page already had a reference fix to follow. On the tool itself, the scout held up on the checks I could verify (maintainer activity, unclaimed, scope). It fell short on fit: the rubric checks that an issue is viable and not whether it matches my stated profile, so it ranked a styling issue I said I wanted to avoid. I would add a rubric check that reads `scope.md`'s avoid-list.

**Claim comment:** https://github.com/oqtopus-team/qdash/issues/1226#issuecomment-6727497484 (posted 2026-11-08 21:40 UTC, 16:40 local). It says I'll reproduce the clipping on the Chip page and compare with the Metrics page, then follow up with my environment and observations.

**Toolkit**

- Tool: issue-scout, using my own `search-strategy.md`, `rubric.md` and `scope.md`, run via `gh search issues` on 2026-11-08.
- What it produced: a ranked scan in which qdash#1226 and Canvas-Web#192 both passed all required checks as "straightforward", with the reasoning saved in `~/.claude/skills/issue-scout/scan-notes.md`.
- Difficulties: the rubric checks viability only, so it accepted a front-end styling issue that my `scope.md` says to avoid. I tie-broke by hand on maintainer activity.
- What changed: nothing in the tool yet. I recorded the tension in `scan-notes.md` and would add a rubric check that reads the avoid-list in `scope.md`.

---

## Understanding the Issue

**Problem Description**

On the Chip page, the coupling grid shows a small status dot for each coupling. When you hover a dot, a tooltip with the coupling ID and status appears. In the zoomed-out view the tooltip was being cut off. The grid sits inside a pan/zoom container with `overflow-hidden` and a transform, and the tooltip was rendered inside that container, so the container clipped it.

**Expected Behavior**

The tooltip should always be fully visible, the same way it already is on the Metrics page, and it should behave like the other tooltips in the app.

**Current Behavior**

Before my change, the tooltip was clipped at the edges of the grid container. The issue includes a "Good" screenshot (Metrics page) and a "Bad" screenshot (Chip page). **Not verified:** I did not save my own before screenshot or recording of the clipping. My evidence for the "before" state is the issue's screenshot and my reading of the code, not a capture from my machine.

**Affected Components**

- `ui/src/components/features/chip/CouplingGrid.tsx`, which contains the zoomed-out coupling dot and its tooltip.
- `ui/src/components/features/chip/QubitGrid.tsx`, the sibling component that already solves this with the shared tooltip (lines 144-177 at the time).
- `ui/src/components/ui/Tooltip.tsx`, the shared Radix-based `Tooltip`, `TooltipTrigger` and `TooltipContent`.

**Toolkit**

- Tool: Claude Code, used to read the issue and the chip components and to compare the Chip page with the Metrics page. The repo ships an `AGENTS.md` with its conventions. My `CLAUDE.local.md` repo-memory file was meant to carry my own notes but I never filled it in (see Unit 6).
- What it produced: the root-cause picture above, mapping the clipped tooltip to the pan/zoom container and `QubitGrid.tsx` as the pattern to follow.
- Difficulties: my repo-memory file stayed an empty template, so I had no saved orientation notes for qdash to read back.
- What changed: nothing yet. If I did this again I would fill `CLAUDE.local.md` right after the first scoped orientation.

---

## Reproduction Process

**Environment Setup**

- OS: Windows 11 Home (10.0.26200).
- Tooling in `ui/`: Bun 1.4.4, Node v24.16.0, Vitest 4.1.12 (from the test run output), Next.js `^16.4.2` and React `^19.2.8` (from `ui/package.json`).
- Setup: I forked oqtopus-team/qdash to `fukubie/qdash` and cloned it to `C:\Users\abdon\Open_Source\qdash`. The repo's setup guide is `docs/developer-guide/setup.md`. **Not verified:** the exact install commands I ran, which I did not record. I ran the UI checks directly on Windows with Bun. I ran the repo-wide `task check` inside the project's Docker Compose devcontainer: `docker compose -f compose.devcontainer.yaml exec -T devcontainer sh -lc 'cd /workspace/qdash && task check'`.
- Challenges: (1) Running `task check` from a Windows checkout failed at first because three shell scripts had Windows CRLF line endings. I temporarily converted them to LF so the check could run, and restored them afterward. They are not part of the PR, and `git diff origin/develop...HEAD` shows only two files. (2) `bun run test <file>` started Vitest in watch mode and never exited (exit 130 in the ship-gate log), so I use `bun run test --run <file>`.

**Steps to Reproduce**

1. Start the QDash UI and open the Chip page.
2. Zoom the coupling grid out until it uses the compact status-dot view.
3. Hover a coupling status dot near the edge of the grid.
4. Expected observation: the tooltip is cut off by the grid's pan/zoom container, as in the issue's "Bad" screenshot.

**Not verified:** I did not run these steps against a live UI and record the result. The steps above are my reconstruction from the issue and the code, so treat them as the expected reproduction and not as an observed one.

**Reproduction Evidence**

- **Commit showing reproduction:** none. I did not make a separate reproduction commit. The first fix commit on my fork is `aa27fc9` (https://github.com/fukubie/qdash/commit/aa27fc95), but it is a fix and not a reproduction.
- **Screenshots/logs:** none captured. The PR description says so openly and uses clearly labelled conceptual ASCII diagrams instead of screenshots.
- **My findings:** from reading the code, the original tooltip was a hover-only element inside the `overflow-hidden` pan/zoom subtree of `CouplingGrid`, so the container clipped it. I did not post a follow-up reproduction comment on the issue after my claim, so the issue thread has only my claim comment from me.

**Toolkit**

- Tool: the course's repro-check skill exists in `tools/repro-check/`, but I have no record of running it on #1226. I did the reproduction work by reading the code with Claude Code.
- What it produced: the code-reading analysis above and no runnable reproduction record.
- Difficulties: I never captured a live before-state, which is the weakest part of my evidence. The PR's reviewers have to take the clipping on the issue's word, and on the code.
- What changed: nothing. A real before and after screenshot is what I would add first.

---

## Solution Approach

**Analysis**

In `CouplingGrid.tsx`, the zoomed-out coupling dot rendered its tooltip as a child of the grid, inside the `TransformComponent`. That subtree has `overflow-hidden` and a CSS transform, so a tooltip positioned inside it cannot extend past the container's edge. The Metrics page and the sibling `QubitGrid.tsx` avoid the problem because they use the shared Radix tooltip, which renders through a portal outside the clipping container.

**Proposed Solution**

Pull the dot into a small `CouplingStatusDot` component and render its tooltip with the shared `Tooltip`, `TooltipTrigger` and `TooltipContent` components, following the pattern in `QubitGrid.tsx`. I am deliberately not changing the grid's layout, the pan/zoom behavior, the click behavior (task selection and downloads), or the shared Tooltip component itself.

**Implementation Plan**

My original `plan.md` (kept local, excluded from git) said:

1. Define `CouplingStatusDot` with `label`, `className`, `style` and `onClick` props.
2. Track the tooltip position with React state and `getBoundingClientRect()` on mouse enter.
3. Render the tooltip into `document.body` with `createPortal`, fixed positioning and `zIndex: 9999`.
4. Clear the tooltip state on mouse leave.
5. Run TypeScript and the available frontend tests.

**Deviation (recorded):** steps 2 to 4 were replaced. After the automated reviews on the PR (see Pull Request), I dropped the hand-written portal and used the existing shared Radix Tooltip instead, as in commit `8c609d3`. That removed the manual position code, the `zIndex: 9999` override and the `typeof document` guard, and added an `aria-label` and keyboard-focus support. I kept step 1 (the `CouplingStatusDot` component) and step 5 (checks).

**Not updated:** the local `plan.md` and `test_evidence.md` in my clone still describe the earlier `createPortal` version, so they are stale relative to the final code. They are untracked and not part of the PR.

**Toolkit**

- Tool: Claude Code for the plan, and my ship-gate/pr-precheck rubric to check the plan against the diff.
- What it produced: `plan.md` with the Analysis, Proposed Solution, Implementation Plan and a Deviations section. The pr-precheck `plan_fidelity` check compared the diff to it on the gate runs that got as far as pr-precheck.
- Difficulties: the gate rejected earlier drafts because the plan's Deviations did not match the PR description, for example a skipped lint and a missing component test that were disclosed in one place and not the other.
- What changed: I made the plan and the PR description agree. The final plan is still out of date, as noted above.

---

## Testing Strategy

The checks, and what each showed:

- **Type check:** `cd ui && bunx tsc --noEmit` exited 0 (manifest check `ui-typecheck`, passing in the gate log).
- **Shared Tooltip test:** `cd ui && bun run test --run src/components/ui/__tests__/Tooltip.test.tsx` passed 3 of 3 (manifest check `ui-tooltip-test`). That suite covers the shared primitive, not my component.
- **New regression test:** `ui/src/components/features/chip/__tests__/CouplingStatusDot.test.tsx`, with three cases: the tooltip appears and then dismisses (checked through the Radix `data-state` closed, open, closed), the label shows on keyboard focus, and the click behavior is preserved. Per the PR description, all 3 pass.
- **Full repo check:** `task check`, run in the Docker Compose devcontainer, exited 0 after the latest revision. The PR description reports UI TypeScript, lint and format checks passing, 16 TypeScript-client tests, 174 agent-runtime tests, and a clean secret scan. I got these figures from my own recorded run (`test_evidence.md`) and the PR description. They are not independently re-verified for this story.
- **Excluded:** the full UI Vitest suite on its own (`cd ui && bun run test`), dated 2026-11-08 in my manifest, because several baseline tests call un-mocked backend API endpoints and need a local API server.

**Before and after:** I do not have a recorded before and after run of a live UI. My before/after evidence is the new regression test (first written for the portal version in `00fa25f`, then rewritten for the Radix tooltip in `8c609d3`) plus the issue's screenshots. **GitHub CI has not run on the latest commit**, because workflows from a fork need a maintainer's approval. So my new test has so far run only on my machine and in my devcontainer, not in the project's CI.

**Toolkit**

- Tool: ship-gate with my `ship-manifest.md` (the `ui-typecheck` and `ui-tooltip-test` checks), plus pr-precheck's `test_evidence` check.
- What it produced: a PASS on 2026-11-08 20:51 for head `7d0e75f`, after 15 BLOCK runs. Many of those blocks were about test evidence: prose-only before/after, only the unrelated Tooltip suite run, and no `task check` output.
- Difficulties: `bun run test` ran in watch mode and made the gate fail with exit 130. The gate also checks a head that is no longer the final commit.
- What changed: I added `--run` to the manifest command, wrote the regression test, saved raw outputs, and ran `task check`. I have not re-run the gate on `8c609d3`.

---

## Implementation Notes

**2026-11-08, afternoon (about 16:40 to 17:10)**

I claimed the issue at 16:40 and made the first fix commit `aa27fc9` at 17:10. It extracted `CouplingStatusDot` and rendered its tooltip into `document.body` with `createPortal`, `position: fixed`, coordinates from `getBoundingClientRect()` and `zIndex: 9999`.

**2026-11-08, 17:15 to 20:11**

I ran the ship-gate over and over. It stopped me for the missing voice guide, the watch-mode test script, the missing `claude` CLI, and then pr-precheck failures (see the Unit 7 entry). One wrong turn: at one point my PR draft cited a passing `CouplingStatusDot` regression test that was not yet committed. At another, my `test_evidence.md` quoted before/after snippets that did not match the actual diff. The gate caught both. I added the test as commit `00fa25f` (22:58 UTC) and a formatting fix `7d0e75f` (00:58 UTC).

**2026-11-08, around 20:51 to 20:57**

The gate passed for `7d0e75f`. I opened PR #1783 at 20:57 local (2026-11-09 01:57 UTC).

**2026-11-08, evening after the PR opened (about 21:03 to 21:50)**

CodeRabbit (21:03 local) and Pullfrog (21:09 local) reviewed the PR. Pullfrog pointed out that my portal duplicated the shared Radix Tooltip, and that `QubitGrid.tsx` in the same container already uses it. I agreed. Its other findings were the `zIndex: 9999` sitting above the app's own `z-[1400]` tooltip layer, viewport edge clipping, stale coordinates on wheel zoom, and an unreachable `typeof document` guard. All of these disappear if I use the shared component. I rewrote the component and the test and committed `8c609d3` at 21:49 local (02:49 UTC). I pushed it and commented on the PR at 21:50.

**2026-11-08, 22:04 to 22:11**

I ran the PR-monitor. It read the state as feedback-in, drafted "no reply", and I posted nothing.

**Toolkit**

- Tool: Claude Code for the edits and the refactor, plus the ship-gate hook on Bash commands.
- What it produced: four commits on the PR branch (`aa27fc9`, `00fa25f`, `7d0e75f`, `8c609d3`), two files changed, +90/-8 against `origin/develop`.
- Difficulties: my first implementation reinvented a component the repo already had. My gate checked evidence and PR hygiene, not design, so it never flagged that. The bot reviewers, who read the neighboring file, did.
- What changed: I rewrote the fix around the shared Tooltip. Next time I would read the sibling component before designing a new approach.

---

## Pull Request

**PR Link:** https://github.com/oqtopus-team/qdash/pull/1783 (title: "fix(ui): portal CouplingGrid tooltip to prevent clipping (#1226)", base `develop`, opened 2026-11-09 01:57 UTC, not a draft)

**PR Description:** It follows the repo's template with Ticket (`Closes #1226`), Summary, Changes, Screenshots/Demo, Verification, Notes for Reviewers and AI Disclosure sections. It says the fix reuses the shared Radix Tooltip and lists the checks (`task check` exit 0, UI tests, 3 new regression tests, 3 shared Tooltip tests). It says plainly that browser screenshots were not captured, and that GitHub CI on the latest commit is waiting on maintainer approval. It also discloses that AI assistance was used. **Note:** the PR title still says "portal", which describes my first implementation and is a little stale now that the code uses the shared Tooltip.

**Maintainer Feedback:**

Automated bot feedback (not a human review, not an approval):

- 2026-11-08 21:03 local (02:03 UTC): CodeRabbit left a review (a "commented" review, not an approval) with one inline finding: give the status-dot button an accessible name and show the tooltip on keyboard focus. It also posted its walkthrough summary at 20:57 local.
- 2026-11-08 21:09 local (02:09 UTC): Pullfrog reviewed and recommended replacing the hand-rolled portal with the existing shared Tooltip, with several inline findings (z-index above the app's overlay scale, viewport clamping, stale position, the unreachable guard).
- 2026-11-08 21:50 local (02:50 UTC): I replied on the PR (https://github.com/oqtopus-team/qdash/pull/1783#issuecomment-6731063026), pointing at `8c609d3`, and I wrote that the change replaced the custom portal with the shared Tooltip and that `task check` passed in the devcontainer.
- 2026-11-08 21:56 local (02:56 UTC): Pullfrog re-reviewed `8c609d3` and reported "No new issues found". It also noted that CI on that commit is `action_required`, gated on maintainer approval, so the rewritten test has not run in CI.

Human maintainer review:

- **None yet.** No human has commented, reviewed, or approved. There has been no response from `orangekame3`, `dvlpwork` or any other maintainer. The PR review decision is `REVIEW_REQUIRED` and merge state is blocked. Per my playbook, silence is normal for now and no nudge is due until about 2026-11-13.

**Status:** Awaiting review. Open, not approved, not merged.

**Toolkit**

- Tool: pr-monitor and my `playbook.md`.
- What it produced: a run report (2026-11-08 22:11) with the PR in the `FEEDBACK-IN` state, a computed first-nudge date of about 2026-11-13 and a walk-away date of about 2026-11-23, and a drafted move of "No reply to send." The playbook sets 4 quiet days for first review, 5 for a stalled review, and 14 for walk-away, derived from five recent merged outside-contributor PRs.
- Difficulties: the monitor reads bot comments as feedback, so it labelled the state "feedback-in" even though no human has reviewed. The playbook's rule that bot comments don't reset the quiet-day count is what keeps that straight. The monitor also flagged that CodeRabbit's description check may be stale, and I cannot tell whether it re-ran.
- What changed: I sent nothing. The playbook has the nudge text, aimed at `orangekame3`, ready for when the first-review threshold passes.

---

## Learnings and Reflections

**Technical Skills Gained**

- How React portals and Radix primitives fix overflow clipping, and why `getBoundingClientRect()` is the right anchor inside a transformed subtree.
- Reading a repo's own conventions (`AGENTS.md`, the shared `ui/src/components/ui/` primitives) before inventing a component.
- Writing a Vitest and Testing Library regression test around Radix's `data-state` and an `aria-label`.
- Running a monorepo's `task check` inside its Docker Compose devcontainer from Windows, and handling CRLF line endings in shell scripts.
- Using `gh` to read issue and PR threads, and telling bot reviews from human review.

**Challenges Overcome**

- The ship-gate blocked me 15 times before it passed. The first few were setup problems; the rest were real gaps: prose-only evidence, a regression test I had cited but not committed, snippets that did not match the diff, and a PR description missing the repo's template sections.
- `task check` would not run from Windows until I handled the CRLF problem in three scripts, without leaving that change in the PR.
- The bot review showed that my own implementation was the wrong shape. I rewrote it onto the shared Tooltip in one follow-up commit, and Pullfrog's second review found no new issues.

**What I'd Do Differently Next Time**

- Read the neighboring component (`QubitGrid.tsx`) first. That alone would have saved the portal detour.
- Capture a real before screenshot or recording before touching code, and an after one when done. This is the biggest gap in my evidence.
- Fill in the Unit 6 files (voice guide and `CLAUDE.local.md`) before posting anything upstream. As it is, my claim comment went out before the voice guide existed.
- Re-run the gate on the final commit and not just the one before the bot refactor, and update `plan.md` when the plan changes.
- Post a short reproduction follow-up on the issue after claiming, as my claim said I would.

---

## Resources Used

- The issue thread and its "Good" and "Bad" screenshots: https://github.com/oqtopus-team/qdash/issues/1226
- `ui/src/components/features/chip/QubitGrid.tsx` and `ui/src/components/ui/Tooltip.tsx` in qdash, the existing pattern I ended up following.
- qdash's `AGENTS.md`, which asks for the shared primitives in `ui/src/components/ui/` and for `task check` before pushing.
- Pullfrog's review on PR #1783, which is what pointed me to the shared Tooltip: https://github.com/oqtopus-team/qdash/pull/1783
- CodeRabbit's accessibility finding on the same PR, which led to the `aria-label` and the keyboard-focus tooltip.

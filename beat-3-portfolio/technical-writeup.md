# Fixing a Tooltip Bug in QDash

## Overview

I worked on [QDash issue #1226](https://github.com/oqtopus-team/qdash/issues/1226). The problem was a small tooltip that could get cut off when someone zoomed out on the Chip page. I opened [pull request #1783](https://github.com/oqtopus-team/qdash/pull/1783) to fix it. My first approach worked around the clipping with custom code, but review feedback showed me a better way. I changed the fix to use the tooltip component that QDash already had, added tests, and ran the project checks. A maintainer approved and merged my PR into the `develop` branch.

## What happened

### Finding the issue

I used my issue-scout workflow to look for an open-source task I could finish and test. I chose QDash issue #1226. It was focused on one visible problem, so I could learn the project without changing a large part of it.

The problem was in `CouplingGrid.tsx`. The status dots show information when a user points at them. The grid sits inside a pan and zoom area that hides content outside its bounds. When a tooltip crossed that boundary, part of it could disappear. The status dot still worked, but its label was harder to read.

I read the related code and planned a small fix that would keep the current click behavior.

### My first approach

At first, I made a separate `CouplingStatusDot` component and used React `createPortal` to show its tooltip outside the clipped grid. I calculated the dot position with `getBoundingClientRect()` and placed the tooltip with fixed positioning. This avoided the container boundary, but it also meant I had to manage the tooltip position and layering myself.

I learned that solving the visible bug was not enough. The solution also had to fit the rest of the codebase.

### Feedback and a better fix

Automated review feedback raised problems with my custom approach. The project already used a shared Radix Tooltip component in `QubitGrid.tsx`. I had missed that pattern when I started.

I changed the implementation to use `Tooltip`, `TooltipTrigger`, and `TooltipContent`. This removed the manual positioning and custom portal logic. It also gave the status dot keyboard focus support and an accessible label. I kept the existing click behavior.

This was the most important change I made. I stopped adding new code for a problem that the project already knew how to solve.

### Proof from the work

The final PR description recorded these checks after the refactor.

```text
Dedicated CouplingStatusDot regression tests  3 passed
Shared Tooltip component tests               3 passed
TypeScript client tests                     16 passed
Agent runtime tests                        174 passed
Full task check                             Passed, exit code 0
```

These results matter because the fix touched a shared UI pattern. I needed to check both the new behavior and the rest of the project. I ran the full `task check` inside the development container. On Windows, I also had to deal with line ending problems in shell scripts. I fixed those locally to run the checks and kept the temporary changes out of the PR.

The final PR also described the change in approach.

> Removed custom portal rendering, manual viewport positioning, and z-index overrides.

This excerpt comes from the PR description and records what was removed in the revision. It is not a direct code diff. The final diff changed `CouplingGrid.tsx` and added `CouplingStatusDot.test.tsx`. The tests covered tooltip visibility, dismissal, accessibility, and click behavior.

### The result

After the changes, Pullfrog reported no new issues. The human maintainer `orangekame3` approved the PR and commented, "Thanks LGTM." The PR was merged into `develop` as commit [`e58f7b3`](https://github.com/oqtopus-team/qdash/commit/e58f7b30f3ea60d1b6e86132640698fdd1ae2b57).

Getting the change merged showed me that good open-source work is not only about writing code. It is also about listening to review feedback, following existing patterns, testing carefully, and explaining what changed.

## Limitations and what's next

I did not capture real browser screenshots of the tooltip before and after the fix. I also did not save a live reproduction record before I started coding. The PR included a diagram to explain the clipping, but a diagram is not the same as direct evidence.

Next time, I want to reproduce the problem first and save the result. I also want to search nearby files for existing components before I design a new one. Finally, I will keep my plan, test notes, and PR title up to date when my implementation changes. These steps should make my next contribution easier to review and easier to trust.

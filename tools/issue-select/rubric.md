# Rubric: is this a good first issue?

All recency thresholds are measured against the bundle's `captured:` date
in eval mode, and against today in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| not-archived | The `archived:` flag on the `repo:` line of the repo-facts block (live: the archived banner on the repo front page). | `archived: no`. An archived repo is read-only and can never merge the PR, so this fails. | required |
| maintainer-alive | The `last 5 default-branch commits` list in the repo-facts block; take the **newest date in the list**, not the first line (the list is not always sorted). Live: the newest commit date above the file list. | The newest default-branch commit is dated within **90 days** of the capture date. Commits authored by a bot count only when the commit message shows it merged a human's pull request. | required |
| repo-in-use | The `latest release` line in the repo-facts block; if it reads `none published`, fall back to the `last 5 default-branch commits` list. Live: the Releases box in the right sidebar. | Either the latest release is dated within **365 days** of the capture date, **or** (no release ever published) at least **3** of the last 5 default-branch commits are dated within **180 days** of the capture date. A project with no release channel is not dead; a project with neither releases nor recent commits is. | required |
| scope-bounded | The issue title and body, its labels, and the full comment thread; plus the `linked PRs:` states on the `this issue:` line. | Passes unless **any** of the following is present: (a) the issue is an **umbrella or tracking issue**: it calls itself one ("megaissue", "umbrella", "tracking issue"), or its body is a list of **other issue or PR numbers**, or it asks for an **open-ended sweep with no enumerated end** ("add annotations to the codebase", "all modules", "every page"), or its items are each an independent change meant for a separate pull request. A **finite, enumerated list of edits that together form one deliverable and could ship in a single pull request is NOT an umbrella** — an issue that names the specific files to touch is well scoped, not over-scoped, even when it names several; (b) the desired end state is not settled — the thread shows unresolved design or product debate with no maintainer decision, a maintainer says the fix reaches core internals, or the issue has **2 or more closed-unmerged linked PRs** (earlier attempts that died); (c) the issue proposes a **new product feature or behavior change** AND nothing shows the project has accepted it — that is, the issue carries **no labels at all**, no maintainer comment supports it, and the opener is not an OWNER/MEMBER/COLLABORATOR. A request that hides an unmade product decision (what the feature should be, whether the project wants it) is not a first issue. **Documentation tasks, bug reports, refactors, and test work are never failed by this condition**, however terse, and any project-applied triage label (`type::documentation`, `type::bug`, `good first issue`, `help wanted`) is evidence the project accepted the issue; (d) it is a usage or support question rather than a change to the project. A terse body, a missing reproduction, or a bare checklist is **not** a scope failure — grade the size of the work asked for, not the polish of the writeup. | required |
| unclaimed | The `assignees:` and `linked PRs:` fields on the `this issue:` line, plus every comment in the thread (an in-thread PR mention counts even when the sidebar shows none; when sidebar and thread disagree, believe the thread). | Passes unless **any** of the following is present: (a) an assignee is set; (b) any linked or thread-mentioned PR is in the **open** state; (c) a claim comment ("I'll take this", "can I work on this", "working on this", "in progress") dated within **180 days** of the capture date that no maintainer has since released. Claims older than 180 days are stale and do not block, and neither do closed-unmerged PRs; a stale-bot nudge does not clear a live claim. **Path Review house rule (live mode, course repo only):** other students' claim comments and their open PRs do not block — condition (c) and student PRs under (b) are waived there; a set assignee still blocks. | required |
| ai-policy-allows | The `contribution policy` line in the repo-facts block. Live: `CONTRIBUTING.md` in the repo root or `.github/`, the docs it links out to, and any `AI_POLICY.md`. | Passes unless the policy states an **outright refusal** of AI-assisted or AI-generated contributions ("we do not accept AI-generated code"). Conditions are not bans: disclosure, human review, personal understanding, and testing requirements all **pass**, as does silence (no policy stated). Our workflow is AI-assisted, so a stated ban is a dead end however good the issue looks. | required |
| labeled-newcomer-friendly | The issue's `labels:` line. | The issue carries a `good first issue`, `easy`, or equivalent newcomer label. | preferred |
| maintainer-responsive | The `maintainer first-response sample` block in the repo-facts. | At least one of the 5 sampled issues drew an owner/member/collaborator reply within 30 days. | preferred |
| small-enough-to-be-seen | The star count on the `repo:` line and the `open issues + PRs` count. | Under 1000 open issues + PRs, where a newcomer PR is less likely to sit in a large queue. | preferred |

## Verdict rule

**Accept if and only if every `required` check grades `pass`.** A single
required `fail` rejects the issue; the remaining required checks are still
graded and reported, so the summary shows every reason it was rejected,
not just the first.

`unclear` on a required check counts as **fail**: a first issue whose
liveness, scope, claim state, or contribution policy cannot be verified
from the evidence is not a first issue worth taking.

`preferred` checks never change the verdict. They are graded and reported,
and on an accepted issue they rank it against the other accepted
candidates — an accepted issue with a newcomer label, a responsive
maintainer sample, and a short queue is preferred over an accepted issue
without them.

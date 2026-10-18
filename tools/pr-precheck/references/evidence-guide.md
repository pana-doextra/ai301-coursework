# Evidence guide: where evidence lives in a PR package

Read evidence in pairs. Candidate assertions must be verified against
artifacts. Eval bundles are complete; never follow source lines to live
issues or substitute evidence from another package.

## Plan fidelity (harness category: silent-drift)

**Eval:** `## Plan context` contains the accepted plan, scope/exclusions,
files, repro, test plan, and deviations. Pair it with `## Candidate PR` >
`### Diff` and `### Commits`; compare `### Title` and `### Description`
claims to that same implementation. Look for missing promises as well as
extra hunks.

**Live:** pair `plan.md` (scope, named files, approach, test plan, and
`## Deviations`) and the issue's posted plan/repro with every hunk of
`git diff main...HEAD` and `git log main..HEAD`. Line one of `pr_draft.md`
is the title; the remainder is the description. Uncommitted edits cannot
support a claim about the committed PR diff.

**Good:** each substantive change and delivered behavior belongs to the
plan or a reasoned same-issue deviation. Explicitly bounded partial
work can pass; silent omission, unrelated cleanup, and exact-plan claims
contradicted by the diff fail. A deviation explains what changed and why,
not merely that the author decided to change it.

## Test evidence (harness category: not-tested)

**Eval:** pair repro and test plan in `## Plan context` with `### Test
evidence`. Read check/suite requests in `## Repo facts` and thread.
Inspect test assertions in the diff but do not equate added with executed.
Track distinct failure modes and controls separately.
Count the distinct runs promised in the test plan, not every sibling
method changed by one mechanism. An executed representative regression
test plus suite can cover the shared fix without one transcript per method.

**Live:** pair `plan.md` and the issue's repro (or house repro pack) with
`test_evidence.md`: exact commands, before on main or preserved Unit 2
output, branch after, environment, controls, and actual check outcomes.
Requested checks live in the PR template and `docs/CONTRIBUTING.md`;
cross-check the Testing boxes in `pr_draft.md`.

**Good:** identified runs give observable results distinguishing fixed
from not fixed, with relevant controls and distinct triggers accounted
for. Compact outcome reports can suffice in frozen bundles; do not demand
raw logs/screenshots the repo never requests. Disclosed unavailable
platforms or failed checks can pass with decisive bounded evidence still
present. No observed behavior, silent missing coverage, or no suite/check
evidence fails. Live Path Review captures all four assignment commands;
report an empty integration suite as no tests ran, never as a pass.
Pre-opening CI stays unticked; post-opening CI is inspected separately.

## Diff quality (harness category: unreviewable)

**Eval:** inspect all `### Diff` hunks and `### Commits` against the plan.
Read context for debug prints, abandoned helpers, commented-out attempts,
unrelated formatting, dependency churn, and drive-by refactors.

**Live:** inspect the complete `git diff main...HEAD`, not just its stat,
and `git log main..HEAD`. Check tests/docs as well as production code.
`plan.md`, `comment.md`, `pr_draft.md`, and `test_evidence.md` are review
inputs and are never committed to the Path Review fix.

**Good:** the fix and necessary tests/docs are visible without unrelated
churn or temporary debris. Coherent multi-file work can pass; a debug
leftover can fail. Size and commit count do not measure reviewability.

## Standards and comms (harness category: standards-wall)

**Eval:** `## Repo facts` states template/contributing asks and AI-use
policy. `## Thread highlights` contains maintainer direction. Pair these
with title, description, commits, tests/docs. No template or disclosure
policy means do not invent one. Description-versus-diff truth belongs
to plan fidelity above.
Frozen diffs show excerpts, so do not invent a contradiction in unshown
caller/helper code or demand exact diagnostic text without a stated need.

**Live:** read target `.github/PULL_REQUEST_TEMPLATE.md`,
`docs/CONTRIBUTING.md`, linked policy, and issue thread. Apply `scope.md`
fork/branch/issue rules. Pair each request with `pr_draft.md` and the
committed diff. Read personal `voice-guide.md` for separate notes. The
course always requires actual AI-use disclosure in Notes for Reviewers.

**Good:** applicable sections contain real content or explained N/A;
boxes match recorded results; issue, branch, commits and required
supporting files honor stated conventions; required tool/extent disclosure
is explicit. Terse compliance passes. Empty boilerplate, missing required
disclosure, and silently ignored maintainer asks fail. AI mentions and
review concerns alone do not create disclosure obligations.

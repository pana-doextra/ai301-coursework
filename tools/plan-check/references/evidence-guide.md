# Evidence guide: where evidence lives in a plan package

A plan package has six parts: the repo-facts block, the issue, the thread
highlights, the repro evidence, the candidate plan, and the candidate plan
comment. The families below say which part each kind of evidence lives in
and what good looks like there.

A standing rule for every family: **the plan is not required to use
headings or the words below.** Plans state their cause in a sentence, bound
themselves in a clause, and name their test inline. Look for the substance
wherever it sits, including inside a paragraph. A missing heading is never
a finding; a missing fact is.

## Diagnosis and grounding

**Where it lives.** The plan's cause is usually its first substantive
sentence or a line beginning "Cause:" or "Diagnosis:"; in a terse plan it
can be a subordinate clause ("...so the commits keep their pre-push
flags"). The evidence it must answer to is the `## Repro evidence` block,
specifically its artifact, its `Actual:` line, and its control or contrast
runs. A second source of grounding is the issue body (which may already
name a file and line) and `## Thread highlights` (an owner may already have
located the culprit).

Live: the cause is in the student's `plan.md`; the evidence is their posted
repro comment on the issue thread (or the house repro pack as quoted in the
drafts), plus the issue body and the thread itself.

**What good looks like.** The repro's discriminating runs are the
instrument. A reproduction that times the same operation with the pager
removed, or runs the same command with highlighting off, has told you where
the cost lives; a diagnosis that places the cause somewhere those runs have
already exonerated does not follow from the evidence, however confidently
and specifically it is written. Good grounding survives every run in the
block. A cause taken from the issue or an owner's comment is well grounded
provided the repro is consistent with it — adopting an owner's analysis is
grounding, not a shortcut. Beware the well-written wrong cause: precise
file names, line numbers, and a named internal type are not grounding, and
a plan that cites the thread for a cause the thread did not establish is
borrowing authority, not evidence.

## Scope

**Where it lives.** The plan's boundary: a "Scope:" line, a "Change:"
clause, an "In scope / Not in scope" pair, or the list of files the plan
says it will touch. Deferrals often sit at the end of a scope statement
("...worth its own issue; I will file it separately"). Read against the
`## Issue` section, which defines the one behavior in question.

Live: the scope section of `plan.md` and the files it names, read against
the issue the branch belongs to.

**What good looks like.** One bounded change addressing the reported
behavior, where every named file or area sits on the code path the
diagnosis implicates. A bounded change can touch two sites when one
diagnosis covers both; what makes it one change is the single cause, not
the single file. An explicit not-in-scope line is the clearest signal and
the commonest form of good scoping, but a plan whose named work is
obviously narrow is bounded without one. A drive-by rewrite announces
itself in a few recognisable ways: an unrelated bug folded in "while I'm in
there", a refactor or reformat of the surrounding module, a dependency
bump, or a goal so broad ("make undo work") that no file list could bound
it. Deferring a related symptom is good scoping and reads as a strength.

## Executability

**Where it lives.** The plan's "Approach", "Changes", "Files", or numbered
steps; in a terse plan, the clause naming where the change goes ("the push
completion callback in `pkg/gui/controllers/sync_controller.go` adds the
commits context to its post-push refresh scope").

Live: the approach and files sections of `plan.md`.

**What good looks like.** A stranger with the repo checked out knows which
file to open and what to do first, without asking the author anything. Two
things make that true: a code location specific enough to find once (file,
function, handler, call site — a described site can be as good as a path),
and the mechanism of the change, not just its goal. "Call
`generate_default_bindings()` before registering bat's custom bindings,
then override `home`/`end` afterwards" is executable. "Poke around the
editor components and figure out where the undo history lives" names a
research task, and the giveaway is that the plan's own author does not yet
know where the change goes. Ordered steps help but are not required, and
flagged uncertainty about a site ("the exact fix site may move one level
during implementation") is precision, not vagueness: the stranger still
knows where to start.

## Test plan

**Where it lives.** The plan's "Test plan" or "Test:" line, read side by
side with the `## Repro evidence` block's numbered steps, controls, and
`Expected:` / `Actual:` lines.

Live: the test-plan section of `plan.md`, read against the repro steps in
the student's posted repro comment.

**What good looks like.** The repro already established a procedure that
makes the bug visible; a decisive test plan re-runs it and says what must
be seen instead. Good test plans name the observation — the color flips
without leaving the view, the command exits 0 and draws output, the pager
lands on the last line immediately, the three pattern spellings each print
the fixture path — and they usually name the repro's controls as having to
stay unchanged. Two failures recur. The first is the suite stand-in: "run
`cargo test --workspace` and make sure nothing regresses" tests everything
except this bug, since the suite passed before the fix too. The second is
the goal restated as its own test: "undo works after toggling" names no
run, no input, and no observation anyone else could make. A planned
automated regression test strengthens a plan but does not replace naming
the observable outcome.

## Honesty

**Where it lives.** Spread across the plan and the comment rather than
collected: risk and unknown statements wherever they sit, hedging verbs
("I suspect", "I have not yet measured", "pending the benchmark"), claims
about code the author may not have read, claims about performance or other
platforms, and the `## Deviations` heading at the end of a live `plan.md`.

Live: `plan.md` including its `## Deviations` section, and `comment.md`,
read against the posted repro comment and the actual diff once a build has
started.

**What good looks like.** Every factual claim is one the package has
established, and everything else is marked as what it is. The honest
register is easy to recognise: a named open question, a stated trade-off
with the condition that would resolve it, a site flagged as possibly
moving. False confidence is the opposite move — an unverified cause
asserted flatly, a cost claim with no measurement behind it, another
platform claimed with no run shown. Absence is not evidence here: a short
plan on a well-isolated bug may honestly have no risks to report, and
having nothing uncertain to say is not the same as hiding something. After
a build, the thing to check is whether what actually changed is recorded
under `## Deviations`; a deviation that exists only in the diff is the
failure. Note that committing to deliver — a PR, a follow-up, a timeline —
belongs to this register and is not overclaiming; it is what a plan comment
is for.

## Comms

**Where it lives.** Two separate sources, and they fail in different ways.
*Thread signals*: `## Thread highlights`, with each comment's author role
(OWNER, CONTRIBUTOR, NONE) — direction given, approaches rejected,
constraints set, culprits located, patches already posted. *Stated
conventions*: the repo-facts block's `contribution policy` line and
`bug reports:` template line. Both are read against the
`## Candidate plan comment` (and, for direction, the plan itself).

Live: the issue thread on GitHub for signals; `CONTRIBUTING.md` in the repo
root or `.github/`, any `AI_POLICY.md`, and the docs they link to for
conventions; `comment.md` is the candidate side.

**What good looks like.** Thread-aware means the plan reads as though
written after the thread, not before it: it follows direction a maintainer
gave, or departs from it and says why. The failure to watch for is a plan
overtaken by its own thread — proposing to document a workaround when the
owner has already located the culprit in code, posted a patched binary, and
had the reporter confirm it. The plan may be internally excellent and still
be work nobody needs. Disagreement stated with a reason is fine; silence
about direction already given is not.

On conventions, the only question that changes a verdict is whether the
repo's stated policy *requires* disclosing AI assistance. A policy that
requires it (and may also ask for the tool used and the extent of the
assistance) must be met by a plain sentence in the comment; a strong plan
with no disclosure still fails where disclosure is required. Most repos
require nothing, and silence is then correct — do not manufacture a
requirement from a maintainer's remark that AI-generated PRs are hard to
review, from a vouch flow for new contributors, from a human-review
obligation, or from a rule that comments be written in the contributor's
own words. Read a ban for what it covers: forbidding AI-written comment
text is not forbidding AI-assisted work, and a comment written in the
contributor's own voice already complies with it.

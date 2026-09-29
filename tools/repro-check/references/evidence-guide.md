# Evidence guide: where proof lives in a reproduction package

The map `rubric.md` reads with. For every family of proof a check names,
this file says where to look and what good looks like when you get there.

Two shapes of package, and the locations differ:

- **Eval bundle** — one markdown file with fixed sections: a front-matter
  list (`source:`, `captured:`), `## Repo facts`, `## Issue`,
  `## Thread highlights`, `## Candidate claim comment`, and
  `## Candidate repro report`. The bundle is the whole world; fetch nothing.
- **Live** — the issue thread on GitHub, the repo's own docs, and the
  student's draft comment file(s) on disk.

A standing rule for both: the package is what the drafts contain and quote.
Something true of the student's machine but absent from the draft is not
evidence, because the stranger reading the posted comment cannot see it.

## Environment

**Where it lives.** In the bundle, inside `## Candidate repro report` — usually
an opening `Environment:` line or a `**Environment.**` sentence, but it may be
scattered through the prose or visible only in a shell prompt inside a
transcript, and all of those count. Read it against two other places: the
version and OS lines in the `## Issue` body (what the issue targets), and the
`bug reports:` line of `## Repo facts`, which names the fields that repo's
own template asks every reporter for. Live: the same record in the draft repro
comment, against the issue's filled-in template fields and the bug template in
`.github/ISSUE_TEMPLATE/`.

**What good looks like.** A reader could assemble the same setup from the text
alone: platform, the version or commit under test, and — where the issue turns
on it — the install route, the build profile, the driver or backend, the shell.
`lazygit 0.64.1 (release binary), git 2.55.0, Ubuntu 24.04` is sufficient;
`on my machine, latest version` is not. Where the tested environment differs
from the issue's, the difference is named in words rather than left for the
reader to spot: *"The issue was filed from v0.63.1 on Termux; same behavior
here"* is the move. Three traps: a strong artifact sitting above no record at
all reads as thorough and is not — check for the record separately from the
output; a version stated only inside a `$ tool --version` transcript line does
count; and the fields the repo's template asks for are the fields its
maintainers will ask for again, so a missing one is a real gap, not pedantry.

One more comparison lives here: the `latest release` line of `## Repo facts`
(live: the Releases box in the right sidebar). A report run against the current
release or repository tip tells a maintainer the bug is still live; one run only
against the older version the issue was filed from leaves that open.

## Steps

**Where it lives.** In `## Candidate repro report`, as a numbered list, a prose
walkthrough, or a shell transcript that is itself the steps. Read against the
issue body, which often carries its own `Steps:` block or a minimal
reproduction command — that is the standard the report is trying to meet.
Live: the same, in the draft, against the issue's reproduction section.

**What good looks like.** Starting state through trigger, with nothing for the
reader to invent. The starting state is explicit or trivially implied
(`git init -q t && cd t && touch new.txt` states it; "in a repo with some
changes" does not, when which changes decide the outcome). The triggering
command, keystroke, or input appears verbatim and character-exact, not
paraphrased — on an issue about a specific input, *"I searched for something
that wasn't there"* is not a step. Shorter than the issue's own list is fine
when the short path reaches the same trigger. Length and formatting are not
the measure: a four-line transcript can be complete and a ten-step headed
list can omit the one step that matters.

## Behavior shown

**Where it lives.** The artifacts inside `## Candidate repro report`: fenced
command transcripts, output excerpts, log tails, tracebacks, panic messages,
exit codes, state dumps, screenshot descriptions. The thing to read them
against is the failure the `## Issue` body describes — its error string,
exception type, panic text, stack frames, exit code, or described visible
behavior. Live: the same, with the issue body and thread on GitHub.

**What good looks like.** The artifact carries the issue's own signature, not
a generic sign of trouble. Match on the identifying detail: `panic: not a
string` and `Error: unable to parse hcl ... Missing key/value separator` are
both failures and are not the same failure; exit 101 and exit 1 are different
outcomes. Read the transcript's **input** as closely as its output — the
invocation and the sample document in the report have to be the ones the issue
named, because a mistyped input reproduces a different bug and its output will
still look like evidence. Absence can be the artifact when the bug is a silent
no-op, provided the report shows both the command that produced nothing and a
state check proving the expected effect is missing. And a faithful attempt that
did **not** trigger the behavior, reported as such, is a real result: a
cannot-reproduce backed by a recorded environment and the issue's exact trigger
is proof, and belongs in the thread.

A second artifact sometimes sits beside the failing one: the same command
shifted off the trigger, a near-miss input, an unaffected version, a
pre-condition check. That is a **control**, and it lives in the same fenced
blocks. A control is what turns "I saw a crash" into "I found the edge" — the
boundary scan panics, the scan one step away completes normally — so it is
worth noticing when present. It is never required for a package to be postable.

## Honesty

**Where it lives.** The seam between the report's assertions and its artifacts:
the `Analysis`, `Actual`, and concluding sentences of
`## Candidate repro report`, plus any sentence in `## Candidate claim comment`
about what has already been established. Read every such sentence against what
the fenced blocks above it actually contain. Live: the same seam in the draft.

**What good looks like.** Each load-bearing statement traces to something
visible in the package. The failures have a shape worth knowing: a conclusion
that renames the artifact into the issue's language (*"exactly the class of
failure the issue describes"*, over output that is a different failure);
unverifiable scale offered as evidence (*"all my machines"*, *"every single
day"*, *"everyone I know"*); runs claimed but never shown while the argument
leans on them (*"also confirmed on 4.53.2"*, *"ran it ten times"*) — the extra
run may well have happened, but the reader cannot check it, so it cannot carry
weight; and a cause stated as a finding when nothing in the package
established it. The test is whether the unshown run is doing the work: a
repeat count standing behind the outcome the report already shows — including
a cannot-reproduce saying how many attempts it made before concluding nothing
happened — is precision about a negative, not hidden proof. A control run in
the environment already recorded, whose
result is simply stated (*"dropping `-r` reports 1, 4, 7, 10"*), is not that
pattern — it reaches for no unrecorded machine and no unverifiable scale, and
whether its transcript is shown is a `control-or-contrast` question, not an
honesty one. Hedges marked as hedges are honest and pass: *"I suspect"*,
*"this may be"*, *"the reporter points at X, which would fit"*. The line is
not confidence. It is confidence that outruns the package's own evidence.

## Comms

**Where it lives.** Three places meeting. The words: `## Candidate claim
comment` and `## Candidate repro report`. The issue they answer to: the
`## Issue` title and body. The house standard: the `contribution policy` and
`bug reports:` lines of `## Repo facts`. Live: the drafts, the issue thread,
and `CONTRIBUTING.md` in the repo root or `.github/`, any `AI_POLICY.md`, the
docs those link out to, and the issue template — plus `scope.md` in this skill
directory, whose Path Review house rules change how a classmate's existing
claim on the same issue is read.

**What good looks like.** The claim could only have been written under this
issue: it names the actual symptom, file, or code path, in the writer's own
words. It promises investigation and nothing beyond it — an intended direction
(*"plan: find where the stash-name prompt decides to appear"*) is a promise
kept by looking, while a fix, a PR, or a date is a promise kept only by
shipping. Register is not the test: a warm, emoji-laden, specific claim is
fine, and a formal boilerplate one that would sit under any issue in the
tracker is not.

On policy, read the stated words and only those. A **disclosure requirement**
is a policy that says contributors must state when AI assistance was used;
where one exists, at least one of the two comments has to say so plainly, and
silence fails however good the proof above it is. These are **not** disclosure
requirements, and treating them as such will fail packages that should pass:
permissive-with-responsibility language (*you are responsible for what you
submit*), a maintainer noting that review time is scarce or that AI-generated
PRs are hard to assess, an obligation to review or test your own work, a rule
that comments to maintainers be written by a human in their own words rather
than pasted from a model, a standard contribution guide, and no AI policy at
all. An outright ban on AI-assisted contribution is a different thing again:
nothing in this workflow can honestly comply with it — but read what the ban
covers. A repo that welcomes AI-assisted coding with a human in the loop and
only forbids AI-*written comment text* has not banned AI-assisted
contribution; a comment in the contributor's own voice complies with that
rule, and owes no disclosure.

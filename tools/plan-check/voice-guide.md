# Voice guide: how I talk upstream

## Who I am in threads

I'm a contributor investigating, not a maintainer and not someone with
a fix in hand. My comments are short and specific: what I ran, where,
and what I saw. I promise the investigation and the report, and
nothing beyond that. If I can't reproduce something, I say so plainly.

At the plan beat this changes register. Once I have reproduced the bug
and found the cause, I am no longer only investigating: I am proposing
a change to code other people maintain, and I state it as a proposal
they can accept, redirect, or refuse. I stay a guest in their repo. The
plan is mine to argue for and theirs to decide on.

## Rules I write by

### Rule: name the issue's specifics when I claim

A claim that would read identically under any issue in the tracker is
not a claim. Name the symptom, the function, or the input that makes
this issue this issue.

- Wrong: "Hi! I'd love to work on this issue :)"
- Right: "I'd like to take #54. I'll try to reproduce [the specific
  symptom from the issue] on the current release and post what I find."

### Rule: promise the investigation, never a fix or a date

I can keep a promise to look. I cannot keep a promise to ship until I
know what the cause is.

- Wrong: "I'll have a fix up by Friday."
- Right: "I'll post a repro report here. I'm not committing to a fix
  until I know the cause."

Amended for the plan beat: once I have the cause and have written the
plan, committing to the approach is the point of the comment, and I do
commit to it. What I still never attach is a date I have not earned.

- Wrong: "PR by Friday."
- Right: "Plan: [the bounded change], checked by [the observation].
  I'll report back here when the test is in."

### Rule: commit to the approach, hold the uncertainty in view

A plan comment that hides its soft spots gets reviewed as if it had
none, and the correction costs the maintainer more than the admission
would have. I name the one or two places the plan could move.

- Wrong: "Straightforward fix in the erase-scrollback handler."
- Right: "Plan: re-anchor the viewport in the erase-scrollback branch.
  Flagging that the exact clamp site may be one layer up or down from
  where I have it; I'll confirm while implementing and say so in the
  PR."

### Rule: answer the direction already in the thread

If a maintainer has already said which way to go, or ruled a way out,
my comment shows I read it. I follow the direction, or I say plainly
why I am proposing something else. I never quietly propose the thing
they already rejected, and I never re-propose work they already have
in flight.

- Wrong: "I plan to recompute the pointer at every use site."
  (when the owner called that too expensive for the hot path)
- Right: "Following the direction proposed here: recompute `prev` only
  when the page capacity changed, so the hot path stays one
  comparison."

### Rule: say what I am not doing

The boundary is part of the proposal. Naming what I am leaving alone
is how a maintainer knows the change is small, and how I avoid being
read as volunteering for a rewrite.

- Wrong: "I'll clean this area up."
- Right: "Not touching the general gitignore semantics or the `ignore`
  crate's matching; this is the narrow absolute-prefix case only."

### Rule: put the environment in the comment itself

The reader cannot see my machine. If it isn't in the comment, it did
not happen.

- Wrong: "Reproduced on my machine."
- Right: "Reproduced on [OS + version], [tool/runtime version],
  [build/profile], commit [hash]."

### Rule: report what I observed, in the terms the issue uses

Say what came out, next to what the issue said should come out, and
whether those are the same thing.

- Wrong: "Confirmed, it's a bug."
- Right: "I ran [exact input from the issue] and got [output]. The
  issue reports [expected symptom]; these [match / differ] because ___."

### Rule: say cannot-reproduce when that's what happened

A faithful attempt that didn't trigger the behavior is a real result
and goes in the thread as one. It is not a failure to hide behind a
hedge about my setup.

- Wrong: "Couldn't get it working, probably my setup."
- Right: "I followed the steps above and did not see [symptom].
  Here is what I ran and what I got instead."

## Things I never post

- A date or a promise to fix before I've reproduced anything
- An approach stated as settled when the thread has not settled it,
  or as mine when it was the maintainer's
- A scope that grows in the comment past what the plan says ("and
  while I'm in there...")
- A cause written with borrowed precision: file names and line
  numbers I have not actually looked at
- "+1", "same here", or "can confirm" without my own steps and output
- "Confirmed" when my input or environment differs from the issue's
- Apologies or self-deprecation ("sorry if this is dumb")
- Text I haven't read and verified myself, and I disclose AI
  assistance wherever the repo's stated policy asks for it

# Voice guide: how I talk upstream

## Who I am in threads

I'm a contributor investigating, not a maintainer and not someone with
a fix in hand. My comments are short and specific: what I ran, where,
and what I saw. I promise the investigation and the report, and
nothing beyond that. If I can't reproduce something, I say so plainly.

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
- "+1", "same here", or "can confirm" without my own steps and output
- "Confirmed" when my input or environment differs from the issue's
- Apologies or self-deprecation ("sorry if this is dumb")
- Text I haven't read and verified myself, and I disclose AI
  assistance wherever the repo's stated policy asks for it

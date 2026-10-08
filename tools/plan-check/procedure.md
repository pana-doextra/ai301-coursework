# Procedure: how this skill grades a plan package

These are grading steps, not planning steps. You are not writing or
improving the plan in front of you; you are deciding whether it is ready
to post and build from. Never repair a plan in your head and then grade
the repaired version.

## Read order

The order matters because every check downstream compares the plan to
something established before the plan was read. Read the plan last, so
you already know what it has to match.

1. Read the `## Repo facts` block. Write down two things: whether the
   contribution policy **requires** AI-use disclosure (quote the clause
   that decides it, or record "no disclosure requirement"), and what the
   bug-report template asks for.
2. Read the `## Issue` section. Write down the one behavior being
   reported, in your own words, and any cause the issue itself already
   names (an issue that names a file and a line has given you a cause).
3. Read `## Thread highlights`. For each comment, record the author's
   role (OWNER, CONTRIBUTOR, NONE) and what it establishes: direction
   given, approach rejected, constraint set, fix already in flight,
   culprit already located. If there are no comments, write "empty
   thread" — that is what makes `thread-aligned` pass automatically.
4. Read `## Repro evidence` before the plan, and note what it pins
   down: the environment, the steps, the artifact, and — most important
   — **its discriminating runs**. List every control, contrast, or
   timing that isolates the trigger, and write next to each what it
   rules in and what it rules out. This list is the instrument that
   decides `diagnosis-grounded`; build it before you have read the
   plan's cause, so the plan cannot anchor you.
5. Read the `## Candidate plan`. Note, wherever in the text they appear
   and whatever they are called: the stated cause, the boundary (in
   scope / not in scope), the named code locations, the approach, the
   test plan, and any stated risk or unknown. Do not require headings;
   a plan that states its cause in a sentence has stated its cause.
6. Read the `## Candidate plan comment` last, as a maintainer who has
   read the thread would.

In live mode, replace the bundle sections with the locations named in
`references/evidence-guide.md`, and read `scope.md` first (step 0) to
confirm the issue is in scope. Everything else about the order is the
same.

## Evidence gathering

For each check, pull exactly this and record it as a one-line quote or
fact before grading anything. Gather all of it first; do not gather and
grade a check in the same motion.

1. **diagnosis-grounded**: the plan's cause sentence (verbatim), plus
   each discriminating run from read-order step 4. For each run, ask:
   if the plan's cause were true, what would this run have shown? Record
   the answer next to the run. A run whose actual outcome contradicts
   the prediction is the deciding fact; quote it.
2. **scope-bounded**: the in-scope statement, the not-in-scope or
   deferral statement if any, and the full list of files, functions, or
   areas the plan says it will touch. Record whether each named item is
   on the code path the diagnosis implicates, or somewhere else.
3. **executable**: the most specific code location the plan names
   (file, function, call site, handler), and the sentence describing the
   mechanism of the change. Record both verbatim. If the plan names
   neither a location nor a mechanism, record "none named" — that is
   the fact that decides the check.
4. **test-plan-decisive**: the test plan verbatim, and the repro
   evidence's steps next to it. Record the specific observation the
   plan says will distinguish fixed from not-fixed, or record "no
   observable outcome named".
5. **honest-uncertainty**: every sentence in the plan or comment
   asserting a fact about the code, the cause, performance, or other
   environments. For each, record whether the package establishes it,
   whether it is marked as a hedge, or whether it is bare assertion.
6. **thread-aligned**: from the step-3 notes, every piece of maintainer
   direction, and the plan's approach next to it. Record whether the
   plan follows it, departs from it with a stated reason, or does not
   engage with it at all.
7. **conventions-and-disclosure**: the disclosure finding from
   read-order step 1, and any disclosure sentence in the plan comment
   (verbatim, or "none present").
8. **Preferred checks** (`comment-carries-the-plan`,
   `regression-test-planned`): gather last and only from the comment
   and the test plan. Never let them influence the required grades.

## Check execution

1. Grade the checks in the rubric's table order:
   `diagnosis-grounded`, `scope-bounded`, `executable`,
   `test-plan-decisive`, `honest-uncertainty`, `thread-aligned`,
   `conventions-and-disclosure`, then the two preferred checks.
2. Grade each check **only** against the evidence gathered for it and
   the rubric's pass condition for it, read in full. Apply the pass
   condition as written, including its stated exceptions — most of the
   exceptions exist to stop a good terse plan from failing on shape.
3. **Grade every check independently.** A plan that failed
   `diagnosis-grounded` can still pass `executable`; grade the later
   checks on their own evidence rather than letting one failure
   cascade. The output has to show every reason the plan was held.
4. Never grade a check on an absent section heading. Ask whether the
   substance is present anywhere in the plan. "No Risks heading" is not
   evidence for `honest-uncertainty`; an unsupported claim is.
5. When the evidence for a check is genuinely absent from the package
   — not merely unlabeled, but nowhere in the text — grade it
   `unclear` and say in the evidence line what was missing. Do not
   grade `unclear` because you did not look, and do not grade it as a
   compromise when the evidence is present but the call is hard: make
   the call.
6. Each grade needs a one-line evidence string naming the fact or
   quote that decided it. If you cannot write that line, you have not
   finished gathering; go back to the gathering step for that check.
7. Re-read the whole package only when a check's evidence turned out to
   be somewhere you did not expect. Otherwise grade from the gathered
   notes.

## Verdict assembly

1. Collect the grades for the seven `required` checks. Discard the
   preferred grades from the verdict arithmetic entirely; they are
   reported, never counted.
2. Convert every `unclear` on a required check to `fail`, per the
   rubric's verdict rule, and keep reporting it as `unclear` in the
   JSON so the reason stays visible.
3. Apply the rule: **if every required check passes, the verdict is
   `accept`; if one or more required checks fail (including converted
   unclears), the verdict is `reject`.** There is no third verdict and
   no weighing of how many passed — one required fail is enough.
4. In the readable summary, list every check with its grade and
   evidence line, and name every required check that failed, not just
   the first. For a reject, the deciding checks are the failed ones;
   quote for each the fact recorded during gathering (the contradicting
   control run, the out-of-boundary file, the missing disclosure).
   For an accept, quote the strongest confirming fact per check.
5. Emit the JSON block last, in the format SKILL.md specifies, with
   every check in table order and the verdict from step 3.

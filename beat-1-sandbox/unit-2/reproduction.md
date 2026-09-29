# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

[pana-doextra]

---

## Posted upstream

**Claim comment**

[https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-5882924863]

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Four full runs, in order:

1. full run, rubric as first written — `agreement: 19/20 scored items  (bar: 18/20: PASS)`
2. full run, after the `conventions-and-disclosure` and prose-control revisions —
   `agreement: 19/20 scored items  (bar: 18/20: PASS)`
3. full run, after the repeat-count revision — `agreement: 20/20 scored items  (bar: 18/20: PASS)`
4. final full run, the one committed here — `agreement: 20/20 scored items  (bar: 18/20: PASS)`

Runs 1 and 2 both scored 19/20 but disagreed on *different* packages: run 1 missed
`pkg-03`, run 2 missed `pkg-09`. The last score matches the agreement line in the
committed `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
`categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3
wrong-target 4/4`.

Between the full runs I re-graded single packages six times each rather than using
`--only` once. That mattered: this eval is not deterministic, and a single `--only` run
cannot tell a fixed rubric bug from a coin flip. `pkg-03` graded 3 accept / 2 reject
under the original rubric — it was never a clean failure, and one `--only` run would
have told me whichever answer it happened to draw.

**Package analysis**

`pkg-03` (BurntSushi/ripgrep#2779, category `clear-accept`).

Gold label: `accept` — "exact-steps repro on current version with a minus-replace control
matching the owner's trigger note; version delta acknowledged; human-voiced comment
satisfies the repo's AI-comment rule". My rubric decided `reject` on run 1, on
`failed: conventions-and-disclosure`.

Re-grading it six times showed the disagreement was not one bug but two, and neither was
deterministic.

The first is `conventions-and-disclosure`. The repo-facts line reads: "AI-assisted coding
is welcome with a human in the loop who understands the work; comments to maintainers
must be written by humans in their own words, and AI-generated comments may be hidden."
My check listed what is *not* a disclosure requirement, but that list named nothing like
a human-authorship rule, so some runs fell through to my last sentence — "A policy
banning AI-assisted contribution outright also fails here" — and read it as a ban. The
failing run said so in as many words: `policy states "comments to maintainers must be
written by humans in their own words" — an outright ban on AI-generated comments`. That
is the wrong reading twice over: the policy explicitly *welcomes* AI-assisted coding, and
a rule about who writes the comment text is a rule this workflow satisfies by writing the
comment myself. It is not a disclosure obligation and it is not a ban.

The second surfaced only once the first was fixed. `honest-narration` began failing on
the report's control run:

```
Dropping -r '$1' from the same command reports 1, 4, 7, 10 correctly, which
matches the owner's note that --replace is required to trigger it.
```

My check failed "asserted extra runs ... whose output never appears while the conclusion
leans on them". That condition was aimed at appeals to an *unrecorded machine* ("also
confirmed on 4.53.2") or *unverifiable scale* ("ran it ten times"). This run claims
neither: it happens in the environment the report already recorded, and its result is
reported as an observation. Whether its transcript is pasted is a `control-or-contrast`
question, and that check is `preferred`, so it cannot decide a verdict. Fixing both
conditions took `pkg-03` from 3/6 to 6/6 `accept`.

**Check rationale**

Quoted as currently written in `tools/repro-check/rubric.md`:

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| conventions-and-disclosure | The `contribution policy` line of the repo-facts block, plus its `bug reports:` template line. Live: `CONTRIBUTING.md` in the repo root or `.github/`, any `AI_POLICY.md`, the docs those link to, and the issue template. Read against **both** posted comments. (Claim-side: gradeable on a claim-only draft.) | If the repo's stated policy **requires disclosing AI assistance** on contributions or comments, at least one of the two comments says plainly that AI assistance was used; silence where disclosure is required fails, however strong the proof is. If the policy states no disclosure requirement, this check passes with no disclosure present — and these are **not** requirements: permissive-with-responsibility language, a maintainer's note that review time is scarce or that AI-generated PRs are hard to assess, a human-review or testing obligation, a rule that comments to maintainers be written by a human in their own words rather than pasted from a model, a standard contribution guide, or no AI policy at all. Do not infer a requirement from a maintainer merely mentioning AI. A policy banning AI-assisted contribution outright also fails here: our workflow cannot honestly comply — but read what the ban covers. A rule against AI-*written comment text*, or against AI-generated PRs with no human in the loop, is not a ban on AI-assisted contribution: a comment the contributor wrote in their own voice already complies with it, and owes no disclosure. | required |

The shape of this row is set by the fact that exactly one package in twenty turns on it,
and it is the only package that turns on it. `pkg-20` is a gold `reject` whose note reads
"excellent repro on every proof check; ghostty's stated AI policy requires disclosing all
AI usage and the comments do not disclose". Every other proof check passes on it. So this
check is the sole thing standing between a polished package and the wrong verdict — which
means it has to fire on a real disclosure requirement, and it has to stay silent on
everything that merely *mentions* AI.

The long enumeration of non-requirements is what I revised, not what I first wrote. I
started with the principle alone — read the stated words, require disclosure only where
disclosure is required — and it was not enough, because the model needs to know what to
do at the edges, not just what the rule is. Each clause in that list is a shape that
appears in the set: `pkg-05`'s conda policy is permissive-with-responsibility, and
`pkg-03`'s ripgrep policy is the human-authorship rule I added after it misfired.

What I rejected in favour of it was the tempting simplification: "pass unless the policy
contains the word *disclose*". That is easy to apply and it gets `pkg-20` right, but it
decides by keyword rather than by meaning, and it would pass a policy that required
attribution in different words. The check reads the obligation, not the vocabulary.

**Trade-offs**

The `honest-narration` revision changed a package's result, and not the one I was aiming
at. Fixing `pkg-03` I rewrote the failing pattern as "asserted extra environments **or
repeat counts** whose output never appears". That is a sharper rule, and on the next full
run it took `pkg-09` — a gold `accept` — from accept to reject, dropping the score back to
19/20. Re-grading it six times: 2 accept / 4 reject. The reason is one sentence in that
report: "I ran this 5 times and also re-ran with the second command's arguments padded".
My new wording named exactly that.

The distinction I had collapsed is direction. `pkg-09` is an honest cannot-reproduce; its
repeat count stands behind a *negative* result, and saying how many attempts you made
before concluding nothing happened is being precise, not leaning on hidden proof. An
unshown repeat count is inflation when it props up a *positive* claim the package's own
artifacts never show — `pkg-13`'s "guaranteed reproducible" backed by nothing. Splitting
those two cases took `pkg-09` to 6/6 accept, and `pkg-03` stayed at 6/6.

Canaries, re-graded three times each rather than with `--only`, because a single run
cannot distinguish a loosened rule from a lucky draw: `pkg-13` rejected 3/3, still listing
`honest-narration` among its failures every time, and `pkg-08` rejected on every run,
also still failing `honest-narration`. Loosening the clause did not stop it firing where
it should.

What I accept this gives up: a report that asserts a repeat count behind a *negative*
result it never actually ran. "I tried ten times and it never happened" now passes
`honest-narration` on the strength of one shown attempt, and my rubric cannot tell a real
ten from an invented one. I take that trade because the alternative rejects `pkg-09`, and
because the honest cannot-reproduce is the case this rubric most needs to protect — it is
two of the eight clear-accepts (`pkg-09`, `pkg-10`), and a rubric that punishes people for
reporting a negative carefully teaches the wrong habit upstream.

One caveat I would rather state than hide: individual packages in this set grade 3 to 6
accepts out of 6, so `20/20` is one draw from a distribution, not a fixed score. The two
packages I hardened are 6/6 each and the bar is 18, so there is margin — but a re-run
could still land 19/20 on a package I did not probe.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.

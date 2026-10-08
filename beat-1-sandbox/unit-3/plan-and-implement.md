# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`.

---

## Posted upstream

**GitHub username**

pana-doextra

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6048512907

Following up on my reproduction above with a plan.

**Cause.** `_detect_sections()` in `ingestion/parsers/resume_parser.py`
builds four regexes per header, and all four anchor the header to the
very start of a line (`^...` under `re.MULTILINE`, or `\n...`). With
`    Education:` the character after the anchor is a space, so the
header never matches. My repro isolated this with a control run: the
same content unindented returns `['Education', 'Skills']`, while the
issue's indented input returns `[]`. Only the leading whitespace
differs between those two runs, which rules out the header set and the
`parse()` entry point — both runs share them and the control is
correct.

**Plan.** One bounded change: allow optional horizontal whitespace
after each anchor in that pattern list (`[ \t]*`, not `\s*` — `\s`
matches newlines, so `\s*` could swallow blank lines and start a match
on an earlier line than the header's own).
Then remove the five `@pytest.mark.xfail(strict=True)` markers citing
#54, since `strict=True` turns a newly-passing test into a CI failure
until the marker goes, and add a regression case for the indented
input.

**How I'll check it.** Re-running the repro: the issue's exact snippet
must go from `[]` to `['Education', 'Skills']`, the unindented control
must stay unchanged, and the three tests the issue names must go from
`3 xfailed` to `3 passed`. I'll post the before/after output.

**Not in scope**, on purpose: the `return list(set(detected))` on the
last line of `_detect_sections()` makes the returned order
nondeterministic. It is a real wart — it's why I assert on the sorted
result rather than the order — but it isn't this bug, and I'd rather
file it separately than widen this change. Also leaving
`_strip_markdown()`, the PDF path, and `SECTION_HEADERS` alone.

Two things I'm flagging rather than claiming I've settled. The
CONTRIBUTING example commit describes this fix as "strip the line
before matching headings"; I read that as the same fix stated
differently and went with widening the anchors because it's the
smaller diff, but I'm happy to write it the other way if you prefer.
And the issue names three tests while five markers cite #54 — I expect
all five to pass from the one cause, but I haven't confirmed the other
two yet; if one doesn't, I'll report it here rather than chase it in
this change.

---

## Your branch

**Branch**

fix/54-resume-section-whitespace

**Evidence**

My Unit 2 reproduction steps, re-run against the built change. Steps 1 and 2 use the
repo venv; steps 3 and 4 use the system interpreter, which is the one with `pytest`
installed.

Before the fix:

```
### BEFORE - step 1: issue's exact reproduction
$ python -c "
from ingestion.parsers.resume_parser import ResumeParser
r = ResumeParser()
res = r.parse('\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n')
print(sorted(res.metadata['detected_sections']))
"
[]

### BEFORE - step 2: unindented control
$ python -c "
from ingestion.parsers.resume_parser import ResumeParser
r = ResumeParser()
res2 = r.parse('John Smith\njohn@example.com\n\nEducation:\n- B.S. Computer Science\n\nSkills: Python\n')
print(sorted(res2.metadata['detected_sections']))
"
['Education', 'Skills']

### BEFORE - step 3: the three tests the issue names
$ python -m pytest tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections -v
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text XFAIL [ 33%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience XFAIL [ 66%]
tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections XFAIL [100%]
============================= 3 xfailed in 0.75s ==============================

### BEFORE - step 4: full parser suite
$ python -m pytest tests/unit/test_resume_parser.py -q
5 passed, 5 xfailed in 0.61s
```

After the fix:

```
### AFTER - step 1: issue's exact reproduction
$ python -c "
from ingestion.parsers.resume_parser import ResumeParser
r = ResumeParser()
res = r.parse('\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n')
print(sorted(res.metadata['detected_sections']))
"
['Education', 'Skills']

### AFTER - step 2: unindented control
$ python -c "
from ingestion.parsers.resume_parser import ResumeParser
r = ResumeParser()
res2 = r.parse('John Smith\njohn@example.com\n\nEducation:\n- B.S. Computer Science\n\nSkills: Python\n')
print(sorted(res2.metadata['detected_sections']))
"
['Education', 'Skills']

### AFTER - step 3: the three tests the issue names
$ python -m pytest tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections -v
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text PASSED [ 33%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience PASSED [ 66%]
tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections PASSED [100%]
============================== 3 passed in 0.56s ==============================

### AFTER - step 4: full parser suite
$ python -m pytest tests/unit/test_resume_parser.py -q
11 passed in 0.45s
```

The step-2 control is unchanged, which is the point of including it: the fix removed
the failure without altering the path that already worked. I also confirmed the new
regression test genuinely guards the bug by reverting the source fix and re-running it
alone — it failed, then passed again once restored.

## Eval iterations

**Run history**

1. Calibration smoke run (`--only calib-01,calib-02,calib-03,calib-04
   --include-calibration`): all 4 matched their worksheet labels. Not scored — the
   harness reports `agreement: 0/0 scored items` for a calibration-only run.
2. First full run: **19/20** (`bar: 18/20: PASS`, all five categories matched).
3. Partial re-grade after revising one check (`--only
   pkg-14,pkg-01,pkg-15,pkg-19,pkg-06,pkg-02,pkg-20`): **7/7**. No bar printed; partial
   runs cannot decide the bar or the category floor.
4. Confirming full run: **20/20** (`bar: 18/20: PASS`).

The last score, 20/20, is the agreement line in the committed `eval-run.txt`.

**Package analysis**

`pkg-14` (category `clear-accept`). Gold label: **accept**. My rubric decided
**reject** on the first full run — the only disagreement in that run.

It failed on my `honest-uncertainty` check. The grader's evidence line was: *"Cause
asserted flatly before tracing; claims 0.44.1 'predates the reattach-path change in
0.44.2' and that fresh attach 'performs the same queries' with nothing in the package
establishing either."*

Reading the package, my rubric was being too strict rather than the gold label being
wrong. The plan diagnoses an OSC-colour-query leak on reattach in zellij, and it does
ground that cause: it cites the fresh-attach run, the reattach leak, the 0.44.1 clean
run, and the cache control, and it carries a real stated risk about draining a
legitimate keystroke. The sentences my check caught were explanatory framing wrapped
around evidence the repro did show — why the control behaves as it does, which release
a change landed in — not claims the plan's correctness rested on. My pass condition
said "no claim outruns the package's own evidence" and drew no line between a
load-bearing claim and incidental framing, so any unproven detail anywhere in the plan
could sink it. That is what fired here.

**Check rationale**

From `tools/plan-check/rubric.md`, the `honest-uncertainty` row as it reads now:

> No **load-bearing** claim outruns the package's own evidence. A load-bearing claim is
> one the plan's correctness rests on: the cause itself, the boundary, or the promise
> that the change will fix the behavior. Grade those. Do **not** fail a plan on
> incidental framing — a parenthetical about why a control behaves as it does, a
> version's provenance, a mechanism named while interpreting a run the repro did show.
> Interpreting your own evidence is what a diagnosis is; only a claim that would have to
> be separately verified, and that the plan leans on, counts here. A plan may be
> confident where the evidence is settled and **is not required to carry risks,
> unknowns, or a deviations note** — a short plan on a well-isolated bug with nothing
> uncertain to report passes with none. What fails is false confidence that changes what
> a maintainer would believe: an unverified cause asserted as established fact with no
> evidence offered for it, a performance or compatibility claim with nothing behind it,
> a guess about a code path presented as something the author has read, an environment
> or platform asserted as tested that the package never ran, or a deviation that exists
> in the work but not in the plan. A cause presented *with* the runs that ground it is
> not a flat assertion even where one supporting detail is unproven. Hedges marked as
> hedges pass ("I suspect", "I have not yet measured", "the exact site may move"), and
> naming an open question or an untestable platform is a strength, never a fail.
> Promising a PR, a follow-up, or a timeline is **not** this failure: a plan comment
> commits to an approach, and committing to deliver it is the register's normal
> business. When a plan both grounds its cause in named runs and states a real risk or
> unknown, it is in the honest register; grade it `pass` unless a specific load-bearing
> claim contradicts or outruns the package.

It reads that way because of `pkg-14`. The first draft opened with the flat rule "No
claim outruns the package's own evidence", which treats every sentence in a plan as
equally weighted and is why pkg-14 was rejected. The revision introduces the
load-bearing/incidental distinction and names the three things that actually carry a
plan — the cause, the boundary, the promise that the change fixes the behavior — so the
check grades those and explicitly leaves explanatory framing alone.

Two things I rejected while writing it. First, deleting the check: that was never an
option, since the failure family it covers (unknowns dressed up as certainty) is one
the lecture named and the eval set is built around. Second, a structure-shaped version —
"the plan must have a Risks section." That would have been easy to apply consistently,
but `calib-01` is a gold **accept** that carries no risks section at all, so a
structural rule would have failed a package the staff labelled ready. The clause "is
not required to carry risks, unknowns, or a deviations note" is there specifically to
keep that from happening, and it is why the final rule judges claims rather than
sections. The closing sentence was added last, after noticing the grader needed a
positive instruction about what the honest register looks like, not just a list of
failures.

**Trade-offs**

The revision loosened the check, and a loosened check can flip a package that already
agreed. Before the confirming full run I re-ran `--only` with `pkg-14` plus canaries
drawn from the previous full run's `category` column — `pkg-01` (wrong-cause),
`pkg-06`, `pkg-15`, `pkg-19` (scope-creep), `pkg-02` (clear-accept) and `pkg-20`
(thread-convention, the 2-package category where a single flip cannot be bought back on
volume). All seven agreed: pkg-14 flipped to accept and no canary moved.

I chose those canaries from the per-check results rather than at random. Dumping the
`--out` JSON showed which packages had `honest-uncertainty` among their failing
required checks: `pkg-01`, `pkg-06`, `pkg-07`, `pkg-11`, `pkg-12`, `pkg-15`, `pkg-16`
and `pkg-19`. Every one of them also failed at least one other required check —
`diagnosis-grounded`, `scope-bounded`, or `thread-aligned` — so under a verdict rule of
"accept only if every required check passes", none of them could flip on this change
alone. That is the reason nothing else moved, and I knew it before spending the $4 full
run rather than after.

What the check now gives up: a plan that states its cause with real grounding and a
stated risk, but slips in one confident unverified claim about a code path it never
read, will pass `honest-uncertainty` as long as that claim is not what the plan leans
on. I accept that. The alternative cost me a gold `accept`, and in a live review that
kind of detail surfaces as a review comment, whereas a wrongly-held plan never gets
posted at all.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

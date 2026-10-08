# Plan — Issue #54: resume section detection fails on leading whitespace

## Diagnosis

`ResumeParser._detect_sections()` in
`ingestion/parsers/resume_parser.py` builds four regexes per candidate
header and every one of them anchors the header to the very start of a
line, with no allowance for leading horizontal whitespace:

```python
patterns = [
    rf"^{re.escape(section)}\s*$",
    rf"^{re.escape(section)}\s*[:|-]",
    rf"\n{re.escape(section)}\s*$",
    rf"\n{re.escape(section)}\s*[:|-]",
]
```

`^` (under `re.MULTILINE`) and `\n` both match immediately before the
first character of a line. When the line is `    Education:`, the next
character is a space, so `education` never matches, and the header is
missed. Nothing downstream of the match is at fault: the header text,
the lowercasing, and the `[:|-]` suffix handling are all correct.

This follows directly from my unit 2 reproduction, which isolated the
cause with a control run rather than inferring it. Quoting the repro
report:

> Indented input (issue's exact repro):
>
> ```
> >>>print(res.metadata['detected_sections'])
> []
> ```
>
> Control run, same content with no leading indentation:
>
> ```
> >>>print(res2.metadata['detected_sections'])
> ['Education', 'Skills']
> ```

The only difference between the two runs is the leading whitespace, and
the result flips from `[]` to the full expected list. That pins the
failure to the line-start anchoring above and rules out the header set,
the input plumbing, and the `parse()` entry point — all of which are
shared by both runs and behave correctly in the control.

## Scope

**In scope:** the anchoring of the four patterns in `_detect_sections()`
so that a header preceded only by horizontal whitespace on its own line
is detected, exactly as the same header without indentation already is.

**Not in scope**, deliberately:

- **The `return list(set(detected))` on the last line.** It makes the
  returned order nondeterministic across runs. It is a real wart and it
  affects how I write the test plan below, but it is not this bug: the
  indented case returns `[]`, not a differently-ordered list. If the
  maintainers want it fixed I will file it separately rather than fold
  it in here.
- ~~**`_strip_markdown()` and the PDF path.**~~ **Superseded during the
  build — `_strip_markdown()` is now in scope. See `## Deviations`.**
  The original wording also contained a factual error: it said both
  "call `_detect_sections()`". The PDF path does, and `_parse_markdown()`
  calls `_strip_markdown()` and then `_detect_sections()` in sequence —
  but `_strip_markdown()` itself never calls `_detect_sections()`. That
  wrong claim was the stated reason for leaving `_strip_markdown()` out,
  and it is exactly what the build then tripped over. The PDF path
  remains out of scope and needs no change.
- **The `SECTION_HEADERS` set.** No header is missing; the matching is
  what fails.
- **Any reformatting or refactoring of the surrounding module.**

## Files I'll touch

- `ingestion/parsers/resume_parser.py` — `_detect_sections()`, the
  `patterns` list (around lines 133–137), **and, added during the build,
  the header-stripping substitution in `_strip_markdown()` at line 101;
  see `## Deviations`.**
- `tests/unit/test_resume_parser.py` — removing the five
  `@pytest.mark.xfail(strict=True, reason="issue #54: ...")` markers
  that currently pin this bug, and adding one regression case for the
  indented-header input.

## Approach

1. Allow optional horizontal whitespace after each anchor, so the
   pattern list becomes the same four shapes prefixed with `[ \t]*`:

   ```python
   patterns = [
       rf"^[ \t]*{re.escape(section)}\s*$",
       rf"^[ \t]*{re.escape(section)}\s*[:|-]",
       rf"\n[ \t]*{re.escape(section)}\s*$",
       rf"\n[ \t]*{re.escape(section)}\s*[:|-]",
   ]
   ```

   I use `[ \t]*` rather than `\s*` on purpose: `\s` matches newlines,
   so `\s*` could consume one or more blank lines between the anchor
   and the header, letting a match start at an earlier line than the
   header's own. `[ \t]*` keeps the match on a single line.

   `docs/CONTRIBUTING.md`'s example commit message for this bug
   describes the fix as "Strip the line before matching headings". I
   read that as the same fix stated differently, and I am open to
   writing it that way if the maintainers prefer it. I chose to widen
   the anchors instead because stripping would mean restructuring
   `_detect_sections()` to iterate line by line, while the pattern
   list already encodes the four header shapes the parser recognises;
   widening the anchor is the smaller diff and leaves those four
   shapes intact. Happy to switch on request.

2. Run the three tests the issue names and confirm they now pass
   rather than `xfail`. Because the markers are `strict=True`, a
   passing test under the marker reports as `XPASS` and **fails** the
   run — so removing the markers is part of the fix, not optional
   cleanup.

3. Remove the five `xfail` markers that cite issue #54, and add one
   regression test asserting the indented input detects the same
   sections as the unindented control.

## Test plan

This re-runs my unit 2 repro steps against the change, with what I
expect to see after the fix.

**1. The issue's exact reproduction** (before: printed `[]`):

```python
from ingestion.parsers.resume_parser import ResumeParser
r = ResumeParser()
res = r.parse('\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n')
print(sorted(res.metadata['detected_sections']))
```

Expected after the fix: `['Education', 'Skills']`. I sort the result
because `_detect_sections()` returns `list(set(...))` and the raw order
is not stable — the set of sections is the assertion, not its order.

**2. The control run must stay unchanged** (before: already correct):

```python
res2 = r.parse('John Smith\njohn@example.com\n\nEducation:\n- B.S. Computer Science\n\nSkills: Python\n')
print(sorted(res2.metadata['detected_sections']))
```

Expected after the fix: `['Education', 'Skills']`, exactly as before.
This is the run that isolated the bug; if it changes, the fix has
broken the path that already worked.

**3. The three tests the issue names** (before: `3 xfailed`):

```
pytest tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text \
       tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience \
       tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections -v
```

Expected after the fix, with the markers removed: `3 passed`, no
`xfail` and no `xpass`.

**4. The rest of the parser suite must not regress:**

```
pytest tests/unit/test_resume_parser.py -v
```

Expected: all pass, including the two further tests whose `xfail`
markers cite #54. This step is a guard, not the proof — steps 1 to 3
are what show the bug is fixed.

I will save the before and after output for each of steps 1 to 3.

## Risks and unknowns

- **Over-matching.** Permitting leading whitespace means a line like
  `    skills: python` inside a prose paragraph could register as a
  header. The indentation-only change does not make this materially
  worse than the current unindented behavior (which already matches
  `skills: python` at line start), but I have not surveyed real resume
  text for it, so I am naming it rather than claiming it is safe.
- **Five markers, three named tests.** The issue names three tests but
  five markers cite #54. I expect all five to pass once the anchoring
  is fixed, since they share the cause, but I have not yet confirmed
  the other two; if any still fails, that is a second defect and I will
  report it on the thread rather than widen this change to chase it.
- **Trailing whitespace** on a header line is already handled by the
  existing `\s*$`; I have not tested it explicitly and am not changing
  it.

## Deviations

**One deviation: I widened the scope to include `_strip_markdown()`,
which the plan above explicitly placed out of scope.**

What happened. The plan predicted all five `xfail` markers citing #54
would pass from the one cause, and flagged as an unknown that I had
only confirmed three. Once the anchoring fix landed, three went
`XPASS(strict)` as expected and **two did not**:
`test_parse_markdown_resume` and `test_strip_markdown_syntax` kept
failing.

Why. They fail on the same defect one function higher in the same
file. `_strip_markdown()` strips markdown headers with
`re.sub(r"^#+\s+", ...)` — anchored at line start with no allowance for
indentation, exactly like the patterns in `_detect_sections()`. So an
indented `## Contact` is never stripped. It is the same bug class, the
same file, and a one-line change of the same shape:

```python
- text = re.sub(r"^#+\s+", "", content, flags=re.MULTILINE)
+ text = re.sub(r"^[ \t]*#+\s+", "", content, flags=re.MULTILINE)
```

Why I folded it in rather than deferring, which is what the plan said
I would do. `docs/CONTRIBUTING.md` requires that fixing a seeded bug
includes dropping the `xfail` marker from **every** test covering it.
Leaving two markers in place that cite #54 while calling #54 fixed
would contradict that rule, and would leave the suite asserting the
bug is still present. Deferring would have meant either shipping an
inconsistent marker set or filing a second issue for one line of the
same defect. I judged one coherent change to be the better result, and
I am declaring the widening rather than letting it pass silently.

A correction, not just a widening. The not-in-scope bullet justified
leaving `_strip_markdown()` out by saying it "calls
`_detect_sections()`". That was wrong: `_parse_markdown()` calls the
two in sequence, but `_strip_markdown()` never calls
`_detect_sections()`. I had not read the call graph as carefully as the
sentence implied, and the false claim is precisely what made the
boundary look safe. The bullet is struck through above.

What this does not change: the diagnosis, the approach, the test plan,
and every other out-of-scope line (the `list(set(detected))` ordering
wart, the PDF path, `SECTION_HEADERS`) all held exactly as written.

Because the posted plan comment told the maintainers `_strip_markdown()`
was out of scope, that comment is no longer true, and a follow-up
comment on the thread says so.

### Test plan results

| Step | Before | After |
|---|---|---|
| 1. Issue's exact repro | `[]` | `['Education', 'Skills']` |
| 2. Unindented control | `['Education', 'Skills']` | `['Education', 'Skills']` (unchanged) |
| 3. The three named tests | `3 xfailed` | `3 passed` |
| 4. Full parser suite | `5 passed, 5 xfailed` | `11 passed` |

The regression test was verified to fail with the fix reverted, so it
genuinely guards the behavior. `ruff`, `black` and `mypy` all pass via
the repo's pre-commit hooks. The wider unit suite is `94 passed, 8
xfailed` (those 8 cite other issues), alongside 14 collection errors
from missing third-party packages (`jose`, `structlog`) that are
identical on `main` and unrelated to this change.

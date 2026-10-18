# Unit 4 - Test and Submit

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/100

This is the existing PR for issue #54. A complete checked replacement title
and description are prepared in the local Path Review clone's pr_draft.md.
The user is applying that draft manually because this session's GitHub
integration cannot edit the PR. No duplicate PR was opened.

**Branch**

`fix/54-resume-section-whitespace`

## Eval iterations

**Run history**

The official harness used the installed skill at
`C:/Users/DELL/.claude/skills/pr-precheck`, with its pinned Sonnet model.
One initial launcher attempt failed with `FileNotFoundError: [WinError 2]`
before producing grades. Adding the installed native Claude executable to
this command's PATH resolved it; the harness itself was not modified.

Completed scored runs, in order:

1. Full run: `agreement: 18/20 scored items  (bar: 18/20: PASS)`.
2. Confirming full run: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`.
3. After revising the rubric, partial retry plus canaries:
   `agreement: 10/10 scored items`. Packages: pkg-01, pkg-02, pkg-03,
   pkg-04, pkg-07, pkg-11, pkg-14, pkg-16, pkg-17, pkg-20.
4. Final full run, saved by the harness with `--save-run eval-run.txt`:
   `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

The final transcript's category line is:

```text
categories: clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3
```

The committed `eval-run.txt` is the unedited final full transcript, including
its model and installed-file fingerprints. Partial runs did not overwrite it.

**Package analysis**

`pkg-02`: the final rubric verdict is `accept`; the gold label is `accept`.
Earlier runs rejected it because the grader treated save_text, save_html,
and save_svg as three independently required failure-mode demonstrations.
The plan instead names one shared export-before-write mechanism and asks
for the issue repro, the successful-save control, and pytest. The diff
applies that same change to all three methods. The package's test evidence
says:

> Expected-after per the plan: the record survives the failed save, and
> a successful save still clears it (covered by the new test's second
> half). `pytest tests/ -q` passes (1462 passed, 12 skipped).

This is decisive representative evidence plus the planned control and suite,
so the final behavior-evidence check passes without inventing a requirement
for separate transcripts for identical sibling fixes.

**Check rationale**

The following is the complete current behavior-evidence row, quoted exactly
from the submitted `tools/pr-precheck/rubric.md`:

```text
| behavior-evidence | Plan's test plan and accepted repro's inputs, controls, and distinct failure modes alongside candidate test evidence. | Evidence identifies a run or concrete artifact and an observable branch result distinguishing fixed from not fixed for the delivered behavior, against established before and expected after. Each distinct planned failure mode and relevant planned control is covered or explicitly disclosed as untested with a reason and bounded claim. Distinct means different triggers or outcomes the test plan specifically promises to exercise, not every sibling method receiving the same fix. A representative repro plus an executed regression assertion and suite can verify one shared mechanism without separate transcripts for identical sibling changes. Silent omission of a separately named failure mode fails even if another passes. Candid partial verification can pass if decisive evidence for the delivered claim remains; disclosure alone with no decisive observation cannot. Generic tests-pass assertions, added test files, or expectations without run results are insufficient. | required |
```

This revision distinguishes separate promised triggers from sibling methods
receiving one mechanism. The earlier requirement was interpreted too broadly
on pkg-02. The claims check and procedure were also clarified so a frozen diff
excerpt's unseen callers/helpers and incidental diagnostic or test-case labels
are not treated as concrete contradictory delivery evidence. The skill still
rejects actual silent drift, missing decisive observations, and unsupported
claims of checks or platforms tested.

**Trade-offs**

Loosening evidence requirements could hide missing coverage, so the partial
retry included already-agreeing testing canaries pkg-04, pkg-07, and pkg-14;
drift canaries pkg-03 and pkg-17; and both standards-wall packages pkg-01 and
pkg-20. All ten selected verdicts agreed. Both standards cases remained
rejects, so the smallest category did not silently flip.

One false rejection remains in the final run: `pkg-13` grades `reject` while
gold is `accept`. Its behavioral-check evidence is quoted exactly here:

> Repro before/after is concrete (code 1 -> 0, full URL), but the planned no-ampersand control is only asserted as 'unchanged' with no run result and no untested disclosure.

The bundle actually states, "No-ampersand control unchanged." The grader
read that compact report as insufficient even though the same package shows
the repro's full URL and exit-code change, records the functional suite, and
explicitly defers percent-sign expansion. This is a false-rejection trade-off
of the current evidence rule/execution, not a claim that the gold label is
wrong. The final result still meets the 18/20 bar and every category floor.

The live run against issue #54, the committed diff, plan.md, pr_draft.md, and
test_evidence.md returned `accept` with all seven required checks passing.
The checked draft contains the explained Markdown deviation and actual AI-use disclosure.

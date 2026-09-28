# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54

**Verdict output**

**Scope confirmed.** `scope.md` names `codepath/pathreview-ai301-fa26-s1` as the only
source of candidates; the URL is issue #54 in that repo, so it is in scope. The Path
Review house rule applies: other students' claim comments and their open PRs do not
block, but a set assignee still does.

**Checks (thresholds measured against today, 2026-09-28)**

- **not-archived** — pass. Repo line reads `archived: no`.
- **maintainer-alive** — pass. Newest date in the commit list is 2026-09-16
  (Aburke225), 12 days ago, inside the 90-day threshold.
- **repo-in-use** — pass. `latest release: none published`, so the fallback governs:
  all 5 of the last default-branch commits (2026-09-16 x3, 2026-08-24 x2) fall within
  180 days, clearing the "at least 3" bar.
- **scope-bounded** — pass. One function, `_detect_sections()` in `resume_parser.py`,
  with a stated cause (patterns anchored at line start) and a runnable repro. Not an
  umbrella — no list of issue numbers, no open-ended sweep, and the named edit ships in
  one PR. Design is settled: the maintainer filed it and states the expected output,
  there is no debate in the thread, and there are zero closed-unmerged linked PRs. It is
  a bug report, so the feature-endorsement condition does not apply; it is not a support
  question.
- **unclaimed** — pass, **by house rule**. `assignees: none`; no linked PRs (the
  timeline holds only four `labeled` events from 2026-09-10, no `connected` or
  `cross-referenced`), and none of the repo's 5 open PRs reference #54. The two claim
  comments are from DuBaem (NONE) dated today — recent and unreleased, which would fail
  condition (c) anywhere else, but the Path Review house rule waives a classmate's claim
  comments. No assignee is set, so the one signal the rule does not waive is clear.
- **ai-policy-allows** — pass. The policy is not at the root; `README.md` links
  `docs/CONTRIBUTING.md`, which was fetched in full and contains no statement on AI,
  generative AI, LLM, assistant, or Copilot use. Silence passes. Its actual terms — five
  green CI jobs, a completed PR template, 48-hour review response, deleting the `xfail`
  marker, no bulk lint fixes — are conditions to follow, not a ban.
- **labeled-newcomer-friendly** *(preferred)* — pass. Labels: `bug`,
  `good first issue`, `ingestion`, `tier-1`.
- **maintainer-responsive** *(preferred)* — fail. None of the 6 sampled
  recently-updated issues drew any owner/member/collaborator reply.
- **small-enough-to-be-seen** *(preferred)* — pass. 76 open issues + PRs, well under
  1000.

**Verdict: accept.** All six required checks pass. The one preferred failure — an
unresponsive maintainer sample — cannot change the verdict, and by design: this is the
same signal that would have wrongly sunk `issue-01` in the eval set, which is why it is
`preferred` rather than `required`.

One tension worth naming, since the rubric decides and I do not: DuBaem reproduced this
issue hours ago and is visibly working it. The rubric passes it anyway because the house
rule says a shared classroom issue costs nobody anything and credit attaches to the PR
you open. That is the rule working as written, not a gap in it — but I am going in
knowing I am not alone on this one.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54",
  "checks": [
    {"name": "not-archived", "grade": "pass",
     "evidence": "repo line reads 'archived: no'"},
    {"name": "maintainer-alive", "grade": "pass",
     "evidence": "newest default-branch commit is 2026-09-16 by Aburke225, 12 days before today, inside the 90-day threshold"},
    {"name": "repo-in-use", "grade": "pass",
     "evidence": "'latest release: none published' triggers the fallback, and all 5 recent default-branch commits fall within 180 days"},
    {"name": "scope-bounded", "grade": "pass",
     "evidence": "one function _detect_sections() in resume_parser.py with a stated cause and runnable repro; no umbrella, settled design, zero closed-unmerged linked PRs"},
    {"name": "unclaimed", "grade": "pass",
     "evidence": "assignees none and no linked PRs (timeline has only four 2026-09-10 'labeled' events); DuBaem's two 2026-09-28 claim comments are a classmate's and waived by the Path Review house rule"},
    {"name": "ai-policy-allows", "grade": "pass",
     "evidence": "docs/CONTRIBUTING.md, linked from README, contains no statement on AI or generative AI use; silence passes"},
    {"name": "labeled-newcomer-friendly", "grade": "pass",
     "evidence": "labels are bug, good first issue, ingestion, tier-1"},
    {"name": "maintainer-responsive", "grade": "fail",
     "evidence": "0 of 6 recently updated issues drew an owner/member/collaborator reply"},
    {"name": "small-enough-to-be-seen", "grade": "pass",
     "evidence": "76 open issues + PRs, under the 1000 threshold"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Four runs, in order:

1. `--only issue-01,issue-06,issue-09,issue-12,issue-20` — `agreement: 4/5 scored items`
2. `--only issue-01,issue-20,issue-05,issue-15` — `agreement: 3/4 scored items`
3. `--only issue-01,issue-05,issue-10,issue-20` — `agreement: 4/4 scored items`
4. full run — `agreement: 20/20 scored items  (bar: 18/20: PASS)`

The first three were partial `--only` runs used to probe the cases my thresholds were
deliberately tuned around, so they print no bar verdict. The last score matches the
agreement line in the committed `eval-run.txt`:
`agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
`categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`.

**Issue analysis**

`issue-01` (conda/conda#16475, category `clear-accept`).

Gold label: `accept` — "docs task with a stated home and scope; active repo, unclaimed".
My rubric's final decision: `accept`. It agreed on the full run, but only after two
revisions; it rejected this issue on both of the first two partial runs, and it was the
only scored issue my rubric ever got wrong.

Both rejections came from my `scope-bounded` check, on the note line
`failed: scope-bounded, labeled-newcomer-friendly (preferred), maintainer-responsive
(preferred)`. Only the untagged name mattered.

The first rejection was a condition I had written to catch unendorsed feature requests.
It required that an issue asking for something new be endorsed by a maintainer, and
`issue-01` is opened by `dashagurova (CONTRIBUTOR)` with `labels: type::documentation`
and `Comments (0 total, first 0 shown)` — no maintainer had spoken in the thread at all,
so the check fired. The reasoning was wrong because the condition was aimed at product
decisions and a docs request is not one. I narrowed it to exempt documentation, bug
reports, refactors, and test work, and to treat any project-applied triage label as
evidence the project had already accepted the issue.

That exposed the second, larger error. The issue body asks to "Add a new task page", then
"Update `manage-pkgs.rst`", "Update `pip-interoperability.rst`", "Update
`new-features.md`", and "Consider a global `troubleshooting.rst` entry". My umbrella
condition failed anything that "describes a list of sub-items", so a five-part list
tripped it. But the two real umbrellas in the set look nothing like this: `issue-10` is
titled "Documentation request megaissue" and its body is a bare list of other issue
numbers (`- #2580`, `- #3953`, ...), and `issue-05` is "Adding more type annotations to
the codebase", an open-ended sweep with no enumerated end. The distinguishing property is
not whether the issue contains a list — it is whether the items are one deliverable or
many. `issue-01` names specific files and ships in one PR; `issue-10` is a pointer to
work that lives elsewhere. Rewriting the condition around "could this ship in a single
pull request" fixed `issue-01` without moving `issue-05` or `issue-10`.

**Check rationale**

Quoted as currently written in `tools/issue-select/rubric.md`:

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | The `last 5 default-branch commits` list in the repo-facts block; take the **newest date in the list**, not the first line (the list is not always sorted). Live: the newest commit date above the file list. | The newest default-branch commit is dated within **90 days** of the capture date. Commits authored by a bot count only when the commit message shows it merged a human's pull request. | required |

Two decisions in that row were forced by the eval set rather than chosen on instinct.

The first is what the check reads. The obvious way to ask "is the maintainer alive?" is
response latency, and the bundles hand you a `maintainer first-response sample` block
that invites exactly that. But `issue-01` is a gold `accept` whose sample is:

```
  #16275 (opened 2026-06-24 by a maintainer): 32.9 days
  #16493 (opened 2026-08-04): no maintainer comment in thread
  #16231 (opened 2026-06-15): no maintainer comment in thread
  #16023 (opened 2026-05-05): no maintainer comment in thread
  #16026 (opened 2026-05-05): no maintainer comment in thread
```

Four of five drew no maintainer reply at all, and the fifth took over a month. Any
required check of the form "a maintainer replied within 30 days" rejects a clear-accept
issue. So liveness reads commits, and latency is demoted to a `preferred` check where it
cannot sink anything.

The second is the "newest date in the list, not the first line" clause, which exists
because of one bundle. `issue-07` lists its commits out of order:

```
  - 2023-02-01 by Komi7: fix git clone url
  - 2025-02-10 by soerenwolfers: Update README.md
  - 2025-02-10 by soerenwolfers: Update README.md
  - 2025-02-10 by soerenwolfers: Update README.md
  - 2023-08-22 by wting: Merge branch 'wting_default_python3'
```

Reading the first line gives 2023-02-01; the newest is actually 2025-02-10. Both are far
outside 90 days, so `issue-07` rejects either way — but a rubric that silently depends on
the list being sorted is a rubric that will read the wrong date on some repo where it
matters.

**Trade-offs**

The check gives up the "committing but not reviewing" repo. Because liveness reads commit
recency and the response-latency signal is only `preferred`, a repo whose maintainers
push code every week but never answer an issue or merge an outside PR passes
`maintainer-alive` cleanly. That is the failure mode `calib-03` was built to teach
("textbook write-up, repo stopped shipping in 2024 and PRs pile up unreviewed"), and my
rubric catches that particular bundle only because its commits also stopped — not because
it noticed the unreviewed queue. A repo that kept committing while ignoring contributors
would slip through, and I accept that: the alternative threshold rejects `issue-01`, and
trading one real accept for one hypothetical reject is the wrong trade on this set.

Nothing else moved when I demoted latency, and here is how I know. The three `dead-repo`
rejects are all caught by signals that have nothing to do with response times:
`issue-02`'s newest commit is 2023-12-09, `issue-07`'s is 2025-02-10 — both far outside
the 90-day window against a 2026-08-05 capture — and `issue-17` is `archived: yes`, which
`not-archived` fails outright. The final run confirms it at the category level:
`dead-repo 3/3`, with `agreement: 20/20 scored items`.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

**1. Fit to my interests and the time available.** I work mainly in Python, and I want to
get better at tracing a bug from a report to the function that causes it in a codebase I
did not write. Issue #54 is almost exactly that exercise at the smallest possible size:
one function, `_detect_sections()` in `resume_parser.py`, one wrong assumption (the
section-header patterns are anchored at the start of a line, so indented text from a PDF
matches nothing), and a seven-line reproduction I can paste into a REPL. The repo is
FastAPI plus React with a RAG and ingestion pipeline, which is the stack I have actually
built in, so I will spend my time on the bug rather than on the language. Text and resume
parsing is also adjacent to the spaCy and pandas work I have done. On time: the issue is
`tier-1`, it names three existing failing tests in `tests/unit/test_resume_parser.py`,
and `docs/CONTRIBUTING.md` tells me the fix is done when those tests pass and I delete
their `@pytest.mark.xfail(strict=True)` markers. A task with a written definition of done
is one I can finish in the time I have.

**2. What the verdict identified correctly, and what I weighed that the rubric could
not.** The verdict got the two things I would have gotten wrong by eye. It found the
contribution policy, which is not at the repo root where I looked first — there is no
`CONTRIBUTING.md`, no `.github/CONTRIBUTING.md`, and no `AI_POLICY.md`; the policy is in
`docs/CONTRIBUTING.md`, one click away through the README. Since my workflow is
AI-assisted, a ban there would have been a dead end, and I would not have found it
without checking. It was also right to pass the issue on `unclaimed` despite two claim
comments, because the house rule waives a classmate's claim and no assignee is set.

What I weighed that the rubric could not is the competition. DuBaem posted a reproduction
on this exact issue hours before I picked it, including the `1 xfailed` output from the
same test I would be fixing. The rubric passes it because the house rule says a shared
issue costs nobody anything, and that is correct about credit — but it says nothing about
whether I would rather work somewhere quiet. I chose #54 anyway: it is the best fit on
the merits, and the two alternatives my skill also accepted are still there if I want to
switch. #15 (agent session state) has zero comments but no runnable repro and no named
failing tests; #47 (API docs curl examples) is quieter and smaller but has no bug to
trace at all.

**3. Anticipated difficulty in claiming it.** Low on the claim itself and moderate on
everything after. Nobody can block me — no assignee, no linked PR — and the house rule
says to claim anyway. The real friction is downstream. The maintainer sample shows zero
owner or collaborator replies across six recently updated issues, so I should not expect
an answer to my claim comment or quick review on my PR; that was my rubric's one
preferred failure and I am taking the issue with it in view. `docs/CONTRIBUTING.md` also
warns that a first PR from a new fork sits at "waiting for approval to run workflows"
until a maintainer releases CI, and green CI across all five jobs is part of the
submission requirement — so my slowest step is likely to be waiting on someone else, not
writing the fix. And I have to remember that removing the `xfail` markers is part of the
fix, not cleanup: with `strict=True`, CI fails with `XPASS(strict)` if I fix the bug and
leave them in.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

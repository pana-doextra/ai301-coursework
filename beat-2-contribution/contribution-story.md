# Contribution: Add missing French translations for the radio info widget (EdgeTX #8002)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Nhat Minh Thu Le
**GitHub Username:** pana-doextra  <!-- TODO: confirm this is your profile name; it is the account `gh` is logged in as -->
**Issue:** https://github.com/EdgeTX/edgetx/issues/8002
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built the issue-scout strategy and rubric, then patched both after live scans**

I wrote a search strategy of three `good first issue` queries (C++, Python, JavaScript) that drops assigned and 90-day-stale issues and ranks by fewest comments, and ran the scout on the live scan; it accepted all five of its top candidates, which was misleading. tamadalab/welfare-time#117 passed `unclaimed` even though its body reserved the issue for a university course and said outside PRs would be closed, and `maintainer-responsive` was counting minutes-old issues the owner had opened, which show nothing about how maintainers treat outsiders. I patched `unclaimed` to fail on maintainer-stated reservations and changed the `maintainer-responsive` sampling to use non-maintainer issues at least 30 days old, and I logged the ranking bias in `scan-notes.md` without changing the strategy. A later scan rejected a stale repo on `maintainer-alive`, so the patched rubric held, but the same bias showed up again, because four of the five graded issues were under an hour old.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

<!-- TODO (yours to write): how you actually found this issue and which tool surfaced it. I only
verified the facts below; I did not run the scout, so I have not invented a verdict. -->

I chose [EdgeTX/edgetx#8002](https://github.com/EdgeTX/edgetx/issues/8002), "translation", a
feature request opened on 2026-11-05 by Pat687426. It asks for five French strings that were
left out of the French translation file: `TR_DATE_TIME_WIDGET` ("Date & Heure"),
`TR_RADIO_INFO_WIDGET` ("Info Radio"), `TR_LOW_BATT_COLOR` ("Batterie faible"),
`TR_MID_BATT_COLOR` ("Batterie moyenne") and `TR_HIGH_BATT_COLOR` ("Batterie chargée"). When
I checked on 2026-11-11 the issue was open, had no assignee and no comments, and carried only
the `enhancement ✨` label. It has no `good first issue` label, so the three label-based
queries in my Unit 5 strategy would not have found it. That makes it a fair test of the scout
on an issue it did not pick. The reporter gave the exact replacement text, so the change is
small and the wording is already settled. The scope is five lines in one file, which fits a
first contribution and leaves room to learn how EdgeTX organises translations (one
`radio/src/translations/i18n/<lang>.h` per language, with the defines used by the colour-LCD
widget in `radio/src/gui/colorlcd/widgets/radio_info.cpp`).

The repo is active (last push 2026-11-11) and has a recent precedent for exactly this kind
of fix. [PR #8001](https://github.com/EdgeTX/edgetx/pull/8001) by maintainer pfeerick,
merged 2026-11-05, replaced the same five English placeholders in `es.h` with Spanish. Its
description says FR, FI and NL still have English placeholders because nobody had posted
translations for them. #8002 supplies the French ones. I hope to learn the EdgeTX
translation workflow and the PR conventions (the `chore(color):` title style, backports to
`2.12`), and how a maintainer treats a first-time contributor on a small change.

- **Scout verdict:** [TODO: run `/issue-scout` on the issue URL and paste its verdict here. If it says reject and you proceed anyway, say so and why.]
- **Claim comment:** claim pending 2026-11-11 <!-- replace with the link the moment the claim lands -->
- **Honest read of the tool:** [TODO after the scout run. Starting point: the label-based
  strategy would have missed this issue entirely, and at 6 days old with zero comments it is
  also the kind of young, uncommented issue my Unit 5 notes say the ranking over-rewards.]

**Toolkit**

- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: [the scan and its verdict on this issue, described]
- Difficulties: [where it fell short, or "none met" if none did]
- What changed: [what you patched or would update, or "nothing"]

---

## Understanding the Issue

**Problem Description**

Five user-visible strings for the radio info widget and its battery-level colour options
were added to EdgeTX in PR #4845 with English placeholder text in the non-English
translation files. Spanish has since been filled in (#8001). The French file `fr.h` still
shows English for them, so a radio set to French displays "Date & Time", "Radio Info",
"Low battery", "Mid battery" and "High battery" in the middle of an otherwise French UI.

**Expected Behavior**

With the radio language set to French, those five labels appear in French, using the text
the reporter proposed:

| Define | Proposed French |
|---|---|
| `TR_DATE_TIME_WIDGET` | Date & Heure |
| `TR_RADIO_INFO_WIDGET` | Info Radio |
| `TR_LOW_BATT_COLOR` | Batterie faible |
| `TR_MID_BATT_COLOR` | Batterie moyenne |
| `TR_HIGH_BATT_COLOR` | Batterie chargée |

**Current Behavior**

`radio/src/translations/i18n/fr.h` on `main` (checked 2026-11-11, lines 1413-1417) holds
the English text, identical to `en.h`:

```c
#define TR_DATE_TIME_WIDGET       "Date & Time"
#define TR_RADIO_INFO_WIDGET      "Radio Info"
#define TR_LOW_BATT_COLOR         "Low battery"
#define TR_MID_BATT_COLOR         "Mid battery"
#define TR_HIGH_BATT_COLOR        "High battery"
```

**Affected Components**

- `radio/src/translations/i18n/fr.h`: the five defines to change (the only file I expect to touch).
- `radio/src/translations/i18n/en.h`: the source strings, unchanged, used as the reference.
- `radio/src/gui/colorlcd/widgets/radio_info.cpp`: the colour-LCD widget that uses the defines, unchanged.
- `radio/src/translations/sim_string_list.h`: also references the defines, unchanged.

**Toolkit**

- Tool: [TODO: e.g. `gh issue view` / `gh api` to read the issue and the source at `main`]
- What it produced: [the issue text, the five lines in `fr.h` vs `en.h`, and PR #8001 as precedent]
- Difficulties: [none met, or what]
- What changed: [nothing, or what]

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

<!-- Note: this is a translation gap, so the lightest honest reproduction is reading fr.h against
en.h. A stronger one is running the EdgeTX Companion/simulator with French selected and showing the
English labels in the radio info widget settings. Say which one you actually did. -->

**Steps to Reproduce**

1. [Step 1, e.g. fork and clone EdgeTX/edgetx, check out `main`]
2. [Step 2, e.g. open `radio/src/translations/i18n/fr.h` and search for `TR_DATE_TIME_WIDGET`, or run the simulator with the language set to French]
3. [Observed result: the five labels are English]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

The root cause is a missing translation, not a code bug. PR #4845 added the widget and its
five strings, and merged with the English text copied into every language file, as is
EdgeTX's usual practice for strings a translator has not yet supplied. `es.h` was fixed in
#8001. `fr.h` was not, because no French text had been posted; #8002 now provides it. All
five defines already exist in `fr.h` (lines 1413-1417), so nothing needs adding and no
other file has to change.

**Proposed Solution**

Replace the five English values in `fr.h` with the French text from the issue and keep the
existing `#define` name and column alignment. Save the file as UTF-8 like the rest of
`fr.h`, since "chargée" has an accent and the file already uses accented characters
throughout.

Deliberately NOT changing:

- The other languages. FI and NL still have the same English placeholders (per #8001) but
  nobody has supplied text for them, and mixing languages would make the PR harder to review.
- Other untranslated French strings that sit close by, for example `TR_DEL_DIR_NOT_EMPTY`
  ("Directory must be empty before deletion") and the `TR_MIX_*_PREC` strings. They are not
  in the issue.
- `en.h`, the widget code, or the `TR_*` names.

Open question for the maintainers: "Batterie chargée" for `TR_HIGH_BATT_COLOR` means
"charged battery", while the English is "High battery" and the Spanish is "Batería llena".
I plan to use the reporter's wording as given and mention this in the PR, rather than
override a native speaker's choice.

**Implementation Plan**

1. Comment on #8002 to claim it, and confirm the proposed wording with the reporter or a maintainer.
2. Fork `EdgeTX/edgetx`, branch from `main`.
3. Edit lines 1413-1417 of `radio/src/translations/i18n/fr.h` to the five French strings, keeping alignment and UTF-8 encoding.
4. Build or run a translation check (see Testing Strategy) and confirm only `fr.h` changed in the diff.
5. Open a PR titled in the project's style (for example `chore(color): add French 🇫🇷 translations for radio info widget`), following the shape of #8001 (before/after table, only `fr.h` changes, a line on the `2.12` backport).

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

<!-- Draft checks to confirm or replace once you have run them:
1. Before/after: `git diff` shows exactly five changed lines, all in fr.h.
2. Before/after: grep the five defines in fr.h; before they equal en.h, after they match the table.
3. The project's own translation build/check (look at radio/src/translations/translation.py and the
   CI workflow) completes without errors for the French build.
4. If a simulator is available: select French and screenshot the widget and its colour options. -->

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**2026-11-11**

Picked #8002. Read the issue and confirmed it is open, unassigned and has no comments. Checked
`fr.h`, `en.h` and `es.h` on `main`: the five French defines still hold the English text, and
`es.h` already has Spanish from #8001. Read #8001 as the model for the PR shape. Claim comment
not yet posted.

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [EdgeTX issue #8002](https://github.com/EdgeTX/edgetx/issues/8002): the request and the proposed French text.
- [EdgeTX PR #8001](https://github.com/EdgeTX/edgetx/pull/8001): the Spanish fix for the same five strings, used as the template for scope, PR title and description.
- [EdgeTX PR #4845](https://github.com/EdgeTX/edgetx/pull/4845): the change that introduced the widget strings with English placeholders.

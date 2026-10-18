# Procedure: how this tool grades a PR package

Grade the package that exists. Do not repair code, rewrite claims, run
missing tests, or substitute a better plan while producing a verdict.

## Read order

1. Establish mode and single item identity. Live: read `scope.md` first,
   verify the target, and apply frame refusals; then read `voice-guide.md`.
   Eval: skip both; the supplied bundle is all the evidence.
2. Read rubric, procedure, and evidence guide; verify substantive content
   before grading. Stop on an empty rubric or procedure as the frame says.
3. Read repo facts, issue, and thread. Record reported behavior, explicit
   maintainer constraints, template asks, required checks, and exact AI
   policy or its absence. Live: obtain these from the issue and target
   template/contributing docs, not assumptions about GitHub.
4. Read accepted repro and plan before the candidate. List promised
   behavior, scope exclusions, changed sites, deviations, expected
   before/after, controls, and each distinct planned failure mode.
5. Read the entire diff and commit list. Map each substantive hunk to
   the plan or an explained deviation. Record missing promised behavior
   as well as extra changes; inspect context for debris.
6. Read test evidence against the step-4 test list, then read title and
   description last. Compare claims against artifacts already read.
   Use the evidence guide's live locations when there is no bundle.

## Evidence gathering

Gather all pairs before grading. Save a short fact or quote and location
for every rubric row.

1. `plan-fidelity`: pair each changed file/behavioral hunk with its plan
   or deviation purpose; pair each promise with implementation or declared
   deferral. Record why deviations belong to this issue, not just that
   they were admitted.
2. `claims-match-artifacts`: pair material title/description claims with
   confirming or contradicting hunks/runs. Include exact-plan claims,
   completeness, platforms, and testing claims. Prose cannot replace proof.
3. `behavior-evidence`: one entry per distinct trigger and control with
   input/command, established before, expected after, observed branch
   result, and explicit reason for any untested item. Record whether
   remaining observations prove the qualified delivered claim.
   Derive the required runs from the plan's test plan, not from counting
   changed methods. Identical sibling fixes can share representative
   behavioral proof; independently promised triggers cannot.
4. `repo-checks-evidence`: pair requested checks/planned suite with actual
   commands, outcomes, failure details, or disclosed limitations. Separate
   failed, not run, and no tests collected from success. Identify binding
   gates and their stage; future CI cannot run on an unopened PR.
5. `reviewable-diff`: inspect every hunk, including tests/docs, for debug
   lines, dead code, unrelated formatting, and extra work. Record concrete
   debris or the bounded hunks establishing reviewability.
6. `repo-standards`: pair each applicable template/policy/thread request
   with substantive content or missing response. Live: record source fork,
   branch, issue link, every template section, truthful checkboxes, and
   issue-specific marker removals.
7. `disclosure-compliant`: quote the policy clause and actual disclosure,
   or record no stated disclosure requirement. Apply the separate course
   disclosure requirement only in live mode.
8. `regression-guard`: pair automated assertions with recorded runs and,
   live, issue-specific xfail/suppression removals. Do not count preference
   grades in the verdict.

## Check execution

1. Execute all checks independently in table order, using each complete
   pass condition including honest-shortfall exceptions. A failure never
   contaminates other checks.
2. Grade pass when facts satisfy the condition; fail when they contradict
   it or explicitly omit a requirement; unclear when a necessary fact
   cannot be established. Difficulty is not uncertainty. Re-read relevant
   sections if evidence may have been missed; otherwise use gathered notes.
3. Write one concise evidence line naming the decisive quote, hunk,
   outcome, or missing fact. Never infer a test ran from plausible code.
   Treat frozen hunks as excerpts: unseen caller/helper code is unknown,
   not contradictory evidence. Do not turn incidental test-case wording
   into a delivery failure when decisive repro and suite evidence agree.
4. Report procedure gaps and mark affected checks unclear, without
   inventing steps. Live: report voice-rule violations separately, quoting
   the rule and offending draft language.

## Verdict assembly

1. Accept only if all required grades pass. A required fail or unclear
   rejects. Preserve original grades in output; exclude preferred checks.
2. Print each check, grade, and evidence in table order. On reject, name
   the first non-passing required check as the deciding check and quote
   its fact; also report all other required failures and missing evidence.
   On accept, state bounded delivery and disclosed limitations without
   creating a qualified third verdict.
3. Emit the frame's fenced JSON last, with item, checks, verdict in that
   order, every row exactly once, and permitted grade/verdict values.
   Nothing follows the closing fence.

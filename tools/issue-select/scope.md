# Scope: where to look, and who is looking

<!--
This file is the skill's field of view. The rubric (rubric.md) decides
whether an issue is GOOD; the scope decides which issues are candidates
at all, and whose hands the issue would land in. It applies in live mode
only: in eval mode the bundle is the whole world and this file is
ignored.

Two parts. Staff wrote the first; you write the second.
-->

## Where candidates come from

Only issues in the course's Path Review repository are candidates:

- Repo: `codepath/pathreview-ai301-fa26-s1` <!-- paste your section's repo from the Unit 1 Check-In page -->

Do not search, fetch, or grade issues from any other repository, however
promising. The wider GitHub comes later in the course; for now the field
is Path Review.

**Path Review house rule.** Path Review is a classroom, and your
classmates are not strangers. Ignore the usual claim signals here: other
students' claim comments (and there may be several on one issue) do not
block an issue, and finding some on the issue you want is normal. Claim
anyway: course credit attaches to the pull request you open, not to
whether it merges, so a shared issue costs nobody anything. Everything
else in the rubric applies as written.

## Your fit profile

<!-- YOU write this part: a few sentences about you. What languages and
tools you have actually used, what you want to get better at, anything
you want to avoid. The skill uses this only to RANK the issues your
rubric accepts, never to change a verdict: fit cannot rescue an issue
your rubric rejects, and cannot sink one it accepts. -->

I work mainly in Python and TypeScript/JavaScript, and I am comfortable
in C/C++. On the backend I have built with FastAPI, Node.js, PostgreSQL,
Redis, Apache Spark, and MLflow; on the frontend with React and Expo Go.
My day-to-day tooling is Git, Docker Compose, Nginx, VS Code, and Azure
AI Services, and I have shipped data and ML work with scikit-learn,
pandas, NumPy, matplotlib, TensorFlow, PyTorch, spaCy, and Hugging Face.

I want to get better at reading an unfamiliar production codebase and
landing a change in it: tracing a bug from a report to the function that
causes it, matching the project's existing conventions, and writing the
test that proves the fix. Rank Python and TypeScript issues highest,
since I can move fastest there and spend my effort on the codebase rather
than on the language.

Prefer issues with a reproduction I can run locally, and prefer bugs and
bounded docs work over open-ended feature design. Rank down issues whose
main difficulty is build or environment setup I cannot reproduce (native
toolchains, GPU-only paths, or a cluster I do not have), and issues in
languages I have not written, since the point is to finish a first
contribution rather than to fight the setup.

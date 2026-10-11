# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

### Where it lives

**Eval mode:** Read the package's Plan context first, especially the
stated scope, files, intended behavior, test plan, and any recorded
deviations. Compare that with the Candidate PR's diff, commits, and
description.

**Live mode:** Read `plan.md`, including its `## Deviations` section.
Compare it with the committed branch diff from `git diff main...HEAD`
and with the title and description in `pr_draft.md`.

### What good looks like

Every material change in the diff is supported by the original plan or
by an honestly recorded deviation. Work intentionally changed or left
out can still be acceptable when the difference is recorded and the PR
description accurately represents the final implementation.

Silent drift includes extra implementation visibly present in the supplied
diff but not covered by the plan or a deviation, planned material work omitted
without explanation, or a PR description that claims more or less than the diff
actually contains. Do not infer hypothetical drift from code or callers that
are not present in the package. A planned shared-helper change is not itself
drift merely because it could affect other callers; require supplied evidence
that a preserved behavior actually changed.

**Supports:** `plan-fidelity`.

## Test evidence (harness category: not-tested)

### Where it lives

**Eval mode:** Read the Plan context's test plan and reproduction
evidence, then compare them with the Candidate PR's Test evidence and
the testing claims in its description. Also read Repo facts for any
repository-required commands or checks.

**Live mode:** Read the test plan in `plan.md`, the captured command
output in `test_evidence.md`, and the Testing section of
`pr_draft.md`. Compare these with the original issue reproduction and
the checks required by the repository's PR template and
`docs/CONTRIBUTING.md`.

### What good looks like

Evidence identifies the command that was run and gives a concrete,
observable result. For behavior-specific reproduction, before/after
evidence should make the changed behavior clear. Repository-required
checks may be supported by a specific reported result such as pass/fail
status, test counts, or another concrete outcome; the entire raw terminal
transcript is not required.

A vague sentence such as "tests pass" with no command or specific result
is not enough. Missing verification, unrelated tests, or evidence that
does not exercise the reported behavior is insufficient.

A failing command can still be valid evidence when its real output and
the reason for the failure are reported honestly.

**Supports:** `test-evidence`.

## Diff quality (harness category: unreviewable)

### Where it lives

**Eval mode:** Inspect the Candidate PR's unified diff and commit list.
Compare each changed file and material hunk with the implementation,
tests, documentation, and deviations described by the plan.

**Live mode:** Inspect `git diff main...HEAD` and the commits on the
student's branch. Use `plan.md` and its deviations to determine whether
each changed file and hunk belongs to the proposed work.

### What good looks like

The diff is focused enough for a maintainer to review the intended
change directly. Each material hunk supports the fix, its tests,
required documentation or changelog work, or an honestly recorded
deviation.

Unrelated cleanup, drive-by refactors, formatting churn, debug output,
temporary files, dead code, commented-out experiments, or accidental
generated content make the PR unreviewable even when the core fix is
correct.

**Supports:** `diff-quality`.

## Standards and comms (harness category: standards-wall)

### Where it lives

**Eval mode:** Read the Repo facts block for the repository's PR
template requirements, contribution instructions, maintainer
directions, and any stated AI-assistance or disclosure policy. Compare
those requirements with the Candidate PR's title and description.

**Live mode:** Read the repository's
`.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, relevant
issue-thread instructions, and any stated policy that applies. Compare
them with the title and description in `pr_draft.md`. Also read
`voice-guide.md` for personal writing rules, while remembering that
voice rules do not change the verdict unless the rubric explicitly
uses them.

### What good looks like

Every explicit repository requirement that applies is satisfied. The PR
template sections are filled with real content or marked `N/A` with a
clear reason when genuinely inapplicable, and any required AI-use
disclosure is present.

Do not invent requirements that the repository does not state.
Boilerplate that leaves a required section unanswered, ignores explicit
maintainer direction, or omits a required disclosure is not compliant.

Whether factual claims in the description match the diff is graded
under plan fidelity rather than here.

**Supports:** `standards-and-comms`.

---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Grade exactly one PR package to answer one question: is this pull
request ready to submit?

A PR package consists of the candidate PR title and description, the
committed branch diff, test evidence, and commits, read against the
plan the PR claims to implement and the issue that plan belongs to.

Do not grade from overall impression or polish. Execute the checks in
`rubric.md` using the workflow in `procedure.md` and the evidence
locations described in `references/evidence-guide.md`.

## Inputs and modes

Run in exactly one of two modes.

- **Live mode:** grade the student's own PR before it is opened. Read:
  - `plan.md`, including its `## Deviations` section;
  - the committed branch diff relative to `main`, produced with
    `git diff main...HEAD`;
  - the draft PR title and description from `pr_draft.md`;
  - the test evidence from `test_evidence.md`;
  - the issue URL supplied by the student.

  Gather issue-side evidence from the real Path Review repository,
  including the issue thread, the repository PR template, and
  `docs/CONTRIBUTING.md`.

  For a house-chain student, use the supplied house plan and house
  reproduction pack instead of the student's own plan and reproduction
  history, while grading the same PR evidence with the same checks.

  Treat `git diff main...HEAD` as the complete committed change being
  proposed by the branch. Do not substitute uncommitted working-tree
  edits for that diff.

  If a required live-mode artifact is missing, report which input is
  missing and stop rather than inventing its contents.

- **Eval mode:** the supplied package bundle is the whole world. Use
  only facts contained in that bundle. Do not fetch GitHub, inspect a
  working directory, read unrelated files, or supplement missing
  evidence from outside knowledge. Run every rubric check and apply
  the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before grading anything else.

Use its `Repo:` line to determine the only repository this skill may
grade, and apply the house rules written in that file.

If the `Repo:` line still contains an unfilled placeholder, stop
without grading and tell the student to fill it with the correct Path
Review repository. Never guess the repository.

If the requested issue or PR belongs to a repository outside the
configured scope, refuse to grade it and explain that it is outside
the skill's configured scope.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and compare the outgoing PR title
and description against the student's writing rules.

Report any broken voice-guide rule in the readable summary and identify
the rule that was broken.

The voice guide does not change the verdict by itself unless a check in
`rubric.md` explicitly uses it as evidence.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md` to obtain the complete set of checks, their evidence
requirements, pass conditions, weights, and the verdict rule.

Use `references/evidence-guide.md` to determine where each evidence
family lives in eval packages and in live PR work, and what evidence is
sufficient to support a grade.

Execute `procedure.md` exactly as written. It determines the read
order, evidence-gathering process, check execution, and verdict
assembly.

Do not silently add missing procedure steps. If `procedure.md` is
silent about something necessary to complete a check, report the gap
rather than improvising around it.

If `rubric.md` contains no student-written checks, or `procedure.md`
contains no student-written operating steps, refuse to grade and state
which required component is still empty.

## Verdict and output

The verdict is binary:

- `accept` means the PR is ready to submit.
- `reject` means the PR should be held.

There is no third verdict and no score.

Grade every rubric check as `pass`, `fail`, or `unclear`, with one
evidence line naming the fact or quote that decided the grade.

A short readable per-check summary may appear before the machine-readable
result. In live mode, include any voice-guide notes in that summary.

End every completed grading run with the following fenced JSON block.
It must be valid JSON, must preserve this schema and field order, and
nothing may appear after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the specific
  fact, diff line, test output, plan statement, repository requirement,
  or quote that decided it. "Looks fine" is not evidence.

- **Grade the artifact, not the polish.** A short PR with complete,
  verified evidence can be ready, while a polished description can
  still hide scope drift, missing tests, unsupported claims, or
  repository-standard violations.

- **The rubric decides.** Apply each check exactly according to its
  stated pass condition. If a result feels wrong even though the rubric
  says it passes, keep the grade; the rubric should be revised later.

- **The procedure decides how.** Follow `procedure.md` as written.
  Report procedural gaps instead of silently inventing a workflow.

- **Claims require proof.** A sentence in a PR description saying a
  test passed, behavior changed, or scope stayed bounded is not proof
  by itself. Verify the claim against the diff, plan, test evidence, or
  repository evidence required by the rubric.

- **Treat `unclear` as the verdict rule directs.** If the rubric's
  verdict rule does not explicitly say otherwise, treat `unclear` as a
  failing result. A PR whose required claim cannot be verified is not
  ready to submit.
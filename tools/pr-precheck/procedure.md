# Procedure: how this tool grades a PR package

## Read order

1. Determine the mode before grading anything.
   - In live mode, confirm the scope first as required by `SKILL.md`, then read
     the issue and repository requirements needed for the package.
   - In eval mode, use only the supplied bundle and do not fetch anything.

2. Read the accepted plan before reading the implementation diff.
   Record:
   - the problem the plan says it is solving;
   - the files or code areas it expects to change;
   - the behavior it intends to change;
   - the work it explicitly leaves out;
   - the tests or reproduction steps it promises;
   - any recorded `## Deviations`.

3. Read the repository requirements that apply to the PR.
   In live mode, this includes the PR template, `docs/CONTRIBUTING.md`,
   relevant issue-thread instructions, and any stated disclosure policy.
   In eval mode, use the equivalent repository-facts material in the bundle.

4. Read the committed branch diff next.
   Record every changed file and the purpose of each material hunk.
   Treat `git diff main...HEAD` as the complete proposed implementation in live
   mode.

5. Read the test evidence after the diff.
   Record each command that was run, its printed output, the behavior it
   exercises, and whether it corresponds to the plan's test plan or a
   repository-required check.

6. Read the candidate PR title and description last.
   List each factual claim it makes about:
   - what changed;
   - what did not change;
   - tests or checks run;
   - issue closure;
   - repository requirements;
   - deviations or intentionally omitted work.

Reading the plan before the diff prevents the implementation from redefining
its own scope, and reading the description after the evidence allows its claims
to be checked against what was actually changed and tested.

## Evidence gathering

Gather evidence for each rubric check before assigning any grades.

### plan-fidelity

1. List the files, behaviors, and tests named by the plan.
2. List every material file and behavior changed by the diff.
3. Match each diff change to:
   - the original plan; or
   - a recorded deviation explaining what changed and why.
4. Compare the PR description's scope and implementation claims with the actual
   diff.
5. Record any:
   - extra change visibly present in the supplied diff but not supported by the
     plan or deviations;
   - planned material work silently omitted;
   - description claim contradicted by the supplied diff or evidence.
6. Judge only observable package evidence. Do not invent downstream drift from
   callers or code that are not shown. If the plan explicitly requires a
   shared-helper change while saying another path keeps its existing semantics,
   treat that as internally consistent unless the supplied diff or evidence
   actually demonstrates that the preserved path changed.

### test-evidence

1. Extract the plan's promised reproduction steps and test plan.
2. Extract the repository-required checks.
3. For every relevant verification claim, locate:
   - the command that was run; and
   - a concrete observable result, such as before/after behavior, pass/fail
     status, test counts, or other specific reported outcome. Full raw
     terminal output is not required when the result is otherwise specific
     and credible.
4. Match the before/after evidence to the reported issue behavior.
5. Record any promised or required verification that is absent, stated only
   as a vague claim such as `tests pass`, or unrelated to the behavior it
   claims to prove.
6. Do not treat a failing command as missing evidence when its real output and
   reason are honestly shown.

### diff-quality

1. Inspect each changed file and each material hunk.
2. Determine whether each change directly supports:
   - the implementation;
   - its tests;
   - required documentation or changelog work; or
   - an honestly recorded deviation.
3. Record unrelated cleanup, debug output, temporary code, commented-out
   experiments, accidental generated content, or other review debris.

### standards-and-comms

1. List the repository's explicit PR-template and contribution requirements.
2. Identify any required AI-use or other disclosure requirement.
3. Compare the title and description against those requirements.
4. Compare factual statements in the description with the diff and test
   evidence.
5. Record missing required sections, unsupported claims, missing disclosures,
   or invented requirements.
6. Treat a genuinely inapplicable template section marked `N/A` with a clear
   reason as satisfied unless repository policy says otherwise.

Reuse gathered facts across checks when the same evidence applies. Do not
re-read the entire package when the needed fact has already been recorded.

## Check execution

Execute the rubric checks in this order:

1. `plan-fidelity`
2. `test-evidence`
3. `diff-quality`
4. `standards-and-comms`

For each check:

1. Read that check's Evidence and Pass condition from `rubric.md`.
2. Compare the gathered evidence directly with the pass condition.
3. Assign exactly one grade:
   - `pass` when the evidence satisfies the condition;
   - `fail` when the evidence demonstrates that the condition is not satisfied;
   - `unclear` when evidence required to decide is missing or ambiguous.
4. Record one concise evidence line identifying the specific fact, output,
   diff observation, plan statement, or repository requirement that decided
   the grade.
5. Do not infer missing proof from a confident PR description.
6. Do not fail one check automatically because another check failed. Grade the
   distinct question owned by each rubric row.
7. Do not punish an honestly disclosed shortfall merely because the final work
   differs from the original plan. Judge it according to the rubric's stated
   conditions and the recorded deviations.

If evidence needed for a check is genuinely unavailable, grade that check
`unclear` rather than silently assuming either success or failure.

## Verdict assembly

1. Collect the grade and evidence line for every rubric check.
2. Apply the verdict rule from `rubric.md` exactly.
3. Return `accept` only when every required check passes.
4. Return `reject` if any required check is `fail` or `unclear`.
5. Preferred checks, if any are added later, may appear in the output but never
   change the verdict.

When explaining a reject verdict in the readable summary, identify the first
failing or unclear required check in rubric order as the primary deciding
check. If additional required checks also fail or are unclear, report those as
well.

For every check in the final JSON, use the concise evidence line recorded
during check execution. Then emit the final `accept` or `reject` verdict using
the exact output format required by `SKILL.md`.

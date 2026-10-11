# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | Compare the branch diff and PR description against the accepted plan, including its scope, planned changes, test plan, and any recorded `## Deviations`. | Pass if every material change visible in the supplied diff is supported by the plan or an honestly recorded deviation, and the PR description accurately describes what the diff does. Judge drift from observable package evidence rather than hypothetical effects in code or callers not shown. When the plan explicitly requires a shared-helper change while preserving a caller or path's semantics, do not infer a contradiction merely because the helper's return behavior changes; fail only when the supplied diff or evidence actually shows the preserved behavior was changed. Planned work that was changed or intentionally left out may still pass when the difference is explicitly recorded. Fail explicit extra work, undisclosed material omissions, or description claims contradicted by the supplied diff. | required |
| test-evidence | Compare `test_evidence.md` or the package's Test evidence against the plan's test plan, the reproduced issue behavior, the PR's testing claims, and the repository's required checks. | Pass if the evidence credibly demonstrates the relevant behavior and required verification. Behavior-specific reproduction evidence should show observable before/after results when that distinction matters. Repository checks may be supported by the exact command and a concrete reported outcome such as pass/fail status, test counts, or other specific result; their entire raw terminal output is not required. Tests visible in the diff may also establish coverage when their assertions directly exercise the claimed behavior and the evidence reports that the relevant suite passed. Fail vague assertions such as only `tests pass`, missing verification, or evidence that does not exercise the behavior it claims to prove. An honestly reported failing command may still provide valid evidence of what was run and what remains incomplete. | required |
| diff-quality | Inspect every changed file and hunk in the diff against the stated implementation, tests, documentation, and recorded deviations. | Pass if the diff is focused and reviewable: each change serves the stated work and there is no unrelated cleanup, debug output, commented-out experimental code, accidental generated content, or other debris. Fail when unrelated or accidental changes remain, even if the core fix itself is correct. | required |
| standards-and-comms | Compare the PR title and description against the repository facts, PR template, contribution requirements, and any stated AI-assistance or disclosure policy. Compare factual claims in the description with the diff and supplied evidence. | Pass if the PR satisfies every explicit repository requirement that applies, fills or reasonably marks required template sections, includes any required AI-use disclosure, and does not make unsupported factual claims. A section may be `N/A` when it genuinely does not apply and the reason is clear. Do not invent requirements the repository does not state. | required |

## Verdict rule

Grade every check as `pass`, `fail`, or `unclear`.

Return `accept` only when every required check passes.

Return `reject` when any required check fails or is unclear. An
`unclear` grade means the available evidence is insufficient to verify
the required condition and therefore does not count as a pass.

Preferred checks, if any are added later, may provide feedback but
never change the verdict.

Base the final verdict on the individual check results rather than an
overall impression of the PR.

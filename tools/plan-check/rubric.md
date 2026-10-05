
# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | Compare the Candidate plan's diagnosis against the Issue, Repro evidence, control runs, and relevant Thread highlights. | Pass if the proposed cause is consistent with the reproduced observations and is not contradicted by the evidence. A specific internal mechanism may remain a working hypothesis when it plausibly explains the observations and the plan describes how to validate it before depending on it. Reject a diagnosis contradicted by control runs, one that ignores decisive observations, or one that relies on an unverified mechanism without a validation path.| required |
| change-addresses-cause | Compare the Candidate plan's approach and proposed changes against its diagnosis and the failure shown in Repro evidence. | Pass if the proposed change addresses the evidenced failure mechanism rather than merely hiding the symptom, bypassing the failure, or changing unrelated behavior. The connection between the proposed change and the observed bug must be understandable. | required |
| bounded-scope | Compare the Candidate plan's Scope and Changes against the Issue and relevant maintainer constraints in Thread highlights. | Pass if the work is one coherent, bounded change with identifiable implementation locations and meaningful limits on what will not change. A deliberately narrower solution may pass when excluded work is explicitly acknowledged. Unrelated refactoring or unexplained expansion fails. | required |
| executable-by-stranger | Read the Candidate plan's approach, named files or code areas, and implementation steps alongside the Issue and Repro evidence. | Pass if another developer can identify the target code area, intended behavior, implementation approach, and how to begin the work without inventing essential design decisions. Exact function names are not mandatory when the plan identifies specific components and provides a concrete discovery step to locate them. Fail plans that leave the implementation location, core approach, or essential behavior to be invented by the implementer. Do not require excessive detail or a particular document format.| required |
| test-decisive | Compare the Candidate plan's Test plan against the failing steps, inputs, actual behavior, and expected behavior in Repro evidence. | Pass if the proposed verification can distinguish the reproduced failure from the intended fixed behavior using specific observable expected-after results. A faithful manual reproduction is acceptable; an automated test is not mandatory. Running a general test suite alone, without checking the reported behavior, is insufficient. | required |
| comment-and-conventions | Compare the Candidate plan comment against the Candidate plan, Thread highlights, and Repo facts, including explicit contribution and AI-assistance policies. | Pass if the comment faithfully represents the proposed work, respects relevant maintainer direction, and follows explicit repository contribution requirements. Required AI disclosure must be present when the repository requires it; do not invent disclosure requirements where none are stated. Unsupported claims or commitments outside the plan fail. | required |

## Verdict rule

Grade every check as pass, fail, or unclear using the evidence named in its row.

- Accept (READY) only when every required check passes.
- Reject (HOLD) when any required check fails or is unclear.
- Unclear means the available evidence is insufficient to determine whether the pass condition is met. It is not an assumed pass.
- Preferred checks, if added later, provide feedback but never change the verdict.
- Report an evidence-based reason for every grade.
- The final verdict must follow the individual check results, not an overall impression of the plan.

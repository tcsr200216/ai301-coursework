# Evidence guide: where evidence lives in a plan package

This guide identifies where to find the evidence required by `rubric.md` and what a passing result looks like. Use the captured package as the source of truth in eval mode. In live mode, use the actual issue, its reproduction, repository documentation, and the supplied drafts.

## Diagnosis and grounding

### Where it lives

**Eval mode:** Read the Issue, Thread highlights, and Repro evidence sections. Compare these with the Candidate plan's Diagnosis, Approach, and Changes.

Pay particular attention to control runs, observed errors, timings, inputs, and expected versus actual behavior.

**Live mode:** Read the original GitHub issue, the student's posted reproduction comment, and relevant issue-thread discussion. Compare that evidence with the diagnosis and proposed approach in `plan.md`.

### What good looks like

The proposed cause explains the behavior actually demonstrated by the reproduction and is not contradicted by its control runs. A plausible statement made in an issue comment is not sufficient proof by itself.

If the cause remains uncertain, the plan acknowledges that uncertainty and describes how it will validate the hypothesis before relying on it.

The proposed change must address the evidenced failure mechanism rather than merely hiding its symptoms or bypassing the failure.

For example, if a slowdown still happens with the pager disabled, a plan that attributes the slowdown exclusively to pager key bindings is contradicted by the evidence.

**Supports:** `diagnosis-grounded`, `change-addresses-cause`.

## Scope

### Where it lives

**Eval mode:** Read the Candidate plan's Scope, Changes, and Approach. Compare the proposed work against the Issue and relevant constraints from Thread highlights.

**Live mode:** Read the in-scope and out-of-scope statements in `plan.md`, inspect the named implementation locations, and compare them with the original issue and relevant maintainer comments.

### What good looks like

The plan describes one coherent, bounded change with identifiable implementation locations and meaningful limits on what will not change.

A deliberately narrower solution can pass when the excluded portion of the issue is explicitly acknowledged.

Unrelated refactoring, unexplained API changes, additional features, or broad rewrites outside the intended fix do not satisfy this condition.

Do not reject an otherwise bounded plan merely because it is concise or does not use a particular document format.

**Supports:** `bounded-scope`.

## Executability

### Where it lives

**Eval mode:** Read the Candidate plan's Approach, Changes, and named implementation files or code areas. Compare these with the relevant behavior described in the Issue and Repro evidence.

**Live mode:** Read the proposed implementation steps and file paths in `plan.md`. Use the repository's source and tests to verify that the described implementation locations and intended behavior are understandable.

### What good looks like

Another developer can identify where to begin, what behavior to change, and which essential implementation decisions the plan proposes.

The plan connects the intended code changes to the documented failure without requiring the reader to invent the implementation strategy.

A short plan with one precise change may be executable. A lengthy plan containing only broad intentions, such as "improve the parser," is not.

Do not require a line-by-line implementation or a particular number of files.

**Supports:** `executable-by-stranger`.

## Test plan

### Where it lives

**Eval mode:** Compare Candidate plan → Test plan with the original inputs, commands or actions, observed failure, and expected behavior in Repro evidence.

**Live mode:** Compare `plan.md`'s proposed verification with the exact reproduction commands and output posted during Unit 2. Include any relevant regression tests already present in the repository.

### What good looks like

The test plan provides a repeatable way to distinguish the original failure from the intended fixed behavior.

It identifies what will be executed and the specific observable result expected after implementation.

The verification should exercise the affected behavior, not merely demonstrate that unrelated functionality still works.

A faithful manual test can pass. An automated test is not mandatory unless the repository explicitly requires one.

Running a general test suite without checking whether the reported bug is gone is insufficient by itself.

For example, when reproducing a parser exception, the after-fix verification should exercise the same input through the real parser and establish that the expected result is returned without the original exception.

**Supports:** `test-decisive`.

## Honesty

### Where it lives

**Eval mode:** Read the Candidate plan's Diagnosis, Risks, Unknowns, Approach, and Candidate plan comment. Compare factual claims with the Issue, Thread highlights, and Repro evidence.

**Live mode:** Read the risks and unknowns in `plan.md` and compare the draft public comment with the actual reproduction and planned work.

After implementation, inspect the `## Deviations` section to determine whether any departures from the posted plan were accurately recorded.

### What good looks like

The plan distinguishes observations from assumptions and hypotheses. It does not present an unverified cause, untested implementation, or uncertain outcome as established fact.

Material unknowns are acknowledged, with validation planned before implementation depends on them.

The public comment promises only investigation, changes, or verification that the plan actually contains.

A deviation made during implementation is recorded with what changed and why. If no deviation occurred, the plan states that truthfully.

Do not reject a plan merely because it acknowledges uncertainty. Reject unsupported certainty or uncertainty that the plan relies on without any way to resolve it.

**Supports:** the uncertainty conditions in `diagnosis-grounded` and the factual-accuracy requirements of `comment-and-conventions`.

## Comms

### Where it lives

**Eval mode:** Compare Candidate plan comment with Candidate plan, Thread highlights, and Repo facts. Read any stated contribution requirements, issue templates, and AI-assistance policies in the captured repository facts.

**Live mode:** Read the student's draft `comment.md`, the original GitHub issue and its discussion, and relevant repository documentation such as `CONTRIBUTING.md` or `AI_POLICY.md` when available.

### What good looks like

The comment accurately summarizes the proposed diagnosis, bounded change, and planned verification.

It respects relevant maintainer instructions, addresses the actual issue, and does not claim results that the evidence has not established.

The comment follows the repository's explicit contribution requirements.

If the repository requires AI-assistance disclosure, the required disclosure must be present. If the repository has no stated disclosure requirement, do not invent one.

A statement of intent to investigate or implement the described plan is acceptable. Unsupported findings, promises outside the plan, or ignoring explicit maintainer constraints fail.

**Supports:** `comment-and-conventions`.
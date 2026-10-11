# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/141

**Branch**

fix/69-json-array-fallback

## Eval iterations

**Run history**

The evaluator runs, in order, were:

- 14/20
- 2/2 (`pkg-02`, `pkg-04`)
- 5/5 (`pkg-05`, `pkg-08`, `pkg-11`, `pkg-13`, `pkg-16`)
- 4/4 (`pkg-01`, `pkg-03`, `pkg-07`, `pkg-12`)
- 19/20
- 5/5 (`pkg-16`, `pkg-03`, `pkg-06`, `pkg-09`, `pkg-17`)
- 20/20

The final committed evaluation record says:

> agreement: 20/20 scored items  (bar: 18/20: PASS)

**Package analysis**

I analyzed `pkg-16`.

My rubric's final verdict for `pkg-16` was `accept`, and the gold label was also `accept`.

Earlier, this package exposed that my `plan-fidelity` check was too willing to infer silent drift from behavior that was not actually demonstrated by the supplied package. I revised the check so that it grades the observable supplied evidence instead of inventing hypothetical effects in unseen callers.

The final rubric specifically accounts for a plan that intentionally changes a shared helper while requiring another caller or path to preserve its semantics. In that situation, a helper-level change alone is not enough to prove drift. The supplied diff or evidence has to actually show that the preserved behavior changed.

After that revision, `pkg-16` was accepted while the real silent-drift canaries still rejected correctly. In the final run, `pkg-16` appears as:

> pkg-16  clear-accept    accept  accept   yes

**Check rationale**

I revised the `test-evidence` check during evaluation. Its current pass condition is:

> Pass if the evidence crediblydemonstrates the relevant behavior and required verification. Behavior-specific reproduction evidence should show observable before/after results when that distinction matters. Repository checks may be supported by the exact command and a concrete reported outcome such as pass/fail status, test counts, or other specific result; their entire raw terminal output is not required. Tests visible in the diff may also establish coverage when their assertions directly exercise the claimed behavior and the evidence reports that the relevant suite passed. Fail vague assertions such as only `tests pass`, missing verification, or evidence that does not exercise the behavior it claims to prove. An honestly reported failing command may still provide valid evidence of what was run and what remains incomplete.

I changed it because the first version required too much raw terminal transcript. That caused legitimate clear-accept packages to fail even when they supplied the exact command and a concrete result such as a test count or pass/fail outcome.

The revised wording still requires behavior-specific evidence and rejects vague claims such as only saying `tests pass`, but it no longer requires an entire raw terminal log when a precise command and concrete outcome are sufficient to verify the claim.

**Trade-offs**

The main trade-off in the `test-evidence` revision is that it accepts summarized command results instead of requiring the entire raw terminal transcript. That makes the check less strict about the presentation of evidence, but it still requires a specific command and concrete result and still rejects vague or missing verification.

I tested that trade-off directly with targeted evaluator runs. After changing the rule, I re-ran `pkg-02` and the `pkg-04` not-tested canary together and got `2/2`. I then re-ran the remaining clear-accept packages and got `5/5`, while a separate set of reject canaries got `4/4`.

I also refined `plan-fidelity` after `pkg-16` exposed an over-strict inference. I re-ran `pkg-16` together with the silent-drift canaries `pkg-03`, `pkg-06`, `pkg-09`, and `pkg-17`, and that targeted run scored `5/5`. The final full evaluator run then reached:

> categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3

> agreement: 20/20 scored items  (bar: 18/20: PASS)

This showed that the revisions fixed the false rejection without losing the intended reject behavior in the canary categories.
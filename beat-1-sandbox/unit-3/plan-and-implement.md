# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of the plan, the branch I built it on, and the evaluation runs that produced
`eval-run.txt`.

---

## Posted upstream

**GitHub username**

tcsr200216

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5987284790

## Implementation plan for issue #69

Following up on my [original claim](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5859902159) and [reproduction report](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5860242442), I've inspected the parser and prepared a bounded implementation plan.

**Diagnosis:** Both JSON decoding paths in `rag/generator/output_parser.py` pass their decoded values directly to `_parse_json_output()`. A top-level JSON array produces a Python `list`, but that helper assumes a dictionary and calls `.items()`, causing the reproduced `AttributeError`.

**Planned change:** Check the decoded type in both the raw-JSON and fenced-JSON paths. Keep the existing structured behavior for dictionaries, and route unsupported top-level shapes through the existing plaintext fallback, preserving the original response.

**Scope:** Limit source changes to `rag/generator/output_parser.py` and `tests/unit/test_output_parser.py`. I will remove the issue H-02 `xfail` marker and strengthen regression coverage for raw, fenced, and empty arrays, while checking that object parsing remains unchanged. I will not redesign array parsing, change `FeedbackSection`, or refactor unrelated behavior.

**Verification:** Rerun my original Unit 2 reproduction, the formerly expected-failing regression test, and the full output-parser test file. After the fix, the same array input should return one `general_feedback` section preserving the original input, without raising `AttributeError`. The H-02 test should pass normally rather than report XFAIL.

I'll implement this on my fork's `fix/69-json-array-fallback` branch after the plan review, record any deviations, and report the actual test results. These are planned outcomes, not claims that the fix has already been implemented.

---

## Your branch

**Branch**

`fix/69-json-array-fallback`
https://github.com/tcsr200216/pathreview-ai301-fa26-s3/tree/fix/69-json-array-fallback

**Evidence**

### BEFORE — Unit 2 reproduction

Before implementing the change, I reproduced the original issue with the same top-level JSON array used in Unit 2.

Command:

```bash
.venv/bin/python - <<'PY'
import json
from rag.generator.output_parser import parse_review_output

raw_output = json.dumps([
    "First feedback item",
    "Second feedback item"
])

print("Input:", raw_output)

result = parse_review_output(raw_output)

print("Result:", result)
PY
```

Output:

```text
Input: ["First feedback item", "Second feedback item"]

Traceback (most recent call last):
  File "<stdin>", line 10, in <module>
  File ".../rag/generator/output_parser.py", line 48, in parse_review_output
    return _parse_json_output(data)
           ^^^^^^^^^^^^^^^^^^^^^^^^
  File ".../rag/generator/output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
                      ^^^^^^^^^^
AttributeError: 'list' object has no attribute 'items'
```

I also reran the repository regression test on the implementation branch before making the fix:

```bash
.venv/bin/pytest tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback --runxfail -vv
```

Relevant output:

```text
tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback FAILED

rag/generator/output_parser.py:48: in parse_review_output
    return _parse_json_output(data)

data = ['First feedback item', 'Second feedback item']

rag/generator/output_parser.py:68: in _parse_json_output
    for key, value in data.items():
E   AttributeError: 'list' object has no attribute 'items'

FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
1 failed in 0.18s
```

### AFTER — reproduction against the built change

After implementing the approved plan on `fix/69-json-array-fallback`, I reran the same direct reproduction.

Command:

```bash
.venv/bin/python - <<'PY'
import json
from rag.generator.output_parser import parse_review_output

raw_output = json.dumps(["First feedback item", "Second feedback item"])
print("Input:", raw_output)

result = parse_review_output(raw_output)
print("Result:", result)
PY
```

Output:

```text
Input: ["First feedback item", "Second feedback item"]
2026-10-04 23:01:08 [info     ] plaintext_output_parsed        content_length=47
Result: [FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]
```

The original `AttributeError` no longer occurs. The complete original JSON array is preserved as the content of one `general_feedback` section.

I also checked the new regression tests against both the original and fixed parser.

Against the original parser:

```text
test_json_array_fallback                FAILED
test_fenced_json_array_fallback         FAILED
test_empty_json_array_fallback          FAILED

3 failed, 18 deselected in 0.12s
```

All three failed through the original bug:

```text
AttributeError: 'list' object has no attribute 'items'
```

After restoring the implementation:

```text
test_json_array_fallback                PASSED
test_fenced_json_array_fallback         PASSED
test_empty_json_array_fallback          PASSED

3 passed, 18 deselected in 0.07s
```

The complete output-parser test file passed:

```text
21 passed
```

The repository-required checks also passed.

`make check`:

```text
ruff check .                                      All checks passed
black .                                           110 files left unchanged
mypy api/ core/ ingestion/ rag/ agent/ safety/   Success: no issues found in 76 source files
```

`make test-unit`:

```text
378 passed, 52 xfailed, 4 warnings in 7.01s
```

The remaining expected failures belonged to other known issues, and the warnings were unrelated to `output_parser.py` or its tests.

## Eval iterations

**Run history**

I ran the evaluator multiple times while refining the `plan-check` grading skill.

1. Initial full evaluation: **19/20**
   - `pkg-14` was the only disagreement.

2. Focused retry of `pkg-14` after refining the rubric: **1/1**
   - `pkg-14` changed to the correct accept verdict.

3. Canary evaluation of `pkg-14`, `pkg-01`, `pkg-02`, `pkg-04`, `pkg-06`, and `pkg-10`: **6/6**
   - The refinement corrected `pkg-14` without changing the expected decisions for the sampled accept and reject packages.

4. Full evaluation after the rubric refinement: **20/20**

5. Final full evaluation after repairing and rechecking `procedure.md`: **20/20**

The final `eval-run.txt` records:

```text
agreement: 20/20 scored items  (bar: 18/20: PASS)
```

**Package analysis**

I analyzed `pkg-14`.

The gold label for `pkg-14` was **accept**. My initial rubric produced **reject**, making it the only disagreement in the first full evaluation.

The package described a zellij OSC response issue. Its proposed diagnosis was a plausible explanation of the observed reproduction, but the exact internal mechanism was not yet completely established. The plan included a concrete validation path using the relevant components and debugging evidence before depending on that mechanism.

My initial interpretation of `diagnosis-grounded` was too strict because it effectively required the proposed internal mechanism to already be proven at planning time. My interpretation of `executable-by-stranger` was also too restrictive because it leaned toward requiring an exact function-level target even when the plan already identified the relevant components and gave a concrete discovery step.

I refined the rubric so that a specific internal mechanism may remain a working hypothesis when it plausibly explains the observations, is not contradicted by the available evidence, and the plan describes how it will be validated before implementation depends on it.

After the refinement, `pkg-14` received **accept**, matching its gold label. The focused retry scored 1/1, the six-package canary scored 6/6, and the subsequent full evaluations scored 20/20.

**Check rationale**

The final `diagnosis-grounded` check in `tools/plan-check/rubric.md` reads exactly:

> | diagnosis-grounded | Compare the Candidate plan's diagnosis against the Issue, Repro evidence, control runs, and relevant Thread highlights. | Pass if the proposed cause is consistent with thereproduced observations and is not contradicted by the evidence. A specific internal mechanism may remain a working hypothesis when it plausibly explains the observations and the plan describes how to validate it before depending on it. Reject a diagnosis contradicted by control runs, one that ignores decisive observations, or one that relies on an unverified mechanism without a validation path.| required |

I revised this check because the earlier version caused the false rejection on `pkg-14`.

The important distinction is between an unsupported diagnosis and a working hypothesis. A working hypothesis can pass when it is consistent with the observed evidence and includes a concrete validation path. A diagnosis should still fail when control runs contradict it, when it ignores decisive observations, or when it depends on an unverified mechanism without explaining how that mechanism will be validated.

This keeps the check evidence-grounded without requiring every internal implementation detail to already be proven before the implementation begins.

**Trade-offs**

The trade-off in this refinement is that `diagnosis-grounded` can now accept a plan whose precise internal mechanism has not yet been proven.

That flexibility is useful because planning often happens before all implementation-level details are known, but it creates a risk that a plausible hypothesis could later turn out to be wrong.

I bounded that risk by requiring the hypothesis to be consistent with the reproduced observations and by requiring the plan to include a concrete validation path before depending on the hypothesis. A contradicted diagnosis or an unsupported mechanism without validation still fails.

I also reran a canary set after making the change:

```text
pkg-14
pkg-01
pkg-02
pkg-04
pkg-06
pkg-10
```

The result was:

```text
6/6
```

`pkg-14` changed to the correct accept verdict, while the sampled wrong-cause, thread-convention, scope-creep, unbuildable, and clear-accept packages continued to match their gold labels.

The following full evaluation reached **20/20**, and the final full evaluation also reached **20/20**, giving me evidence that the rubric refinement fixed the false rejection without broadly weakening unrelated decisions.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; the skill files are in
`tools/plan-check/`.

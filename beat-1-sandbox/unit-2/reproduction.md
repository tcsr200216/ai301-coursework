# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`.

---

## Your identity upstream

**GitHub username**

tcsr200216

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5859902159

I'd like to work on this issue for my coursework. I'll reproduce the reported top-level JSON array fallback failure in `output_parser.py`, document the environment and exact output, and report back with what I find before making changes.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5860242442

## Reproduction report

I was able to reproduce the top-level JSON array failure reported in issue #69.

### Environment

- Repository: `tcsr200216/pathreview-ai301-fa26-s3` (fork of `codepath/pathreview-ai301-fa26-s3`)
- Commit: `2f4e82f52efbcfcc57d65b3fa5348672163ca088`
- OS: macOS 27.2
- Architecture: arm64
- Python: 3.11.16
- pytest: 9.1.1
- Installation/setup: cloned my fork and ran `make setup`

The project setup completed successfully with:

```text
Setup complete. Run 'make run' to start the application.
```

I did not modify `output_parser.py` or remove the existing `xfail` marker before reproducing the issue.

### Reproduction steps

I first verified the Python version used by the project's virtual environment:

```bash
.venv/bin/python --version
```

Output:

```text
Python 3.11.16
```

I then reproduced the issue directly from the repository root by passing a top-level JSON array to `parse_review_output()`:

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

The input passed to the parser was:

```json
[
  "First feedback item",
  "Second feedback item"
]
```

### Expected behavior

`parse_review_output()` should handle a top-level JSON array without crashing.

The existing regression test also expects the returned result to be a list:

```python
result = parse_review_output(raw_output)

assert isinstance(result, list)
```

### Actual behavior

The direct reproduction prints the input successfully:

```text
Input: ["First feedback item", "Second feedback item"]
```

but then fails with:

```text
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

The JSON input is parsed into a Python `list`, and the JSON handling path reaches `_parse_json_output()`. Execution then fails when `.items()` is called on that list.

### Existing regression test confirmation

The repository contains:

```text
tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
```

Running it normally:

```bash
.venv/bin/pytest \
  tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback \
  -rxX
```

reports:

```text
XFAIL tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
- issue #69 (manifest H-02): output parser calls .items() on a JSON array fallback
```

I then ran the same test using `--runxfail`, which executes it as a normal test without modifying the source file:

```bash
.venv/bin/pytest \
  tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback \
  --runxfail -vv
```

It fails at the same code path:

```text
rag/generator/output_parser.py:68: in _parse_json_output
    for key, value in data.items():
                      ^^^^^^^^^^
E   AttributeError: 'list' object has no attribute 'items'

FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
- AttributeError: 'list' object has no attribute 'items'
```

### Result

I reproduced the behavior described in issue #69 through two paths:

1. A direct call to `parse_review_output()` using a top-level JSON array.
2. The repository's existing `test_json_array_fallback` regression test run with `--runxfail`.

Both use the same top-level JSON array and fail with:

```text
AttributeError: 'list' object has no attribute 'items'
```

This matches the failure described in issue #69.

## Eval iterations

**Run history**

I used partial runs while revising the rubric and full runs to check the complete scored package set.

1. Smoke run with `--limit 3`: **3/3** agreements.
2. First full run: **18/20** agreements. The disclosure category was not matched because `pkg-20` was incorrectly accepted, so this run did not satisfy the category floor.
3. Partial run on `pkg-10,pkg-20,pkg-01,pkg-02`: **4/4** agreements after revising the behavior and repository-conventions checks.
4. Full confirming run: **18/20** agreements, with every category matched. The remaining disagreements were `pkg-03` and `pkg-19`.
5. Partial run on `pkg-03,pkg-20,pkg-01`: **2/3** agreements. A loosened repository-conventions rule fixed one case but caused `pkg-20` to be accepted again.
6. Partial run on `pkg-03,pkg-19,pkg-20,pkg-01`: **4/4** agreements after refining `repo-conventions` and making `claim-tone` a required check.
7. Final full run saved to `eval-run.txt`: **20/20** agreements.

Final category results:

```text
clear-accept: 8/8
disclosure: 1/1
no-evidence: 4/4
unfollowable-comms: 3/3
wrong-target: 4/4

agreement: 20/20 scored items
```

**Package analysis**

I analyzed `pkg-20` because it exposed a gap in my first version of the `repo-conventions` check.

On my first full run, my rubric decided **accept**, while the gold label was **reject**.

The package's repo facts state:

> "contribution policy (CONTRIBUTING.md + AI_POLICY.md): strict AI rules. All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance; the human in the loop must fully understand the work; AI-assisted issues and comments must be reviewed and edited by a human before submission"

The candidate claim and reproduction report did not contain the required AI-assistance disclosure. My original conventions check was too general, so it did not treat the missing disclosure as a required failure.

I revised the check so that when the repo facts explicitly require AI-assistance disclosure, the package must actually contain that disclosure and silence does not count as disclosure. After that revision, `pkg-20` was correctly rejected.

I later loosened the conventions wording while working on `pkg-03`, which caused `pkg-20` to be incorrectly accepted again. That showed me that the check needed to distinguish between an explicit disclosure requirement and a policy that only requires comments to be written by humans in their own words.

I refined that distinction and re-ran the affected packages. In the final rubric, `pkg-20` was rejected, matching the gold label.


**Check rationale**

The final `repo-conventions` check in my uploaded `rubric.md` reads:

```text
| repo-conventions | Repo-facts contribution policy/templates compared with the claim comment and repro report | Pass if the package follows the repository's explicit contribution requirements. If repo-facts say AI assistance must be disclosed, the package must contain that disclosure; silence does not count as disclosure. If the policy only says comments must be written by humans in their own words and does not require disclosure, do not fail unless the package itself shows that rule was violated. | required |
```

I chose this wording because my earlier version missed the disclosure requirement in `pkg-20`. An intermediate revision then became too broad in the opposite direction and risked treating a human-authorship rule as if it were automatically an AI-disclosure requirement.

The final wording separates those cases. It enforces an explicit disclosure rule when one exists, but it does not invent a disclosure requirement when the repository only says that comments must be written by humans in their own words. This let the rubric enforce stated repository policy without making assumptions beyond the provided repo facts.

**Trade-offs**

The final `repo-conventions` check is intentionally narrow: it rejects missing disclosure only when the repository facts explicitly state that disclosure is required.

That means the check could miss an unstated or undocumented convention, but I preferred that trade-off over inventing requirements that were not present in the package.

After refining the check, I used:

```text
--only pkg-03,pkg-19,pkg-20,pkg-01
```

as a targeted re-run. All four packages agreed with their gold labels (**4/4**) before I spent another full eval run.

This gave me a canary for both sides of the change: `pkg-20` checked that the disclosure wall was still enforced, while the other packages helped confirm that the stricter wording was not creating new false rejections. The final full run then reached **20/20** agreement.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
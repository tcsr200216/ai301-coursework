# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69

**Verdict output**

Scope: all three are in codepath/pathreview-ai301-fa26-s3 — inside the scoped source. Path Review house rule applied (classmate claim signals don't block).

Repo-level evidence (shared by all three, graded per issue):
- Last default-branch commit 2026-09-16 by human Aburke225 (4 days before today, 2026-09-20); same account opened all three issues → maintainer-alive.
- archived: false, last push 2026-09-16 → repo-in-use.
- docs/CONTRIBUTING.md exists; no AI ban anywhere (no AI_POLICY.md, policy silent on AI use) → contribution-policy passes.
- Repo has 0 PRs total (open or closed); all three issues opened 2026-09-10, 10 days old, no assignees, no linked PRs → attempt-history and no-collision clean.

Ranked read-out — all three accepted:

1. #69 — Output parser crashes on top-level JSON array (best fit). RAG/LLM code path, which is exactly the AI/LLM engineering you want to grow into, and it's Python-debugging shaped work you already do in other languages. tier-1 + good first issue, a named 2–4h estimate, two named files, and an xfail-marked covering test (tests/unit/test_output_parser.py, verified present) — so the repro is a local test run, not a guess.
2. #72 — verify_password raises UnknownHashError instead of returning False. Backend/API security logic with an xfail test to drive it, 1–2h, tightest scope of the three. Ranked below #69 only because it teaches nothing new about LLM work. Note: two classmates (rafiatasafi, xyin20) pushed commits referencing it today from their own coursework repos — under the house rule that does not block you, and there's no assignee or open PR, so no-collision passes.
3. #67 — Review creation does not verify profile ownership. Real backend fit (REST endpoint, ownership scoping through Profile.user_id, data-flow reasoning). Ranked last: tier-2, no good first issue label, no effort estimate, and no covering test named — the body even cuts off mid-code-block, so you'd be writing the repro yourself.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last default-branch commit 2026-09-16 by human Aburke225, 4 days before today (2026-09-20); same account opened the issue"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16, within 90 days"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "One outcome: handle top-level JSON array in output_parser.py fallback; two named files, 2-4h estimate, no unresolved design decision"},
      {"name": "attempt-history", "grade": "pass", "evidence": "Opened 2026-09-10 (10 days old); repo has 0 PRs total, so no closed unmerged attempts"},
      {"name": "no-collision", "grade": "pass", "evidence": "assignees: []; no linked or open PRs; 0 comments on the thread"},
      {"name": "contribution-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md present and silent on AI use; no AI_POLICY.md in the tree"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last default-branch commit 2026-09-16 by human Aburke225, 4 days before today; same account opened the issue"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16, within 90 days"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "One outcome: make verify_password fail closed in core/security.py; 1-2h estimate, xfail-marked covering test named"},
      {"name": "attempt-history", "grade": "pass", "evidence": "Opened 2026-09-10 (10 days old); repo has 0 PRs total, so no closed unmerged attempts"},
      {"name": "no-collision", "grade": "pass", "evidence": "assignees: []; no open PR (repo has 0 PRs); only classmate commit references from rafiatasafi/xyin20 coursework repos, which the Path Review house rule says do not block"},
      {"name": "contribution-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md present and silent on AI use; no AI_POLICY.md in the tree"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/67",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last default-branch commit 2026-09-16 by human Aburke225, 4 days before today; same account opened the issue"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16, within 90 days"},
      {"name": "newcomer-scope", "grade": "pass", "evidence": "One outcome: scope create_review() through Profile.user_id like get_review()/list_reviews() already do; not an umbrella issue, no TBD decision"},
      {"name": "attempt-history", "grade": "pass", "evidence": "Opened 2026-09-10 (10 days old); repo has 0 PRs total, so no closed unmerged attempts"},
      {"name": "no-collision", "grade": "pass", "evidence": "assignees: []; no linked or open PRs; 0 comments and no timeline references"},
      {"name": "contribution-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md present and silent on AI use; no AI_POLICY.md in the tree"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

Authentication-failed run:

> `agreement: 0/0 scored items`

> `20 item(s) errored; fix and re-run.`

First successful full run:

> `agreement: 16/20 scored items  (bar: 18/20: below the bar)`

Partial rerun after revising the rubric:

> `agreement: 4/4 scored items`

Final complete saved run:

> `agreement: 18/20 scored items  (bar: 18/20: PASS)`

The final run also reported:

> `categories: claimed 4/4  clear-accept 6/8  dead-repo 3/3  policy 1/1  scope 4/4`

**Issue analysis**

Scored issue: `issue-19`

My rubric's decision was `reject`.

The gold label was `accept`.

The final eval output was:

> `issue-19  accept  reject   NO     failed: newcomer-scope`

The issue said:

> `There are two potential causes which should be fixed:`

> `1. The matchers are slow for certain rewrites (quadratic instead of linear)`

> `2. UI update is waiting for the matching thread to finish`

It also listed several:

> `Additional suggestions:`

My rubric interpreted the multiple causes and implementation directions conservatively and failed the `newcomer-scope` check. The gold label instead treated those as different possible approaches toward one primary outcome: preventing the UI from freezing.

**Check rationale**

Current check from my final `rubric.md`:

> `| newcomer-scope | Issue body and Comments section | Pass if the issue has one primary contribution outcome and is not an umbrella/tracking issue, pure usage/support question, or unresolved design request. Multiple possible causes or implementation approaches do not fail this check by themselves when they all serve one outcome. Fail if a required product/design/input decision is explicitly unresolved or TBD. | required |`

I kept this check because I wanted to reject issues that are genuinely open-ended, such as tracking issues, support questions, or unresolved design work, while still allowing multiple possible implementation approaches when they all lead to one clear outcome.

**Trade-offs**

After revising the rubric, I reran the four previous disagreements with `--only` and got:

> `issue-14  accept  accept   yes`

> `issue-15  reject  reject   yes`

> `issue-19  accept  accept   yes`

> `issue-20  reject  reject   yes`

> `agreement: 4/4 scored items`

However, the final complete run later reported:

> `issue-19  accept  reject   NO     failed: newcomer-scope`

The trade-off is that the `newcomer-scope` check protects me from choosing vague or design-heavy first issues, but it can still reject a borderline issue where multiple causes or possible approaches all lead to one bounded outcome.

---

## Selection rationale

**Selection rationale**

1. Issue #69 fits my interests because it is related to RAG and LLM output handling, which is an area I want to learn more about. It also fits the available time because the issue estimates the work at 2–4 hours, points to the relevant parser and test files, and already has an xfail test that can be used to reproduce the bug.

2. The verdict correctly identified that the repository is active, there is no active contribution collision, the issue has one clear outcome, and the contribution policy does not block the workflow. The rubric could not fully measure which accepted issue would be most useful for my own learning. I chose #69 because it gives me experience with LLM-related debugging while still being reasonably scoped.

3. I expect claiming the issue to be fairly straightforward because there is no assignee, no linked or open PR, and no issue comment showing active work. The main difficulty will be following the Unit 2 claim process correctly and reproducing the bug before attempting a fix.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

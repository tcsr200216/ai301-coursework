# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo-facts block: last 5 default-branch commit dates and authors, `maintainer first-response sample`; issue Comments section: commenter `author_association` | Pass if at least one non-bot default-branch commit occurred within 60 days of the capture date, OR a maintainer responded within 60 days in the response sample or issue thread. If none of these sources establish recent human maintainer activity, grade `unclear`. | required |
| repo-in-use | Repo-facts block: `archived`, last 5 default-branch commit dates, and `last push to any branch` | Pass if the repository is not archived and either at least one of the last 5 default-branch commits or the last push occurred within 90 days of the bundle capture date. | required |
| newcomer-scope | Issue body and Comments section | Pass if the issue has one primary contribution outcome and is not an umbrella/tracking issue, pure usage/support question, or unresolved design request. Multiple possible causes or implementation approaches do not fail this check by themselves when they all serve one outcome. Fail if a required product/design/input decision is explicitly unresolved or TBD. | required |
| attempt-history | Repo-facts block: issue open date and `linked PRs`; Comments section for claim, abandoned-work, or unassignment history | Pass if the issue is not both older than 365 days and associated with 2 or more closed unmerged PRs or repeated abandoned contribution attempts. | required |
| no-collision | Repo-facts block: `this issue: assignees` and `linked PRs`; Comments section for active work claims and PR mentions | Pass if there is no current assignee, no open PR addressing the issue, and no comment within the last 30 days showing another contributor is actively working on it. Closed unmerged PRs do not count as active claims. | required |
| contribution-policy | Repo-facts block: `contribution policy` | Fail only if the policy explicitly bans AI-generated or AI-assisted contributions. Pass if AI use is allowed with conditions or if the policy is silent. | required |

## Verdict rule

Accept an issue only if every `required` check passes.

If any `required` check fails, reject the issue.

Treat `unclear` on a required check as a failure and reject the issue.

`preferred` checks, if present, do not change the accept/reject verdict and are used only to rank accepted issues.

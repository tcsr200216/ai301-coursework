# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks
| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | Repro report environment record compared with the issue's stated environment/version | Pass if the report names the project or tool version, OS, and install method or commit. If the tested version differs from the issue, the difference is explained. | required |
| steps-rerunnable | Repro report setup steps and trigger command | Pass if another person can start from the named release or commit and follow the steps without guessing missing setup or commands. | required |
| input-matches | Issue input and command compared with the repro report input and command | Pass if the repro uses the same bug-triggering input and command as the issue, or clearly explains an intentional difference. | required |
| expected-vs-actual | Repro report expected behavior and actual behavior | Pass if the report clearly states what was expected and what actually happened. | required |
| artifact-shown | Repro report terminal output, traceback, test output, or screenshot | Pass if real output is shown to support the reported run instead of only describing what happened. | required |
| behavior-matches | Repro artifact compared with the behavior or error described in the issue | Pass if the evidence either shows the same reported behavior, or supports an honest cannot-reproduce after a faithful attempt with relevant differences called out. A setup, syntax, dependency, or unrelated error presented as a successful reproduction does not pass. | required |
| outcome-honest | Repro report conclusion compared with the shown evidence | Pass if the conclusion says only what the evidence supports. An evidenced cannot-reproduce passes; claiming reproduction when the artifact shows a different failure does not. | required |
| repo-conventions | Repo-facts contribution policy/templates compared with the claim comment and repro report | Pass if the package follows the repository's explicit contribution requirements. If repo-facts say AI assistance must be disclosed, the package must contain that disclosure; silence does not count as disclosure. If the policy only says comments must be written by humans in their own words and does not require disclosure, do not fail unless the package itself shows that rule was violated. | required |
| claim-tone | Claim comment | Pass if the claim describes the work or investigation without promising a fix or deadline, asking for the issue to be reserved, or making claims not supported by the package. | required |

## Verdict rule

Accept only if every required check passes.

If any required check fails or is unclear, reject.

Preferred checks give feedback but do not change the verdict.
# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

### Where it lives

In eval packages, look at the issue context for the version or environment the bug was reported on, and look at the repro report for the version, OS, install method, or commit that was actually tested.

In live mode, compare the GitHub issue with the environment details in the draft repro comment and the repo's setup docs if needed.

### What good looks like

The report names the project or tool version, OS, and install method or commit used. If the tested version is different from the issue, the report explains the difference.

## Steps

### Where it lives

Look at the setup and reproduction steps in the repro report, including install commands, checkout or version details, test input, and the command used to trigger the issue.

Compare the input and trigger command with the ones described in the original issue.

### What good looks like

Another person should be able to start from the named version or commit and follow the steps without having to guess a missing command, input, or setup step.

If the reproduction uses different input or a different trigger command than the issue, the difference should be clearly explained.

## Behavior shown

### Where it lives

Look at the behavior described in the issue, then compare it with the repro report's expected behavior, actual behavior, and artifacts such as terminal output, traceback, test output, or screenshots.

### What good looks like

The comments follow the repository's explicit contribution requirements.

If repo-facts require AI-assistance disclosure, the package must contain the required disclosure. Do not treat silence as proof that no disclosure is needed.

If a policy only says comments must be written by humans in their own words and does not require an AI disclosure, do not fail the package unless the package itself shows that rule was violated.

The claim should not promise a fix or deadline, ask for the issue to be reserved, or claim results that the evidence does not support.
## Honesty

### Where it lives

Compare what the repro report says happened with the actual evidence and artifacts included in the report.

Also compare any conclusion about reproducing or not reproducing the issue with the commands, outputs, and environment shown.

### What good looks like

The report should only claim what the evidence supports.

An honest report saying the issue could not be reproduced can still be valid if the steps and evidence support that result.

It should not claim the bug was reproduced if the artifact shows a different failure or unrelated behavior.

## Comms

### Where it lives

Look at the claim comment and repro comment, along with the repository's contribution policy, issue templates, and any repo-facts about contribution requirements.

In live mode, compare the draft comment with the issue thread and the repository's contribution documentation.

### What good looks like

The comments follow the repository's explicit contribution requirements.

If the repo requires AI-assistance disclosure, the claim or repro package must include the disclosure required by that policy, including the tool or extent of assistance when requested.

The claim should describe the planned investigation without promising a fix or deadline.

The repro comment should only state behavior that was actually observed and supported by the evidence.
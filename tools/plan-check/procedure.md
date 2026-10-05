# Procedure: how this skill grades a plan package

## Read order

Follow this order before assigning any grades. Establish what the issue and reproduction actually show before evaluating the proposed fix.

1. Read `scope.md` and confirm the target repository. In eval mode, use the provided package snapshot as the evidence source. Do not replace captured facts with information from the live GitHub issue.
2. Read **Repo facts**. Record explicit contribution policies, AI-assistance requirements, and relevant conventions. Do not invent unstated policies.
3. Read **Issue**. Record the reported problem, expected behavior, environment, and constraints.
4. Read **Thread highlights**. Record relevant maintainer instructions, clarifications, objections, and constraints. Distinguish maintainer instructions from unverified contributor theories.
5. Read **Repro evidence** before reading the candidate plan. Record the commands or steps, inputs, observed behavior, expected behavior, and control runs. Identify which explanations the observations support or contradict.
6. Read **Candidate plan**. Extract its diagnosis, proposed changes, in-scope and out-of-scope boundaries, implementation locations, approach, test plan, and risks or unknowns. Identify these elements by meaning even if the plan does not use separate headings.
7. Read **Candidate plan comment**. Record what the contributor publicly claims, proposes, promises, and intends to verify.

Do not assign a grade merely because a diagnosis sounds plausible. Compare it against the reproduction established in step 5.

## Evidence gathering

Gather evidence for each rubric check using the following steps.

1. For `diagnosis-grounded`, compare the Candidate plan's diagnosis with the Issue, Repro evidence, control runs, and relevant Thread highlights. Record observations supporting or contradicting the proposed cause. A contributor's theory is not proof by itself. A plausible internal mechanism may remain a working hypothesis if the plan acknowledges the uncertainty and describes how it will validate the mechanism before depending on it.

2. For `change-addresses-cause`, identify the failure mechanism supported by the reproduction. Compare it with the Candidate plan's approach and proposed changes. Record whether the change addresses that mechanism or merely hides its symptoms, bypasses the failure, or changes unrelated behavior.

3. For `bounded-scope`, inspect the Candidate plan's Scope and Changes alongside the Issue and maintainer constraints in Thread highlights. Record the proposed implementation locations, intended change, explicit exclusions, and additional work. A deliberately narrower approach is acceptable when the excluded portion is acknowledged. Do not require every possible improvement.

4. For `executable-by-stranger`, inspect the Candidate plan's approach, target code areas, and proposed steps. Determine whether another developer could identify where to begin, what behavior to change, and which essential implementation decisions have been made. Exact function names are not mandatory if specific components and a concrete discovery step are provided. Do not reject a concise plan merely because it lacks lengthy explanations or a particular format.

5. For `test-decisive`, compare the Candidate plan's Test plan with the Repro evidence's original input, failing steps, actual behavior, and expected behavior. Record the proposed verification and specific result expected after implementation. Determine whether that result distinguishes the fixed behavior from the original failure. A faithful manual test can satisfy this check; do not require an automated test unless the repository explicitly requires one.

6. For `comment-and-conventions`, compare the Candidate plan comment with the Candidate plan, Thread highlights, and Repo facts. Record unsupported public claims, promises outside the plan, conflicts with maintainer direction, and violations of explicit repository policies. Require AI-assistance disclosure only when the repository's stated policy requires it.

Use the relevant section of `references/evidence-guide.md` whenever an evidence location or interpretation needs clarification.

For live grading, gather equivalent information from the actual issue, its reproduction comment, its discussion thread, repository contribution documentation, and the supplied plan and comment drafts.

## Check execution

Execute all checks in the order they appear in `rubric.md`:

1. `diagnosis-grounded`
2. `change-addresses-cause`
3. `bounded-scope`
4. `executable-by-stranger`
5. `test-decisive`
6. `comment-and-conventions`

For every check:

1. Locate the evidence specified in that check's Evidence column, using the material gathered above.
2. Compare that evidence directly with the check's Pass condition. Judge the proposed outcome, not the plan's length, formatting, or number of headings.
3. Assign exactly one grade:
   - **pass:** The available evidence satisfies the pass condition.
   - **fail:** The available evidence demonstrates that the pass condition is not satisfied or directly contradicts it.
   - **unclear:** Evidence needed to decide is missing, ambiguous, or insufficient.
4. Record a concise explanation tied to the package's actual evidence. Quote or identify the relevant observation, statement, or omission.
5. Do not silently supply missing assumptions, implementation decisions, test outcomes, or policies from general knowledge.
6. Do not automatically fail multiple checks for one weakness. Evaluate the distinct question each check owns. A check may pass even when another check fails.
7. Reuse previously gathered evidence instead of rereading the entire package unnecessarily.

If the candidate plan contains a credible but unverified diagnosis, apply the rubric's stated uncertainty condition: determine whether the uncertainty is acknowledged and whether validation is planned before implementation relies on it.

If there is insufficient evidence to determine whether a required condition is satisfied, grade that check `unclear`, not `pass`.

## Verdict assembly

1. Collect the grade and evidence-based reason for every rubric check.
2. Identify which checks are marked `required` and which, if any, are marked `preferred`.
3. Apply the verdict rule in `rubric.md`:
   - Return **accept (READY)** only when every required check passes.
   - Return **reject (HOLD)** if any required check fails or is unclear.
   - Treat `unclear` on a required check as a reason to hold, never as an assumed pass.
   - Preferred checks provide feedback but never change the verdict.
4. In the final explanation, identify the deciding required check or checks. Quote or reference the package evidence that explains the decision.
5. When returning HOLD, explain what is unsupported, contradicted, missing, or outside the allowed scope. Give feedback tied to the failed or unclear condition rather than proposing unrelated improvements.
6. Produce the final per-check grades and verdict using the output format required by `SKILL.md`. Do not substitute a separate grading format or override the rubric based on an overall impression.

The same package, rubric, evidence guide, and procedure should lead to the same verdict regardless of who executes the grading.

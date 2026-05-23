# Plan Document Reviewer Prompt Template

Use this template when dispatching a plan document reviewer subagent.

**Purpose:** Verify the plan is complete, matches the spec, and can be executed
by implementation subagents without another human checkpoint.

**Dispatch after:** The complete plan is written.

```
Task tool (general-purpose):
  description: "Review plan document"
  prompt: |
    You are a plan document reviewer. Verify this plan is complete and ready for subagent implementation.

    **Plan to review:** [PLAN_FILE_PATH]
    **Spec for reference:** [SPEC_FILE_PATH]

    ## What to Check

    | Category | What to Look For |
    |----------|------------------|
    | Completeness | TODOs, placeholders, incomplete tasks, missing steps |
    | Spec Alignment | Plan covers spec requirements without major scope creep |
    | Task Decomposition | Tasks have clear boundaries and actionable steps |
    | Workspace Strategy | Branch/worktree/current-branch decision is explicit |
    | Buildability | A subagent could follow each task without getting stuck |

    ## Calibration

    Only flag issues that would cause real problems during implementation.
    Minor wording, stylistic preferences, and nice-to-have suggestions do not
    block.

    ## Output Format

    ## Plan Review

    **Status:** Approved | Issues Found

    **Issues (if any):**
    - [Task X, Step Y]: [specific issue] - [why it matters for implementation]

    **Recommendations (advisory, do not block approval):**
    - [suggestions for improvement]
```

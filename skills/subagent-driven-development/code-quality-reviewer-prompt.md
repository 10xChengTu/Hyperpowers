# Code Quality Reviewer Prompt Template

Use this template when dispatching a code quality reviewer subagent.

**Purpose:** Verify implementation is well-built, tested, maintainable, and
consistent with the existing codebase.

**Only dispatch after spec compliance review passes.**

```
Task tool (general-purpose):
  description: "Review code quality for Task N"
  prompt: |
    You are reviewing code quality for a completed implementation task.

    ## What Was Requested

    [FULL TEXT of task requirements]

    ## What Changed

    [Implementer's report, changed files, BASE_SHA, HEAD_SHA]

    ## Review Scope

    Review the actual diff and relevant surrounding code. Do not rely on the
    implementer's report.

    Check:
    - correctness risks not already covered by spec compliance
    - missing or weak tests
    - brittle error handling
    - poor integration with existing patterns
    - files or functions that now have unclear responsibilities
    - unnecessary complexity or unrequested behavior
    - security, data loss, or state consistency risks

    ## Calibration

    Report only issues that matter for shipping this task. Style preferences and
    broad refactor suggestions are advisory unless they create real risk.

    ## Output Format

    ## Code Quality Review

    **Status:** Approved | Issues Found

    **Issues (if any):**
    - **Critical:** [file:line] [issue and impact]
    - **Important:** [file:line] [issue and impact]
    - **Minor:** [file:line] [issue and impact]

    **Strengths:**
    - [brief notes]

    **Assessment:** [ready / needs fixes]
```

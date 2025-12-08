# Guide to Using CodeRabbit and the Code Review Process

## Summary Workflow

1. [**CodeRabbit Account Management** - Create account, notify the team](#1-coderabbit-account-management)
2. [**Reviewer Configures CodeRabbit** - Check and update config for the project](#2-reviewer-configures-coderabbit)
3. [**Member Codes and Review Locally** - Install extension, run CodeRabbit locally before pushing](#3-member-codes-and-reviews-locally)
4. [**Member Creates Pull Request** - Re-check CodeRabbit feedback on the PR](#4-member-creates-pull-request)
5. [**Reviewer Review the PR** - Evaluate the code and CodeRabbit feedback](#5-reviewer-reviews-the-pull-request)
6. [**Member Fixes Based on Feedback** - Fix code based on feedback (repeat steps 4-5 if needed)](#6-member-fixes-based-on-feedback)

## Detailed Steps

### 1. CodeRabbit Account Management

👤 Responsible: Management Level

- Create a CodeRabbit account and purchase a subscription plan.
- Connect CodeRabbit to the repository.
- Add the Project Leader account with the Admin role.

👤 Responsible: Project Leader

- Add project members' account with the Member role.
- Notify all team members about using CodeRabbit.
- Management seat assignment for members.

> **Note**: Currently used only for projects under the `briswell-ltd` organization.

### 2. Reviewer Configures CodeRabbit

👤 Responsible: Reviewer.

- Check the `.coderabbit.yaml` file in the repository.
- Update configuration to match the project. Mainly focus on:
  + path_filters: Define which files CodeRabbit should review or ignore.
  + path_instructions: Add instructions for CodeRabbit based on specific rules (if needed).
  + pre_merge_checks: Adjust naming rules for PR titles (if needed).
  + knowledge_base: When adding new rules, update the `filePatterns` under `code_guidelines`.

### 3. Member Codes and Reviews Locally

👤 Responsible: Member.

#### 3.1. Install and Use the CodeRabbit Extension

- Install the CodeRabbit extension for VSCode or Cursor.
- Run a CodeRabbit review locally after completing a part or the whole feature.
- Do not create a PR (Ready for review) without running a local review first.

#### 3.2. Handle Issues by Priority Level

The priority levels are:

    🔴 CRITICAL - Severe issues that could cause system failures, security breaches, or data loss.
    🟠 MAJOR - Significant problems that impact functionality or performance.
    🟡 MINOR - Issues that should be addressed but don't critically impact the system.
    🔵 TRIVIAL - Low-impact suggestions for code quality improvements.
    ⚪ INFO - Informational comments or context without requiring action.

##### CRITICAL and CODING_STANDARD (MANDATORY)

- All issues of these types must be fixed.
- If unsure how to fix a CRITICAL issue → Ask the Project Leader before creating a PR.
- Do not create a PR if any CRITICAL issues or CODING_STANDARD violations remain.

##### MAJOR / MINOR / TRIVIAL (RECOMMENDED)

Not mandatory to fix immediately.
Consider fixing if:

  + The fix is clear.
  + The suggestion is reasonable and improves code quality.
  + If unsure → Ask the Project Leader.

#### 3.3. Re-check the Entire Code Against Coding Rules

### 4. Member Creates Pull Request

👤 Responsible: Member.

#### 4.1. Re-check on the PR

- After creating the PR, apply the `coderabbit-review` label so CodeRabbit can review it automatically.

  **Mandatory**: Any remaining CODING_STANDARD or CRITICAL issues must be fixed.

#### 4.2. Communicate with CodeRabbit

Note: Always use English when communicating with CodeRabbit.

Guidelines for communication:

- Provide accurate information about project rules.
- Explain clearly and specifically.
- Do not provide incorrect information, as CodeRabbit will "learn" and apply it in future reviews.

Example: CodeRabbit review
```
⚠️ Potential issue | 🟠 Minor
The variable name `_` doesn't follow naming conventions. 
Variable names should not start with underscore. Consider renaming to `type`.

Suggested change:
- const { type: _, ...rest } = formModel;
+ const { type, ...rest } = formModel;
```

Correct feedback:
```
@coderabbit This variable follows our team's naming convention for unused variables.
We use underscore (_) for values that are intentionally destructured but not used.
```

Incorrect feedback:
```
@coderabbit This is wrong.
❌ (No clear explanation)

@coderabbit We use snake_case for everything.
❌ (Incorrect - CodeRabbit will learn bad information)
```

### 5. Reviewer Reviews the Pull Request

👤 Responsible: Reviewer.

Review CodeRabbit's suggestions and classify them:

- Valid: Keep them for the member to fix.
- Invalid: Reject and provide a clear explanation so CodeRabbit can "learn".

Example of good feedback:
```
@coderabbit This suggestion doesn't apply. We use camelCase 
for all variable names according to our CODING_STANDARDS.md.
```

Examples of feedback to avoid:
```
This review is wrong. ❌
Not applicable. ❌
```

Note: The reviewer will also perform manual review and provide feedback for the member to fix.
Additionally, the reviewer may communicate further with CodeRabbit (using English).

If CodeRabbit repeatedly reviews a rule incorrectly, the reviewer must update the instruction and `knowledge_base` sections in the `.coderabbit.yaml` file of the project.

### 6. Member Fixes Based on Feedback

👤 Responsible: Member.

Apply the reviewer's feedback as usual and push the updated code. At this point:

  - CodeRabbit will automatically review the new changes (Step 4).
  - The reviewer will check again until all requirements are met (Step 5).

Repeat until all feedback has been addressed and the PR is merged.
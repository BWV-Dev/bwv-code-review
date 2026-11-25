# Code review workflow

Use this workflow for pull requests (PRs) between branches in the same repository. Complete project setup once. For each PR, the Member requests review. The Reviewer runs `bwv-code-review` and requests the BWV Workflows security review before deciding whether to approve the PR.

| Step | Responsible | Action |
| --- | --- | --- |
| 1 | Reviewer and Project Leader | [Prepare the project](#1-prepare-the-project) |
| 2 | Reviewer | [Review the PR with the skill](#2-review-the-pr-with-the-skill) |
| 3 | Reviewer | [Request a security review on the PR](#3-request-a-security-review-on-the-pr) |
| 4 | Reviewer and Member | [Resolve feedback and verify fixes](#4-resolve-feedback-and-verify-fixes) |

## 1. Prepare the project

**Reviewer:** Adapt the [Node.js](nodejs/CODING_STANDARDS.md) or [PHP](php/CODING_STANDARDS.md) template and commit it as `CODING_STANDARDS.md` at the project root. Sign in to GitHub CLI (`gh`) and install the skill from the project root. See the [Skills Hub installation guide](https://github.com/BWV-Dev/bwv-skills-hub/blob/HEAD/docs/install.md).

```sh
npx skills add BWV-Dev/bwv-skills-hub --skill bwv-code-review
```

**Project Leader:** Connect the repository to BWV Workflows and request access to the security-review recipe. Add the workflow below to `.bwv/workflows.yml` on each target branch, replacing `develop` with the branch name. See the [onboarding guide](https://bwv-dev.github.io/bwv-workflows-docs/getting-started/onboard-repository) and [recipe](https://bwv-dev.github.io/bwv-workflows-docs/recipes/security-review).

```yaml
version: 1

workflows:
  - name: security-review
    on:
      manual:
        pull_request:
          base: [develop]
    pullRequestBase: true
    onSuccess:
      comment: ${{ steps.review_report.output.markdown }}
    steps:
      - id: review
        uses: BWV-Dev/bwv-workflows-recipes/recipes/security-review/v1/review.yml@security-review/v1
```

## 2. Review the PR with the skill

**Reviewer:** Once the Member requests review, open the agent in the project repository. Enter the following command in the agent chat, replacing `123` with the open PR number. The skill reads `CODING_STANDARDS.md` from the root of the reviewed commit. To review against a specification, use `--doc` with the specification text or a file path.

```text
/bwv-code-review 123
```

The skill returns its findings in the agent chat and asks which ones to post. Make that selection after validating the findings in step 4.

## 3. Request a security review on the PR

**Reviewer:** Using an account with repository write access, post this command in the open PR conversation.

```text
/bwv run security-review
```

Follow the run in the `bwv/security-review` check. Read the separate **Security review** comment for the reviewed head commit and findings by severity. The check can pass with open findings. If the run fails, inspect the failed step, address the cause, and use **Retry**. If it passes but the report is missing, check the status comment for a posting error and verify `onSuccess.comment` on the base branch before requesting another run.

## 4. Resolve feedback and verify fixes

**Reviewer:** Check the findings and post only those that require a fix on the PR.

- **Code review skill:** Confirmed `CRITICAL` findings and findings tagged `CODING_STANDARD` must be fixed before approval.
- **Security review:** Confirmed `HIGH` findings must be fixed before approval. For confirmed `MEDIUM` and `LOW` findings, the Reviewer decides whether a fix is required before approval.

**Member:** Apply the agreed fixes, run the relevant tests, and push the changes.

**After a push:** The Reviewer runs `bwv-code-review` on the updated PR and posts `/bwv run security-review` again. Each run produces a separate report on success.

**Reviewer:** Verify the required fixes and confirm that both reviews cover the latest PR commit before approving. Use the head commit shown in the security report to check its scope.

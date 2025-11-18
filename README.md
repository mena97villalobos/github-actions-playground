# Repository Overview

This repository provides automated workflows to streamline and
standardize the cherry-picking process across release branches. By
leveraging GitHub Actions and label-driven automation, the project
removes manual overhead for developers and release managers when
backporting changes.

The automation is designed to:

-   Detect merged PRs that require cherry-picking\
-   Validate target branches\
-   Create dedicated cherry-pick branches\
-   Perform cherry-pick operations with conflict handling\
-   Open new pull requests for each cherry-pick\
-   Comment detailed results back on the original PR

This improves consistency, reduces operational burden, and enhances the
release engineering workflow.

------------------------------------------------------------------------

## Key Workflows

### **1. Cherry Pick on Label**

Automatically cherry-picks a merged pull request when a label starting
with `CP:` is applied.

-   Extracts merge commit SHA\
-   Parses target branch from the label (`CP: branch-name`)\
-   Validates target branch exists\
-   Creates a new cherry-pick branch\
-   Opens a PR toward the target branch\
-   Posts a summary back to the original PR

------------------------------------------------------------------------

### **2. Automatic Cherry-Pick on Merge**

Automatically cherry-picks PRs into predefined branches immediately
after merging into `trunk`. Ideal for continuous maintenance across
stable release lines.

------------------------------------------------------------------------

## 📚 Additional Documentation

  -------------------------------------------------------------------------------------------------------------------------------
  Feature               Description                        Link
  --------------------- ---------------------------------- ----------------------------------------------------------------------
  **Label-based         Cherry-pick triggered by `CP:`     [`docs/Label-PR-Cherrypicking.md`](./docs/Label-PR-Cherrypicking.md)
  Cherry-Picking**      labels                             

  **PR-based            Automatically cherry-picks upon    [`docs/PR-Cherrypicking.md`](./docs/PR-Cherrypicking.md)
  Cherry-Picking**      merge                              
  -------------------------------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## Goals of This Repository

-   Simplify backport workflows\
-   Increase developer efficiency\
-   Reduce manual cherry-pick errors\
-   Ensure consistent branch maintenance\
-   Provide clear visibility through automated PR comments

------------------------------------------------------------------------

## Tech Used

-   **GitHub Actions**\
-   **GitHub CLI (`gh`)**\
-   **Shell scripting**\
-   **Git automation**\
-   **Branch and release management tooling**

------------------------------------------------------------------------

## Feedback & Contributions

Feel free to open issues or submit PRs to improve the automation or
documentation. Contributions are always welcome!

# Repository Overview

This repository provides automated workflows to streamline and
standardize the cherry-picking process across release branches. By
leveraging GitHub Actions and label-driven automation, the project
removes manual overhead for developers and release managers when
backporting changes.

The automation is designed to:

-   Detect merged PRs that require cherry-picking
-   Validate target branches
-   Create dedicated cherry-pick branches
-   Perform cherry-pick operations with conflict handling
-   Open new pull requests for each cherry-pick
-   Comment detailed results back on the original PR

This improves consistency, reduces operational burden, and enhances the
release engineering workflow.

------------------------------------------------------------------------

## Key Workflows

### **1. Cherry-Pick on Label**

Automatically cherry-picks a merged pull request when a label starting
with `CP:` is applied.

-   Extracts merge commit SHA
-   Parses target branch from the label (`CP: branch-name`)
-   Validates target branch exists
-   Creates a new cherry-pick branch
-   Opens a PR toward the target branch
-   Posts a summary back to the original PR

Proper documentation can be found on [`docs/cherrypicking-on-label.md`](./docs/cherrypicking-on-label.md)

------------------------------------------------------------------------

### **2. Cherry-Pick on PR Merge**

Automatically cherry-picks PRs into predefined branches immediately
after merging into `trunk`. Ideal for continuous maintenance across
stable release lines.

Proper documentation can be found on [`docs/cherrypicking-on-pull-request.md`](./docs/cherrypicking-on-pull-request.md)

------------------------------------------------------------------------

### **3. Create CP Label on Release Branch Creation**

When a new branch starting with release is created, this workflow
automatically:

- Creates a label named CP: <branch-name>
- Assigns a red color for visibility
- Adds a description with the branch name
- Provides consistent label naming for cherry-picking workflows

This ensures release branches are automatically prepared for
label-triggered cherry-picking.

Full documentation can be found on [`docs/create-cp-label.md`](./docs/create-release-label.md)

------------------------------------------------------------------------

### **4. Delete CP Label on Release Branch Deletion**

When a `release*` branch is deleted, this workflow:
- Automatically removes the corresponding CP: <branch-name> label
- Prevents stale or orphaned labels from accumulating
- Keeps repository metadata clean and aligned with active release
branches

Full documentation can be found on [`docs/delete-cp-label.md`](./docs/delete-release-label.md)

------------------------------------------------------------------------

## Goals of This Repository

-   Simplify backport workflows
-   Increase developer efficiency
-   Reduce manual cherry-pick errors
-   Ensure consistent branch maintenance
-   Provide clear visibility through automated PR comments

------------------------------------------------------------------------

## Tech Used

-   **GitHub Actions**
-   **GitHub CLI (`gh`)**
-   **Shell scripting**
-   **Git automation**
-   **Branch and release management tooling**

------------------------------------------------------------------------

## Feedback & Contributions

Feel free to open issues or submit PRs to improve the automation or
documentation. Contributions are always welcome!

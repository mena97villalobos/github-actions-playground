# Cherry Pick on Label --- GitHub Action Documentation

## Overview

**Cherry Pick on Label** is a GitHub Action that automatically
cherry-picks a merged pull request into one or more target branches
based on a label applied to the PR. When a closed and merged PR on the
`trunk` branch receives a label starting with `CP:`, the workflow:

1.  Extracts the merge commit SHA of the original PR\
2.  Parses the label to determine the target branch\
3.  Creates a new branch based on the target\
4.  Attempts a cherry-pick\
5.  If successful, pushes the branch and automatically opens a new PR\
6.  Comments the cherry-pick results back on the original PR

------------------------------------------------------------------------

## Permissions

``` yaml
permissions:
  contents: write
  pull-requests: write
```

These permissions allow the Action to:

-   Fetch and push branches\
-   Create new pull requests\
-   Leave comments on existing PRs

------------------------------------------------------------------------

## Requirements

### Repository Requirements

-   Default branch is or includes `trunk`.
-   Target branches referenced by CP labels must exist.

### Labeling Convention

Labels must follow the format:

-   `CP:target_branch`
-   `CP: target_branch`

### GitHub Setup

-   `${{ secrets.GITHUB_TOKEN }}` must be available.
-   GitHub CLI (`gh`) is required (preinstalled on GitHub-hosted
    runners).

------------------------------------------------------------------------

## How It Works

### Trigger

Triggered when a PR receives a `labeled` event.

### Conditions

Runs only when:

-   PR is **closed**
-   PR is **merged**
-   Base branch is **trunk**
-   Label begins with **CP:**

### Workflow Steps

1.  Checkout repository\
2.  Configure git user\
3.  Retrieve merge commit SHA\
4.  Parse CP label for target branch\
5.  Validate branch exists\
6.  Create cherry-pick branch\
7.  Cherry-pick commit\
8.  Create PR on success\
9.  Add summary comment

------------------------------------------------------------------------

## Outputs

### `success_branches`

Branches where cherry-pick succeeded.

### `failed_branches`

Branches where cherry-pick failed or did not exist.

------------------------------------------------------------------------

## Implementation Details

### Git Configuration

``` bash
git config --global user.name "${{ github.actor }}"
git config --global user.email "${{ github.actor }}@users.noreply.github.com"
```

### Retrieve Merge SHA

``` bash
gh pr view "$PR_NUMBER" --repo "$REPO" --json mergeCommit --jq '.mergeCommit.oid'
```

### Branch Validation

``` bash
git ls-remote --exit-code origin "$target_branch"
```

### Cherry-pick Execution

``` bash
git cherry-pick -x "$MERGE_SHA"
```

### PR Creation

``` bash
gh pr create   --base "$target_branch"   --head "$new_branch"   --title "Cherry pick #${PR_NUMBER} into ${target_branch}"   --body "Automatic cherry-pick of #${PR_NUMBER}."
```

### Summary Comment

A structured summary is posted back to the original PR listing
successful and failed cherry-picks.

------------------------------------------------------------------------

## Summary

This GitHub Action streamlines developer workflow by automatically
performing cherry-picks based on PR labels, opening new PRs for each
cherry-pick, and posting a summary comment. This reduces manual work and
enforces consistent branch maintenance.

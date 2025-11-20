# Cherry Pick Automation GitHub Action

This workflow automates the **cherry-picking of merged pull requests** into one or more target branches based on specific labels.  
It uses the GitHub CLI (`gh`) to automatically create new pull requests for each successful cherry-pick.

---

## Permissions

The workflow requires the following permissions in your repository or calling workflow:

```yaml
permissions:
  contents: write
  pull-requests: write
```

These permissions allow the workflow to:
- Push new branches (`contents: write`)
- Create and comment on pull requests (`pull-requests: write`)

---

## Requirements

To trigger the automation successfully, the merged pull request must meet the following conditions:

1. **Merged PR:** The pull request must be merged (not just closed).
2. **Base Branch:** The PR’s base branch must be `trunk`.
3. **Cherry-Pick Label:** The PR must include a label named `cherry_pick`.
4. **Target Branch Labels:** To specify target branches, use labels following the format:

   ```
   CP: release/v1.0
   CP: hotfix/login-issue
   ```

   Each label starting with `CP: ` (note the space before the branch name is required) represents a target branch to cherry-pick into.

---

## How It Works

1. **Trigger:**  
   Runs when a pull request is **closed and merged** into `trunk`.

2. **Label Extraction:**  
   The workflow extracts all PR labels and checks for those beginning with `CP:` to determine target branches.

3. **Cherry-Pick Process:**  
   For each valid target branch:
    - Fetches and checks out the target branch.
    - Creates a new branch:
      ```
      feature/cherry-pick-<PR_NUMBER>-to-<target_branch>
      ```
    - Executes a `git cherry-pick` of the merged commit.
    - Pushes the new branch to origin.
    - Creates a pull request from the new branch into the target branch.

4. **Conflict Handling:**  
   If a conflict occurs during the cherry-pick, the operation is aborted and that branch is marked as failed.

5. **PR Comment Summary:**  
   After execution, the workflow posts a comment on the original PR summarizing the success or failure for each target branch.

---

## Outputs

| Output Name       | Description |
|--------------------|-------------|
| `success_branches` | List of branches where the cherry-pick succeeded. |
| `failed_branches`  | List of branches where the cherry-pick failed or were not found. |

These outputs are used in the final step to generate a summary comment.

---

## Implementation Details

### Git Configuration
Before performing cherry-picks, the workflow configures Git to use the GitHub Actions actor identity:

```bash
git config --global user.name "${{ github.actor }}"
git config --global user.email "${{ github.actor }}@users.noreply.github.com"
```

### Branch Validation
Before attempting to cherry-pick, the workflow validates that the target branch exists remotely:

```bash
git ls-remote --exit-code origin "$target_branch"
```

If the branch does not exist, it’s skipped and added to the failure list.

### PR Creation
Successful cherry-picks automatically generate pull requests using the GitHub CLI:

```bash
gh pr create   --base "$target_branch"   --head "$new_branch"   --title "Cherry pick #${PR_NUMBER} into ${target_branch}"   --body "Automatic cherry-pick of #${PR_NUMBER} into \`${target_branch}\`."
```

### Summary Comment Example
Example comment posted to the original PR:

### 🧩 Cherry-pick Summary for #123

✅ **Successful cherry-picks:**
- `release/v1.0`

⚠️ **Failed or skipped cherry-picks:**
- `hotfix/ui-fix (not found)`

---

## ✅ Example Label Configuration

To cherry-pick a merged PR into two branches (`release/1.0` and `release/2.0`):

| Label Name               | Description                 |
| ------------------------ | --------------------------- |
| `cherry_pick`            | Enables the workflow to run |
| `CP: release/exp-22.0.0` | Target branch 1             |
| `CP: release/exp-21.0.0` | Target branch 2             |

When the PR is merged, the workflow will create:
- `feature/cherry-pick-<PR>-to-release-exp-22.0.0`
- `feature/cherry-pick-<PR>-to-release-exp-21.0.0`

Each with its own PR into the respective branches.

---

## Summary

| Feature           | Description                                               |
| ----------------- | --------------------------------------------------------- |
| Trigger           | On merged PR with `cherry_pick` label                     |
| Automation        | Cherry-picks merged commits into multiple target branches |
| Tooling           | Git + GitHub CLI                                          |
| Conflict handling | Safe abort on conflict                                    |
| Reporting         | PR comment summarizing all results                        |
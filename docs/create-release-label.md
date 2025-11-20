# Documentation: Create CP Label on Release Branch

## Overview

This GitHub Action automatically creates a label whenever a new
`release*` branch is created.\
The label name follows the format:

    CP: <branch-name>

The label includes: - A red color (`ff0000`) - A description:
`Label for automatic cherry picking (<branch-name>)`

## Trigger

The workflow triggers on branch creation via the `create` event.\
Because GitHub ignores branch filters for the `create` event, the
workflow uses a conditional check:

``` yaml
if: startsWith(github.ref, 'refs/heads/release')
```

## File Location

Place this file in:

    .github/workflows/create-cp-label.yml

## Purpose

This automation ensures consistency in labeling release branches and
prepares the repository for automated cherry-picking workflows.

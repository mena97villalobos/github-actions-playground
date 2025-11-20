# Documentation: Delete CP Label on Release Branch Deletion

## Overview

This GitHub Action deletes the corresponding cherry-pick label when a
`release*` branch is deleted.\
The deleted label follows the format:

    CP: <branch-name>

## Trigger

Triggered by the `delete` event whenever a branch is removed.\
Since GitHub ignores branch filters on delete events, the job restricts
execution with:

``` yaml
if: startsWith(github.event.ref, 'release')
```

## File Location

Place this workflow in:

    .github/workflows/delete-cp-label.yml

## Purpose

Ensures repository cleanliness by automatically removing labels
associated with deleted release branches.

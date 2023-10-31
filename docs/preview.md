# Preview Environment

The following document outlines our Github Workflows which faciliate a preview environment deployment pipeline.

## Required Secrets

The following are secrets which are provided by a Skpr platform team member.

* SKPR_PREVIEW_REGISTRY_URL
* SKPR_PREVIEW_REGISTRY_USERNAME
* SKPR_PREVIEW_REGISTRY_PASSWORD

## Examples

### Create Preview Environment

Creates a preview environment as part of a pull request workflow.

This workflow will:

* Package the application using the same approach as `skpr package`.
* Deploy the application onto Preview environment infrastructure.
* Execute post deploy commands after the preview environment is deployed.

```yaml
name: Create Preview Environment

on:
  pull_request:
    types: [ synchronize, opened, reopened, ready_for_review ]

concurrency:
  group: preview-${{ github.head_ref }}
  cancel-in-progress: true

jobs:
  deploy:
    uses: skpr/gh-workflows/.github/workflows/preview-create.yml@main
    secrets:
      skpr_preview_registry_url: ${{ secrets.SKPR_PREVIEW_REGISTRY_URL }}
      skpr_preview_username: ${{ secrets.SKPR_PREVIEW_USERNAME }}
      skpr_preview_password: ${{ secrets.SKPR_PREVIEW_PASSWORD }}
    with:
      ref: ${{ github.event.pull_request.head.ref }}
      eks_cluster_name: NEEDS TO BE CONFIGURED
      k8s_namespace: NEEDS TO BE CONFIGURED
      domain: NEEDS TO BE CONFIGURED
      post_deploy_command: drush deploy
```

### Delete Preview Environment

Deletes a preview environment when a pull request is closed or moved to a draft state.

```yaml
name: Delete Preview Environment

on:
  pull_request:
    types: [ closed, converted_to_draft ]

concurrency:
  group: preview-${{ github.head_ref }}
  cancel-in-progress: true

jobs:
  deploy:
    uses: skpr/gh-workflows/.github/workflows/preview-delete.yml@main
    secrets:
      skpr_preview_username: ${{ secrets.SKPR_PREVIEW_USERNAME }}
      skpr_preview_password: ${{ secrets.SKPR_PREVIEW_PASSWORD }}
    with:
      ref: ${{ github.event.pull_request.head.ref }}
      eks_cluster_name: NEEDS TO BE CONFIGURED
      k8s_namespace: NEEDS TO BE CONFIGURED
```

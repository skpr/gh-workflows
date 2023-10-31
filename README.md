# GitHub Actions Workflows

This repository contains GitHub Action workflows to make it easy to automate your development pipeline.

## Usage

Example use below, see documentation for more [details](#documentation) on each workflow.

```yaml
jobs:
  my_job:
    uses: skpr/gh-workflow/.github/workflows/workflow_file.yml@main
    with:
      input_a: abc
      input_b: def
```

## Documentation

* [Preview Environment](/docs/preview.md) - Create preview environments as part of a pull request workflow.

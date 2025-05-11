# runs-on

runs-on related workflows

## Example

In another shared workflow, use `harmony-labs/runs-on/.github/workflows/setup.yaml@main` to allow user to pass in a string or JSON array of strings with runner information:

```
name: xrd-preview-clean
on:
  workflow_call:
    inputs:
      runs-on:
        description: 'Runner configuration: either a single string (e.g., "ubuntu-latest") or a JSON array'
        type: string
        default: 'ubuntu-latest'
        required: false

jobs:
  setup:
    uses: harmony-labs/runs-on/.github/workflows/setup.yaml@main
    with:
      runs-on: ${{ inputs.runs-on }}
  
  your-workflow:
    needs: setup
    strategy:
      matrix:
        include: ${{ fromJSON(needs.setup.outputs.matrix) }}
    runs-on: ${{ matrix.runs-on }}
    steps:
      - // ...
```

This shared workflow can be called with an array to use `runs-on`:

```
with:
  runs-on: |
    [
      {"runs-on": "runs-on=${{ github.run_id }}", "runner": "2cpu-linux-x64"}
    ]
```

Or a string to use a Github provider runner:

```
with:
  runs-on: ubuntu-latest
```

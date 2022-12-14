# jsonc-syntax-check

This Action tests the validity of jsonc (ie. JSON - Comments) files.

It does this by running a Perl script inside a Docker container.

Table of Contents

- [Check all **jsonc** files](#check-all-files)
- [Check only added/updated files](#check-only-addedmodified-files)

## Check all files

    # jsonc-syntax-check.yml

    name: JSON comments check

    on:
      push:
        paths:
          - '**.jsonc'

    jobs:
      jsonc_syntax_check:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - name: jsonc-syntax-check
            uses: stevieb9/jsonc-syntax-check@1.02
              with:
                pattern: "*.jsonc"

## Check only added/modified files

    # jsonc-syntax-check.yml
    
    name: JSON comments check

    on:
      push:    
        paths:
          - '**.jsonc'
      pull_request:

    jobs:
      jsonc_syntax_check:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
        - id: changed_files
          uses: jitterbit/get-changed-files@v1
          with:
            format: csv

        - name: jsonc-syntax-check
          uses: stevieb9/jsonc-syntax-check@1.02
          with:
            pattern: "*.jsonc"
            files: ${{ steps.changed_files.outputs.added_modified }}

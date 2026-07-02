## AutoCoder GitHub Action

AutoCoder is a reusable composite GitHub Action that generates code from GitHub issue descriptions using ChatGPT. It reads an issue body, sends the content to the OpenAI API, saves generated files, and can be used as part of an automated pull request workflow.

### Usage

```yaml
name: AutoCoder

on:
  issues:
    types: [opened, reopened, labeled]

jobs:
  generate_code:
    if: contains(github.event.issue.labels.*.name, 'autocoder-bot')
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: AutoCoder
        uses: renbaum/AutoCoder@v1
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          REPOSITORY: ${{ github.repository }}
          ISSUE_NUMBER: ${{ github.event.issue.number }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          SCRIPT_PATH: scripts/script.sh
          LABEL: autocoder-bot

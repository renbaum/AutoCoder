# AutoCoder
 
AutoCoder is a reusable GitHub Action that helps automate code generation from GitHub issues. The action reads the content of an issue, uses the issue body as a prompt, sends that prompt to ChatGPT through the OpenAI API, and saves the generated code into files inside the repository. It is designed for workflows where developers describe a task in a GitHub issue and want an automated assistant to create an initial implementation.
 
The project demonstrates how GitHub Actions can interact with external APIs, process issue metadata, run shell scripts, generate repository files, and prepare changes for review through pull requests. AutoCoder uses labels to decide which issues should be processed, so only issues marked with the configured label are sent to the automation process.
 
This repository also serves as an example of building a reusable composite GitHub Action. Other projects can include AutoCoder in their own workflows by referencing this repository and providing the required inputs, such as the GitHub token, repository name, issue number, OpenAI API key, script path, and trigger label.
 
## Inputs
 
| Name              | Description                                                                 | Required | Default              |
|-------------------|-------------------------------------------------------------------------------|----------|-----------------------|
| `GITHUB_TOKEN`    | GitHub token used for GitHub API authentication and to open pull requests.    | Yes      | —                     |
| `REPOSITORY`      | The repository the action runs against, in `owner/repo` format.               | Yes      | —                     |
| `ISSUE_NUMBER`    | The number of the issue that triggered the action.                            | Yes      | —                     |
| `OPENAI_API_KEY`  | API key used to authenticate with the OpenAI API.                             | Yes      | —                     |
| `SCRIPT_PATH`     | Path to the script that talks to ChatGPT and generates the code.              | Yes      | `scripts/script.sh`  |
| `LABEL`           | Issue label that triggers the AutoCoder workflow.                             | Yes      | `autocoder-bot`      |
 
## Outputs
 
| Name                | Description                                                |
|---------------------|--------------------------------------------------------------|
| `pull_request_url`  | URL of the pull request automatically created by the action. |
 
## Using the AutoCoder Composite Action
 
To use this action, add a workflow file such as `.github/workflows/main.yml` to your repository:
 
```yaml
name: AutoCoder
 
on:
  issues:
    types: [opened, reopened, labeled]
 
permissions:
  contents: write
  pull-requests: write
  issues: read
 
jobs:
  interact-with-chatgpt:
    if: contains(github.event.issue.labels.*.name, 'autocoder-bot')
    runs-on: ubuntu-latest
 
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
 
      - name: AutoCoder Composite Action
        uses: your-username/autocoder-action@v1
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          REPOSITORY: ${{ github.repository }}
          ISSUE_NUMBER: ${{ github.event.issue.number }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          SCRIPT_PATH: scripts/script.sh
          LABEL: autocoder-bot
```
 
Make sure to replace `your-username/autocoder-action` with the actual name of your repository, and to add `OPENAI_API_KEY` as a secret in your repository settings. The workflow above triggers whenever an issue is opened, reopened, or labeled, and only runs the automation when the issue carries the `autocoder-bot` label.
 
### How it works
 
1. `actions/checkout` checks out the repository so the action has access to its files.
2. The AutoCoder action fetches the triggering issue's body via the GitHub API.
3. The issue body is sent to the OpenAI API as a prompt.
4. The generated code is parsed and written to files under the `autocoder-bot/` directory.
5. A pull request is opened automatically with the generated changes, ready for review.

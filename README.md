# AutoCoder

AutoCoder is a reusable GitHub Action that helps automate code generation from GitHub issues. The action reads the content of an issue, uses the issue body as a prompt, sends that prompt to ChatGPT through the OpenAI API, and saves the generated code into files inside the repository. It is designed for workflows where developers describe a task in a GitHub issue and want an automated assistant to create an initial implementation.

The project demonstrates how GitHub Actions can interact with external APIs, process issue metadata, run shell scripts, generate repository files, and prepare changes for review through pull requests. AutoCoder uses labels to decide which issues should be processed, so only issues marked with the configured label are sent to the automation process.

This repository also serves as an example of building a reusable composite GitHub Action. Other projects can include AutoCoder in their own workflows by referencing this repository and providing the required inputs, such as the GitHub token, repository name, issue number, OpenAI API key, script path, and trigger label.

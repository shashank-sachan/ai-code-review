# AI Code Review Tool

This tool provides automated code review for pull requests using either Google Gemini or OpenAI GPT models. It reads a git diff from stdin, sends it to the selected AI provider, and posts the review as a comment on a GitHub pull request.

## Features
- Supports both Gemini and OpenAI (GPT-4.1, GPT-3.5-turbo) as review providers
- Posts review comments directly to GitHub PRs
- Simple CLI interface with provider selection

## Setup

### 1. Build the Tool
```sh
cd .github/ai-review
go build -o ai-review main.go gemini.go openai.go
```

### 2. Required Environment Variables
Set the following environment variables before running the tool:

#### For All Providers
- `GITHUB_OWNER`         – GitHub repository owner (e.g., `my-org`)
- `GITHUB_REPO`          – GitHub repository name (e.g., `my-repo`)
- `GITHUB_ISSUE_NUMBER`  – Pull request number (as string or number)
- `GITHUB_TOKEN`         – GitHub token with `repo` scope to post comments

#### For Gemini
- `GEMINI_API_KEY`       – Your Google Gemini API key

#### For OpenAI
- `OPENAI_API_KEY`       – Your OpenAI API key

### 3. Usage

Pipe a git diff to the tool and specify the provider:

```sh
git diff | ./ai-review-bin --provider=gemini
# or
git diff | ./ai-review-bin --provider=openai
```

- The tool will analyze the diff and post a review comment to the specified GitHub PR.
- If the provider flag is omitted, `gemini` is used by default.

### 4. Example

```sh
export GITHUB_OWNER=my-org
export GITHUB_REPO=my-repo
export GITHUB_ISSUE_NUMBER=123
export GITHUB_TOKEN=ghp_...yourtoken...
export GEMINI_API_KEY=...your-gemini-key...
export OPENAI_API_KEY=...your-openai-key...

git diff | ./ai-review --provider=openai
```

## Notes
- The tool expects a unified diff from stdin (e.g., from `git diff`).
- Only one provider is used per run.
- The review is posted as a single comment on the PR.

## Extending
- To add more providers, implement the `Provider` interface in a new file and update the provider selection logic in `main.go`.

## License
See project root for license information. 
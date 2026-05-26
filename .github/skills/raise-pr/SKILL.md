---
name: raise-pr
description: Create GitHub pull requests from the CLI using gh. Use this skill whenever the user asks to open, create, submit, or raise a PR, especially when they mention branches, base/head refs, release merges, or GitHub CLI workflows.
---

# Raise PR with GitHub CLI

Use this skill to reliably create a pull request with `gh pr create`.

## When To Use

Use this skill when the user wants to:
- create a PR from a local branch
- open a PR against `main` or another base branch
- automate or standardize PR creation from terminal commands

## Required Inputs

Collect these values before running commands:
- `base_branch` (default: `main`)
- `head_branch` (default: current branch)
- `title`
- `body`

If `title` or `body` is missing, ask the user before proceeding.

## Preconditions

Check these quickly:
1. `gh` is installed (`gh --version`)
2. user is authenticated (`gh auth status`)
3. current repo has a GitHub remote
4. head branch exists locally (and push if needed)

If any check fails, explain the exact fix and stop.

## Workflow

1. Confirm final inputs with the user.
2. If `head_branch` is not pushed yet, run:
   - `git push -u origin <head_branch>`
3. Create the pull request:

```bash
gh pr create --base "<base_branch>" --head "<head_branch>" --title "<title>" --body "<body>"
```

4. Return the PR URL from command output.

## Preferred Body Handling

If the body is multiline or long, write it to a temporary file and use:

```bash
gh pr create --base "<base_branch>" --head "<head_branch>" --title "<title>" --body-file /tmp/pr_body.md
```

## Output Format

Return a concise result in this format:

```text
PR created successfully.
Base: <base_branch>
Head: <head_branch>
Title: <title>
URL: <pr_url>
```

If creation fails, return:
- the exact failing command
- the key error line
- one concrete next action

## Example

**Input:**
- base_branch: `main`
- head_branch: `feature-branch`
- title: `My PR`
- body: `Description`

**Command:**

```bash
gh pr create --base "main" --head "feature-branch" --title "My PR" --body "Description"
```


---
allowed-tools: Bash(git diff, git log, git show), FileSystem(writeFile)
---

Find latest version and previous version with `git tag -l --sort=-version:refname | head -n 2` command. Store them in variables.

Find latest commit messages and changed files with `git log --stat $(git tag --sort=-version:refname | head -n 2 | tail -n 1)..$(git tag --sort=-version:refname | head -n 1)` command.

## Commit Analyze Rules

- Identify the general changes made in the commits between latest version and previous version.
- Summarize the changes in a clear and concise manner with `📋 Overview` title.
- List the summarized and grouped changes in a bullet point format with `🔄 Major Changes` title.
- Use emojis to highlight the type of changes made and in titles.
- Prepare historical a change log with `📝 Detailed Changelog` title.
  - Group changes by category semantically.
  - Ignore duplicated or similar changes.
  - Each change should have a commit hash (without any notation or backtick), description, and the type of change (e.g., feature, fix, refactor).
  - Must be in one sentence per change.
- Add list of affected branch names with `🌿 Affected Branches` title.
  - Each branch should have a link to the branch in the repository (e.g., https://github.com/{user|org}/{repo_name}/tree/{branch_name}).
- Add list of affected app and project names (use project.json project name field) with `📁 Affected Projects` title.
- Add full change log link to the end in `**Full Changelog**: https://github.com/ngeenx/ngeen-platform/compare/v{PREVIOUS_VERSION}...v{LATEST_VERSION}` format.

## Git Submodules

- If there are any git submodules, add them to the release note with `📁 Affected Submodules` title.

## Output Rules

- Output must be in markdown format and save in `./dist/release-notes/release-v{LATEST_VERSION}.md` file (with latest version number).
- If the file already exists, overwrite it.

## Create New Release by Github CLI

- Use `gh release create` command to create a release.
- Command: `gh release create v{LATEST_VERSION} -F ./dist/release-notes/release-v{LATEST_VERSION}.md`
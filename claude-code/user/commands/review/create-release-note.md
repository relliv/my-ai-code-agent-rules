---
allowed-tools: Bash(git diff, git log, git show), FileSystem(writeFile)
---

Check latest commit messages and commit descriptions between latest version and previous version and prepare following:

## Content Rules

- Identify the general changes made in the codebase.
- Summarize the changes in a clear and concise manner with `📋 Overview` title.
- List the summarized and grouped changes in a bullet point format with `🔄 Major Changes` title.
- Use emojis to highlight the type of changes made and in titles.
- Prepare historical a change log with `📝 Detailed Changelog` title.
  - Group changes by category semantically.
  - Ignore duplicated or similar changes.
  - Each change should have a commit hash (without any notation or backtick), description, and the type of change (e.g., feature, fix, refactor).
  - Must be in one sentence per change.
- Add list of affected branch names with `🌿 Affected Branches` title.
  - Exclude dev and main branches.
- Add list of affected app and project names (use project.json project name field) with `📁 Affected Projects` title.
- Add full change log link to the end in `**Full Changelog**: https://github.com/ngeenx/ngeen-platform/compare/v1.2.3...v1.2.4` format.

## Git Submodules

- If there are any git submodules, add them to the release note with `📁 Affected Submodules` title.

## Output Rules

- Output must be in markdown format and save in `./dist/release-notes/release-v1.2.4.md` file (with latest version number).
- If the file already exists, overwrite it.

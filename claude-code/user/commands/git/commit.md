---
allowed-tools: All
---

# Semantic Commit Generator

Analyze the staged changes and create a conventional commit. Read `commitlint.config.mjs` file for commit rules if exists.

## Instructions

1. Run `git diff --cached --stat` to see what files are staged
2. Run `git diff --cached` to analyze the actual changes
3. Based on the changes, determine the appropriate commit type:
   - `feat`: A new feature
   - `fix`: A bug fix
   - `docs`: Documentation only changes
   - `style`: Changes that don't affect code meaning (whitespace, formatting)
   - `refactor`: Code change that neither fixes a bug nor adds a feature
   - `perf`: Performance improvement
   - `test`: Adding or correcting tests
   - `build`: Changes to build system or dependencies
   - `ci`: Changes to CI configuration
   - `chore`: Other changes that don't modify src or test files
   - `revert`: Reverts a previous commit
   - If `commitlint.config.mjs` file exists, read it and use it as rules.

4. Identify the scope (optional) - the part of codebase affected (e.g., auth, api, ui)

5. Write a clear, concise commit message following this format:

   ```txt
   <type>(<scope>): <subject>

   [optional body]

   [optional footer(s)]
   ```

6. Rules for the subject line:
   - Use imperative mood ("add" not "added" or "adds")
   - Don't capitalize the first letter
   - No period at the end
   - Max 72 characters

7. Append current running model as co-author with `Co-Authored-By: MODEL_NAME MODEL_VERSION <noreply@anthropic.com>` to the commit message footer.

8. Present the suggested commit message and ask for confirmation
   1. If bypass permissions is on, ignore this step.
   2. If bypass permissions is off, ask for confirmation.

9. If confirmed, run `git commit -m "<message>"`

Finally, display the commit message to user.

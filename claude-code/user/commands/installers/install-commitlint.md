---
allowed-tools: All
---

1. Install required packages with `pnpm install @commitlint/cli @commitlint/config-conventional @commitlint/config-nx-scopes husky -D`
2. Install husky: `npx husky install` command
3. Create `.husky` folder with `mkdir -p .husky` command
4. Create commit-msg hook with `npx husky add .husky/commit-msg 'npx --no -- commitlint --edit ${1}'` command
5. Create `commitlint.config.mjs` file in root dir then put this config:

   ```javascript
   export default {
      extends: ["@commitlint/config-conventional", "@commitlint/config-nx-scopes"],
      rules: {
         // Override or add custom rules here if needed
         // Common customizations:
         "type-enum": [
            2,
            "always",
            [
            "feat", // A new feature
            "fix", // A bug fix
            "docs", // Documentation only changes
            "style", // Changes that do not affect the meaning of the code
            "refactor", // A code change that neither fixes a bug nor adds a feature
            "perf", // A code change that improves performance
            "test", // Adding missing tests or correcting existing tests
            "build", // Changes that affect the build system or external dependencies
            "ci", // Changes to our CI configuration files and scripts
            "chore", // Other changes that don't modify src or test files
            "revert", // Reverts a previous commit
            ],
         ],
         "type-case": [2, "always", "lowercase"],
         "subject-case": [
            2,
            "never",
            ["sentence-case", "start-case", "pascal-case", "upper-case"],
         ],
         "subject-empty": [2, "never"],
         "subject-full-stop": [2, "never", "."],
         "body-leading-blank": [2, "always"],
         "footer-leading-blank": [2, "always"],
         "body-max-line-length": [2, "always", 1000],
         "header-max-length": [2, "always", 150],
      },
   };
   ```

6. If not exists add `prepare` script to `package.json` file in end of scripts section:

   ```json
   {
     "scripts": {
       ...
       "prepare": "husky"
     }
   }
   ```

---
allowed-tools: All
---

Install Tailwind CSS v4 in the current project.

1. Install postcss and necessaary plugins with `pnpm install postcss postcss-import postcss-loader postcss-nesting postcss-preset-env postcss-url postcss-load-config postcss-nested -D` command
2. Install required tailwindcss v4 dependencies with `pnpm install tailwindcss@latest tw-animate-css@latest @tailwindcss/cli@latest @tailwindcss/forms@latest @tailwindcss/nesting@latest @tailwindcss/postcss@latest -D` command
3. Create `.postcssrc.json` file with the following content:

    ```json
    {
    "plugins": {
        "@tailwindcss/postcss": {},
        "postcss-nested": {}
    }
    }
    ```

4. Commit the changes with `git add . && git commit -m "chore: add Tailwind CSS v4 and PostCSS configuration"`
5. Run `test -f tailwind.config.js && echo "Legacy config found" || echo "Legacy config not found"` command
6. If legacy config found: warn user to remove it user with 'Legacy tailwind.config.js config found, configs moved to .css files' command
7. Show how to use tailwindcss to user and finish task.

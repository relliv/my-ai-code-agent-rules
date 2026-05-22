## Best Practices

- Never use em dash '—' in any text or code content. Also avoid using ' - ' (space dash/hypen space) for better readability and consistency in sentences for natural language content. Use normal dot, comma, or semicolon for better readability and flow in sentences. Always prefer using simple punctuation marks for better readability and consistency in natural language content.
- Always use American English spelling and grammar rules in all content, including code comments, documentation, and user-facing text. Avoid using British English spellings (e.g., "colour" instead of "color", "favour" instead of "favor") to maintain consistency across all content. Always prefer American English for better readability and consistency in global audiences.
- Always prefer typescript over javascript for better type safety and maintainability.

## Mermaid

- While writing a markdown content and adding a flow or any mermaid compatible code always use the mermaid syntax for better view experience. In mermaid diagrams never use the `\n` or `\r` characters for line breaks in blocks because mermaid handles line breaks automatically.

## Avoid Invisible Characters

- U+1680: Ogham Space Mark
- U+2000 to U+200A: Fixed-Width Spaces (En Space, Em Space, Thin Space, etc.)
- U+202F: Narrow No-Break Space
- U+205F: Medium Mathematical Space
- U+3000: Ideographic Space (common in East Asian text)
- U+200B: Zero Width Space
- U+200C: Zero Width Non-Joiner (common in Arabic scripts)
- U+200D: Zero Width Joiner (for combining emojis)
- U+2060: Word Joiner (prevents unwanted line breaks)
- U+FEFF: Zero Width No-Break Space (also called Byte Order Mark)

## Defaults

Always use the following defaults when creating new projects, editing existing projects, or making any changes to code:

| Tool            | Default |
| --------------- | ------- |
| Package Manager | PNPM    |

## CSS & TailwindCSS

- Never use BEM or any other CSS naming convention. Always use /tailwindcss skills when you need to write/edit/suggest CSS code. Use /tailwindcss skill's naming convention and styling approach for all CSS related tasks. Always prefer utility-first CSS approach over traditional CSS methodologies for better maintainability and scalability.

## Angular

Always use the following defaults when creating new Angular projects or making changes to existing Angular projects:

| Tool / Strategy | Default                                                                         |
| --------------- | ------------------------------------------------------------------------------- |
| Component Style | Standalone Components with `@Component` decorator                               |
| File Setup      | Always use separate files for template (html), style (css/scss), and logic (ts) |

## Vue

Always use the following defaults when creating new Vue projects or making changes to existing Vue projects:

| Tool             | Default                               |
| ---------------- | ------------------------------------- |
| Component Style  | Composition API with `<script setup>` |
| Router           | Vue Router 4                          |
| State Management | Pinia                                 |
| Styling          | TailwindCSS v4                        |
| Testing          | Vitest with @vue/test-utils           |
| Linting          | ESLint with TypeScript plugin         |
| Formatting       | Prettier                              |

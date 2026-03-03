---
allowed-tools: All
---

Set up Vitest with AnalogJS for the Angular library at `$ARGUMENTS`. Follow every step below in order.

> **Usage:** `/install-angular-analog-vitest <relative-path-to-library>`
> Example: `packages/client/shared/ui/external-ui/primitives/nin-button`

---

## Prerequisites — read first

Before touching any file, gather the following context:

1. Read `$ARGUMENTS/project.json` — note `name`, `sourceRoot`, `projectType`, and the relative path depth (count of `../` levels needed to reach workspace root). Call this **`<depth>`**.
2. Read `$ARGUMENTS/tsconfig.json` — note the `extends` path and `files`/`include` arrays.
3. Read `$ARGUMENTS/tsconfig.lib.json` — note `outDir`, existing `exclude` array.
4. Read `$ARGUMENTS/package.json` if it exists (some libs don't have one — that's fine).
5. Run `ls $ARGUMENTS/src/` to confirm the source root exists.

---

## Step 1 — Install packages (skip if already in root `package.json`)

Check root `package.json` devDependencies. Install **only** what is missing:

```bash
pnpm add -D \
  @analogjs/vite-plugin-angular \
  @analogjs/vitest-angular \
  @nx/vitest \
  @nx/vite \
  @vitest/coverage-v8 \
  @vitest/ui \
  vitest \
  jsdom \
  -w
```

> In this monorepo all of the above are already present — skip the install if versions are already listed.

---

## Step 2 — Create `vite.config.mts`

Create `$ARGUMENTS/vite.config.mts` with the following content.

Replace `<PROJECT_NAME>` with the `name` field from `project.json`.
Replace `<CACHE_PATH>` with `node_modules/.vite/<relative-path-from-workspace-root>` (use the same relative path as the library, replacing `/` with `/`).
Replace `<COVERAGE_PATH>` with `<depth-prefix>/coverage/<relative-path-from-workspace-root>` where `<depth-prefix>` is the correct number of `../` to reach the workspace root.

```typescript
/// <reference types='vitest' />
import { defineConfig } from 'vite';
import angular from '@analogjs/vite-plugin-angular';
import { nxViteTsPaths } from '@nx/vite/plugins/nx-tsconfig-paths.plugin';
import { nxCopyAssetsPlugin } from '@nx/vite/plugins/nx-copy-assets.plugin';

export default defineConfig(() => ({
  root: __dirname,
  cacheDir: '<depth-prefix>/node_modules/.vite/<relative-path-from-workspace-root>',
  plugins: [angular(), nxViteTsPaths(), nxCopyAssetsPlugin(['*.md'])],
  test: {
    name: '<PROJECT_NAME>',
    watch: false,
    globals: true,
    environment: 'jsdom',
    include: ['{src,tests}/**/*.{test,spec}.{js,mjs,cjs,ts,mts,cts,jsx,tsx}'],
    setupFiles: ['src/test-setup.ts'],
    reporters: ['default'],
    coverage: {
      reportsDirectory: '<depth-prefix>/coverage/<relative-path-from-workspace-root>',
      provider: 'v8' as const,
    },
  },
}));
```

**Example** for a library at `packages/client/shared/ui/external-ui/primitives/nin-button` (depth = 7 `../`):

```typescript
/// <reference types='vitest' />
import { defineConfig } from 'vite';
import angular from '@analogjs/vite-plugin-angular';
import { nxViteTsPaths } from '@nx/vite/plugins/nx-tsconfig-paths.plugin';
import { nxCopyAssetsPlugin } from '@nx/vite/plugins/nx-copy-assets.plugin';

export default defineConfig(() => ({
  root: __dirname,
  cacheDir:
    '../../../../../../node_modules/.vite/packages/client/shared/ui/nin-button',
  plugins: [angular(), nxViteTsPaths(), nxCopyAssetsPlugin(['*.md'])],
  test: {
    name: 'nin-button',
    watch: false,
    globals: true,
    environment: 'jsdom',
    include: ['{src,tests}/**/*.{test,spec}.{js,mjs,cjs,ts,mts,cts,jsx,tsx}'],
    setupFiles: ['src/test-setup.ts'],
    reporters: ['default'],
    coverage: {
      reportsDirectory:
        '../../../../../../coverage/packages/client/shared/ui/nin-button',
      provider: 'v8' as const,
    },
  },
}));
```

---

## Step 3 — Create `src/test-setup.ts`

Create `$ARGUMENTS/src/test-setup.ts`:

```typescript
/// <reference types="vitest/globals" />

import '@angular/compiler';
import '@analogjs/vitest-angular/setup-snapshots';

import { setupTestBed } from '@analogjs/vitest-angular/setup-testbed';
import { vi } from 'vitest';

// Mock window.matchMedia for components that use media queries
Object.defineProperty(globalThis, 'matchMedia', {
  writable: true,
  value: vi.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: vi.fn(),
    removeListener: vi.fn(),
    addEventListener: vi.fn(),
    removeEventListener: vi.fn(),
    dispatchEvent: vi.fn(),
  })),
});

// Mock clipboard API
Object.defineProperty(navigator, 'clipboard', {
  value: {
    writeText: vi.fn().mockResolvedValue(undefined),
    readText: vi.fn().mockResolvedValue(''),
  },
  writable: true,
});

// Mock document.queryCommandSupported
Object.defineProperty(document, 'queryCommandSupported', {
  writable: true,
  value: vi.fn().mockReturnValue(false),
});

setupTestBed();
```

> **Note:** If the library uses Zone.js (non-zoneless), also add `import '@analogjs/vitest-angular/setup-zone';` before the other imports and call `setupTestBed({ zoneless: false })` instead.

> **Note:** If the library uses `localforage` or `localStorage`, add the relevant mocks from the pattern used in `packages/client/platform/features/ngn-flame-board-view/src/test-setup.ts`.

---

## Step 4 — Update `tsconfig.spec.json`

If `tsconfig.spec.json` does not exist, create it. If it exists, merge the fields below.

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "<depth-prefix>/dist/out-tsc",
    "target": "es2022",
    "types": ["node", "vitest/globals"]
  },
  "files": ["src/test-setup.ts"],
  "include": [
    "src/**/*.spec.ts",
    "src/**/*.d.ts"
  ]
}
```

---

## Step 5 — Update `tsconfig.lib.json`

Ensure `src/test-setup.ts`, `**/*.spec.ts`, and `**/*.test.ts` are in the `exclude` array (to keep them out of the library build):

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "<depth-prefix>/dist/out-tsc",
    "composite": true,
    "declaration": true,
    "declarationMap": true,
    "inlineSources": true,
    "types": []
  },
  "exclude": [
    "src/**/*.spec.ts",
    "src/test-setup.ts",
    "src/**/*.test.ts",
    "**/*.stories.ts",
    "**/*.stories.js"
  ],
  "include": ["src/**/*.ts"]
}
```

---

## Step 6 — Update `tsconfig.json` references

Add a reference to `tsconfig.spec.json` in the `references` array of `$ARGUMENTS/tsconfig.json` if it's not already there:

```json
{
  "references": [
    { "path": "./tsconfig.lib.json" },
    { "path": "./tsconfig.spec.json" }
  ]
}
```

---

## Step 7 — Add `test` and `test-ui` targets to `project.json`

Edit `$ARGUMENTS/project.json` and add the following targets inside the `"targets"` object:

```json
"test": {
  "executor": "@nx/vitest:test",
  "outputs": ["{workspaceRoot}/coverage/{projectRoot}"],
  "options": {
    "reportsDirectory": "<depth-prefix>/coverage/<relative-path-from-workspace-root>",
    "passWithNoTests": false
  }
},
"test-ui": {
  "executor": "nx:run-commands",
  "options": {
    "command": "vitest --ui",
    "cwd": "<relative-path-from-workspace-root>"
  }
}
```

> Replace `<relative-path-from-workspace-root>` with the path as seen from the workspace root, e.g. `packages/client/shared/ui/external-ui/primitives/nin-button`.

---

## Step 8 — Verify the setup

Run the test target to confirm everything works:

```bash
nx run <PROJECT_NAME>:test
```

Expected output: `✓ 0 tests passed` (no test files yet = success with `passWithNoTests: false` will error — that's expected until spec files are added).

To open the interactive Vitest UI:

```bash
nx run <PROJECT_NAME>:test-ui
```

---

## Step 9 — Commit

```bash
git add $ARGUMENTS
git commit -m "chore(<PROJECT_NAME>): add vitest setup with analogjs"
```

---

## Quick reference — key packages

| Package | Purpose |
|---|---|
| `@analogjs/vite-plugin-angular` | Vite plugin that compiles Angular templates/decorators |
| `@analogjs/vitest-angular` | Angular-specific Vitest setup helpers (`setupTestBed`, `setup-zone`, `setup-snapshots`) |
| `@nx/vitest` | Nx executor (`@nx/vitest:test`) wrapping Vitest CLI |
| `@nx/vite` | Nx Vite plugins (`nxViteTsPaths`, `nxCopyAssetsPlugin`) |
| `@vitest/coverage-v8` | V8-based coverage provider |
| `@vitest/ui` | Browser-based Vitest UI dashboard |
| `vitest` | Test runner |
| `jsdom` | DOM environment for unit tests |

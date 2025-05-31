---
authors:
  - Anton Petrov
status: note
tags:
  - preact
  - storybook
  - vite
---

For smaller projects, such as widgets for admin panels or embeddable components like custom date pickers or selects, it is often reasonable to use a more atomic framework that provides sufficient reactivity without the full overhead of React. One such option is Preact. It takes approximately 3 KB on load and offers a React-like API for building isolated components.

To simplify development and enable isolated component previews, Storybook can be used as a UI kit tool. Combined together, this setup uses Preact with Storybook and Vite under the hood.

This note provides a quick guide for setting up the environment from scratch.

## Dependencies

- Yarn 3+
- Preact
- Storybook
- Vite

## Initialization

In this section, the project is initialized using CLI tools and all required dependencies are installed.

### Node modules prerequisites

Before starting, note that Yarn PnP may not be compatible with many LSP servers, including `typescript-language-server`, since PnP bypasses node_modules and uses an isolated resolution mechanism that LSP servers often don't recognize. To avoid these issues and stick with the familiar node_modules convention, configure Yarn to use the node-modules linker:

```bash
yarn config set --home nodeLinker node-modules
```

### Create project

Initialize the project with Vite:

```bash
yarn create vite project --template preact-ts
```

This installs all required addons and presets for Preact with Vite.

Next, move inside the project root and set up Storybook:

```bash
cd project/
yarn create storybook
```

Follow the CLI instructions. After configuration completes, Storybook will attempt to launch but will fail with the following errors:

```bash
React is not defined
```

## The Error Fix

These errors originate from Vite’s default JSX handling, not from Storybook itself.

Vite applies automatic JSX transformation, assuming React is available at runtime. Since React is not installed, Vite throws a `ReferenceError`.

To fix this, Vite must be configured to handle JSX properly.

First, install `@vitejs/plugin-react` as a development dependency:

```bash
yarn add -D @vitejs/plugin-react
```

Then create or update `vite.config.js`:

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
});
```

This ensures Vite handles JSX transformations correctly while using Preact components.

### Broken aliasing

During Storybook startup, we may encounter the following error:

```bash
Error: Missing "./compat/test-utils" specifier in "preact" package
```

This issue is related to Vite's module resolution. Vite (and Storybook) attempt to resolve React imports, but sometimes do not fully honor `@vitejs/plugin-preact` or encounter gaps in subpath aliasing. To resolve this, we may need to define manual aliases in our Vite config:

```javascript
export default defineConfig({
  ...
  resolve: {
    alias: {
      react: "preact/compat",
      "react-dom/test-utils": "preact/test-utils",
      "react-dom": "preact/compat",
      "react/jsx-runtime": "preact/jsx-runtime",
    },
  },
});

```

This ensures compatibility with packages expecting React-like imports while using Preact under the hood.

## Run Storybook

Start Storybook to verify the setup works as expected:

```bash
yarn storybook
```

## References

- [Storybook for Preact & Vite (storybook.js.org)](https://storybook.js.org/docs/get-started/frameworks/preact-vite?renderer=preact)

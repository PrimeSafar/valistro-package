# AGENTS.md — AI Coding Agent Instructions for `valistro`

This repository contains **`valistro`**, a private Astro component toolkit providing layout (`BaseLayout`) and metadata (`SEO`) components. All AI coding agents working in this repository MUST follow the rules below.

---

## Core Architecture & Language Rules

1. **Do NOT use TypeScript.**
   - Write **pure JavaScript (`.js`)** and native **`.astro`** files only.
   - Never add `tsconfig.json`, `.ts` files, `interface Props`, or TypeScript type annotations inside `.astro` frontmatter fences.
   - Read component props directly from `Astro.props` using standard JavaScript destructuring and default values (e.g., `const { title = "Default" } = Astro.props;`).

2. **`index.js` is the single package entry point.**
   - Every public component created in `src/` (such as `src/components/*.astro` or `src/layouts/*.astro`) **MUST** be exported from root `index.js`.
   - Use named re-exports so consumers can import via `import { ComponentName } from "valistro"`:
     ```js
     export { default as BaseLayout } from './src/layouts/BaseLayout.astro';
     export { default as SEO } from './src/components/SEO.astro';
     ```

3. **Always use absolute URLs for SEO tags.**
   - Canonical URLs (`<link rel="canonical">`), Open Graph URLs (`og:url`), and Open Graph images (`og:image`) **MUST** resolve to full, absolute URLs (`https://...`).
   - Always construct URLs using `new URL(path, siteUrl).href` where `siteUrl` derives from `Astro.site` (with a safe fallback when `Astro.site` is undefined). Never output relative paths in `canonical`, `og:url`, or `og:image`.

4. **Use the `npm link` workflow for local testing.**
   - This package is private and tested locally across consuming Astro projects via `npm link` (run `npm link` in this repo, then `npm link valistro` in the consuming project).
   - Do not publish to the public npm registry unless explicitly instructed.
   - Remember that creating **new files** in this package while a consuming Astro dev server is running requires restarting the dev server for Vite/Astro to pick up the new module paths.

---

## Development Server Management

When starting the Astro dev server in this workspace, use background mode:

```bash
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

---

## Astro Documentation References

Full documentation: https://docs.astro.build

Consult these guides when working on related tasks:
- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)

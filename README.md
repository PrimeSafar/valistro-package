<p align="center">
  <img src="./logo.svg" alt="valistro logo" width="560" />
</p>

# valistro

**valistro** is a lightweight, zero-TypeScript Astro component toolkit designed to standardize page layouts and SEO metadata across Astro projects. Built with pure JavaScript and native `.astro` components, it provides a clean, reusable foundation for document structure, canonical URLs, robots directives, Open Graph tags, and article metadata.

---

## Included Components

- **`BaseLayout`** (`src/layouts/BaseLayout.astro`) — Provides the core HTML5 document shell (`<!DOCTYPE html>`, `<html>`, `<head>`, and `<body>`), renders a default `<slot />` for page content, and automatically forwards all SEO props to the `<SEO />` component.
- **`SEO`** (`src/components/SEO.astro`) — Generates `<title>`, `<meta name="description">`, canonical `<link>`, `robots` directives (`index, follow` / `noindex, nofollow`), Open Graph metadata (`og:*`), and conditional article tags (`article:published_time`, `article:modified_time`, `article:author`, `article:tag`).

---

## Installation & Local Development

Because `valistro` is a private package developed locally, use `npm link` to symlink it into your consuming Astro projects without publishing to the npm registry.

### 1. Register `valistro` globally on your machine

From the root of this repository (`valistro`):

```bash
npm link
```

### 2. Link `valistro` inside your Astro project

Navigate to the Astro project where you want to use `valistro` and run:

```bash
npm link valistro
```

### 3. Import components

Once linked, import `BaseLayout` and `SEO` directly from `"valistro"`:

```js
import { BaseLayout, SEO } from "valistro";
```

---

## Usage Example

Below is an example of an Astro page (`src/pages/blog/astro-toolkit.astro`) using `<BaseLayout>`:

```astro
---
import { BaseLayout } from "valistro";
---

<BaseLayout
  title="Building Reusable Astro Components"
  description="Learn how to share layouts and SEO components across Astro projects with valistro."
  image="/images/astro-toolkit-cover.png"
  article={true}
  publishTime="2026-10-25T00:00:00Z"
  modifiedTime="2026-10-25T12:00:00Z"
  author="Safar"
  tags={["astro", "javascript", "seo"]}
  noindex={false}
>
  <main>
    <article>
      <h1>Building Reusable Astro Components</h1>
      <p>This page's HTML shell and Open Graph metadata are powered by valistro.</p>
    </article>
  </main>
</BaseLayout>
```

### Supported Props (`BaseLayout` & `SEO`)

| Prop | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `title` | `string` | `"Safar's Dev Blog"` | Page title used in `<title>` and `og:title`. |
| `description` | `string` | `""` | Page summary used in `<meta name="description">` and `og:description`. |
| `image` | `string` | `"/og-default.png"` | Path or URL resolved against `Astro.site` for `og:image`. |
| `article` | `boolean` | `false` | Switches `og:type` to `"article"` and enables `article:*` meta tags. |
| `publishTime` | `string` | `undefined` | ISO 8601 publication timestamp (`article:published_time`). |
| `modifiedTime` | `string` | `undefined` | ISO 8601 last-modified timestamp (`article:modified_time`). |
| `author` | `string` | `undefined` | Article author (`article:author`). |
| `tags` | `string[]` | `[]` | Array of topic tags rendered as `<meta property="article:tag">`. |
| `noindex` | `boolean` | `false` | When `true`, outputs `<meta name="robots" content="noindex, nofollow">`. |

> **Tip:** Set `site: "https://yourdomain.com"` in your consuming project's `astro.config.mjs` so `SEO` can construct accurate absolute URLs for `canonical` and `og:image`.

---

## How to Add a New Component

1. **Create the component in `src/`**  
   Add your `.astro` component inside `src/components/` (or `src/layouts/` for layouts). Use plain JavaScript in the frontmatter fence (`---`):

   ```astro
   ---
   // src/components/Button.astro
   const { href = "#", label = "Click me" } = Astro.props;
   ---

   <a class="btn" href={href}>
     <slot>{label}</slot>
   </a>
   ```

2. **Export it in `index.js`**  
   Open `index.js` at the root of the package and re-export your new component:

   ```js
   export { default as BaseLayout } from './src/layouts/BaseLayout.astro';
   export { default as SEO } from './src/components/SEO.astro';
   export { default as Button } from './src/components/Button.astro';
   ```

3. **Import it in your consuming project**  
   ```js
   import { BaseLayout, Button } from "valistro";
   ```

---

## How to Rename the Package

If you want to rename `valistro` to a different package name (for example, `my-new-toolkit`), follow these steps in order:

1. **Unlink the current package from your consuming Astro project(s):**
   ```bash
   cd /path/to/your-astro-app
   npm unlink valistro
   ```

2. **Unlink the global symlink from this package directory:**
   ```bash
   cd /path/to/my-astro-toolkit
   npm unlink -g valistro
   ```

3. **Update `package.json`:**  
   Change the `"name"` field in `package.json`:
   ```json
   {
     "name": "my-new-toolkit",
     "version": "0.0.1"
   }
   ```

4. **Re-link under the new name:**
   ```bash
   # In this toolkit repository:
   npm link

   # In your consuming Astro project:
   cd /path/to/your-astro-app
   npm link my-new-toolkit
   ```

5. **Update your imports:**  
   Replace `from "valistro"` with `from "my-new-toolkit"` across your consuming project.

---

## Troubleshooting

### Newly created files in `valistro` are not detected or cause import errors
Vite and the Astro dev server cache module resolution graphs when starting up. While editing an *existing* component in `valistro` triggers Hot Module Replacement (HMR) as expected, **creating a brand-new file in the package while the dev server is running requires a dev server restart**.

Stop and restart the Astro dev server in your consuming project whenever you:
- Add a new `.astro` or `.js` file inside `src/`
- Add a new export in `index.js`
- Re-link the package via `npm link`

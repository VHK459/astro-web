# Bizarre Belt / Kadmium

A personal portfolio and writing site built with [Astro](https://astro.build/).



## Project Structure

```text
bizarre-belt/
|-- public/
|-- src/
|   |-- components/
|   |-- image/
|   |-- layouts/
|   |-- pages/
|   |   `-- posts/
|   |-- scripts/
|   `-- styles/
|-- astro.config.mjs
|-- package.json
|-- package-lock.json
`-- tsconfig.json
```

## Getting Started

Install dependencies from the project root:

```sh
npm install
```

Start the local development server:

```sh
npm run dev
```

Astro will print a local URL, usually:

```text
http://localhost:4321/
```

Build the production site:

```sh
npm run build
```

Preview the production build locally:

```sh
npm run preview
```


## Recommended frontmatter fields:

| Field | Required | Used by |
| --- | --- | --- |
| `layout` | Yes for the current post design | Tells Astro to render the post with `MarkdownPostLayout.astro`. |
| `title` | Yes | Blog card, post heading, page title, image alt fallback. |
| `pubDate` | Yes | Post header date in `MarkdownPostLayout.astro`. |
| `description` | Recommended | Blog card excerpt and post subtitle. |
| `author` | Recommended | Post byline. |
| `image` | Recommended | Blog card image and post banner. |
| `imageAlt` | Optional | Accessible alt text for the banner image. |
| `tags` | Optional | Metadata for future filtering or taxonomy. |

The Markdown pipeline supports math expressions through `remark-math` and
`rehype-katex`. KaTeX styles are loaded in `BaseLayout.astro`.


```

## Layouts

### `BaseLayout.astro`

The shared site shell. It includes:

- global CSS import
- sticky header
- font preconnects and Google Fonts
- favicon
- KaTeX stylesheet
- initial theme script that reads `localStorage.theme` or system preference
- shared page padding and decorative side rails
- menu script import

Most pages should use this layout directly.

### `MarkdownPostLayout.astro`

The blog post layout. It wraps Markdown posts with:

- banner image
- author/date byline
- title and description
- styled prose body
- back link to `/blog`

Post-specific prose styling is split between this layout and
`src/styles/markdown.css`.

## Components

| Component | Purpose |
| --- | --- |
| `Header.astro` | Sticky site header wrapper. |
| `Navigation.astro` | Primary links for Kadmium, About, and Blog. |
| `AboutGrid.astro` | Structured about/profile content grid. |
| `Menu.astro` | Accessible menu button scaffold. |
| `Footer.astro` | Social footer component, currently not rendered in `BaseLayout`. |
| `Social.astro` | Simple social link component used by `Footer`. |
| `ShaderBackground.astro` | Reserved component for shader/visual background work. |

## Styling System

Global styles are organized in:

```text
src/styles/defaults.css
src/styles/global.css
src/styles/markdown.css
```

`defaults.css` defines the design tokens:

- background, surface, and text colors
- border and grid colors
- accent colors
- code colors
- shadows
- dark color scheme

`global.css` applies:

- base typography
- body grid/dot background
- page side rails
- sticky header styling
- navigation styling
- responsive behavior

`markdown.css` is intended for long-form Markdown content.

## Assets

Use `src/image/` for assets imported into Astro components with the Astro asset
pipeline.

Use `public/` for static files that should be served directly from the site
root. For example:

```text
public/favicon.svg to /favicon.svg
public/asb.png     to /asb.png
```

If an image is referenced from CSS as `url("asb.png")`, make sure the file is
available from the expected public path or update the CSS path.

## Configuration

Astro configuration is in `astro.config.mjs`.

Current configuration:

- enables React integration
- enables Markdown math parsing
- renders math using KaTeX-compatible HTML

```js
export default defineConfig({
  integrations: [react()],
  markdown: {
    remarkPlugins: [remarkMath],
    rehypePlugins: [rehypeKatex],
  },
});
```

## Deployment

This is a static Astro site. A typical deployment flow is:

```sh
npm install
npm run build
```

Then deploy the generated `dist/` directory to a static host such as Netlify,
Vercel, GitHub Pages, Cloudflare Pages, or any server that can serve static
files.

Before deploying, run:

```sh
npm run preview
```

Use the preview server to check routes, images, Markdown posts, math rendering,
and responsive layouts.

## Customization Notes

- Edit the main landing copy in `src/pages/index.astro`.
- Edit profile, project, publication, and research text in
  `src/components/AboutGrid.astro`.
- Edit navigation links in `src/components/Navigation.astro`.
- Add or remove posts in `src/pages/posts/`.
- Change global colors in `src/styles/defaults.css`.
- Change overall layout chrome in `src/layouts/BaseLayout.astro`.
- Change blog post rendering in `src/layouts/MarkdownPostLayout.astro`.

## Known Notes

- `Footer.astro` exists but is currently commented out in `BaseLayout.astro`.
- `Menu.astro` and `src/scripts/menu.js` provide a menu-button scaffold, but the
  active header currently renders `Navigation.astro` directly.
- `post-2.md` still contains starter-style placeholder content and can be
  replaced with real writing.
- The project has Three.js installed, which is useful for future shader or
  interactive background work.

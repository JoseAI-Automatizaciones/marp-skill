# Marp Themes & Styling — Complete Reference

## Table of Contents
1. [Built-in Themes](#built-in-themes)
2. [Theme Classes and Variants](#theme-classes-and-variants)
3. [CSS Variables per Theme](#css-variables-per-theme)
4. [Size Presets](#size-presets)
5. [Creating Custom Themes](#creating-custom-themes)
6. [Theme CSS Rules](#theme-css-rules)
7. [Pagination Styling](#pagination-styling)
8. [Header and Footer Styling](#header-and-footer-styling)
9. [Theme Metadata](#theme-metadata)
10. [Importing and Extending Themes](#importing-and-extending-themes)
11. [Community Themes](#community-themes)

---

## Built-in Themes

Marp Core provides three built-in themes. All support `16:9` (1280×720, default) and `4:3` (960×720) sizes.

### `default`
GitHub Markdown style, optimized for slides. Content vertically centered. Clean and professional.
```markdown
<!-- theme: default -->
```

### `gaia`
Inspired by classic Marp. Uses Lato + Roboto Mono fonts. Four color schemes available.
```markdown
<!-- theme: gaia -->
```

### `uncover`
Simple, minimal, modern. Inspired by reveal.js. Centered text by default.
```markdown
<!-- theme: uncover -->
```

---

## Theme Classes and Variants

### `invert` class (all themes)
Inverted color scheme (dark ↔ light):
```markdown
<!-- class: invert -->
```

### `lead` class (gaia only)
Centers content like a title slide:
```markdown
<!-- _class: lead -->
```

### `gaia` class (gaia theme only)
Alternate color scheme (warm gold/orange):
```markdown
<!-- class: gaia -->
```

### Combining classes
```markdown
---
theme: gaia
class:
  - lead
  - invert
---
```
Or space-separated: `<!-- class: lead invert -->`

---

## CSS Variables per Theme

### `default` Theme Variables
Based on `github-markdown-css`. Key variables:
```html
<style>
:root {
  --color-fg-default: #24292f;
  --color-canvas-default: #ffffff;
  --color-accent-fg: #0969da;
  --h1-color: #1f2328;
  --heading-strong-color: inherit;
  /* Many more from github-markdown-css */
}
</style>
```

### `gaia` Theme Variables
```html
<style>
:root {
  --color-background: #fff8e1;
  --color-foreground: #455a64;
  --color-highlight: #0288d1;
  --color-dimmed: #888;
}
</style>
```

### `uncover` Theme Variables
```html
<style>
:root {
  --color-background: #fdfcff;
  --color-background-code: #f0f0f2;
  --color-background-paginate: rgba(128, 128, 128, 0.05);
  --color-foreground: #202228;
  --color-highlight: #009dd5;
  --color-highlight-hover: #207090;
  --color-highlight-heading: #40b0e0;
  --color-header: rgba(32, 34, 40, 0.4);
  --color-header-shadow: rgba(253, 252, 255, 0.8);
}
</style>
```

---

## Size Presets

Built-in themes provide two presets:
```markdown
<!-- size: 16:9 -->   <!-- 1280×720 (default) -->
<!-- size: 4:3 -->    <!-- 960×720 -->
```

The `size` directive is a Marp Core extension (not available in raw Marpit).

---

## Creating Custom Themes

### Minimal Custom Theme
```css
/* @theme my-theme */

section {
  width: 1280px;
  height: 720px;
  font-size: 40px;
  padding: 40px;
  background-color: #fff;
  color: #333;
}

h1 {
  font-size: 60px;
  color: #09c;
}

h2 {
  font-size: 50px;
}
```

**Required**: The `/* @theme name */` comment. Without it, Marpit won't recognize the CSS as a theme.

### Using in CLI
```bash
marp --theme my-theme.css slide.md
```

### Using in VS Code
`.vscode/settings.json`:
```json
{
  "markdown.marp.themes": [
    "./themes/my-theme.css"
  ]
}
```

Then in Markdown:
```markdown
---
marp: true
theme: my-theme
---
```

---

## Theme CSS Rules

### `section` is the slide element
Every slide renders as a `<section>` element. Style it as your viewport:
```css
section {
  width: 1280px;    /* Slide width */
  height: 720px;    /* Slide height */
  padding: 40px;    /* Content padding */
  font-size: 30px;  /* Base font size */
}
```

### `:root` pseudo-class = `section`
In Marp theme context, `:root` maps to `<section>`, not `<html>`. It has higher specificity than `section`.

### `rem` units auto-transform
`rem` units are calculated relative to the slide's `<section>` font-size, not the page's `<html>`.

### Slide size must be static absolute units
Width/height only accept: `px`, `cm`, `in`, `mm`, `pc`, `pt`, `Q`. No `%`, `vw`, `vh`, etc.

### One size per theme
The slide size is determined per theme and cannot be changed via inline styles or CSS custom properties. Use `@size` metadata for presets.

---

## Pagination Styling

Pagination renders via `section::after` pseudo-element:
```css
section::after {
  font-weight: bold;
  font-size: 0.6em;
  /* Content MUST include attr(data-marpit-pagination) */
  content: attr(data-marpit-pagination) ' / ' attr(data-marpit-pagination-total);
}
```

Available attributes:
- `data-marpit-pagination` — Current page number
- `data-marpit-pagination-total` — Total page count

**Critical**: Theme CSS must include `attr(data-marpit-pagination)` in the `content` declaration or Marpit will ignore it entirely.

---

## Header and Footer Styling

Default: no styling. To position at margins:
```css
section {
  padding: 50px;
}

header, footer {
  position: absolute;
  left: 50px;
  right: 50px;
  height: 20px;
}

header {
  top: 30px;
}

footer {
  bottom: 30px;
}
```

---

## Theme Metadata

Marp Core themes can define extra metadata via CSS comments:

### `@auto-scaling`
Enable auto-scaling features:
```css
/*
 * @theme my-theme
 * @auto-scaling true
 */
```

Options: `true` (all), `fittingHeader`, `math`, `code`, or comma-separated combination:
```css
/* @auto-scaling fittingHeader,code */
```

### `@size`
Define size presets:
```css
/*
 * @theme my-theme
 * @size 16:9 1280px 720px
 * @size 4:3 960px 720px
 * @size 4K 3840px 2160px
 */
```

Disable inherited preset: `@size 4:3 false`

---

## Importing and Extending Themes

### `@import` rule
Import and extend another registered theme:
```css
/* @theme dark-gaia */
@import 'gaia';

section {
  background-color: #222;
  color: #eee;
}
```
The imported theme must be registered in the theme set first.

### `@import-theme` rule
Alternative for Sass/preprocessor compatibility:
```scss
/* @theme custom */
@import-theme 'default';

$accent: #f80;
section { color: $accent; }
```

Import order: `@import` processed before `@import-theme`. Both insert content at the beginning of CSS.

---

## Tweak Theme via Markdown

### `<style>` element (global)
```markdown
<style>
section { background: yellow; }
</style>
```
Merged into output CSS, not visible in rendered HTML.

### `<style scoped>` (single slide)
```markdown
<style scoped>
h1 { color: red; }
</style>

# Only this heading is red
```

### `style` global directive (preferred)
```markdown
---
theme: default
style: |
  section {
    background: #fef;
  }
---
```
Better compatibility with non-Marp Markdown editors.

---

## Community Themes

Notable community themes (install via CSS file):

| Theme | Style | Source |
|-------|-------|--------|
| **Beam** | LaTeX Beamer style | marp-community-themes |
| **Dracula** | Dark Dracula colors | draculatheme.com/marp |
| **Neobeam** | Modern Beamer | github.com/mikael-ros/neobeam |
| **Nord** | Nord color palette | github.com/mastern2k3/marpit-nord-theme |
| **Rosé Pine** | Rosé Pine palettes | github.com/rainbowflesh/Rose-Pine-For-Marp |
| **marpstyle** | Multiple elegant styles | github.com/cunhapaulo/marpstyle |
| **Wave** | Modern wave design | github.com/JuliusWiedemann/MarpThemeWave |
| **marp-theme-academic** | Academic Beamer style | github.com/kaisugi/marp-theme-academic |
| **Cybertopia** | Dark cyberpunk | github.com/noraj/cybertopia-marp |
| **Graph Paper** | Graph paper background | marp-community-themes |
| **Awesome Marp** | Many useful layouts (CN) | github.com/favourhong/Awesome-Marp |
| **teaching-theme** | Designed for teaching (FR) | github.com/eyssette/teaching-theme-for-marp |

Curation sites:
- **Marp Community Themes**: https://rnd195.github.io/marp-community-themes/
- **Marp Template Library**: https://yoanbernabeu.github.io/MARP-Template-Library/

### Using a Community Theme

1. Download the CSS file
2. Pass via CLI: `marp --theme ./path/to/theme.css slide.md`
3. Or register in VS Code settings: `"markdown.marp.themes": ["./themes/beam.css"]`
4. Or add to theme set in CLI config:
```yaml
# .marprc.yml
themeSet: ./themes
```

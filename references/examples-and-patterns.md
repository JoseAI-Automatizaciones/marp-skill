# Marp Examples & Patterns — Complete Reference

## Table of Contents
1. [Complete Slide Deck Examples](#complete-slide-deck-examples)
2. [Title Slide Patterns](#title-slide-patterns)
3. [Content Slide Patterns](#content-slide-patterns)
4. [Image and Background Patterns](#image-and-background-patterns)
5. [Multi-Column Layouts](#multi-column-layouts)
6. [Code Presentation Patterns](#code-presentation-patterns)
7. [Math and Academic Patterns](#math-and-academic-patterns)
8. [Deployment and CI/CD](#deployment-and-cicd)
9. [Project Setup Patterns](#project-setup-patterns)
10. [Plugin Patterns](#plugin-patterns)
11. [Community Integrations](#community-integrations)
12. [Tips and Best Practices](#tips-and-best-practices)

---

## Complete Slide Deck Examples

### Professional Presentation (Gaia Theme)

```markdown
---
marp: true
theme: gaia
paginate: true
_paginate: skip
header: 'Company Name'
footer: '© 2025 Company Name'
transition: fade
style: |
  section.lead h1 {
    text-shadow: 0 2px 4px rgba(0,0,0,0.2);
  }
---

<!-- _class: lead -->
<!-- _header: '' -->
<!-- _footer: '' -->

# <!-- fit --> Product Launch 2025

### Presented by Jane Smith

![bg opacity:.3](https://picsum.photos/1280/720?image=1069)

---

# Agenda

1. Market Overview
2. Product Features
3. Go-to-Market Strategy
4. Timeline & Milestones

---

## Market Overview

- Total addressable market: **$50B**
- Growth rate: **15% YoY**
- Key competitors: 3 major players

> "The market is ripe for disruption" — Industry Report

---

![bg right:45%](https://picsum.photos/720?image=180)

## Product Features

* Real-time collaboration
* AI-powered insights
* Enterprise-grade security
* Cross-platform support

---

<!-- _class: lead invert -->

# <!-- fit --> Questions?

Contact: jane@company.com
```

### Technical Talk (Default Theme)

```markdown
---
marp: true
theme: default
paginate: true
math: katex
---

# Understanding WebAssembly

### A Deep Dive into WASM

---

## What is WebAssembly?

A binary instruction format for a stack-based virtual machine.

```rust
#[no_mangle]
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

---

## Performance Comparison

| Language | Time (ms) | Relative |
|----------|-----------|----------|
| C/WASM   | 12        | 1.0x     |
| Rust/WASM| 13        | 1.08x    |
| JS       | 45        | 3.75x    |
| Python   | 1200      | 100x     |

---

## Memory Model

The linear memory model can be described as:

$$
M[i] = \begin{cases}
  \text{byte}_i & \text{if } 0 \leq i < |M| \\
  \text{trap} & \text{otherwise}
\end{cases}
$$

Where $|M|$ is the current memory size in bytes.

---

<!-- _backgroundColor: "#1a1a2e" -->
<!-- _color: "#e0e0e0" -->

## Architecture

```
┌─────────────┐     ┌──────────────┐
│  .wasm file │────▶│  JS Runtime  │
└─────────────┘     │  ┌────────┐  │
                    │  │ Module │  │
                    │  └────────┘  │
                    └──────────────┘
```
```

### Minimal Presentation (Uncover Theme)

```markdown
---
marp: true
theme: uncover
paginate: true
---

# Clean & Simple

A minimal deck with the uncover theme.

---

## Key Point

Less is more.

---

<!-- _class: invert -->

# Thank You

feedback@example.com
```

---

## Title Slide Patterns

### Centered with Background Image
```markdown
<!-- _class: lead -->
<!-- _paginate: skip -->

![bg opacity:.4 blur:2px](hero-image.jpg)

# <!-- fit --> Main Title

## Subtitle text

---
```

### Split with Author Photo
```markdown
<!-- _paginate: skip -->

![bg left:40%](author-photo.jpg)

# Presentation Title

**Author Name**
Position, Company

Date: January 2025

---
```

### Gradient Background
```markdown
<!-- _paginate: skip -->
<!-- _backgroundImage: "linear-gradient(135deg, #667eea 0%, #764ba2 100%)" -->
<!-- _color: white -->

# <!-- fit --> Bold Statement Title

---
```

---

## Content Slide Patterns

### Two-Column with Background Split
```markdown
![bg right:50% contain](chart.png)

## Key Metrics

- Revenue: $10M (+25%)
- Users: 50K (+40%)
- NPS Score: 72

---
```

### Quote Slide
```markdown
<!-- _class: lead -->

> "The best way to predict the future is to invent it."
>
> — Alan Kay

---
```

### Comparison Slide
```markdown
## Before vs After

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| Speed  | 200ms  | 50ms  | -75%   |
| Size   | 2MB    | 400KB | -80%   |
| Score  | 65     | 95    | +46%   |

---
```

---

## Image and Background Patterns

### Full Bleed Background
```markdown
![bg](landscape.jpg)

---
```

### Dimmed Background with Text
```markdown
![bg brightness:.4](photo.jpg)

# <!-- fit --> White text on dark image

---
```

### Multiple Backgrounds (Gallery)
```markdown
![bg](photo1.jpg)
![bg](photo2.jpg)
![bg](photo3.jpg)

---
```

### Vertical Split Backgrounds
```markdown
![bg vertical](top-image.jpg)
![bg](bottom-image.jpg)

---
```

### Background with Filter Chain
```markdown
![bg sepia:.8 blur:2px opacity:.6](vintage-photo.jpg)

# Styled Background

---
```

### Asymmetric Split
```markdown
![bg left:30%](narrow-image.jpg)

## Content Area (70%)

Lots of space for content when the image is narrow.

---
```

---

## Multi-Column Layouts

### Using CSS Grid (via style directive)
```markdown
---
marp: true
theme: default
style: |
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1em;
  }
---

## Two Columns

<div class="columns">
<div>

### Left Column
- Point A
- Point B

</div>
<div>

### Right Column
- Point C
- Point D

</div>
</div>

---
```

**Note**: Requires `--html` flag in CLI or `markdown.marp.html: "all"` in VS Code for `<div>` rendering.

### Using Background Split as Columns
```markdown
![bg left:50% contain](diagram.svg)

## Right side content

This achieves a visual two-column layout using split backgrounds without needing HTML.

---
```

---

## Code Presentation Patterns

### Code Block (auto-shrinks if theme supports)
````markdown
## API Example

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```

---
````

### Before/After Code
````markdown
## Refactoring

**Before:**
```javascript
function getData(url, callback) {
  fetch(url).then(r => r.json()).then(callback);
}
```

**After:**
```javascript
async function getData(url) {
  const response = await fetch(url);
  return response.json();
}
```

---
````

---

## Math and Academic Patterns

### Theorem Slide
```markdown
---
marp: true
math: katex
theme: default
---

## Theorem: Central Limit Theorem

Let $X_1, X_2, \ldots, X_n$ be i.i.d. random variables with mean $\mu$ and variance $\sigma^2$. Then:

$$
\frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}} \xrightarrow{d} \mathcal{N}(0, 1)
$$

as $n \to \infty$.

---
```

---

## Deployment and CI/CD

### GitHub Pages with GitHub Actions

`.github/workflows/deploy.yml`:
```yaml
name: Deploy Marp to GitHub Pages
on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npx @marp-team/marp-cli@latest slides.md -o public/index.html
      - uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
    steps:
      - uses: actions/deploy-pages@v4
```

### Netlify
`netlify.toml`:
```toml
[build]
  command = "npx @marp-team/marp-cli@latest slides.md -o public/index.html"
  publish = "public"
```

### Vercel
`package.json`:
```json
{
  "scripts": {
    "build": "npx @marp-team/marp-cli@latest slides.md -o public/index.html"
  }
}
```

---

## Project Setup Patterns

### Minimal Project
```
my-deck/
├── slides.md
├── package.json
└── themes/        (optional)
    └── custom.css
```

`package.json`:
```json
{
  "scripts": {
    "dev": "marp -w -p slides.md",
    "build:html": "marp slides.md -o dist/index.html",
    "build:pdf": "marp slides.md -o dist/slides.pdf",
    "build:pptx": "marp slides.md -o dist/slides.pptx"
  },
  "devDependencies": {
    "@marp-team/marp-cli": "^4"
  }
}
```

### Full Project with Config
```
my-deck/
├── slides/
│   ├── intro.md
│   └── main.md
├── themes/
│   └── company.css
├── assets/
│   ├── logo.png
│   └── photos/
├── .vscode/
│   └── settings.json
├── marp.config.mjs
└── package.json
```

`marp.config.mjs`:
```javascript
/** @type {import('@marp-team/marp-cli').Config} */
export default {
  inputDir: './slides',
  output: './dist',
  themeSet: './themes',
  allowLocalFiles: true,
  pdf: false,
  html: true,
}
```

`.vscode/settings.json`:
```json
{
  "markdown.marp.themes": ["./themes/company.css"],
  "markdown.marp.html": "all"
}
```

---

## Plugin Patterns

### markdown-it-container (Multi-column)
```javascript
// engine.mjs
import markdownItContainer from 'markdown-it-container'

export default ({ marp }) => marp.use(markdownItContainer, 'columns')
```

### markdown-it-mark (Highlight)
```javascript
// engine.mjs
import markdownItMark from 'markdown-it-mark'

export default ({ marp }) => marp.use(markdownItMark)
// Converts ==highlighted== to <mark>highlighted</mark>
```

### Compatible markdown-it Plugins
- **@kazumatu981/markdown-it-kroki**: Kroki diagram embedding
- **@kazumatu981/markdown-it-fontawesome**: Font Awesome icons
- Any [markdown-it plugin on npm](https://www.npmjs.com/search?q=keywords:markdown-it-plugin) may work (some may conflict with Marp's built-in extensions)

---

## Community Integrations

| Tool | Description |
|------|-------------|
| **Marp Slides for Obsidian** | Marp plugin for Obsidian |
| **Obsidian Marp Plugin** | Alternative Obsidian integration |
| **marpyter** | JupyterLab extension |
| **GROWI** | Markdown collaboration with Marp support |

### Templates and Examples
- **marp-cli-example**: GitHub Pages/Netlify/Vercel automation (github.com/yhatt/marp-cli-example)
- **marp-to-pages**: Deploy to GitHub Pages template (github.com/ralexander-phi/marp-to-pages)
- **Publish multiple decks**: Multi-presentation site with pretty URLs (github.com/pages-demo/marp)

---

## Tips and Best Practices

### Content Per Slide
- One main idea per slide
- Maximum 5-7 bullet points
- Use `<!-- fit -->` for impact headings

### Images
- Use `![bg]()` for backgrounds, not CSS directives (except for gradients/colors)
- `contain` for diagrams, `cover` for photos
- Split backgrounds for side-by-side layouts

### Theming
- Start with a built-in theme, customize via `<style>` or `style:` directive
- Create custom theme CSS only when significant changes are needed
- Use CSS variables for quick color changes

### Pagination
- Skip on title slides: `_paginate: skip`
- Use `hold` for appendix or bonus slides you don't want counted

### Transitions
- Use sparingly (1-2 types per deck)
- `fade` is the safest default
- Test in browser (transitions only work in bespoke HTML output)

### Export
- PDF for sharing/printing
- PPTX for editing in PowerPoint/Keynote (reduced fidelity)
- HTML for interactive web presentations
- PNG for social media/preview images

### Compatibility
- `marp: true` is only needed for VS Code
- Marp Markdown is valid CommonMark — it looks fine in any Markdown viewer
- Directives in HTML comments are invisible in other editors
- `headingDivider` makes Markdown beautiful even without `---` separators

### Performance
- KaTeX renders faster than MathJax for many formulas
- Use `--image-scale 2` for high-DPI presentation images
- PPTX uses scale factor 2 by default for sharp slides

### Presenter Notes
- Write notes in HTML comments (not directives)
- Export notes separately: `marp --notes slide.md`
- View in presenter view: press P in bespoke HTML
- Notes included in PDF with `--pdf-notes`

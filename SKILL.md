---
name: marp
description: >
  Create, edit, theme, and export professional slide deck presentations using Marp (Markdown Presentation Ecosystem).
  Use this skill whenever the user wants to create slides, presentations, or decks from Markdown using Marp, Marpit,
  or any Marp-related tool. Also trigger when the user mentions: slide decks, Marp CLI, Marp for VS Code, Marpit
  directives, presentation Markdown, converting Markdown to PDF/PPTX/HTML slides, slide transitions, presenter notes,
  fitting headers, split backgrounds, or any Marp theme (default, gaia, uncover). This skill covers the ENTIRE Marp
  ecosystem: syntax, directives, image syntax, themes (built-in and custom), CLI conversion, VS Code integration,
  plugins, transitions, math typesetting, and deployment. Even if the user just says "make me a presentation" or
  "slide deck" without mentioning Marp explicitly, use this skill to produce Marp Markdown output.
---

# Marp — Markdown Presentation Ecosystem Skill

## What is Marp?

Marp converts plain Markdown into slide decks (HTML, PDF, PPTX, images). The ecosystem has four layers:

1. **Marpit** — The skinny framework: Markdown parsing, directives, image syntax, theme CSS system
2. **Marp Core** — Extends Marpit with built-in themes, math, emoji, auto-scaling
3. **Marp CLI** — Command-line converter: HTML, PDF, PPTX, PNG/JPEG, server/watch modes, transitions
4. **Marp for VS Code** — Live preview, IntelliSense, export, custom themes in the editor

## Quick Start — Minimal Slide Deck

```markdown
---
marp: true
theme: default
paginate: true
---

# Slide 1 Title

Content here. Slides are separated by `---`.

---

# Slide 2

- Bullet points
- Work normally

---

![bg right:40%](https://picsum.photos/720?image=29)

# Split Background

Content shrinks to the left.
```

Key rules: `marp: true` is required for VS Code. Slides are separated by `---` (horizontal rule). Directives go in front-matter or HTML comments.

## Reference Files — Read Before Generating

This skill uses progressive disclosure. **Always read the relevant reference file before producing output.**

| Task | Reference File |
|------|---------------|
| Writing slide Markdown, using directives, image syntax, backgrounds, fragmented lists | `references/syntax-and-directives.md` |
| Built-in themes, custom theme CSS, CSS variables, `@size`/`@auto-scaling` metadata | `references/themes-and-styling.md` |
| CLI conversion (PDF/PPTX/HTML/images), config files, transitions, engine, Docker, metadata | `references/cli-reference.md` |
| VS Code extension features, settings, export, diagnostics, custom themes in VS Code | `references/vscode-reference.md` |
| Complete working examples, patterns, best practices, deployment, community resources | `references/examples-and-patterns.md` |

## Core Syntax Cheat Sheet (for quick inline answers)

### Slide Separation
```
---
```
An empty line before `---` may be required per CommonMark. Alternatives: `___`, `***`, `- - -`.

### Directives (YAML in front-matter or HTML comments)

**Global** (whole deck): `theme`, `style`, `headingDivider`, `lang`, `size`, `math`
**Local** (current page + following): `paginate`, `header`, `footer`, `class`, `backgroundColor`, `backgroundImage`, `backgroundPosition`, `backgroundRepeat`, `backgroundSize`, `color`, `transition`
**Spot** (single page only): prefix with `_` → `_paginate`, `_class`, `_backgroundColor`, etc.

```markdown
<!-- theme: gaia -->
<!-- _class: lead -->
<!-- paginate: true -->
<!-- _paginate: skip -->
```

### Image Syntax
```markdown
![](image.jpg)                    <!-- inline image -->
![w:200 h:150](image.jpg)        <!-- resized inline -->
![bg](image.jpg)                  <!-- background (cover) -->
![bg contain](image.jpg)         <!-- background fit -->
![bg right:40%](image.jpg)       <!-- split background -->
![bg left](a.jpg)                <!-- split left -->
![bg vertical](a.jpg)            <!-- vertical stack -->
![bg blur:5px](image.jpg)        <!-- with filter -->
![bg opacity:.5](image.jpg)      <!-- with opacity -->
```

### Fitting Header
```markdown
# <!-- fit --> This heading scales to fill the slide
```

### Math (Marp Core)
```markdown
Inline: $ax^2 + bx + c$
Block: $$ \int_0^\infty f(x)\,dx $$
```
Declare library: `math: mathjax` or `math: katex` in front-matter.

### Fragmented Lists (appear one-by-one)
Use `*` for bullets or `1)` for ordered (instead of `-` or `1.`).

### Presenter Notes
```markdown
<!-- This is a presenter note (HTML comment that is NOT a directive) -->
```

### Scoped Style
```markdown
<style scoped>
h1 { color: red; }
</style>
```

### Transitions (CLI bespoke template only)
```markdown
---
transition: fade
---
```
33 built-in: `fade`, `slide`, `cover`, `push`, `pull`, `wipe`, `cube`, `flip`, `zoom`, `drop`, `reveal`, `clockwise`, `counterclockwise`, `coverflow`, `cylinder`, `diamond`, `explode`, `fade-out`, `fall`, `glow`, `implode`, `in-out`, `iris-in`, `iris-out`, `melt`, `overlap`, `pivot`, `rotate`, `star`, `swap`, `swipe`, `swoosh`, `wiper`. Duration: `transition: fade 1s`.

## Workflow for Creating a Marp Deck

1. **Read** `references/syntax-and-directives.md` for full syntax
2. **Choose a theme**: `default`, `gaia`, or `uncover` (read `references/themes-and-styling.md`)
3. **Write the Markdown** using directives and image syntax
4. **Export**: via CLI (`marp deck.md --pdf`) or VS Code export (read `references/cli-reference.md`)

## When Generating Marp Files

- Always include `marp: true` in front-matter (needed for VS Code)
- Always specify `theme:` in front-matter
- Use `paginate: true` unless the user doesn't want page numbers
- Use `_paginate: false` or `_paginate: skip` on title slides
- Put `---` between every slide (with empty lines around it)
- For backgrounds, use `![bg](url)` syntax, NOT CSS background directives (unless gradients)
- For split layouts, use `![bg left]()` or `![bg right:40%]()`
- Save output as `.md` files — they are valid Markdown everywhere

## File Output

When generating a Marp deck for the user:
- Create a `.md` file with proper front-matter
- If the user needs conversion, explain CLI commands or VS Code export
- If the user wants a custom theme, create a separate `.css` file alongside

## Ecosystem Map

```
Marpit (framework)
  └── Marp Core (themes + features)
       ├── Marp CLI (conversion tool)
       │    ├── HTML output (bespoke/bare templates)
       │    ├── PDF output (via browser)
       │    ├── PPTX output (via browser)
       │    ├── PNG/JPEG output (via browser)
       │    └── Server/Watch/Preview modes
       └── Marp for VS Code (editor integration)
            ├── Live preview
            ├── IntelliSense for directives
            ├── Export (uses Marp CLI internally)
            └── Custom theme support
```

## Important Notes

- PDF/PPTX/image export requires Chrome, Edge, or Firefox installed
- `headingDivider` can auto-split slides at headings (no `---` needed)
- Marp Core enables inline SVG mode by default (pixel-perfect scaling)
- Theme CSS uses `section` as the slide element (`section { width: 1280px; height: 720px; }`)
- `:root` in theme CSS maps to `section`, not `<html>`
- `@theme name` CSS comment is required in custom theme files
- All built-in themes support `invert` class and `4:3`/`16:9` size presets

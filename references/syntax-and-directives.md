# Marp Syntax & Directives — Complete Reference

## Table of Contents
1. [Slide Separation](#slide-separation)
2. [Directives Overview](#directives-overview)
3. [Global Directives](#global-directives)
4. [Local Directives](#local-directives)
5. [Spot Directives](#spot-directives)
6. [Image Syntax](#image-syntax)
7. [Backgrounds](#backgrounds)
8. [Advanced Backgrounds](#advanced-backgrounds)
9. [Image Filters](#image-filters)
10. [Fragmented Lists](#fragmented-lists)
11. [Fitting Header](#fitting-header)
12. [Math Typesetting](#math-typesetting)
13. [Emoji](#emoji)
14. [Presenter Notes](#presenter-notes)
15. [Inline Styles](#inline-styles)
16. [HTML in Marp](#html-in-marp)
17. [Header and Footer Formatting](#header-and-footer-formatting)

---

## Slide Separation

Slides are separated by horizontal rules. An empty line before `---` may be required by CommonMark.

```markdown
# Slide 1

Content

---

# Slide 2

More content
```

Alternative rulers that don't need preceding empty lines: `___`, `***`, `- - -`.

### Heading Divider (auto-split)

The `headingDivider` global directive auto-splits at headings, eliminating the need for `---`:

```markdown
<!-- headingDivider: 2 -->

# Chapter 1
Content for slide 1

## Section A
Content for slide 2 (auto-split before h2)

## Section B
Content for slide 3

# Chapter 2
Content for slide 4
```

- Number value: splits at that level AND higher (e.g., `2` splits at h1 and h2)
- Array value: splits only at specified levels (e.g., `[1, 3]`)

---

## Directives Overview

Directives are YAML values written in **front-matter** or **HTML comments**:

```markdown
---
theme: gaia
paginate: true
---
```

```markdown
<!-- theme: gaia -->
<!-- paginate: true -->
```

Multi-line HTML comment directives:
```markdown
<!--
theme: gaia
paginate: true
backgroundColor: "#123"
-->
```

**Important**: Wrap values with YAML special characters in quotes: `backgroundColor: "#123"`.

---

## Global Directives

Apply to the **entire slide deck**. Only the last value is used if defined multiple times.

| Directive | Description | Example |
|-----------|-------------|---------|
| `theme` | Theme name | `theme: gaia` |
| `style` | Additional CSS (like inline `<style>`) | `style: \|` followed by CSS block |
| `headingDivider` | Auto-split at headings | `headingDivider: 2` |
| `lang` | HTML `lang` attribute for slides | `lang: en-US` |
| `size` | Slide size preset (Marp Core only) | `size: 4:3` |
| `math` | Math library (Marp Core only) | `math: katex` or `math: mathjax` |

### `style` directive example:
```markdown
---
theme: default
style: |
  section {
    background-color: #fef;
  }
  h1 {
    color: #c33;
  }
---
```

### Metadata directives (Marp CLI adds these):
| Directive | Description |
|-----------|-------------|
| `title` | Slide deck title |
| `description` | Slide deck description |
| `author` | Author name |
| `keywords` | Comma-separated keywords |
| `url` | Canonical URL |
| `image` | Open Graph image URL |

---

## Local Directives

Apply to the **current page and all following pages** until overridden.

| Directive | Description | Default |
|-----------|-------------|---------|
| `paginate` | Show page number | (none) |
| `header` | Header content (Markdown supported) | (none) |
| `footer` | Footer content (Markdown supported) | (none) |
| `class` | CSS class on `<section>` element | (none) |
| `backgroundColor` | Slide background color | (theme default) |
| `backgroundImage` | Slide background image CSS | (none) |
| `backgroundPosition` | Background position | `center` |
| `backgroundRepeat` | Background repeat | `no-repeat` |
| `backgroundSize` | Background size | `cover` |
| `color` | Text color | (theme default) |
| `transition` | Slide transition (CLI bespoke only) | (none) |

### Pagination modes

| Value | Page number visible | Counter increments |
|-------|--------------------|--------------------|
| `true` | Yes | Yes |
| `false` | No | Yes |
| `hold` | Yes | No |
| `skip` | No | No |

```markdown
---
paginate: true
_paginate: skip
---

# Title Slide (no page number, not counted)

---

# Slide 2 (shows "1")
```

### Header/Footer with Markdown formatting:
```markdown
---
header: '**Company Name** | Confidential'
footer: '![h:20](logo.png) © 2025'
---
```
Always wrap in quotes to avoid YAML parsing issues.

---

## Spot Directives

Add `_` prefix to apply a local directive to **only the current slide**:

```markdown
<!-- _backgroundColor: black -->
<!-- _color: white -->

# Dark Slide

---

# Normal Slide (back to default)
```

All local directives support the `_` prefix: `_paginate`, `_class`, `_header`, `_footer`, `_backgroundColor`, `_backgroundImage`, `_color`, `_transition`, etc.

---

## Image Syntax

### Inline Images with Resizing

```markdown
![w:200](image.jpg)                 <!-- width only -->
![h:150](image.jpg)                 <!-- height only -->
![w:200 h:150](image.jpg)          <!-- both -->
![width:200px](image.jpg)          <!-- full keyword -->
![height:30cm](image.jpg)          <!-- other units -->
```

Shorthand `w:` and `h:` are preferred. Supports CSS length units (px, cm, em, etc.) except viewport units (vw, vh).

### Alt Text

Keywords are extracted; remaining text becomes the alt attribute:
```markdown
![bg w:300 My alt text](image.jpg)
<!-- "bg" and "w:300" are extracted; alt text = "My alt text" -->
```

---

## Backgrounds

### Basic Background
```markdown
![bg](image.jpg)
```

### Background Size Keywords

| Keyword | Description |
|---------|-------------|
| `cover` | Scale to fill (default) |
| `contain` | Scale to fit |
| `fit` | Alias for `contain` |
| `auto` | Original size |
| `X%` | Scale by percentage |

```markdown
![bg contain](image.jpg)
![bg 150%](image.jpg)
![bg auto](image.jpg)
```

### Background with width/height
```markdown
![bg w:500](image.jpg)
![bg h:400](image.jpg)
```

---

## Advanced Backgrounds

Requires inline SVG mode (enabled by default in Marp Core).

### Multiple Backgrounds (horizontal by default)
```markdown
![bg](image1.jpg)
![bg](image2.jpg)
![bg](image3.jpg)
```
Three images side by side.

### Vertical Multiple Backgrounds
```markdown
![bg vertical](image1.jpg)
![bg](image2.jpg)
![bg](image3.jpg)
```
Add `vertical` keyword to the first image.

### Split Backgrounds
```markdown
![bg left](image.jpg)

# Content on the right

---

![bg right](image.jpg)

# Content on the left
```

### Split with Custom Size
```markdown
![bg left:33%](image.jpg)

# Content takes remaining 67%
```

### Split + Multiple
```markdown
![bg right](image1.jpg)
![bg](image2.jpg)

# Content on the left, two images stacked on the right
```

### Background Colors/Gradients via Directives
```markdown
<!-- _backgroundImage: "linear-gradient(to bottom, #67b8e3, #0288d1)" -->
<!-- _backgroundColor: "#246" -->
<!-- _color: white -->

# Gradient Slide
```

---

## Image Filters

Apply CSS filters via alt text keywords. Work on inline images and advanced backgrounds.

| Filter | Default | With args |
|--------|---------|-----------|
| `blur` | `blur` | `blur:10px` |
| `brightness` | `brightness` | `brightness:1.5` |
| `contrast` | `contrast` | `contrast:200%` |
| `drop-shadow` | `drop-shadow` | `drop-shadow:0,5px,10px,rgba(0,0,0,.4)` |
| `grayscale` | `grayscale` | `grayscale:1` |
| `hue-rotate` | `hue-rotate` | `hue-rotate:180deg` |
| `invert` | `invert` | `invert:100%` |
| `opacity` | `opacity` | `opacity:.5` |
| `saturate` | `saturate` | `saturate:2.0` |
| `sepia` | `sepia` | `sepia:1.0` |

Multiple filters:
```markdown
![brightness:.8 sepia:50%](image.jpg)
![bg blur:5px opacity:.5](background.jpg)
```

**Note**: Filters do NOT work on basic slide backgrounds (`![bg]`), only on advanced backgrounds (inline SVG mode) and inline images.

---

## Fragmented Lists

Lists that appear one-by-one in presentation (bespoke template).

**Bullet**: Use `*` marker (instead of `-` or `+`):
```markdown
* First point (appears first)
* Second point (appears second)
* Third point (appears third)
```

**Ordered**: Use `)` after number (instead of `.`):
```markdown
1) First step
2) Second step
3) Third step
```

Regular non-fragmented lists:
```markdown
- All visible at once
- No animation
```

---

## Fitting Header

Scale heading to fill the slide width. Requires `@auto-scaling` in theme.

```markdown
# <!-- fit --> This text auto-scales to fill the width
```

Works with all heading levels.

---

## Math Typesetting

Marp Core supports MathJax (default) and KaTeX.

### Inline Math
```markdown
Einstein's equation: $E = mc^2$
```

### Block Math
```markdown
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$
```

### Declare Library
```markdown
---
math: katex
---
```
MathJax: better rendering, more LaTeX support. KaTeX: faster rendering.

---

## Emoji

Marp Core converts emoji shortcodes to twemoji SVG images:
```markdown
:smile: :rocket: :heart:
```
Unicode emoji also converted: 😄 🚀 ❤️

---

## Presenter Notes

HTML comments that are NOT directives become presenter notes:
```markdown
# My Slide

Content visible to audience.

<!-- This is a presenter note. Only visible in presenter view. -->
<!-- You can have multiple notes per slide. -->
```

---

## Inline Styles

### Global `<style>`
```markdown
<style>
section { background: #ffc; }
h1 { color: #c30; }
</style>
```
Applies to ALL slides. Merged into theme CSS.

### Scoped `<style scoped>`
```markdown
<style scoped>
h1 { color: blue; }
</style>

# This is blue (only on this slide)
```

### `style` Global Directive (preferred over `<style>` for compatibility)
```markdown
---
style: |
  section { background: #ffc; }
  h1 { color: #c30; }
---
```

---

## HTML in Marp

Marp Core allows a safe subset of HTML by default. Full HTML requires `--html` CLI flag or VS Code setting.

Safe by default: `<br>`, `<span>`, `<div>`, `<i>`, `<b>`, `<strong>`, `<em>`, `<mark>`, `<del>`, `<ins>`, `<sub>`, `<sup>`, `<small>`, `<kbd>`, `<abbr>`, `<details>`, `<summary>`, basic `<table>` elements, and more.

**Always available regardless of HTML setting**: `<!-- comments -->` (directives/notes) and `<style>` tags.

---

## Header and Footer Formatting

Headers and footers support inline Markdown:
```markdown
---
header: '**Bold** and _italic_ header'
footer: '![h:20](logo.png) Page footer with image'
---
```

They render as `<header>` and `<footer>` elements. To position them at slide margins, the theme must use `position: absolute` styling (all built-in themes support this).

You CANNOT use `![bg]()` syntax inside header/footer directives.

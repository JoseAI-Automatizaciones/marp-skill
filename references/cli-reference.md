# Marp CLI — Complete Reference

## Table of Contents
1. [Installation](#installation)
2. [Basic Conversion](#basic-conversion)
3. [Output Formats](#output-formats)
4. [Server and Watch Modes](#server-and-watch-modes)
5. [Browser Options](#browser-options)
6. [Templates](#templates)
7. [Slide Transitions](#slide-transitions)
8. [Custom Transitions](#custom-transitions)
9. [Morphing Animations](#morphing-animations)
10. [Metadata](#metadata)
11. [Theme Options](#theme-options)
12. [Engine Customization](#engine-customization)
13. [Configuration Files](#configuration-files)
14. [Docker](#docker)
15. [Node.js API](#nodejs-api)
16. [Complete Options Table](#complete-options-table)

---

## Installation

### Quick use (no install)
```bash
npx @marp-team/marp-cli@latest slide.md
```

### Package managers
```bash
# macOS/Linux
brew install marp-cli

# Windows
scoop install marp

# npm (local project)
npm install --save-dev @marp-team/marp-cli

# npm (global)
npm install -g @marp-team/marp-cli
```

### Standalone binary
Download from: https://github.com/marp-team/marp-cli/releases

### Requirements
- Node.js v18+ (for npm install)
- Chrome, Edge, or Firefox (for PDF/PPTX/image export)

---

## Basic Conversion

```bash
# Convert to HTML (default)
marp slide.md
marp slide.md -o output.html

# Convert to PDF
marp slide.md --pdf
marp slide.md -o output.pdf

# Convert to PPTX
marp slide.md --pptx
marp slide.md -o output.pptx

# Convert to PNG (first slide only)
marp slide.md --image png
marp slide.md -o output.png

# Convert to multiple PNG images
marp slide.md --images png

# Convert to JPEG
marp slide.md --image jpeg
marp slide.md -o output.jpg

# Export presenter notes as text
marp slide.md --notes
marp slide.md -o notes.txt

# Multiple files
marp slide1.md slide2.md slide3.md

# Directory with structure preservation
marp -I ./slides -o ./output
```

---

## Output Formats

### HTML
Default output. Uses bespoke template with navigation, fullscreen, presenter view.

### PDF (requires browser)
```bash
marp --pdf slide.md
marp --pdf --pdf-notes slide.md           # Include presenter notes
marp --pdf --pdf-outlines slide.md        # Add bookmarks/outlines
marp --pdf --pdf-outlines.pages=false slide.md  # Outlines from headings only
```

### PPTX (requires browser)
```bash
marp --pptx slide.md
marp --pptx --pptx-editable slide.md      # EXPERIMENTAL: editable content
```
PPTX includes rendered slides + presenter notes. Opens in PowerPoint, Keynote, Google Slides, LibreOffice.

**Editable PPTX**: Requires browser + LibreOffice. Lower fidelity, no presenter notes. Not recommended for complex themes.

### Images (requires browser)
```bash
marp --images png slide.md     # All slides: slide.001.png, slide.002.png...
marp --images jpeg slide.md
marp --image png slide.md      # First slide only
marp --image-scale 2 slide.md -o hi-res.png  # 2x resolution
```

### Local Files Security
Browser-based conversion blocks local file access by default:
```bash
marp --pdf --allow-local-files slide.md    # Enable local file access (use with trusted content only)
```

---

## Server and Watch Modes

### Watch mode
```bash
marp -w slide.md              # Auto-reconvert on file changes
```
Opens in browser? Page auto-refreshes on changes.

### Server mode
```bash
marp -s ./slides              # Serve directory on http://localhost:8080
PORT=5000 marp -s ./slides    # Custom port
```
Access converted files via URL. Add query strings for format: `?pdf`, `?pptx`, `?png`, `?jpeg`, `?txt`.

Place `index.md` or `PITCHME.md` in served directory for default redirect.

### Preview window
```bash
marp -p slide.md              # Open immersive preview window
```
Auto-enables watch mode. Not available in Docker.

---

## Browser Options

```bash
# Choose browser
marp --browser firefox slide.md --pdf
marp --browser firefox,chrome slide.md --pdf   # Fallback chain

# Custom browser path
marp --browser-path /path/to/chromium slide.md --pdf

# Protocol
marp --browser-protocol cdp slide.md --pdf          # Chrome DevTools Protocol (default)
marp --browser-protocol webdriver-bidi slide.md --pdf

# Timeout
marp --browser-timeout 60 slide.md --pdf    # 60 seconds
```

Default browser order: `auto` = `chrome,edge,firefox`.

---

## Templates

### `bespoke` (default)
Full-featured HTML presentation:
- Keyboard/swipe navigation
- Fullscreen (F/F11)
- On-screen controller (disable: `--bespoke.osc=false`)
- Fragmented lists
- Presenter view (P key)
- Overview (O or Esc)
- Progress bar (enable: `--bespoke.progress`)
- Slide transitions

### `bare`
Minimal HTML, no JavaScript features. When combined with Marpit engine, produces zero-JS output:
```bash
marp --template bare --engine @marp-team/marpit slide.md
```

---

## Slide Transitions

Only work in `bespoke` template HTML output. Set via `transition` local directive:

```markdown
---
transition: fade
---

# Slide 1

---

# Slide 2 (fade transition from slide 1)
```

### 33 Built-in Transitions
`none`, `clockwise`, `counterclockwise`, `cover`, `coverflow`, `cube`, `cylinder`, `diamond`, `drop`, `explode`, `fade`, `fade-out`, `fall`, `flip`, `glow`, `implode`, `in-out`, `iris-in`, `iris-out`, `melt`, `overlap`, `pivot`, `pull`, `push`, `reveal`, `rotate`, `slide`, `star`, `swap`, `swipe`, `swoosh`, `wipe`, `wiper`, `zoom`

### Custom Duration
```markdown
transition: fade 1s
transition: slide 500ms
```

### Per-slide Transitions
```markdown
---
transition: fade
---

# Fade into this

---

<!-- transition: cube -->

# Cube transition starts here

---

<!-- _transition: none -->

# No transition for just this slide

---

# Back to cube
```

### Disable Transitions
```bash
marp --bespoke.transition=false slide.md
```

Viewers with reduced-motion preference automatically get simple `fade` instead of complex animations.

---

## Custom Transitions

Define via CSS `@keyframes` in theme CSS or `<style>`:

### Simple (same animation for old/new slide)
```css
@keyframes marp-transition-dissolve {
  from { opacity: 1; }
  to { opacity: 0; }
}
```

### Split (different animations)
```css
@keyframes marp-outgoing-transition-slide-up {
  from { transform: translateY(0%); }
  to { transform: translateY(calc(var(--marp-transition-direction, 1) * -100%)); }
}
@keyframes marp-incoming-transition-slide-up {
  from { transform: translateY(calc(var(--marp-transition-direction, 1) * 100%)); }
  to { transform: translateY(0%); }
}
```

### Backward Navigation
`--marp-transition-direction`: `1` for forward, `-1` for backward.

Or define separate backward keyframes:
```css
@keyframes marp-incoming-transition-backward-triangle { /* ... */ }
```

### Custom Default Duration
```css
@keyframes marp-incoming-transition-gate {
  from {
    --marp-transition-duration: 1s;
    clip-path: inset(0 50%);
  }
  to { clip-path: inset(0); }
}
```

### Layer Order
Incoming slide stacks on top by default. Send to back:
```css
@keyframes marp-incoming-transition-xxx {
  from, to { z-index: -1; }
}
```

---

## Morphing Animations

PowerPoint Morph / Keynote Magic Move equivalent. Use `view-transition-name` CSS property:

```markdown
---
transition: fade
style: |
  img[alt="icon"] { view-transition-name: hero-icon; }
---

# Page 1
![icon w:64](icon.svg)

---

# Page 2
![icon w:256](icon.svg)
```

The `icon` image morphs smoothly between sizes/positions. Each `view-transition-name` must be unique per slide.

---

## Metadata

Set via directives or CLI options:

```markdown
---
title: My Presentation
description: A great deck
author: John Doe
keywords: marp, slides
url: https://example.com/deck
image: https://example.com/og.jpg
---
```

CLI overrides:
```bash
marp --title "My Deck" --author "Jane" --description "Overview" slide.md
```

Auto-title: If no title set, first heading is used.

---

## Theme Options

```bash
# Override theme
marp --theme gaia slide.md

# Custom theme file
marp --theme ./my-theme.css slide.md

# Multiple theme files
marp --theme-set theme-a.css theme-b.css -- slide.md

# Theme directory
marp --theme-set ./themes -- slide.md
```

---

## Engine Customization

### Use Marpit directly
```bash
npm i @marp-team/marpit
marp --engine @marp-team/marpit slide.md
```

### Functional engine (plugins)
```javascript
// engine.mjs
import markdownItMark from 'markdown-it-mark'

export default ({ marp }) => marp.use(markdownItMark)
```

```bash
npm i markdown-it-mark
marp --engine ./engine.mjs slide.md
```

### Check engine version
```bash
marp --version
# @marp-team/marp-cli v4.x.x (w/ @marp-team/marp-core v4.x.x)
```

---

## Configuration Files

Supported: `marp.config.js`, `marp.config.mjs`, `marp.config.cjs`, `.marprc` (JSON/YAML), `package.json` (`marp` key), `marp.config.ts`.

### `.marprc.yml` example
```yaml
theme: gaia
allowLocalFiles: true
pdf: true
pdfOutlines: true
options:
  markdown:
    breaks: false
```

### `marp.config.mjs` example
```javascript
import markdownItContainer from 'markdown-it-container'

/** @type {import('@marp-team/marp-cli').Config} */
export default {
  inputDir: './slides',
  output: './public',
  themeSet: './themes',
  engine: ({ marp }) => marp.use(markdownItContainer, 'columns'),
}
```

### `package.json` example
```json
{
  "marp": {
    "inputDir": "./slides",
    "output": "./public",
    "themeSet": "./themes"
  }
}
```

Specify config: `marp --config ./my-config.js`
Skip config: `marp --no-config`

---

## Docker

```bash
# Pull
docker pull marpteam/marp-cli

# Convert
docker run --rm -v $(pwd):/home/marp marpteam/marp-cli slide.md --pdf

# Server mode
docker run --rm -p 8080:8080 -v $(pwd):/home/marp marpteam/marp-cli -s .
```

Also available from GitHub Container Registry: `ghcr.io/marp-team/marp-cli`

---

## Node.js API

```javascript
import { marpCli } from '@marp-team/marp-cli'

const exitStatus = await marpCli(['slide.md', '--pdf'])
```

### Server with wait
```javascript
import { marpCli, waitForObservation } from '@marp-team/marp-cli'

marpCli(['--server', './slides/'])
const { stop } = await waitForObservation()
// Server ready...
stop() // Stop when done
```

---

## Complete Options Table

| Option | Description |
|--------|-------------|
| `-o, --output` | Output file path |
| `-I, --input-dir` | Input directory |
| `-w, --watch` | Watch mode |
| `-s, --server` | Server mode |
| `-p, --preview` | Preview window |
| `--pdf` | Convert to PDF |
| `--pptx` | Convert to PPTX |
| `--pptx-editable` | Editable PPTX (experimental) |
| `--image [png\|jpeg]` | First slide to image |
| `--images [png\|jpeg]` | All slides to images |
| `--image-scale N` | Image scale factor |
| `--notes` | Export presenter notes |
| `--pdf-notes` | Add notes to PDF |
| `--pdf-outlines` | Add bookmarks to PDF |
| `--html` | Enable full HTML |
| `--theme NAME\|PATH` | Override theme |
| `--theme-set PATH...` | Register theme files |
| `--template [bespoke\|bare]` | HTML template |
| `--engine MODULE\|PATH` | Conversion engine |
| `--allow-local-files` | Allow local file access |
| `--browser KIND` | Browser for conversion |
| `--browser-path PATH` | Browser executable path |
| `--browser-timeout N` | Browser timeout (seconds) |
| `--bespoke.osc` | On-screen controller |
| `--bespoke.progress` | Progress bar |
| `--bespoke.transition` | Enable transitions |
| `--title TEXT` | Slide deck title |
| `--description TEXT` | Description |
| `--author TEXT` | Author |
| `--keywords TEXT` | Keywords |
| `--url URL` | Canonical URL |
| `--og-image URL` | Open Graph image |
| `--jpeg-quality N` | JPEG quality (0-100) |
| `-P, --parallel N` | Parallel concurrency |
| `-c, --config-file PATH` | Config file path |
| `--no-config` | Skip config file |
| `-v, --version` | Show version |

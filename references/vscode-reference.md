# Marp for VS Code — Complete Reference

## Table of Contents
1. [Getting Started](#getting-started)
2. [Enabling Marp](#enabling-marp)
3. [Preview](#preview)
4. [IntelliSense](#intellisense)
5. [Export](#export)
6. [Custom Themes](#custom-themes)
7. [Outline and Folding](#outline-and-folding)
8. [Settings Reference](#settings-reference)
9. [Diagnostics](#diagnostics)
10. [GitHub Copilot Integration](#github-copilot-integration)
11. [Web Extension](#web-extension)
12. [Security](#security)

---

## Getting Started

1. Install "Marp for VS Code" extension (`marp-team.marp-vscode`) from VS Marketplace
2. Create a `.md` file
3. Add `marp: true` to front-matter
4. Open VS Code's Markdown preview (Ctrl+Shift+V or Cmd+Shift+V)

---

## Enabling Marp

Marp features activate when `marp: true` is in the front-matter:

```markdown
---
marp: true
---

# Your slide deck
```

### Quick Toggle
Click the Marp toolbar icon → "Toggle Marp feature for current Markdown"
Or Command Palette: `markdown.marp.toggleMarpFeature`

### New Marp Document
File → New File... (Alt+Ctrl+Win+N / Alt+Cmd+Ctrl+N) → Select Marp Markdown template.

---

## Preview

Standard VS Code Markdown preview works with Marp. When `marp: true` is set, preview shows slide deck instead of document.

- Active slide highlights based on cursor position
- Disable highlight: set `markdown.preview.markEditorSelection` to `false`

---

## IntelliSense

When `marp: true` is active:

### Auto Completion
- Ctrl+Space in front-matter or HTML comments shows directive suggestions
- Value completion for `theme`, `paginate`, `transition`, `size`, `math`, `class`
- Press Ctrl+Space again to see directive documentation

### Syntax Highlighting
Recognized directive keys are highlighted differently from surrounding text.

### Hover Help
Hover over any recognized directive to see documentation.

---

## Export

### Via Toolbar
Click Marp toolbar icon → "Export slide deck..." → Choose format and location.

### Via Command Palette
F1 → `Marp: Export slide deck...`

### Supported Formats
| Format | Extension | Requires Browser |
|--------|-----------|:---------------:|
| HTML | `.html` | No |
| PDF | `.pdf` | Yes |
| PPTX | `.pptx` | Yes |
| PNG | `.png` | Yes (first slide only) |
| JPEG | `.jpg` | Yes (first slide only) |
| TXT | `.txt` | No (notes only) |

### Default Export Type
Setting: `markdown.marp.exportType`

### Browser Requirement
PDF, PPTX, and image export require Chrome, Chromium, Edge, or Firefox.
Configure via: `markdown.marp.browser` and `markdown.marp.browserPath`

---

## Custom Themes

### Register Themes
In `.vscode/settings.json`:
```json
{
  "markdown.marp.themes": [
    "https://example.com/remote-theme.css",
    "./themes/local-theme.css"
  ]
}
```

### Create a Theme
```css
/* @theme my-custom-theme */
@import 'default';

section {
  background: #ffd;
  font-family: 'Georgia', serif;
}

h1 {
  color: #c30;
}
```

### Use the Theme
```markdown
---
marp: true
theme: my-custom-theme
---
```

### Live Reload
When editing a registered local CSS file, the preview auto-reloads with changes.

---

## Outline and Folding

### Outline View
Extended to show slide pages in the outline panel. Each slide appears as a navigation entry.

Tip: Use "Sort By: Position" in the outline panel context menu for correct slide order.

### Slide Folding
Fold/unfold slide content in the editor by clicking the fold icon next to slide separators.

### Disable
Setting: `markdown.marp.outlineExtension` → `false`

---

## Settings Reference

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `markdown.marp.themes` | string[] | `[]` | Custom theme CSS URLs/paths |
| `markdown.marp.exportType` | string | `html` | Default export format |
| `markdown.marp.browser` | string | `auto` | Browser for export (chrome/edge/firefox) |
| `markdown.marp.browserPath` | string | (auto) | Path to browser executable |
| `markdown.marp.html` | string | `default` | HTML rendering: `default`, `all`, `off` |
| `markdown.marp.outlineExtension` | boolean | `true` | Enable outline/folding for slides |
| `markdown.marp.diagnostics.slideContentOverflow` | boolean | `false` | Warn on content overflow (experimental) |
| `markdown.marp.pptx.editable` | string | `off` | Editable PPTX export (experimental): `off`, `on`, `smart` |
| `markdown.marp.strictPathResolutionDuringExport` | boolean | `false` | Resolve paths from workspace during export (experimental) |

### HTML Rendering Options
- `default`: Marp's safe allowlist (recommended)
- `all`: All HTML elements allowed (⚠️ security risk with untrusted files)
- `off`: No HTML except directives and `<style>`

---

## Diagnostics

Marp for VS Code detects common issues:

| Diagnostic | Description | Auto-Fix |
|------------|-------------|:--------:|
| `define-math-global-directive` | Recommend declaring math library | ✅ |
| `deprecated-color-setting-shorthand` | Obsolete color shorthands | ✅ |
| `deprecated-dollar-prefix` | Obsolete `$` prefix on directives | ✅ |
| `ignored-math-global-directive` | Math directive ignored (disabled in settings) | |
| `overloading-global-directive` | Same global directive defined multiple times | |
| `unknown-size` | Size preset not defined in theme | |
| `unknown-theme` | Unrecognized theme name | |

### Experimental Diagnostics
| Diagnostic | Description |
|------------|-------------|
| `slide-content-overflow` | Content overflows slide padding area (requires preview open) |

---

## GitHub Copilot Integration

Marp for VS Code provides an export tool for GitHub Copilot agent mode. You can instruct Copilot to export Markdown in a specific format, and it will use workspace settings.

---

## Web Extension

Works in vscode.dev and github.dev with limitations:
- Preview and IntelliSense: ✅
- Export: ❌ (requires Marp CLI / local environment)
- Custom local themes: Limited to remote URLs

---

## Security

### Workspace Trust
Some features are restricted in untrusted workspaces:
- Custom themes: Restricted 🛡️
- Export: Restricted 🛡️
- Full HTML rendering: Restricted 🛡️

Basic preview and IntelliSense work in untrusted workspaces.

### HTML Safety
Even with `markdown.marp.html: "all"`, `<style>` and HTML comments are always parsed by Marpit.

In untrusted workspaces, all HTML is ignored regardless of settings.

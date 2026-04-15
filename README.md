# Marp Skill for Claude Code

A **Claude Code skill** that lets you create, edit, theme, and export professional slide deck presentations using [Marp](https://marp.app) (Markdown Presentation Ecosystem) — all from natural language.

> **What is Marp?** An open-source ecosystem that converts plain Markdown into beautiful slide decks: HTML, PDF, PPTX, and images. No PowerPoint, no design tools — just text. Learn more at [github.com/marp-team/marp](https://github.com/marp-team/marp).

---

## What this skill does

When installed in Claude Code, this skill activates whenever you ask for:

- Slide decks or presentations
- Marp directives, themes, or image syntax
- Converting Markdown to PDF, PPTX, or HTML slides
- Slide transitions, backgrounds, split layouts
- Custom CSS themes for Marp
- Presenter notes, math typesetting, fragmented lists

Claude reads the reference files, understands the full Marp syntax, and generates production-ready `.md` files that work immediately in VS Code or Marp CLI.

## Quick start

### 1. Install the skill

Copy the `SKILL.md` file and the `references/` folder into your Claude Code skills directory:

```
~/.claude/skills/marp/
  SKILL.md
  references/
    syntax-and-directives.md
    themes-and-styling.md
    cli-reference.md
    vscode-reference.md
    examples-and-patterns.md
```

### 2. Use it

Just ask Claude Code to create a presentation:

```
> Make me a 10-slide pitch deck about our product launch
> Create a Marp presentation about machine learning basics
> Add transitions and a custom dark theme to my slides
```

Claude will generate a `.md` file you can preview in VS Code (with the [Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode) extension) or export via CLI:

```bash
npx @marp-team/marp-cli@latest my-deck.md --pdf
```

## What's included

```
marp-skill/
  SKILL.md                              # The skill definition
  references/
    syntax-and-directives.md            # Full Marp/Marpit syntax reference
    themes-and-styling.md               # Built-in themes, custom CSS, variables
    cli-reference.md                    # CLI commands, config, Docker, export options
    vscode-reference.md                 # VS Code extension features and settings
    examples-and-patterns.md            # Working examples, patterns, best practices
  examples/
    marp-intro-es.md                    # Example: full Marp intro presentation (Spanish)
```

## Example presentation

The [`examples/marp-intro-es.md`](examples/marp-intro-es.md) file is a complete Marp presentation that covers:

- What Marp is and how it compares to PowerPoint
- Directives (global, local, spot)
- Image syntax: backgrounds, split layouts, filters
- Fit headers, LaTeX math, fragmented lists, presenter notes
- CLI, VS Code, and Docker workflows
- 33 built-in transitions

You can preview it right now:

```bash
# VS Code: open the file and press Ctrl+Shift+V
# CLI:
npx @marp-team/marp-cli@latest examples/marp-intro-es.md -o intro.html
# Then open intro.html in your browser
```

## Marp ecosystem

This skill covers the full stack:

| Layer | What it does |
|-------|-------------|
| **Marpit** | Core framework — Markdown parsing, directives, image syntax, CSS themes |
| **Marp Core** | Built-in themes (`default`, `gaia`, `uncover`), math, emoji, auto-scaling |
| **Marp CLI** | Export to HTML, PDF, PPTX, PNG/JPEG. Server/watch modes, transitions |
| **Marp for VS Code** | Live preview, IntelliSense, one-click export |

## Links

- [Marp official site](https://marp.app)
- [Marp on GitHub](https://github.com/marp-team/marp)
- [Marp CLI docs](https://github.com/marp-team/marp-cli)
- [Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)
- [Marpit framework](https://marpit.marp.app)
- [Claude Code](https://claude.ai/claude-code)

## License

MIT

---
name: theme-guide
description: How the Fluent OLED VS Code theme is built, its architecture, design principles, and how to modify it. Use this skill whenever the user wants to add new colors, change syntax highlighting, modify workbench UI, add token scopes, fix a color that looks wrong, adjust the design system, or understand the theme structure. Also use when they mention TextMate scopes, semantic tokens, workbench colors, or any visual aspect of the theme.
---

# Fluent OLED Theme Guide

The theme is a single JSON file at `themes/Fluent OLED-color-theme.json`. It has three sections that control every visual aspect of the editor.

## Architecture

```
themes/Fluent OLED-color-theme.json
├── Root metadata (name, type, semanticHighlighting)
├── "colors"              → Workbench UI (lines 5-284)
├── "tokenColors"         → Syntax highlighting via TextMate (lines 285-840)
└── "semanticTokenColors" → Language-aware token overrides (lines 841-861)
```

## Design Principles

Every change should follow these rules:

1. **Pure black backgrounds** — All primary surfaces use `#000000`. This is non-negotiable for the OLED promise (pixels off = power savings).
2. **Monochrome chrome** — The workbench UI uses only white/gray tones. No colored accents in the chrome — the only color lives in syntax tokens and functional indicators.
3. **Functional color is preserved** — Git status (green/yellow/red), errors, warnings, terminal ANSI, and debug icons keep their semantic meaning.
4. **GitHub Dark syntax palette** — Token colors come from the GitHub Dark theme. Don't introduce new syntax colors unless they're from this palette.
5. **Minimalism** — If a border or visual element isn't essential, make it invisible or near-invisible.

## Color Palette Reference

### UI Grays (used in `colors`)

| Hex       | Role                                | Usage guideline                                    |
| --------- | ----------------------------------- | -------------------------------------------------- |
| `#000000` | Pure black                          | All primary backgrounds                            |
| `#060606` | Near-black                          | Peek view backgrounds                              |
| `#0a0a0a` | Surface                             | Widgets, dropdowns, inputs, hover states            |
| `#0d0d0d` | Inactive surface                    | Inactive selections                                |
| `#111111` | Border                              | Panel dividers, section separators                  |
| `#161616` | Elevated surface / subtle border    | Active selections, widget borders, keybinding boxes |
| `#1b1b1b` | Light border                        | Input borders, tree indent guides, menu separators  |
| `#2a2a2a` | Highlight border                    | Active indent guides, secondary button hover        |
| `#3a3a3a` | Focus / accent border               | Focus rings, peek view borders, unfocused tab top   |
| `#484f58` | Dimmed placeholder                  | Input placeholders, ignored git files               |
| `#4a4a4a` | Inactive icons                      | Activity bar inactive items                         |
| `#6e7681` | Muted text                          | Inactive tabs, breadcrumbs, line numbers             |
| `#8b949e` | Secondary text                      | Descriptions, comments, status bar text             |
| `#c9d1d9` | Primary foreground                  | All main text, active elements, accents             |
| `#e6edf3` | Bright foreground                   | Highlighted matches, active line numbers            |

### Syntax Colors (used in `tokenColors` and `semanticTokenColors`)

| Hex       | Token type                                      |
| --------- | ------------------------------------------------ |
| `#ff7b72` | Keywords, control flow, storage, `this`/`self`   |
| `#ffa657` | Types, classes, interfaces, enums, block vars    |
| `#d2a8ff` | Functions, methods, decorators                   |
| `#79c0ff` | Constants, numbers, attributes, readonly vars    |
| `#a5d6ff` | Strings, symbols, headings                       |
| `#7ee787` | Tags (HTML/JSX), CSS classes, regex              |
| `#f85149` | Errors, invalid, sub-methods, `this`             |
| `#8b949e` | Comments (italic)                                |
| `#c9d1d9` | Variables, parameters, plain text                |
| `#6e7681` | Muted markup (blockquotes, fenced code markers)  |

### Functional Colors (don't change these without good reason)

| Hex       | Meaning                              |
| --------- | ------------------------------------ |
| `#238636` | Git added, status bar remote         |
| `#3fb950` | Git added (decoration), debug start  |
| `#d29922` | Warning, git modified                |
| `#f85149` | Error, git deleted, debug stop       |
| `#2f81f7` | Git gutter modified, minimap gutter  |

## Section 1: `colors` — Workbench UI

This object controls every non-code visual element. Keys are dot-separated identifiers defined by the VS Code API.

### How to add or change a workbench color

1. Find the token name in the [VS Code Theme Color Reference](https://code.visualstudio.com/api/references/theme-color)
2. Pick a hex value from the **UI Grays** palette above — don't invent new grays
3. Add or modify the entry in the `"colors"` object
4. For transparency, append a 2-digit hex alpha to the color (e.g., `#c9d1d920` = 12.5% opacity)

### Key areas and their tokens

**Editor core:**
- `editor.background`, `editor.foreground` — The main canvas
- `editor.lineHighlightBackground` — Current line highlight (`#0a0a0a`)
- `editorCursor.foreground` — Caret color
- `editorLineNumber.*` — Line number colors

**Sidebar & Explorer:**
- `sideBar.background`, `sideBar.border`
- `list.*` — Tree/list selection, hover, focus states

**Tabs:**
- `tab.activeBackground`, `tab.inactiveBackground` — Both `#000000`
- `tab.activeBorderTop` — The accent line on active tab (`#c9d1d9`)
- `tab.inactiveForeground` — Dimmed inactive tab text (`#6e7681`)

**Status bar:**
- `statusBar.background` — `#000000`
- `statusBar.foreground` — `#8b949e` (muted, not distracting)

**Terminal:**
- `terminal.background` — `#000000`
- `terminal.ansi*` — Full 16-color ANSI palette from GitHub Dark

**Widgets (autocomplete, hover, quick input):**
- All use `#0a0a0a` background with `#161616` borders

### Alpha transparency convention

When a color needs transparency (selections, highlights, find matches), use these suffixes:

| Alpha | Hex  | Typical use                  |
| ----- | ---- | ---------------------------- |
| ~6%   | `10` | Inactive selections          |
| ~8%   | `15` | Scrollbar resting state      |
| ~9%   | `18` | Word highlights              |
| ~12%  | `20` | Active selections, overlays  |
| ~15%  | `25` | Scrollbar hover              |
| ~27%  | `44` | Find match highlights        |
| ~67%  | `aa` | Primary find match           |

## Section 2: `tokenColors` — Syntax Highlighting

This array of objects maps TextMate scopes to colors. Each entry has:

```json
{
  "name": "Human-readable label",
  "scope": ["scope.name.one", "scope.name.two"],
  "settings": {
    "foreground": "#hex",
    "fontStyle": "italic"  // optional: italic, bold, underline, bold italic
  }
}
```

### How to add a new token color

1. Open a file of the target language in VS Code
2. Run **Developer: Inspect Editor Tokens and Scopes** from the command palette
3. Click on the token you want to color — it shows the TextMate scope stack
4. Find the most specific scope that matches only what you want
5. Add a new entry to `tokenColors` with that scope and a color from the **Syntax Colors** palette

### Scope specificity

More specific scopes override less specific ones. The order in the array also matters — later entries override earlier ones for the same scope.

```
keyword                          → matches all keywords
keyword.control                  → matches only control flow (if, for, while)
keyword.control.flow.ts          → matches only TypeScript flow keywords
```

### JSON keys

All JSON keys use `#79c0ff` (constant blue) regardless of nesting level for a clean, consistent appearance.

## Section 3: `semanticTokenColors` — Language-Aware Overrides

These override `tokenColors` when the language server provides semantic information. They're simpler — just `tokenType.modifier: "#hex"`:

```json
{
  "variable.declaration": "#c9d1d9",
  "variable.readonly": "#79c0ff",
  "function.declaration": "#d2a8ff",
  "type": "#ffa657",
  "class": "#ffa657",
  "interface": "#ffa657",
  "enum": "#ffa657",
  "enumMember": "#79c0ff"
}
```

Semantic tokens are more accurate than TextMate for languages with good language server support (TypeScript, JavaScript, Python, Java, C#, Go, Rust).

### How to add a new semantic token

1. Run **Developer: Inspect Editor Tokens and Scopes** on the target token
2. Look at the "semantic token type" and "semantic token modifiers" fields
3. Combine as `type.modifier` (e.g., `variable.readonly`)
4. Add to the `semanticTokenColors` object

## Testing Changes

1. Press `F5` in the project to open an Extension Development Host
2. Open files in various languages: TypeScript, Python, JSON, Markdown, CSS, HTML
3. Check both syntax colors and workbench UI
4. Use **Developer: Inspect Editor Tokens and Scopes** to verify scopes are colored correctly
5. Test sidebar, terminal, panels, search, settings, and quick input views

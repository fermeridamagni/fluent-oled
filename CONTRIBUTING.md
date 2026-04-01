# Contributing to Fluent OLED

Thank you for your interest in contributing to Fluent OLED. This guide will help you get started.

## Getting Started

1. **Fork** the repository on GitHub
2. **Clone** your fork locally:

   ```sh
   git clone https://github.com/<your-username>/fluent-oled.git
   cd fluent-oled
   ```

3. **Install** dependencies:

   ```sh
   bun install
   ```

4. **Test** the theme by pressing `F5` in VS Code to open an Extension Development Host window

## Project Structure

```
fluent-oled/
├── assets/                # Preview images and media
├── themes/
│   └── Fluent OLED-color-theme.json   # Theme definition
├── package.json           # Extension manifest
├── CHANGELOG.md           # Version history
├── CONTRIBUTING.md        # This file
├── LICENSE                # MIT License
└── README.md              # Documentation
```

## Making Changes

### Theme Colors

The theme is defined in `themes/Fluent OLED-color-theme.json`. It contains three sections:

- **`colors`** — Workbench UI colors (editor, sidebar, tabs, etc.)
- **`tokenColors`** — Syntax highlighting via TextMate scopes
- **`semanticTokenColors`** — Enhanced highlighting for languages with semantic token support

### Design Principles

When making changes, keep these principles in mind:

1. **Pure black backgrounds** — All primary surfaces must use `#000000`
2. **Monochrome UI** — Accent colors should be white/gray only — no color in the chrome
3. **Minimalism** — If a border or decoration isn't necessary, remove it
4. **GitHub syntax palette** — Code highlighting should follow the GitHub Dark color scheme
5. **Functional colors preserved** — Git decorations, errors, warnings, and terminal ANSI colors should remain semantic

### Testing

1. Press `F5` to launch the Extension Development Host
2. Verify changes across multiple file types (TypeScript, Python, JSON, Markdown, CSS, HTML)
3. Check the sidebar, terminal, panels, search, and settings views
4. Test with both light and dark system appearances

## Submitting Changes

1. Create a **feature branch** from `main`:

   ```sh
   git checkout -b feat/your-change
   ```

2. Make your changes and **commit** with a clear message:

   ```sh
   git commit -m "feat: add support for notebook cell borders"
   ```

3. **Push** your branch and open a **Pull Request** against `main`

### Commit Convention

This project follows [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` — New features or color additions
- `fix:` — Bug fixes or color corrections
- `docs:` — Documentation changes
- `chore:` — Maintenance, tooling, or config changes

## Reporting Issues

If you find a color that looks off, a missing scope, or a UI element that doesn't match the theme:

1. Open an [Issue](https://github.com/fermeridamagni/fluent-oled/issues)
2. Include a **screenshot** showing the problem
3. Mention the **file type** and **language** you were editing
4. Note which **editor** you're using (VS Code, Cursor, Antigravity, etc.)

## Code of Conduct

Be kind, be respectful. We're all here to build something great.

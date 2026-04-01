# Fluent OLED

A pure black, minimalist theme for VS Code and VS Code-based editors.

![Editor preview](assets/preview-editor.png)

## About

Fluent OLED strips the editor down to what matters — your code. Every surface is `#000000` pure black, designed for OLED displays where true black means pixels are completely off. No noise, no distractions.

![Code preview](assets/preview-code.png)

## Features

- **Pure black** — `#000000` across the entire workbench: editor, sidebar, tabs, status bar, terminal, panels
- **Monochrome UI** — white/gray accents only, zero color in the chrome
- **GitHub syntax highlighting** — familiar, readable token colors from the GitHub Dark palette
- **Semantic highlighting** — enhanced colors for TypeScript, JavaScript, and other languages with semantic token support
- **Ultra-subtle borders** — hairline `#111111` separators that disappear on OLED screens
- **Universal** — works on VS Code, Cursor, Windsurf, Antigravity, and any VS Code-based editor

## Install

### From source

1. Clone this repository into your extensions folder:

   ```sh
   git clone https://github.com/fermeridamagni/fluent-oled ~/.vscode/extensions/fluent-oled
   ```

2. Restart your editor
3. Open **Preferences → Color Theme** and select **Fluent OLED**

## Color Palette

### UI

| Element          | Color     |
| ---------------- | --------- |
| Background       | `#000000` |
| Surface          | `#0a0a0a` |
| Border           | `#111111` |
| Foreground       | `#c9d1d9` |
| Muted            | `#8b949e` |
| Dimmed           | `#6e7681` |

### Syntax

| Token            | Color     | Preview |
| ---------------- | --------- | ------- |
| Keywords         | `#ff7b72` | 🔴      |
| Functions        | `#d2a8ff` | 🟣      |
| Strings          | `#a5d6ff` | 🔵      |
| Constants        | `#79c0ff` | 🔵      |
| Types / Classes  | `#ffa657` | 🟠      |
| Tags             | `#7ee787` | 🟢      |
| Comments         | `#8b949e` | ⚪      |

## License

[MIT](LICENSE)

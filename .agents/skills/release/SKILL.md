---
name: release
description: How to release a new version of the Fluent OLED VS Code theme to the VS Code Marketplace and Open VSX. Use this skill whenever the user wants to publish, release, bump a version, or ship a new update of the extension. Also use when they mention versioning, changelogs, or marketplace publishing.
---

# Release a New Version

This project uses a GitHub Actions workflow (`.github/workflows/publish.yml`) that automatically publishes to both the **VS Code Marketplace** and **Open VSX** whenever a GitHub release is created. You never need to run `vsce publish` or `ovsx publish` manually.

## Release Steps

### 1. Update `CHANGELOG.md`

Add a new section at the top following [Keep a Changelog](https://keepachangelog.com/) format. Use the version you plan to tag:

```markdown
## [0.1.0] - YYYY-MM-DD

### Added
- New token scope for XYZ

### Changed
- Adjusted border color from #111111 to #0f0f0f

### Fixed
- Missing color for notebook cell borders
```

Use these categories as needed: `Added`, `Changed`, `Fixed`, `Removed`.

### 2. Commit and push

```sh
git add -A
git commit -m "chore: prepare release 0.1.0"
git push origin master
```

Follow the project's [Conventional Commits](https://www.conventionalcommits.org/) convention:
- `feat:` — New features
- `fix:` — Bug fixes
- `docs:` — Documentation
- `chore:` — Maintenance, version bumps

### 3. Create a GitHub release

Use the GitHub CLI to create a release. The tag **must** start with `v` (e.g., `v0.1.0`) — the workflow extracts the version by stripping the `v` prefix and automatically updates `package.json` before packaging.

```sh
gh release create v0.1.0 \
  --repo fermeridamagni/fluent-oled \
  --title "v0.1.0 — Short Description" \
  --notes "## What's New

- Feature or fix description
- Another change

Full changelog: [CHANGELOG.md](https://github.com/fermeridamagni/fluent-oled/blob/master/CHANGELOG.md)"
```

### 4. Verify

The GitHub Actions workflow will automatically:
1. Extract the version from the tag (`v0.1.0` → `0.1.0`)
2. Update `package.json` version
3. Package the `.vsix`
4. Publish to **VS Code Marketplace** (using `VSCE_PAT` secret)
5. Publish to **Open VSX** (using `OVSX_PAT` secret)
6. Attach the `.vsix` file to the GitHub release

Check the workflow run:
```sh
gh run list --repo fermeridamagni/fluent-oled --limit 1
```

Verify the extension is live:
```sh
npx -y @vscode/vsce show fermeridamagni.fluent-oled
```

## Important Notes

- The `VSCE_PAT` and `OVSX_PAT` secrets are stored in the GitHub repository settings. If they expire, regenerate them from [Azure DevOps](https://dev.azure.com) and [Open VSX](https://open-vsx.org) respectively, then update with `gh secret set`.
- The version in `package.json` that you commit doesn't strictly need to match the tag — the workflow overwrites it from the tag. But keep them in sync for consistency.
- Always update `CHANGELOG.md` before creating the release so the changelog is included in the packaged `.vsix`.

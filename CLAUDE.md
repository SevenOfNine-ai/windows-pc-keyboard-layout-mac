# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

German Windows-PC keyboard layout for macOS — maps special characters (`{`, `}`, `[`, `]`, etc.) to the same positions as on a Windows PC keyboard. The layout is a non-executable XML bundle, not software.

**Primary editor tool**: [Ukulele](https://software.sil.org/ukelele/) (GUI only — never hand-edit the `.keylayout` file)

## Key Commands

```bash
# Validate plist files
plutil -lint root/pc-win-de-keyboard.bundle/Contents/Info.plist
plutil -lint root/pc-win-de-keyboard.bundle/Contents/version.plist

# Rebuild icon from source PNG
bash icon/convert-png-to-icns.sh

# Install for current user (manual testing)
git lfs pull
cp -R root/pc-win-de-keyboard.bundle ~/Library/Keyboard\ Layouts/

# Install system-wide
sudo cp -R root/pc-win-de-keyboard.bundle "/Library/Keyboard Layouts/"
```

There is no automated build system, test framework, or CI pipeline. Testing is manual: install the bundle, enable "Deutsch - PC" in System Settings → Keyboard → Input Sources, and verify key output.

## Critical Constraint

**`German - PC.keylayout` must only be edited with Ukulele, never by hand.** XML structural errors break the entire layout, key code mismatches cause wrong characters, and dead-key state machine errors corrupt accent composition.

## Architecture

- `root/pc-win-de-keyboard.bundle/` — standard macOS `.bundle` package
  - `Contents/Resources/German - PC.keylayout` — Apple Keyboard Layout XML (the core file, ~484 lines)
  - `Contents/Info.plist` / `version.plist` — bundle metadata (version `1.0.1`)
  - `Contents/Resources/{de,en}.lproj/` — localization strings
- `icon/convert-png-to-icns.sh` — converts `icon/layout-icon.png` → `.icns` via macOS `sips`
- `docs/security-audit.md` — security analysis confirming zero vulnerabilities
- `images/keyboard-alt.png` — reference image for Alt-layer special character mappings

The keylayout XML contains: key-to-character maps for base/Shift/Option layers, and a finite state machine for dead-keys (circumflex, acute, grave, diaeresis). Tilde `~` is intentionally **not** a dead-key.

## Conventions

- Follow `.editorconfig`: Markdown uses UTF-8 + 4-space indent; JSON uses tabs; YAML uses 2-space indent; LF line endings everywhere
- Commit messages: short, imperative, German (e.g., `Layout Icon angepasst`). One logical change per commit
- New scripts/assets: lowercase kebab-case naming
- Git LFS is used for binary files (icons, images) — run `git lfs pull` after cloning
- DMG installers are created via Ukulele GUI (`File → Export Installer Disk Image...`), not committed to the repo

## Detailed Agent Guidelines

See [AGENTS.md](AGENTS.md) for comprehensive file-level editing rules, safe/unsafe task lists, and PR guidelines.

# AI Agent Guidelines for German Windows-PC Keyboard Layout

Agent instructions for working with the `windows-pc-keyboard-layout-mac` repository. These guidelines are synchronized across multiple platforms (Cursor, Claude, OpenCode) via OpenPackage.

## Project Context

**Repository**: windows-pc-keyboard-layout-mac  
**Purpose**: Distribute a German Windows-PC keyboard layout for macOS  
**Key Technology**: Apple Keyboard Layout XML (`System/Library/DTDs/KeyboardLayout.dtd`)  
**Status**: ✅ Cryptographically transparent, no security risk ([Security Audit](docs/SECURITY-AUDIT.md))

## Essential Guardrails

### 🚫 Critical Safety Boundaries

- **DO NOT edit** `German - PC.keylayout` by hand—use [Ukulele](https://software.sil.org/ukelele/) GUI only
- **DO NOT generate** `.icns` files manually—use `bash icon/convert-png-to-icns.sh`
- **DO NOT commit** generated artifacts like `.dmg` installers
- **DO NOT test** keyboard layouts without macOS environment (requires manual verification)

### ✅ Safe to Edit

- `README.md`, `docs/*.md` — documentation
- `.editorconfig`, `.vscode/` — configuration files
- `AGENTS.md`, `.openpackage/` — agent instructions
- Plist metadata (`Info.plist`, `version.plist`)
- Localization strings (`de.lproj/`, `en.lproj/`)
- Shell scripts with enhanced error handling

## Project Structure Reference

```
pc-win-de-keyboard.bundle/
├── Contents/
│   ├── Info.plist                          (bundle metadata)
│   ├── version.plist                       (version: 1.0.0)
│   └── Resources/
│       ├── German - PC.keylayout           (⚠️ DO NOT edit by hand)
│       ├── German - PC.icns                (⚠️ auto-generated)
│       ├── de.lproj/InfoPlist.strings      (✅ safe to edit)
│       └── en.lproj/InfoPlist.strings      (✅ safe to edit)
icons/                                       (source files for .icns)
images/                                      (README screenshots)
docs/                                        (security audit, technical docs)
```

## Common AI Tasks & Scope

### Documentation & Analysis Tasks
- Write or update `README.md` with installation/usage instructions
- Create new documentation in `docs/` (e.g., troubleshooting guides)
- Review keyboard layout mappings for correctness and completeness
- Generate architectural diagrams explaining layout structure

### Configuration & Metadata Tasks
- Update version numbers in `Info.plist` and `version.plist`
- Modify localization strings in `de.lproj/` and `en.lproj/`
- Refine `.editorconfig` formatting rules
- Update VS Code settings in `.vscode/settings.json`

### Shell Script Maintenance
- Review and enhance `icon/convert-png-to-icns.sh`
- Add error handling and validation
- Document CLI commands (e.g., `plutil -lint` validation)
- Create new utility scripts for development workflows

### Tasks Requiring Human Intervention
- Editing `.keylayout` XML directly (use Ukulele GUI)
- Creating/exporting `.dmg` installers (use Ukulele GUI)
- Icon design and PNG manipulation (use image editor)
- Testing keyboard layout output on macOS (requires live macOS machine)

## Technical Architecture

### Keyboard Layout Format (XML)

- **Standard**: Apple Keyboard Layout DTD
- **Structure**:
  - `keyMapSet id="fb0"` — main layout container
  - `keyMap index="0"` — unmodified keys
  - `keyMap index="1"` — Shift-modified keys
  - `keyMap index="2"` — Option/Alt layer (AltGr equivalent)
  - `actions` — dead-key composition rules

### Dead-Key Implementation

Dead-keys use a finite-state machine approach:
- **States 1–5**: circumflex `^`, acute `´`, grave `` ` ``, diaeresis `¨`, tilde `~`
- **Exception**: Tilde `~` is NOT a dead-key (produces `~` immediately, never a state transition)
- **Composition**: Documented in `<terminators>` section
- **Visual Reference**: See `images/keyboard-alt.png` for special character mappings

### Bundle Packaging (macOS)

- **Format**: `.bundle` (Apple standard package format)
- **Localization**: Two variants: `de.lproj` (German), `en.lproj` (English)
- **Installation Paths**:
  - User-specific: `~/Library/Keyboard Layouts/`
  - System-wide: `/Library/Keyboard Layouts/` (requires `sudo`)
- **Version Tracking**: Dual tracking in `Info.plist` (`CFBundleVersion`) and `version.plist`
- **Bundle ID**: `org.sil.ukelele.keyboardlayout.pc-win-de-keyboard`

### macOS Compatibility

- **Minimum Target**: macOS Sierra (10.12) and later
- **Activation**: Manual via System Settings → Keyboard → Input Sources
- **Lock Screen Limitation**: Custom layouts not available on login/lock screen (OS restriction)

## Code Style & File Format Rules

Follow `.editorconfig` strictly:

- **Default indentation**: 4 spaces (all file types)
- **YAML**: 2-space indentation
- **JSON**: Tab indentation
- **Markdown**: UTF-8 BOM + 4-space indentation
- **Line endings**: LF (Unix-style)
- **Trailing whitespace**: Remove

### Naming Conventions

- Preserve existing bundle/resource names (`German - PC.keylayout`, `de.lproj`, `en.lproj`)
- New scripts/assets: lowercase kebab-case (e.g., `convert-png-to-icns.sh`)

## Validation & Build Commands

Run these to verify correctness:

```bash
# Validate bundle plists
plutil -lint pc-win-de-keyboard.bundle/Contents/Info.plist
plutil -lint pc-win-de-keyboard.bundle/Contents/version.plist

# Download Git LFS objects (for icons, images)
git lfs pull

# Rebuild icon from PNG source
bash icon/convert-png-to-icns.sh

# Install bundle locally (system-wide)
sudo cp -R pc-win-de-keyboard.bundle "/Library/Keyboard Layouts/"
```

## Testing & Verification

**Note**: No automated test framework. Manual testing required on macOS.

### Manual Test Checklist

1. Install the bundle via system-wide copy command above
2. Enable **Deutsch - PC** in System Settings → Keyboard → Input Sources
3. Verify standard key output (typing, numbers, symbols)
4. Test `Option` key (Alt layer) for special characters
5. Confirm dead-key behavior:
   - `^` (circumflex, Shift+Option+J) followed by vowel
   - `` ` `` (grave, `Option+`` ` ``) followed by vowel
   - `´` (acute, `Option+E`) followed by vowel
6. Verify `~` (Shift+Option+N) outputs `~` immediately (not dead-key)
7. Compare against visual reference in `images/keyboard-alt.png`

### Record Test Results

Document in commit messages or PRs:
- macOS version tested
- Test scope (standard keys, special characters, dead-keys, Alt layer)
- Any unexpected behavior

## Git & Pull Request Standards

### Commit Message Style

- **Language**: Short, imperative form (e.g., `Layout Icon angepasst`, `Fix dead-key composition`)
- **Scope**: Prefer one logical change per commit
- **Atomic commits**: Avoid mixing keylayout, docs, and icon updates in single commit

### Pull Request Requirements

Include:
- **Purpose**: Clear description of what changed and why
- **Changed files**: List of modified files
- **Test results**: macOS version, manual test checklist
- **Screenshots**: Updated if keyboard mappings or setup steps changed
- **No artifacts**: Don't commit `.dmg` installers (already in `.gitignore`)

## Security & Transparency

**Certification**: This keyboard layout poses **NO security risk** to users. Full analysis: [Security Audit](docs/SECURITY-AUDIT.md)

### Security Model

- ✅ **XML is non-executable**: Simple data structure, no code execution
- ✅ **All mappings transparent**: Every key-to-character mapping visible in plain-text XML
- ✅ **No I/O capabilities**: Cannot read, write, transmit data
- ✅ **Deterministic**: Same key press always produces same output
- ✅ **No hidden characters**: Only standard, commonly-used Unicode characters
- ✅ **No installation hooks**: Simple file copy, no scripts or daemons execute
- ✅ **No privilege abuse**: Sudo only for standard installations to `/Library/`

Installation is safe even with elevated privileges.

## OpenPackage Integration

This agent guidelines file is maintained in `.openpackage/rules/` as a universal format and automatically synced to platform-specific agent instruction directories via `opkg apply`.

**Sync platforms**: Cursor, Claude, OpenCode

To update all platforms after changes:
```bash
opkg apply --platforms cursor,claude,opencode
```

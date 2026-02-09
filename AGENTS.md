# Repository Guidelines

## Project Structure & Module Organization
- `pc-win-de-keyboard.bundle/` is the distributable keyboard layout bundle.
- `pc-win-de-keyboard.bundle/Contents/Resources/German - PC.keylayout` is the primary layout definition.
- `pc-win-de-keyboard.bundle/Contents/Info.plist` and `pc-win-de-keyboard.bundle/Contents/version.plist` hold bundle metadata.
- `images/` contains README screenshots used for user-facing documentation.
- `icon/` contains icon assets and `icon/convert-png-to-icns.sh` for regenerating `.icns`.
- `.editorconfig` and `.vscode/` define editing and linting defaults for contributors.

## Build, Test, and Development Commands
- `sudo cp -R pc-win-de-keyboard.bundle "/Library/Keyboard Layouts/"`: install locally for manual verification.
- `bash icon/convert-png-to-icns.sh`: rebuild the icon from `icon/layout-icon.png` into bundle resources.
- `plutil -lint pc-win-de-keyboard.bundle/Contents/Info.plist`: validate main bundle plist.
- `plutil -lint pc-win-de-keyboard.bundle/Contents/version.plist`: validate version plist.
- DMG export is done in Ukulele (`File -> Export Installer Disk Image...`); no CLI build pipeline is maintained in this repo.

## Coding Style & Naming Conventions
- Follow `.editorconfig` exactly: default 4-space indentation, LF line endings, trailing-whitespace trimming.
- Markdown files use UTF-8 BOM and 4-space indentation; JSON files use tabs; YAML uses 2 spaces.
- Prefer editing `German - PC.keylayout` with Ukulele to avoid accidental structural regressions.
- Keep existing bundle/resource naming stable (for example, `German - PC.keylayout`, `de.lproj`, `en.lproj`).
- Name new scripts and image assets descriptively with lowercase kebab-case where possible.

## Testing Guidelines
- There is no automated test framework; validation is manual on macOS.
- After changes, install the bundle, enable **Deutsch - PC**, and verify key output in normal typing and with `Alt`.
- Confirm dead-key behavior for `^`, `` ` ``, and `´`, and ensure `~` remains a non-dead key.
- Compare special-character mappings against `images/keyboard-alt.png`.
- Record tested macOS version and test scope in your PR.

## Commit & Pull Request Guidelines
- Keep commit messages short and imperative; repository history is mostly German (for example, `Layout Icon angepasst`).
- Make one logical change per commit; avoid mixing keylayout, docs, and icon updates unless tightly coupled.
- PRs should include: purpose, changed files, manual test notes, and updated screenshots when mappings or setup steps change.
- Do not commit generated release artifacts such as `pc-win-de-keyboard.dmg` (already ignored).

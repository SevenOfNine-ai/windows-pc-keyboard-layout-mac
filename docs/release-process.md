# GitBook Release Process (GitHub Pages)

This repository publishes documentation from `docs/` to GitHub Pages using a GitHub Actions workflow and GitBook-compatible structure.

## Workflow File

- `.github/workflows/gitbook-pages.yml`

GitBook/Honkit structure config:

- `docs/book.json` (maps to lowercase `readme.md` and `summary.md`)

## Trigger

The workflow runs on:

- published GitHub releases
- manual dispatch (`workflow_dispatch`)

## Release Steps

1. Update docs in `docs/`.
2. Ensure version metadata is correct (`openpackage.yml`, plist files if needed).
3. Commit and push changes.
4. Create and publish a GitHub release (typically from a `vX.Y.Z` tag).
5. Wait for `gitbook-pages` workflow to finish.
6. Read published docs on GitHub Pages.

## OpenPackage Publish in CI

OpenPackage publishing is documented in:

- [OpenPackage Publishing](openpackage-publishing.md)

The OpenPackage workflow runs independently from docs deployment and publishes the package to the OpenPackage registry.

## One-Time Repository Setup

In GitHub repository settings:

1. Open `Settings -> Pages`
2. Set source to `GitHub Actions`

## Build Details

The workflow:

1. installs `honkit` (GitBook-compatible builder)
2. builds docs from `docs/` into `_site/`
3. copies shared images from `images/` into `_site/images/`
4. deploys `_site/` to GitHub Pages

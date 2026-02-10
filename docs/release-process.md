# GitBook Deployment Process (GitHub Pages)

This repository publishes documentation from `docs/` to GitHub Pages using a GitHub Actions workflow and GitBook-compatible structure.

## Workflow File

- `.github/workflows/gitbook-pages.yml`

GitBook/Honkit structure config:

- `docs/book.json` (maps to lowercase `readme.md` and `summary.md`)

## Trigger

The workflow runs on:

- push to `main` (including merged pull requests)
- manual dispatch (`workflow_dispatch`)

## Deployment Steps

1. Update docs in `docs/`.
2. Commit changes and merge into `main`.
3. Wait for `gitbook-pages` workflow to finish.
4. Read published docs on GitHub Pages.

## OpenPackage Publish in CI

OpenPackage publishing is documented in:

- [OpenPackage Publishing](openpackage-publishing.md)

The OpenPackage workflow runs independently from docs deployment and publishes the package to the OpenPackage registry.

## One-Time Repository Setup

In GitHub repository settings:

1. Open `Settings -> Pages`
2. Set source to `GitHub Actions`
3. Open `Settings -> Environments -> github-pages`
4. Under deployment branches/tags, allow branch `main` (or configure no restriction)

If this is restricted to tags only, deployments from `main` will be rejected by environment protection.

## Build Details

The workflow:

1. installs `honkit` (GitBook-compatible builder)
2. builds docs from `docs/` into `_site/`
3. copies shared images from `images/` into `_site/images/`
4. deploys `_site/` to GitHub Pages

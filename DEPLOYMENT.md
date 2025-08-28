# Deployment to GitHub Pages

## Overview

This document outlines the steps needed to deploy the Flowbite Marketing UI website to GitHub Pages.

## Prerequisites

- A GitHub repository for the project
- GitHub Pages enabled for the repository
- Node.js and NPM installed
- Hugo installed

## Deployment Process

### 1. GitHub Actions Workflow

Create a GitHub Actions workflow in `.github/workflows/gh-pages.yml` with the following content:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main, master]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: npm

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: "0.128.0"
          extended: true

      - name: Install dependencies
        run: npm ci

      - name: Update browserslist database
        run: npx browserslist@latest --update-db

      - name: Build with webpack
        run: npm run build:webpack

      - name: Build with Hugo
        run: hugo

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./_gh_pages

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v3
```

### 2. Hugo Configuration

The `config.yml` file already has the correct `publishDir: "_gh_pages"` setting for GitHub Pages deployment.

### 3. Build Process

The existing build process works well for GitHub Pages:

- `npm run build:webpack` - Builds the webpack assets
- `hugo` - Builds the Hugo site to `_gh_pages` directory (using config.yml setting)

### 4. GitHub Pages Setup

1. Go to your repository settings
2. Navigate to "Pages" in the sidebar
3. Select "GitHub Actions" as the source
4. The workflow will automatically deploy when changes are pushed to the main branch

## Troubleshooting

- If the build fails, check that all dependencies are correctly specified in package.json
- Ensure Hugo is properly configured for the GitHub Actions environment
- Verify that the publishDir in config.yml is set to "\_gh_pages"
- If you encounter a browserslist error during webpack build, the workflow automatically updates the browserslist database before building

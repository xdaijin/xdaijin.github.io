# AGENTS.md - OpenCode Agent Instructions

## Project Overview
Hugo static blog using hugo-theme-reimu theme (git submodule). Deployed to GitHub Pages.

## Important Commands

### Development
```bash
hugo serve          # Start local dev server
hugo server         # Alias, same as above
```

### Production Build (CI uses this)
```bash
hugo --gc --minify --baseURL "https://xdaijin.github.io/" --cacheDir /tmp/hugo_cache
```

## Tool Versions (CI)
- Hugo: v0.161.1+extended
- Dart Sass: 1.93.2
- Go: 1.25.3
- Node.js: 22.20.0

## Project Structure
```
root/
├── hugo.toml              # Main Hugo config (languages, permalinks, markup)
├── config/_default/params.yml  # Theme-specific configuration
├── content/               # Content files
│   ├── post/             # Blog posts
│   ├── archives/         # Archives page
│   ├── about.md          # About page
│   └── friend.md         # Friends page
├── data/                 # Data files (covers.yml, friends.yml)
├── static/               # Static assets (avatar, images, favicon)
├── themes/hugo-theme-reimu/  # Theme (git submodule - DO NOT MODIFY)
├── resources/            # Hugo generated resources
└── public/               # Build output (git-ignored)
```

## Key Configuration Files

### hugo.toml
- Site title, language settings (zh-CN default)
- Goldmark markdown extensions (passthrough for LaTeX)
- Code highlighting config
- Algolia search output format
- Permalink patterns

### config/_default/params.yml
- All theme customization: comments, search, dark mode, widgets, etc.
- Refer to theme documentation for all options

## Content Creation

### New Post
Create files in `content/post/`. Required front matter:
```yaml
---
title: Your Title
date: 2025-05-21T12:00:00+08:00
lastmod: 2025-05-21T12:00:00+08:00
categories: [Category]
tags: [tag1, tag2]
draft: false
---
```

### Optional Front Matter
- `math: true` - Enable LaTeX (KaTeX/MathJax)
- `mermaid: true` - Enable Mermaid diagrams
- `cover: <url>` - Custom post cover image
- `toc: false` - Disable table of contents
- `sidebar: left|right` - Sidebar position
- `comments: false` - Disable comments

## Important Gotchas

1. **Theme is a submodule** - Do NOT modify files in `themes/hugo-theme-reimu/`. Override by creating same paths in root directory.

2. **Hugo Extended required** - Theme uses SCSS which needs Hugo extended version.

3. **Git submodules** - When cloning: `git clone --recursive <repo-url>` or after clone: `git submodule update --init --recursive`

4. **No root package.json** - Node dependencies only exist within the theme, not needed for regular site operation.

5. **Algolia search** - Site generates `public/algolia.json` but upload to Algolia is manual.

## CI/CD
- GitHub Actions workflow: `.github/workflows/hugo.yaml`
- Deploys to GitHub Pages on `main` branch push
- Uses caching for Hugo build artifacts

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal blog and portfolio website hosted on GitHub Pages. Built with Jekyll (based on Jekyll Now theme) to showcase data science projects, technical writing, and explorations in mathematics, computer science, AI, and quantum computing.

## Development Commands

### Local Development
```bash
# Install Jekyll and dependencies (requires Ruby)
gem install jekyll bundler

# Serve the site locally
jekyll serve

# Serve with live reload and drafts
jekyll serve --livereload --drafts

# Build the site (output to _site/)
jekyll build

# Build for production
JEKYLL_ENV=production jekyll build
```

### Creating Content
```bash
# Create a new post (follow naming convention: YYYY-MM-DD-title.md)
# Posts go in _posts/ directory
touch _posts/$(date +%Y-%m-%d)-your-post-title.md

# Create a new page
# Pages go in root directory
touch your-page-name.md
```

## Site Architecture

### Jekyll Structure
```
realtimdunbar.github.io/
├── _config.yml          # Site configuration
├── _posts/              # Blog posts (markdown)
├── _layouts/            # Page templates
│   ├── default.html     # Base layout with header/footer
│   ├── page.html        # Static page layout
│   └── post.html        # Blog post layout
├── _includes/           # Reusable components
│   ├── analytics.html   # Google Analytics integration
│   ├── disqus.html      # Comments integration
│   ├── meta.html        # Meta tags and SEO
│   └── svg-icons.html   # Social media icons
├── _sass/               # Sass/SCSS stylesheets
│   ├── _reset.scss      # CSS reset
│   ├── _variables.scss  # Style variables
│   ├── _highlights.scss # Code syntax highlighting
│   └── _svg-icons.scss  # Icon styles
├── images/              # Static images for posts
├── style.scss           # Main stylesheet (imports _sass files)
├── index.html           # Home page (lists all posts)
└── about.md             # About page
```

### Content Flow
1. **Posts** (`_posts/*.md`)
   - Named with convention: `YYYY-MM-DD-title.md`
   - Front matter specifies layout, title, author, date
   - Written in Markdown (Kramdown flavor)
   - Supports MathJax for mathematical equations

2. **Layouts** (`_layouts/*.html`)
   - `default.html`: Base template with masthead (avatar, site name, navigation) and footer
   - `page.html`: Extends default for static pages
   - `post.html`: Extends default for blog posts

3. **Includes** (`_includes/*.html`)
   - Modular components injected into layouts
   - `meta.html`: SEO, Open Graph, Twitter Card metadata
   - `svg-icons.html`: Social media links in footer
   - `analytics.html`: Google Analytics tracking code
   - `disqus.html`: Comment system integration

4. **Styles** (`_sass/*.scss` + `style.scss`)
   - `style.scss`: Entry point, imports all partials
   - SCSS compiled to CSS by Jekyll
   - Variables defined in `_variables.scss`
   - Syntax highlighting in `_highlights.scss`

## Key Configuration

### _config.yml Settings
- **Markdown**: Kramdown with GFM (GitHub Flavored Markdown)
- **Syntax Highlighter**: Rouge (GitHub Pages compatible)
- **Permalink Structure**: `/:title/` (clean URLs without dates)
- **Theme**: `jekyll-theme-minimal` (with heavy customization)
- **Plugins**:
  - `jekyll-sitemap`: Automatic sitemap generation
  - `jekyll-feed`: RSS/Atom feed generation

### MathJax Support
MathJax 2.7.1 is loaded in `default.html` for rendering mathematical equations in posts. Use standard LaTeX syntax:
- Inline: `$equation$`
- Display: `$$equation$$`

### Post Front Matter Format
```yaml
---
title: "Post Title"
author: "Tim Dunbar"
date: "Month Day Year"
layout: post  # optional, defaults to post
---
```

## Content Guidelines

### Blog Post Topics
Based on existing posts, the blog focuses on:
- Data science and statistical analysis (Benford's Law, clustering)
- Machine learning (Naive Bayes classifiers)
- Spatial analysis and crime data
- Mathematics and computer science explorations
- AI, quantum computing, and consciousness

### Writing Style
- Technical but accessible
- Includes code snippets and mathematical notation
- Visual support with charts/graphs in `images/` directory
- Analytical and exploratory in nature

## Deployment

### GitHub Pages
- **Hosting**: Automatically deployed via GitHub Pages
- **Branch**: `master` branch serves the site
- **Custom Domain**: Configured via CNAME file (currently empty)
- **Build**: GitHub Pages builds Jekyll automatically on push
- **URL Pattern**: `https://realtimdunbar.github.io/`

### Publishing Workflow
1. Create post in `_posts/` with correct date format
2. Add images to `images/` directory if needed
3. Preview locally with `jekyll serve`
4. Commit and push to `master` branch
5. GitHub Pages automatically rebuilds and deploys

## Site Information

- **Owner**: Tim Dunbar
- **Email**: timothy.c.dunbar@me.com
- **GitHub**: realtimdunbar
- **Description**: Explorer (data science, mathematics, AI, quantum computing)
- **Theme Base**: Jekyll Now (heavily customized)

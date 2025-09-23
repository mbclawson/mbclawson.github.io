# mattclawson.dev
A personal website and blog for Matt Clawson, Data Engineer, built with Hugo and hosted on GitHub Pages.

## What is Hugo?
Hugo is a fast and flexible static site generator written in Go. It takes your content (written in Markdown) and templates, then generates a complete static website that can be hosted anywhere. Static sites are pre-built HTML files that don't require a server-side database or complex backend - they're fast, secure, and easy to host.

## What is a Static Site?
A static site consists of pre-generated HTML, CSS, and JavaScript files that are served directly to users without server-side processing. Benefits include:

- **Fast loading times** - No database queries or server processing
- **High security** - No server-side vulnerabilities
- **Easy hosting** - Can be hosted on CDNs like GitHub Pages, Netlify, Vercel
- **Version control** - Content and code can be tracked in git
- **Cost effective** - Many hosting options are free or very cheap

## Project Structure
```
personal-site/
archetypes/          # Content templates for new posts
assets/              # Asset processing (SCSS, JS, images)
content/             # Your content (Markdown files)
   posts/            # Blog posts go here
data/                # Data files (YAML, JSON, TOML)
i18n/                # Translation files
layouts/             # Custom HTML templates (if needed)
public/              # Generated static site (git ignored)
resources/           # Hugo's cache and processed assets
static/              # Static files (copied as-is to public/)
hugo.toml            # Site configuration
```

## Quick Start

### Prerequisites

1. Install Hugo: https://gohugo.io/installation/
   ```bash
   # macOS
   brew install hugo
   ```

### Local Development

1. **Start development server:**
   ```bash
   hugo server -D
   ```
   - Site will be available at http://localhost:1313
   - `-D` flag includes draft posts
   - Auto-reloads when you make changes

3. **Create new content:**
   ```bash
   # Create a new blog post
   hugo new posts/my-new-post.md

   # Create a new page
   hugo new about.md
   ```

4. **Build for production:**
   ```bash
   hugo
   ```
   - Generates site in `public/` directory
   - Only includes published content (draft: false)

## Key Hugo Commands

| Command | Description |
|---------|-------------|
| `hugo server` | Start development server |
| `hugo server -D` | Start server including drafts |
| `hugo new posts/title.md` | Create new blog post |
| `hugo new about.md` | Create new page |
| `hugo` | Build site for production |
| `hugo --minify` | Build with minified output |
| `hugo check` | Check for issues |

## Content Management

### Writing Posts
1. Create new post: `hugo new posts/my-post.md`
2. Edit the frontmatter (metadata between `+++`)
3. Set `draft = false` when ready to publish
4. Write content in Markdown below the frontmatter

### Frontmatter Example
```toml
+++
title = "My Awesome Post"
date = 2025-09-21
description = "A brief description"
draft = false
tags = ["hugo", "web development"]
categories = ["tech"]
+++
```

## GitHub Pages Deployment

This site is configured to deploy to GitHub Pages. The workflow:

1. Push changes to the `main` branch
2. GitHub Actions builds the site using Hugo
3. Deploys the generated files to GitHub Pages
4. Site is live at your GitHub Pages URL

## Theme
This site uses the [Hugo Coder](https://github.com/luizdepra/hugo-coder) theme, which provides:
- Clean, minimal design
- Dark mode support
- Responsive layout
- Social media integration
- Syntax highlighting for code

## Configuration
Main settings are in `hugo.toml`:
- Site title, description, and author info
- Theme configuration
- Menu structure
- Social media links

## Useful Resources
- [Hugo Documentation](https://gohugo.io/documentation/)
- [Hugo Coder Theme Docs](https://github.com/luizdepra/hugo-coder)
- [Markdown Guide](https://www.markdownguide.org/)
- [GitHub Pages Docs](https://docs.github.com/en/pages)

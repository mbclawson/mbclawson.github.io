# mattclawson
My personal website and blog built with Hugo and hosted on GitHub Pages.

## What is Hugo?
Hugo is a fast and flexible static site generator written in Go. It takes your content (written in Markdown) and templates, then generates a complete static website that can be hosted anywhere. Static sites are pre-built HTML files that don't require a server-side database or complex backend - they're fast, secure, and easy to host.

## Project Structure
```
personal-site/
archetypes/          # Content templates for new posts
content/             # Your content (Markdown files)
   posts/            # Blog posts go here
public/              # Generated static site (git ignored)
resources/           # Hugo's cache and processed assets
static/              # Static files (copied as-is to public/)
hugo.toml            # Site configuration
```

## Using Hugo

| Command | Description |
|---------|-------------|
| `hugo server` | Start development server |
| `hugo server -D` | Start server including drafts |
| `hugo new posts/title.md` | Create new blog post |
| `hugo new about.md` | Create new page |
| `hugo` | Build site for production |
| `hugo --minify` | Build with minified output |
| `hugo check` | Check for issues |


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


---
layout: page
title: "Jekyll Quick Reference"
---

# Jekyll Quick Reference

## Directory Structure

```
.
├── _config.yml      # Site configuration
├── _posts/          # Blog posts (YYYY-MM-DD-title.md)
├── _notes/          # Custom collection
├── _layouts/        # HTML templates
├── _includes/       # Reusable snippets
├── index.md         # Home page
└── Gemfile          # Ruby dependencies
```

## Front Matter

Every page/post starts with YAML front matter:

```yaml
---
layout: post
title: "My Post Title"
date: 2026-01-01
---
```

## Running Locally

```bash
bundle install
bundle exec jekyll serve
```

Then visit `http://localhost:4000`.

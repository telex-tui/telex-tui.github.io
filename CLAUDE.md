# CLAUDE.md

Workflow and conventions for this repo. See `docs/internal/README.md` for full detail.

## What this is

Plain HTML blog. No static site generator, no build step. One `style.css`, posts in `blog/`.

## Key rules

- **Never add a new post to `blog/index.html` until explicitly asked to publish.**
- New posts go in `blog/your-slug.html` only.
- Match the header/footer/nav of existing posts exactly — it's identical across all files.
- HTML-escape `<`, `>`, `&` inside all `<pre>` blocks.

## Local preview

```bash
python3 -m http.server 8080
# http://localhost:8080/blog/your-post.html
```

## Syntax highlighting classes

`.kw` keywords · `.ty` types · `.fn` function names · `.str` strings · `.mac` macros · `.cm` comments + attributes · `.num` numbers

## Publishing checklist

1. Add `.post-item` entry to top of list in `blog/index.html`
2. `git add blog/your-post.html blog/index.html`
3. `git commit -m "Publish: Title"`
4. `git push` — GitHub Pages deploys from `main` automatically

# CLAUDE.md

Workflow and conventions for this repo. See `docs/internal/README.md` for full detail.

## What this is

Plain HTML blog. No static site generator, no build step. One `style.css`, posts in `blog/`.

## Key rules

- **Never add a new post to `blog/index.html` until explicitly asked to publish.**
- New posts go in `blog/your-slug.html` only.
- Match the header/footer/nav of existing posts exactly — it's identical across all files.
- HTML-escape `<`, `>`, `&` inside all `<pre>` blocks.
- **Every hidden (unpublished) page must be listed on `blog/drafted-ai-crazies.html`** — the "DraftedAICrazies" nav link. Add new drafts there, and remove a post from it when it's published to `blog/index.html`.
- Hidden pages get `<meta name="robots" content="noindex, nofollow">`.

## Hidden posts and DraftedAICrazies

A post is "hidden" when its file is in `blog/` but it isn't listed in `blog/index.html`. Hidden is not private: anyone with the URL can read it, and the repo is public.

`blog/drafted-ai-crazies.html` lists every hidden page and tells readers it's a test bed for unfinished, AI-assisted work they shouldn't rely on. It's linked from the nav on every page (Book · Blog · DraftedAICrazies · GitHub). See `docs/internal/README.md` for how it's organised.

## Local preview

```bash
python3 -m http.server 8080
# http://localhost:8080/blog/your-post.html
```

## Syntax highlighting classes

`.kw` keywords · `.ty` types · `.fn` function names · `.str` strings · `.mac` macros · `.cm` comments + attributes · `.num` numbers

## Publishing checklist

1. Add `.post-item` entry to top of list in `blog/index.html`
2. Remove the post from `blog/drafted-ai-crazies.html` (and drop its `noindex` tag)
3. `git add blog/your-post.html blog/index.html blog/drafted-ai-crazies.html`
4. `git commit -m "Publish: Title"`
5. `git push` — GitHub Pages deploys from `main` automatically

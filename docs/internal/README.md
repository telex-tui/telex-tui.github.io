# Internal docs

Working notes for maintaining this blog. Not published to the site.

Live site: https://telex-tui.github.io/ · Blog: https://telex-tui.github.io/blog/ · Drafts: https://telex-tui.github.io/blog/drafted-ai-crazies.html

## Repo structure

```
index.html          — site homepage
style.css           — all styles (one file, no build step)
blog/
  index.html        — blog listing page (published posts only)
  drafted-ai-crazies.html — "DraftedAICrazies": lists every hidden page
  drafti.html       — older working index for the challenge series
  draft*.html       — hidden drafts (unpublished)
  *.html            — individual posts
assets/             — images, favicon
docs/internal/      — this directory
```

No static site generator. Everything is hand-rolled HTML.

## Writing a new post

1. Create `blog/your-post-slug.html` matching the structure of an existing post.
2. Add `<meta name="robots" content="noindex, nofollow">` to its `<head>`.
3. List it on `blog/drafted-ai-crazies.html` (see below).
4. Preview locally (see below).
5. When ready to publish, follow "Publishing a post".

**Do not add to `blog/index.html` until the post is ready to publish.**

## Hidden posts and the DraftedAICrazies page

A post is hidden when its file is in `blog/` but it isn't listed in `blog/index.html`. Hidden is not private: anyone who has the URL can read it, and the repo itself is public.

`blog/drafted-ai-crazies.html` is the one page that lists **every** hidden page. It's linked from the nav on every page as "DraftedAICrazies", and it opens with a warning that this is a test bed for unfinished, AI-assisted work that readers shouldn't rely on, quote or share. The page itself has `noindex, nofollow`.

How it's organised:

- **Draft articles** — one `.post-item` card per hidden article (title, date, filename, one-line description). Currently `draft53.html` (Angle Brackets in Rust: Generics in Traits and Impls), `draft54.html` (What dyn Redacts: Type Erasure in Rust; its figures are in `assets/dyn-redaction/`), `20-rules-of-thumb-in-rust.html`, `draft51.html` (STM32), `draft3.html`, `draft2.html` and `draft1.html`.
- **Challenge series** — the two challenge series, each challenge with links to part A (problem + multiple choice) and part B (solution):
  - Rust Design Challenges 1–27 = `draft10a/b.html` … `draft36a/b.html`
  - Rust Idioms Challenges 1–14 = `draft37a/b.html` … `draft50a/b.html`
- `drafti.html` — the older "Draft Index" page for the challenges. It has its own inline styling and isn't linked from the nav; it's listed on DraftedAICrazies.

Naming: hidden articles have so far used `draftN.html` (next free number: `draft55`; a multi-part draft uses `draftN-part1.html`, `draftN-part2.html`, …), except `20-rules-of-thumb-in-rust.html`, which already has its final slug.

Keeping it current:

- New hidden page → add it to `blog/drafted-ai-crazies.html` in the same commit.
- Publishing a post → remove it from `blog/drafted-ai-crazies.html` in the same commit as adding it to `blog/index.html`.
- To check nothing is missing, every `blog/*.html` file should be linked from either `blog/index.html` or `blog/drafted-ai-crazies.html`:

```bash
cd blog
comm -23 <(ls *.html | grep -vx -e index.html -e drafted-ai-crazies.html | sort) \
         <(cat index.html drafted-ai-crazies.html | grep -o 'href="[^"/]*\.html"' | sed 's/href="//;s/"//' | sort -u)
# prints nothing when every page is listed
```

## Post structure

Every post follows this template:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Post Title - Telex</title>
  <meta name="description" content="One sentence.">
  <link rel="stylesheet" href="../style.css">
  <link rel="icon" type="image/png" href="../assets/telex-tui.png">
</head>
<body>
  <header>...</header>       <!-- identical across all posts -->

  <main class="container">
    <article class="post">
      <h1>Post Title</h1>
      <div class="post-meta">Month YYYY</div>
      <!-- content -->
    </article>
  </main>

  <footer>
    <div class="container">
      MIT - Copyright &copy; 2025 Mark Branion
    </div>
  </footer>
</body>
</html>
```

Series posts also include `.series-nav` and `.series-index` blocks — see any `rust-patterns-*.html` for the pattern.

## Syntax highlighting

Code blocks use `<pre><code>` with inline `<span>` classes. All defined in `style.css`:

| Class  | Color       | Use                        |
|--------|-------------|----------------------------|
| `.kw`  | purple      | keywords (`pub`, `fn`, `let`, `match`, `unsafe`) |
| `.ty`  | green       | types (`DataFrame`, `Vec`, `Option`) |
| `.fn`  | light purple| function names             |
| `.str` | blue        | string literals            |
| `.mac` | accent      | macros (`vec!`, `println!`) |
| `.cm`  | muted       | comments and attributes (`// ...`, `#[derive]`) |
| `.num` | blue        | number literals            |

HTML-escape all `<`, `>`, `&` inside code blocks.

ASCII diagrams go in bare `<pre>` blocks (no `<code>`). Same escaping rules apply.

## Local preview

```bash
python3 -m http.server 8080
```

Open `http://localhost:8080/blog/your-post.html`.

Must use a server — root-relative links (`/blog/`, `/`) break under `file://`.

## Publishing a post

1. Add a `.post-item` entry at the top of the list in `blog/index.html`, remove the post from `blog/drafted-ai-crazies.html`, and delete its `noindex` meta tag:

```html
<li class="post-item">
  <a href="your-post-slug.html">
    <h2>Post Title</h2>
    <div class="post-meta">Month YYYY</div>
    <p class="post-excerpt">One-sentence description.</p>
  </a>
</li>
```

2. Commit and push:

```bash
git add blog/your-post-slug.html blog/index.html blog/drafted-ai-crazies.html
git commit -m "Publish: Post Title"
git push
```

GitHub Pages deploys from `main` automatically. Live within ~30 seconds at
`https://telex-tui.github.io/blog/your-post-slug.html`.

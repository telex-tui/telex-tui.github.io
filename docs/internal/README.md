# Internal docs

Working notes for maintaining this blog. Not published to the site.

## Repo structure

```
index.html          — site homepage
style.css           — all styles (one file, no build step)
blog/
  index.html        — blog listing page
  *.html            — individual posts
assets/             — images, favicon
docs/internal/      — this directory
```

No static site generator. Everything is hand-rolled HTML.

## Writing a new post

1. Create `blog/your-post-slug.html` matching the structure of an existing post.
2. Preview locally (see below).
3. When ready to publish, add an entry to `blog/index.html` and commit + push.

**Do not add to `blog/index.html` until the post is ready to publish.**

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

1. Add a `.post-item` entry at the top of the list in `blog/index.html`:

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
git add blog/your-post-slug.html blog/index.html
git commit -m "Publish: Post Title"
git push
```

GitHub Pages deploys from `main` automatically. Live within ~30 seconds at
`https://telex-tui.github.io/blog/your-post-slug.html`.

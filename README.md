# blog.gtank.cc

Source for [blog.gtank.cc](https://blog.gtank.cc/), a [Hugo](https://gohugo.io/)
site deployed to GitHub Pages. Ported from a Ghost(Pro) export while preserving
each post's title, publish/update dates, tags, body content, and URL slug.

## Layout

- `content/posts/` — one Markdown file per post; `slug:` in front matter fixes
  the URL. Permalinks are `/:slug/` (see `hugo.toml`) to match the old Ghost URLs.
- `static/content/images/` — post images, kept at their original Ghost paths so
  in-body image URLs did not have to change.
- `static/CNAME` — custom domain for GitHub Pages (`blog.gtank.cc`).
- `themes/PaperMod` — theme (git submodule).
- `.github/workflows/hugo.yml` — builds and deploys on push to `main`.

## Local development

```sh
git clone --recurse-submodules <repo>
hugo server -D        # http://localhost:1313/
```

## Deploy

Push to `main`. The GitHub Actions workflow builds with Hugo extended and
publishes to GitHub Pages. In the repo's **Settings → Pages**, set the source to
**GitHub Actions**, and point `blog.gtank.cc` DNS at GitHub Pages.

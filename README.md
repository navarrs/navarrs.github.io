# navars.xyz

Personal website of Ingrid Navarro — publications and research posts.

Built with [Jekyll](https://jekyllrb.com/) 4 and the gem-based
[`bulma-clean-theme`](https://github.com/chrisrhymes/bulma-clean-theme). Every push to `master`
is built and deployed to GitHub Pages by
[.github/workflows/jekyll.yml](.github/workflows/jekyll.yml).

## Quick start

```bash
./bin/serve
```

Then open <http://localhost:4000>. Edit a file, save, and the page reloads by itself.

The script uses your local Ruby if you have one and falls back to Docker otherwise, so it works
with nothing installed but Docker. The **first run** pulls the `ruby:3.1` image and installs ~36
gems into `vendor/bundle/` — expect a few minutes. Later runs reuse that and start in seconds.

```bash
./bin/serve --detach      # run in the background instead of the foreground
./bin/serve --status      # is it up? on which ports?
./bin/serve --logs        # follow the log of a backgrounded server
./bin/serve --stop        # stop a backgrounded server
./bin/serve --port 4001   # port 4000 already taken
./bin/serve --build       # build once into _site/ and exit, no server
./bin/serve --docker      # force Docker even if Ruby is installed
./bin/serve --native      # force local Ruby
```

The server listens on localhost only; `--public` binds every interface instead, which is worth
avoiding on a shared or internet-facing machine.

There is no way to preview this site without building it: `assets/css/app.scss` imports the theme's
Sass from the gem, so opening the `.md` files or an unbuilt `index.html` in a browser shows nothing
useful. The first build also needs network access to reach rubygems.org.

## Preview with Docker (no Ruby needed)

This is what `./bin/serve` runs. Spelled out, in case you want to tweak it:

```bash
docker run --rm -it \
  --user "$(id -u):$(id -g)" \
  -e HOME=/tmp \
  -e GEM_HOME=/srv/vendor/bundle \
  -e BUNDLE_PATH=/srv/vendor/bundle \
  -v "$PWD":/srv -w /srv \
  -p 127.0.0.1:4000:4000 -p 127.0.0.1:35729:35729 \
  ruby:3.1 \
  bash -c 'export PATH="$GEM_HOME/bin:$PATH"
           gem install bundler -v 2.5.7 --no-document
           bundle install
           exec bundle exec jekyll serve --host 0.0.0.0 --livereload'
```

A few of those flags are load-bearing:

- **`ruby:3.1`** matches the Ruby pinned in CI. Avoid `jekyll/jekyll` (stale, ships an older Jekyll
  that conflicts with `Gemfile.lock`) and any `-alpine` image (`sass-embedded` / `google-protobuf`
  have no musl builds in the lockfile).
- **`--user "$(id -u):$(id -g)"`** — without it, `_site/`, `.jekyll-cache/` and `vendor/` end up
  root-owned in your working tree and you need `sudo` just to delete them.
- **`GEM_HOME` / `BUNDLE_PATH` under `vendor/bundle`** — that path is already in `.gitignore` and in
  the `exclude:` list in `_config.yml`. Any other location would get copied into the built site.
- **`-p 127.0.0.1:...`** publishes to loopback only. A bare `-p 4000:4000` binds *every* interface,
  which on a shared machine puts the preview on the network for anyone to reach.
- **Port `35729`** is livereload's websocket; without it `--livereload` won't refresh the browser.
- **`--host 0.0.0.0`** here is the *container's* interface, not the host's — the container has its
  own network namespace, and `-p 127.0.0.1:...` above is what restricts real access.

## Preview with a local Ruby

Use Ruby **3.1–3.3**. 3.1 matches CI; 3.4+ removed default gems that Jekyll 4.3.3 still expects,
and Ubuntu 22.04's `ruby-full` package is only 3.0.

```bash
rbenv install 3.1.7 && rbenv local 3.1.7   # or asdf, rvm, mise…
gem install bundler -v 2.5.7
bundle config set --local path vendor/bundle
bundle install
bundle exec jekyll serve --livereload
```

## Where the content lives

| Path | What it is |
| --- | --- |
| [index.md](index.md) | Homepage. Mostly composes the includes below. |
| [_includes/](_includes/) | `about.html`, `research.html`, `post-card.html` — the homepage sections. |
| [_publications/](_publications/) | One file per paper. Numeric filename prefix controls display order. |
| [_posts/](_posts/) | Research blog posts, named `YYYY-MM-DD-slug.md`, rendered with [_layouts/post.html](_layouts/post.html). |
| [_data/navigation.yml](_data/navigation.yml) | Top navigation links. |
| [assets/](assets/) | `img/`, `posts/<slug>/` for per-post images, `files/navars.pdf`, and `css/app.scss` (sets `$primary`, then imports the theme's Sass). |

A publication's front matter looks like this — set unused fields to `null` rather than deleting them:

```yaml
---
layout: default
title: "Paper title"
authors: A. Author, <b class="text-primary">Ingrid Navarro</b> and C. Author
where: Conference or journal, year
paper_url: https://arxiv.org/abs/...
code: https://github.com/...
poster: null
video: null
blogpost_link: /slug/     # link to a matching post in _posts/, or null
thumbnail: assets/img/publications/name.png
id: paper_name
abstract: "<p>…</p>"
---
```

## Deployment

Pushing to `master` triggers the workflow: Ruby 3.1, `bundle exec jekyll build` with
`JEKYLL_ENV=production`, then `actions/deploy-pages`. The custom domain comes from
[CNAME](CNAME). Your local `_site/` is never deployed — it exists only for previewing.

## Troubleshooting

- **Changes to `_config.yml` don't show up.** Jekyll doesn't watch that file. Restart the server.
- **Nothing rebuilds on save** (mostly macOS/Windows, where file events don't cross the bind mount):
  add `--force_polling` to the `jekyll serve` command.
- **`Address already in use`** — `./bin/serve --port 4001`. On a shared machine this usually means
  another user took 4000.
- **`Conflict. The container name "/navars-site" is already in use`** — a preview server is already
  running. `./bin/serve --status` to see it, `--stop` to clear it.
- **Stale or weird output** — delete `_site/` and `.jekyll-cache/`; both are gitignored and are
  regenerated on the next build.
- **`Could not find <gem> in locally installed gems`** — run `bundle install` again; the first one
  needs network access to rubygems.org.
- **Permission-denied on `_site/` or `vendor/`** — something ran Docker without `--user`. Fix with
  `sudo chown -R "$(id -u):$(id -g)" _site vendor`.

Two harmless quirks in `_config.yml`, so you don't chase them while debugging a build: `collections:`
is defined twice (YAML keeps the second, map-form one and silently drops the first), and
`menubar: research_menu` points at a `_data/research_menu.yml` that doesn't exist — inert, since
`show_sidebar: false`.

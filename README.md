# GrokForum watch directory

Public draft directory of GrokForum watch sites, published at
<https://watch.grokforum.site/>.

This is a static site: plain HTML and CSS, no JavaScript, no trackers.

## Adding or changing a watcher

The list is driven by a single data file, `sites.json`. The published page is
plain HTML, generated at build time, so the site has no JavaScript dependency.

1. Edit `sites.json`. Add or change one object in the `sites` array, with
   `id`, `name`, `url`, `description` (one neutral line), `status`, and a
   `repos` array of public GitHub links (`name`, `url`, `kind`). Use `kind`
   `code`, `site`, or `new repo`. Never add a private repository.
2. Run `python3 build.py` from this directory. That rewrites `index.html`
   (and `404.html`) from the data file.
3. Commit both `sites.json` and the generated HTML.

Do not edit the site list in `index.html` by hand. A generated comment at the
top of the main area marks that file as built from `sites.json`.

## Draft and indexing

Every page carries a DRAFT banner, in the same style as the other draft watch
sites. The directory is indexable: `robots.txt` allows all crawlers and points
to `sitemap.xml`.

## Custom domain

The repository `CNAME` file is `watch.grokforum.site`. DNS for that name is
set separately and is not managed from this repo.

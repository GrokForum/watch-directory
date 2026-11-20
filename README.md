# GrokForum watch directory

Public draft directory of GrokForum watch sites, published at
<https://watch.grokforum.site/>.

This is a static site: plain HTML and CSS, no JavaScript, no trackers.

## Adding or changing a watcher

The list is driven by a single data file, `sites.json`. The published page is
plain HTML, generated at build time, so the site has no JavaScript dependency.

1. Edit `sites.json`. Add or change one object in the `sites` array, with
   `id`, `name`, `url`, `path`, `description` (one neutral line), `status`, and a
   `repos` array of public GitHub links (`name`, `url`, `kind`). Use `kind`
   `code`, `site`, or `new repo`. Never add a private repository.
   The landing intro is the `intro` field at the top of `sites.json`.
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

## Later: serving the projects from this host

This site is currently a directory of links. The planned end state is that
`watch.grokforum.site` serves the projects themselves, from fresh, clean
public repositories, once each project has passed its gate.

The reserved path for each project is the `path` field in `sites.json`, for
example `/crimewatch/`, `/publicfunds/`, `/mediawatch/`, `/politicianwatch/`
and `/tenderwatch/`. The landing page stays at `/`. Those project folders
are not built yet. When a project is ready, point it at its reserved path
rather than only linking out to a separate host.

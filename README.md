# PEPR IA FOUNDRY website

FOUNDRY is a PEPR IA research project on the foundations of robustness
and reliability in artificial intelligence. The Hugo site uses the
Beautiful Hugo theme and runs on GitHub Pages.
[Live website](https://foundry-ai.github.io/) · [Beautiful Hugo](https://github.com/halogenica/beautifulhugo)

## Local preview and build

Use Git, Go, and Hugo Extended. The deployment workflow uses Hugo
Extended `0.150.0` and Go `1.25.1`. Use that Hugo version for production
checks. [Hugo 0.150.0 downloads](https://github.com/gohugoio/hugo/releases/tag/v0.150.0) · [Hugo installation guide](https://gohugo.io/installation/)

Initialize the theme submodule and start the preview server from the
repository root. An uncached build needs network access to fetch the
pinned theme module declared in `go.mod`.

```sh
git submodule update --init --recursive
hugo server --renderToMemory
```

Open the local URL printed by Hugo. The server watches source files for
changes. Add `--buildDrafts` to include draft pages in the preview.

Build into a fresh temporary directory to check production output.

```sh
build_dir=$(mktemp -d)
hugo --gc --minify --baseURL https://foundry-ai.github.io/ \
  --destination "$build_dir/public" --cacheDir "$build_dir/cache" --noBuildLock
```

## Project structure

| Location | Purpose |
| --- | --- |
| `hugo.toml` | Site settings, production URL, and main navigation |
| `content/_index.md` | Homepage text |
| `content/page/about.md` | Scientific abstract, consortium map, and guide to the main pages |
| `content/page/team.md` | Consortium overview and team roster |
| `content/page/publications/` | Publications index (`_index.md`) with introductory text and a HAL link, plus one Markdown file per paper |
| `content/page/news/` | News section index (`_index.md`) and individual news items |
| `archetypes/default.md` | Front matter template for new pages |
| `assets/` | Resources processed by Hugo, including the site logo |
| `static/` | Files copied directly to the site, including the consortium map image and styles |
| `static/css/foundry.css` | Shared site styles, including page titles and subtitles |
| `layouts/partials/` | Site-level head and footer hooks and shared publication markup |
| `layouts/page/publications/` | Templates for the publication list and individual paper pages |
| `layouts/tags/term.html` | Template for listings such as `/tags/conference/` |
| `layouts/shortcodes/consortium-map.html` | Shared interactive consortium map for the About and Team pages |
| `themes/beautifulhugo/` | Active Beautiful Hugo theme, pinned as a Git submodule |
| `.github/workflows/hugo.yaml` | GitHub Pages build and deployment workflow |
| `public/`, `resources/` | Generated output and resources, ignored by Git |
| `AGENTS.md` | Editing, verification, and release guidance for coding agents |

## Adding pages

Add regular pages under `content/page/` and news items under
`content/page/news/`. Hugo can create either kind from the default
archetype.

```sh
hugo new content page/my-page.md
hugo new content page/news/my-update.md
```

The archetype marks new pages as drafts. Set the draft flag to `false`
when the page is ready to publish. Add a `[[menu.main]]` entry in
`hugo.toml` if the page belongs in the main navigation.

### Adding publications

Add a Markdown file under `content/page/publications/` named after the
paper's HAL ID, for example `hal-05805492.md`. If a HAL ID is unavailable,
use the arXiv ID with an `arxiv-` prefix or a short title slug. Set `title`
in sentence case in the front matter, capitalizing the first word after
a colon. Put the abstract in the Markdown body. For a summary of the
paper, set `abstract_heading: "Summary"` under `params`.

Under `params`, set `authors` as a list in citation order, `venue`, and
`publication_date`. Add `hal_url`, `arxiv_url`, and `publication_url`
for verified HAL, arXiv, and publisher or conference records, respectively.
Use a quoted ISO date such as `"2026-12-09"`,
`"2026-12"`, or `"2026"` to match the precision of the source. The index
uses this date to list the newest papers first.

Add the year, publication type, and abbreviated venue to the top-level
`tags` field, for example `tags: ["2026", "conference", "NeurIPS"]`.
Use `conference`, `journal`, or `preprint` for papers and `book chapter`
or `software` for those HAL records. Preprints and software without a
venue use the year and type tags. Append each team represented in the
author list once, using the six team names from the
[Team roster](content/page/team.md). Count affiliate members as `GHOST`.
For publication tags, use `GHOST` for Victor Boone, `FAIRPLAY` for
Jung-Hun Kim and Hugo Richard, and `SCOOL` for Rémy Degenne.

Set `draft: true` for entries awaiting review and preview them with
`hugo server --renderToMemory --buildDrafts`.

## Publishing

A push or merge to `main` triggers the GitHub Pages release workflow.
The workflow checks out submodules, builds `public/`, and deploys the
generated artifact. It also supports manual runs from GitHub Actions.
[Build and deploy workflow](.github/workflows/hugo.yaml)

Publishing requires GitHub write access. After publishing, confirm the
workflow succeeded and open the affected pages on the live site.
[GitHub Actions runs](https://github.com/foundry-ai/foundry-ai.github.io/actions)

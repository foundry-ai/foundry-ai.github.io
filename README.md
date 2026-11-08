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
| `content/page/team.md` | Consortium overview and team roster |
| `content/page/publications.md` | Publications page with the project's HAL embed |
| `content/page/news/` | News section index (`_index.md`) and individual news items |
| `archetypes/default.md` | Front matter template for new pages |
| `assets/` | Resources processed by Hugo, including the site logo |
| `static/` | Files copied directly to the site, including the consortium map image and styles |
| `layouts/partials/` | Site-level head and footer template hooks |
| `layouts/shortcodes/consortium-map.html` | Team page map links and portrait labels |
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

## Publishing

A push or merge to `main` triggers the GitHub Pages release workflow.
The workflow checks out submodules, builds `public/`, and deploys the
generated artifact. It also supports manual runs from GitHub Actions.
[Build and deploy workflow](.github/workflows/hugo.yaml)

Publishing requires GitHub write access. After publishing, confirm the
workflow succeeded and open the affected pages on the live site.
[GitHub Actions runs](https://github.com/foundry-ai/foundry-ai.github.io/actions)

# Maintenance Instructions

This repository is the source for [my website](https://yiduo-wang-32.github.io/yiduo-wang-32/). The site is built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), a theme for [MkDocs](https://www.mkdocs.org/), which turns the Markdown files in `docs/` into a static website.

## Repository layout

| Path | What it holds |
| --- | --- |
| [`README.md`](./README.md) | The home page content: profile, about, research, publications, projects. It is shown on both GitHub and the website |
| [`docs/index.md`](./docs/index.md) | The website home page. It only pulls in `README.md`, so don't edit it |
| [`docs/images/`](./docs/images/) | Images, such as the portrait |
| [`docs/pdfs/`](./docs/pdfs/) | Paper PDFs linked from the publications list |
| [`docs/css/main.css`](./docs/css/main.css) | Custom styles, such as `.profile-pic` |
| [`docs/fonts/`](./docs/fonts/) | Custom fonts used by `main.css` |
| [`mkdocs.yml`](./mkdocs.yml) | Site settings: title, theme, colors, navigation links, Markdown extensions |
| [`.github/workflows/main.yml`](./.github/workflows/main.yml) | The GitHub Actions workflow that builds and deploys the site |

## Making quick changes online

For small text edits, edit [`README.md`](./README.md) directly on GitHub and commit. The site redeploys automatically (see [Deploying](#deploying)).

For bigger changes, set up a local copy so you can preview your edits first.

## Local setup

1. Create a dedicated environment. This requires [Miniconda](https://docs.anaconda.com/miniconda/install/) or [Anaconda](https://docs.anaconda.com/anaconda/install/).

   ```bash
   conda create -n mkdocs python
   conda activate mkdocs
   ```

2. Install Material for MkDocs, which also installs MkDocs, and the include-markdown plugin:

   ```bash
   pip install mkdocs-material mkdocs-include-markdown-plugin
   ```

3. Clone the repository:

   ```bash
   git clone git@github.com:yiduo-wang-32/yiduo-wang-32.git
   ```

## Updating content

- **Home page text:** edit `README.md`. The [include-markdown plugin](https://github.com/mondeja/mkdocs-include-markdown-plugin) copies it into `docs/index.md` when the site is built, so the GitHub profile and the website always match.
- **Adding a publication:** put the PDF in `docs/pdfs/` and add an entry to the Publications section of `README.md`, with a `([download](docs/pdfs/<file>.pdf))` link.
- **Images, CSS, fonts:** add files under `docs/`.
- **Navigation bar links:** edit the `nav:` section of `mkdocs.yml`.
- **Theme and colors:** edit the `theme:` section of `mkdocs.yml`.

### Writing `README.md` so it works in both places

- **Write paths relative to the repository root**, such as `docs/pdfs/paper.pdf`. Those paths work on GitHub, and the plugin rewrites them to `pdfs/paper.pdf` for the website.
- **Use an HTML `<img>` tag for images that need styling**, such as `<img src="docs/images/portrait.jpeg" class="profile-pic" width="240" height="240">`. GitHub doesn't support the `{.class}` attribute syntax and would show it as plain text. GitHub ignores the `class`, so `width` and `height` set the size there.
- **Put GitHub-only content below the `<!-- website-end ... -->` comment.** The website includes `README.md` only up to that comment, so anything below it, like the link to this file, appears only on GitHub.

## Previewing locally

```bash
mkdocs serve
```

Then open <http://127.0.0.1:8000>. The page reloads automatically when you save a file in `docs/`, `mkdocs.yml` or `README.md`.

## Deploying

Every push to `main` triggers the [GitHub Actions workflow](./.github/workflows/main.yml). The workflow builds the site and pushes it to the `gh-pages` branch, which GitHub Pages serves.

- The workflow checks out the repository with the `DEPLOY_KEY` repository secret. If deploys start failing at the checkout step, check that the secret and its matching deploy key still exist under *Settings → Secrets and variables → Actions* and *Settings → Deploy keys*.
- To build the site locally without deploying, run `mkdocs build`. The output goes to `site/`, which is git-ignored.
- To deploy manually from your machine, bypassing Actions, run `mkdocs gh-deploy`.

## Troubleshooting

**Git shows every file as modified, but nothing has changed.** This usually happens after the folder is copied or restored from a backup or cloud drive: the file permissions change, and Git counts that as a change. Run `git diff --summary`. If every line reads `mode change 100644 => 100755`, reset the permissions:

```bash
git ls-files -z | xargs -0 chmod 644
```

# docu-builder-theme

Org-wide **WinCC OA documentation theme** for
[`@winccoa-tools-pack/npm-winccoa-docu-builder`](https://github.com/winccoa-tools-pack/npm-winccoa-docu-builder)
and
[`winccoa-docu-builder`](https://github.com/winccoa-tools-pack/github-actions-winccoa/tree/main/actions/winccoa-docu-builder).

Single source of truth for HTML extras and **Doxygen Awesome** assets used by
WinCC OA projects in `winccoa-tools-pack`.

## What this repo contains

Top-level files only (the package merges **top-level** projectDocu files):

| File | Role |
| --- | --- |
| `extra_header.html` | Doxygen header + Awesome extension init |
| `extra_footer.html` | Community footer |
| `doxygen-awesome.css` | Doxygen Awesome base theme (v2.3.4) |
| `doxygen-awesome-sidebar-only.css` | Sidebar-only layout |
| `doxygen-awesome-sidebar-only-darkmode-toggle.css` | Dark-mode toggle styling |
| `doxygen-awesome-*.js` | Dark mode, copy button, paragraph link, TOC, tabs |
| `doxygen-awesome.LICENSE` | Upstream MIT license (jothepro) |

Project-specific advanced Doxygen fragments (aliases, WARN_LOGFILE, QHP, …)
stay in each consumer repo as `.winccoa-docu-builder/`.

## How consumers use it

### Recommended: GitHub Action inputs

```yaml
- uses: winccoa-tools-pack/github-actions-winccoa/actions/winccoa-docu-builder@main
  with:
    path: src/MyProject
    winccoa-version: '3.21'
    docker-image: ghcr.io/winccoa-tools-pack/winccoa:v3.21.3-debian12-all
    theme-repository: winccoa-tools-pack/docu-builder-theme
    theme-ref: main
    project-docu-paths: |
      .winccoa-docu-builder
```

The action checks out this repo into `.docu-builder-theme/` and prepends it to
`project-docu-paths` automatically.

### Manual checkout

```yaml
- uses: actions/checkout@v4
  with:
    repository: winccoa-tools-pack/docu-builder-theme
    ref: main
    path: .docu-builder-theme

- uses: winccoa-tools-pack/github-actions-winccoa/actions/winccoa-docu-builder@main
  with:
    project-docu-paths: |
      .docu-builder-theme
      .winccoa-docu-builder
```

### Local CLI

```bash
git clone https://github.com/winccoa-tools-pack/docu-builder-theme.git .docu-builder-theme
npx @winccoa-tools-pack/npm-winccoa-docu-builder build ./src/MyProject \
  -v 3.21 \
  --project-docu ./.docu-builder-theme \
  --project-docu ./.winccoa-docu-builder
```

## Merge order

Left → right:

1. **Theme** (this repo) — header/footer/CSS/JS
2. **Project** (`.winccoa-docu-builder`) — `advanced_doxygenConfig.txt` and overrides

`advanced_doxygenConfig.txt` fragments are concatenated. Other top-level files
are last-wins.

## Versioning

- Default consumers should pin `theme-ref: main` during early adoption, then
  move to annotated tags (`v1.0.0`, …) once the theme stabilizes.
- Doxygen Awesome is vendored at **v2.3.4**; bump deliberately and test HTML + QHP.

## License

- Theme packaging and WinCC OA community extras: MIT (see `LICENSE`)
- Doxygen Awesome CSS/JS: MIT (see `doxygen-awesome.LICENSE`)

---

<!-- markdownlint-disable-next-line MD033 -->
<center>Made with ❤️ for and by the WinCC OA community</center>
